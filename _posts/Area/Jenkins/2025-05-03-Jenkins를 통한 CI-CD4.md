---
title: "사이드 프로젝트를 위한 Jenkins 구축하기4 - 파이프라인 문법 알아보기"
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

# 젠킨스 파이프라인 문법 알아보기
## 주요 파이프라인 문법 유형
- 파이프라인은 앞서 알아본 바와 같이 두 가지 유형이 있다
    - 선언적 파이프라인
    - 스크립트 파이프라인

## 선언적 파이프라인 문법 요소
### 1. 기본 구조 요소
- pipeline: 모든 선언적 파이프라인의 최상위 블록 
- agent: 파이프라인 또는 특정 단계를 실행할 환경을 지정 
- stages: 모든 stage 블록의 컨테이너 
- stage: 각 개별 단계를 정의하는 블록 
- steps: 각 stage 내에서 실행할 작업들을 정의 
### 2. 섹션 요소
- environment: 환경 변수 설정을 위한 섹션 
- options: 파이프라인 옵션 설정 (예: timeout, timestamps) 
- parameters: 파이프라인 매개변수 정의 
- triggers: 파이프라인 트리거 방식 정의 
- tools: 자동으로 설치하고 PATH에 추가할 도구 지정 
### 3. 조건부 및 제어 구문
- when: 조건부 실행을 위한 지시어 
- parallel: 병렬 실행을 위한 지시어 
- post: 스테이지나 파이프라인 완료 후 실행할 작업 정의 (always, success, failure 등)

## 스크립트 파이프라인 문법 요소
### 1. 기본 구조 요소
- node: 작업이 실행될 젠킨스 에이전트를 지정하는 블록 
- stage: 파이프라인의 개별 단계를 정의 
### 2. Groovy 프로그래밍 요소
- 변수 선언: def variable = value 형식으로 변수 선언 
- 조건문: if-else, switch-case 등의 일반 Groovy 조건문 
- 반복문: for, while 등의 루프 구문 
- 함수 정의: 재사용 가능한 함수 생성 
- 예외 처리: try-catch-finally 블록을 사용한 오류 처리 
### 3. 고급 기능
- 클로저(Closures): Groovy의 클로저 문법을 활용한 콜백 함수 
- 병렬 처리: parallel() 함수를 사용한 병렬 작업 실행 
- 동적 단계 생성: 런타임에 동적으로 파이프라인 단계를 생성 

## 공통 문법 요소 (DSL)
### 1. 기본 단계(Steps)
- sh: 쉘 명령어 실행 
- bat: Windows 배치 명령어 실행 
- echo: 메시지 출력 
- checkout: SCM에서 코드 체크아웃 
- dir: 특정 디렉토리에서 명령 실행 
### 2. 고급 단계
- withCredentials: 보안 자격 증명을 사용하는 블록 
- withEnv: 특정 환경 변수를 설정하는 블록 
- timeout: 시간 제한 설정 
- retry: 실패 시 재시도 
- emailext: 이메일 알림 전송 

## 공유 라이브러리 문법
- @Library: 공유 라이브러리를 가져오는 어노테이션 
- vars/: 전역 변수 및 함수를 정의하는 디렉토리 
- src/: 클래스 및 더 복잡한 코드 구조를 위한 디렉토리 
- resources/: 비코드 리소스를 저장하는 디렉토리 

## 개인적인 궁금증1
- stage('Docker Build')에서 'Docker Build'는 이미 정의되어 있는 값인가?
    - 이 값들은 사용자 정의값
    - 보편적인 파이프라인 스테이지 이름을 검색함

### 일반적인 젠킨스 파이프라인 스테이지 유형
| 스테이지 이름 | 목적 | 일반적인 명령어 예시 |
|------------|------|-------------------|
| `Checkout` | 소스 코드 저장소에서 코드를 가져옴 | `checkout scm`, `git clone` |
| `Initialize` | 환경 변수 설정 및 초기화 작업 수행 | `env.BUILD_VERSION = "1.0.${BUILD_NUMBER}"` |
| `Build` | 소스 코드 컴파일 및 빌드 | `mvn clean package`, `gradle build`, `npm run build` |
| `Unit Test` | 단위 테스트 실행 | `mvn test`, `npm test`, `pytest` |
| `Integration Test` | 통합 테스트 실행 | `mvn integration-test`, `npm run test:integration` |
| `Code Analysis` | 코드 품질 및 보안 취약점 분석 | `sonar-scanner`, `eslint`, `checkstyle` |
| `Security Scan` | 보안 취약점 스캔 | `owasp dependency-check`, `snyk test` |
| `Package` | 애플리케이션 패키징 | `mvn package`, `npm pack`, `jar -cvf` |
| `Publish Artifacts` | 빌드 결과물 저장 | `archiveArtifacts artifacts: 'target/*.jar'` |
| `Docker Build` | Docker 이미지 생성 | `docker build -t myapp:${BUILD_NUMBER} .` |
| `Docker Push` | Docker 이미지를 레지스트리에 업로드 | `docker push myapp:${BUILD_NUMBER}` |
| `Deploy to Dev` | 개발 환경에 배포 | `kubectl apply -f k8s/dev/`, `helm upgrade` |
| `Deploy to Test` | 테스트 환경에 배포 | `kubectl apply -f k8s/test/` |
| `Deploy to Staging` | 스테이징 환경에 배포 | `kubectl apply -f k8s/staging/` |
| `Deploy to Production` | 프로덕션 환경에 배포 | `kubectl apply -f k8s/prod/` |
| `Smoke Test` | 배포 후 기본 기능 테스트 | `curl -f https://app.example.com/health` |
| `Performance Test` | 성능 테스트 실행 | `jmeter -n -t perf-test.jmx` |
| `Approval` | 수동 승인 단계 | `input message: '프로덕션 배포를 승인하시겠습니까?'` |
| `Rollback` | 문제 발생 시 이전 버전으로 롤백 | `kubectl rollout undo deployment/myapp` |
| `Notification` | 빌드 결과 알림 | `mail to: 'team@example.com', subject: 'Build Result'` |
| `Cleanup` | 임시 파일 및 리소스 정리 | `docker system prune -f`, `rm -rf target/` |

## 개인적인 궁금증2
- stage - steps 의 관계
### stage의 값(이름)
- stage('이름') 에서 따옴표 안의 값은 관리자/개발자가 임의로 지정한 값
- 단순히 파이프라인의 해당 단계를 식별하고 UI에 표시하기 위한 레이블이며, 특별한 기능 혹은 동작을 정의하지 않음

### steps 블록
- steps 블록은 실제로 해당 stage에서 실행할 명령어나 작업을 정의
- 쉘 명령어, 젠킨스 플러그인 호출, 변수 설정 등 실제 작업 내용이 포함
- 실제 기능 수행은 이 steps 내부의 명령어에 의해 이루어짐
