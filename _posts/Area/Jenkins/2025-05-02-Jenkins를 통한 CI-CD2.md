---
title: "사이드 프로젝트를 위한 Jenkins 구축하기2"
date: 2025-05-02
categories: [DevOps, Infrastructure]
tags: [Jenkins, CI/CD, Ubuntu, Server Setup, Side Project, Installation Guide]

---

## 들어가며
> 이번 포스트는 책 내용을 참고하지 않고, AI & 구글 검색을 통해 수행하였습니다.
> 제 홈서버인 ubuntu를 기반으로 하고 있습니다  
{: .prompt-tip }

# 계기
- 사이드 프로젝트에서 ci/cd를 젠킨스로 선정함
    - 세팅을 많이 해보지 않는 상태로 레퍼런스가 많고, 성능이 어느정도 검증된 것을 선택하는 것이 좋다고 판단함


# GitHub Repository에서 Spring boot 소스를 받아 docker를 통해 배포해보기
## Jenkins의 플러그인 설치하기
### 설치 목록
|플러그인 구분| 플러그인 이름|
|------|------|
| 소스 | Git |
| 소스 | Git Integration |
| 소스 | GitHub Integration |
| 가상환경 | Docker |
| 가상환경 | Docker Pipeline |
| 파이프라인 | Pipeline |

- Git, Github,Pipeline 플러그인은 최초 추천 설치 플러그인으로 이미 설치되어 있었음
- 나머지는 검색해서 설치함

## Jenkins에 Github 연결 설정하기
1. Jenkins 관리 > Credential 추가 
![젠킨스 Credentail 추가](/assets/images/jenkins/Jenkins2.png)
- 설정에서 github 에서 personal Access Token 을 발급하고, Secret Text에 추가함
- Secret Text로 생성했더니 jenkins 빌드 시 authentication error가 발생함
    - Username with password로 변경해 credential을 생성하니 인증 에러 해결!

2. Jenkins 대시보드 > System > 중간쯤에 Github Server 에서 Add Github Server 클릭
![젠킨스 Github Server 추가](/assets/images/jenkins/Jenkins3.png)
    - Test Connection 성공 시 연결 완료

## 배포를 위한 JDK 설정
- ![JDK 설정](/assets/images/jenkins/Jenkins4.png)
    - 해당 경로에 ubuntu에 설정한 jdk 경로 지정

## Jenkins 파이프라인 생성
- git repository 연결
![git repository 연결](/assets/images/jenkins/Jenkins5.png)
- 파이프라인 스크립트 추가

```gradle
pipeline {
    agent any
    
    tools {
        jdk 'openjdk-21'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // 재시도 옵션 추가 및 shallow clone 사용
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: 'main']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [
                        [$class: 'CloneOption', depth: 1, noTags: false, reference: '', shallow: true],
                        [$class: 'CheckoutOption', timeout: 30],
                        [$class: 'CleanBeforeCheckout']
                    ],
                    submoduleCfg: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/wodydtns/psychologyTest',
                        credentialsId: 'github-PAT'
                    ]]
                ])
                sh 'ls -la'
            }
        }
        
        stage('Build') {
            steps {
                // 저장소 클론이 성공했을 때만 실행
                dir('back/psychologyTest') {
                    sh 'ls -la'
                    script {
                        if (fileExists('gradlew')) {
                            sh 'chmod +x ./gradlew'
                            sh './gradlew clean build'
                        } else if (fileExists('mvnw')) {
                            sh 'chmod +x ./mvnw'
                            sh './mvnw clean package'
                        } else {
                            error 'Neither Gradle nor Maven wrapper found'
                        }
                    }
                }
            }
        }
        
        stage('Docker Build') {
            steps {
                dir('back/psychologyTest') {
                    sh 'if [ ! -f Dockerfile ]; then echo "Dockerfile not found"; exit 1; fi'
                    sh 'docker build -t springboot-app:${BUILD_NUMBER} .'
                    sh 'docker images | grep springboot-app'
                }
            }
        }
        
        stage('Docker Deploy') {
            steps {
                sh 'docker ps -a | grep springboot-app && docker stop springboot-app || echo "No container to stop"'
                sh 'docker ps -a | grep springboot-app && docker rm springboot-app || echo "No container to remove"'
                sh 'docker run -d -p 8080:8080 --name springboot-app springboot-app:${BUILD_NUMBER}'
                sh 'docker ps | grep springboot-app'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            cleanWs()
        }
    }
}

```
## 문제점 해결 과정
1. spring boot의 자바 버전과 jenkins의 자바 버전 차이
    - spring boot는 17, jenkins는 21을 사용
    - spring boot를 21로 변경함(gradle에서 수정)
2. docker 파일 오작성
    - docker 파일에서 복사할 파일 이름을 잘못 작성했음

## 차후 해야할 것
- 스크립트에 대한 분석
    - 개략적으론 보았지만, 절차를 정확하게 모르겠으므로 디버깅에 취약할 수 있어 분석 해야겠음