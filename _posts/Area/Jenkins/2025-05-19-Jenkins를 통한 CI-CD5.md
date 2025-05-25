---
title: "사이드 프로젝트를 위한 Jenkins 구축하기4 - 파이프라인 문법 알아보기"
date: 2025-05-19
categories: [DevOps, Infrastructure]
tags: [Jenkins, CI/CD, Ubuntu, Server Setup, Side Project, Installation Guide]

---

## 들어가며
> 이번 포스트는 책 내용을 참고하지 않고, AI & 구글 검색을 통해 수행하였습니다.
> 제 홈서버인 ubuntu를 기반으로 하고 있습니다  
{: .prompt-tip }

## 빌드 트리거 설정
![트리거 설정](/assets/images/jenkins/Jenkins7.png)

## Github 웹 훅 설정
![Github webhook 설정](/assets/images/jenkins/Jenkins8.png)
- Payload URL: http://your-jenkins-url/github-webhook/
- Content type: application/json
- 이벤트 선택: 필요한 이벤트 선택 (일반적으로 "Just the push event")

## 파이프라인 스크립트에 트리거 추가
```
triggers {
        githubPush() // GitHub 푸시 이벤트에 반응하는 트리거 추가
    }
```