---
title: "사이드 프로젝트를 위한 Jenkins 구축하기3 - 파이프라인 훑어보기"
date: 2025-05-03
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

# 젠킨스 파이프라인 훑어보기
## 젠킨스 파이프라인
- 젠킨스에 강력한 자동화 도구 세트를 추가하는 플러그인 모음
- 파이프라인은 코드로 정의된 일련의 자동화 프로세스
- 복잡한 빌드, 테스트, 배포 과정을 일관되게 자동화 해줌

## 파이프라인 플로우 차트 예시
![파이프라인 플로우 차트 예시](/assets/images/jenkins/Jenkins6.png)


## 파이프라인 주요 구성 요소
### Jenkinsfile
- 파이프라인의 정의를 담고 있는 텍스트 파일
- 소스 코드 관리 시스템에 저장
- Jenkinsfile은 프로젝트의 빌드, 테스트, 배포 단계를 정의하며, 이를 통해 파이프라인을 코드로 관리

### Stage
- 파이프라인의 주요 단계를 정의하는 블록
- 각 스테이지는 빌드, 테스트, 배포와 같은 특정 작업을 수행
- 스테이지는 파이프라인의 진행 상황을 시각적으로 보여주는 역할도 수행

### Step
- 스테이지 내에서 실행되는 개별 작업 단위
- Jenkins에게 무엇을 수행할지 지시
- 선언적 파이프라인과 스크립트 파이프라인 모두에서 기본 구성 요소로 사용

## 파이프라인의 두 가지 문법
### 선언적 파이프라인(Declarative Pipeline)과 스크립트 파이프라인(Scripted Pipeline)
|특성|선언적 파이프라인|스크립트 파이프라인|
|--|--|--|
|문법| 더 엄격하고 구조화된 문법| Groovy 기반의 유연한 문법|
시작 구문|	pipeline { } 블록으로 시작|	node { } 블록으로 시작|
학습 곡선|	상대적으로 낮음, 초보자에게 적합|	상대적으로 높음, Groovy 지식 필요|
유연성|	제한적, 사전 정의된 구조 내에서만 작동|	매우 높음, 복잡한 로직 구현 가능|
프로그래밍 모델|	선언적(declarative) 프로그래밍 모델|	명령형(imperative) 프로그래밍 모델|
오류 처리|	내장된 오류 처리 메커니즘 (post 섹션)|	직접 try-catch 블록 구현 필요|
조건문|	when 지시어 사용|	일반 Groovy 조건문 사용 (if-else)|
병렬 실행|	parallel 지시어로 간단히 구현|	더 복잡한 구현 필요|
재사용성|	공유 라이브러리를 통해 제한적|	함수 정의와 호출로 높은 재사용성|
가독성|	높음, 표준화된 구조|	복잡한 파이프라인에서는 낮을 수 있음|
유지보수|	일반적으로 더 쉬움|	복잡한 코드는 유지보수가 어려울 수 있음|
단계 정의|	stages와 steps 블록 필수|	더 자유로운 구조|
### 선언적 파이프라인(Declarative Pipeline) 예시
```
pipeline {
    agent any
    
    environment {
        MAVEN_HOME = tool 'Maven'
        VERSION = '1.0.0'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean package"
                echo "Building version ${VERSION}"
            }
        }
        
        stage('Test') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn test"
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo "Deploying to production..."
                // 배포 스크립트
            }
        }
    }
    
    post {
        success {
            echo "Pipeline completed successfully!"
            emailext (
                subject: "Build Successful: ${currentBuild.fullDisplayName}",
                body: "The build was successful.",
                to: "team@example.com"
            )
        }
        failure {
            echo "Pipeline failed!"
            emailext (
                subject: "Build Failed: ${currentBuild.fullDisplayName}",
                body: "The build failed. Please check the logs.",
                to: "team@example.com"
            )
        }
    }
}

```
### 스크립트 파이프라인(Scripted Pipeline) 예시
```
node {
    def mvnHome = tool 'Maven'
    def version = '1.0.0'
    
    try {
        stage('Checkout') {
            checkout scm
        }
        
        stage('Build') {
            sh "${mvnHome}/bin/mvn clean package"
            echo "Building version ${version}"
        }
        
        stage('Test') {
            sh "${mvnHome}/bin/mvn test"
            junit '**/target/surefire-reports/*.xml'
        }
        
        // 조건부 배포
        stage('Deploy') {
            if (env.BRANCH_NAME == 'main') {
                echo "Deploying to production..."
                // 배포 스크립트
            } else {
                echo "Skipping deployment for branch ${env.BRANCH_NAME}"
            }
        }
        
        // 성공 알림
        currentBuild.result = 'SUCCESS'
        emailext (
            subject: "Build Successful: ${currentBuild.fullDisplayName}",
            body: "The build was successful.",
            to: "team@example.com"
        )
    } catch (Exception e) {
        // 실패 처리
        currentBuild.result = 'FAILURE'
        emailext (
            subject: "Build Failed: ${currentBuild.fullDisplayName}",
            body: "The build failed: ${e.message}",
            to: "team@example.com"
        )
        throw e
    } finally {
        // 항상 실행되는 정리 작업
        echo "Pipeline execution completed"
    }
}

```
## 파이프라인의 장점
1. 코드로서의 파이프라인
    - 빌드 프로세스를 코드로 정의하여 버전 관리 시스템에서 관리

2. 내구성
    - Jenkins 서버가 예기치 않게 다시 시작되더라도 파이프라인 작업 지속 가능

3. 일시 중지 가능
    - 파이프라인은 사용자 입력을 기다리기 위해 일시 중지 가능

4. 다양한 기능
    - 조건부 실행, 병렬 실행, 루프, 변수 등 다양한 프로그래밍 기능을 지원
5. 확장성
    - 공유 라이브러리를 통해 공통 파이프라인 코드를 재사용 가능

## 배운 점
- 젠킨스의 기본 파이프라인 구조를 이해할 수 있었음