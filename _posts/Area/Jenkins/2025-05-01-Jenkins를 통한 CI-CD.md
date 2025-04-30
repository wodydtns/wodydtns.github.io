---
title: "사이드 프로젝트를 위한 Jenkins 구축하기"
date: 2025-05-01
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

# 설치 절차
## 젠킨스 저장소 키 추가
```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

```

## 젠킨스 저장소 저장
```
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

```

## 젠킨스 설치
```
sudo apt update
sudo apt install jenkins -y
```

## 포트번호 변경
- 젠킨스의 기본 포트는 8080
- 차후 프로젝트의 백엔드 사이드의 기본 포트가 8080이므로, 다른 포트로 변경함

### 포트 변경 시행착오
- 최초에 /etc/default/jenkins의 HTTP_PORT를 변경했으나, 기본 포트가 변경되지 않음
- systemd에서 관리함을 확인하고, /usr/lib/systemd/system/jenkins.service의 포트 번호를 변경함
- 해당 파일의 Environment="JENKINS_PORT=\[포트번호\]" 를 변경함

## 나머지
- 기본 추천 패키지로 설치
- 관리자를 만들지 않고, URL도 변경하지 않음

## 기본 설치 완료
![젠킨스 설치 완료](/assets/images/jenkins/jenkins1.png)