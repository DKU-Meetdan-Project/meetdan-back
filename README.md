## 🍽️ Meetdan Backend

Meetdan은 단국대학교 학생들을 위한 과팅(미팅) 매칭 서비스입니다.  
본 레포지토리는 Meetdan 서비스의 백엔드 서버를 담당하며,  
Spring Boot 기반 REST API를 제공합니다.

<br>

## 📌 Project Overview

- 단국대학교 학생 대상 과팅 매칭 서비스
- 사용자 인증/인가, 미팅 도메인 로직, 검색, 동시성 제어 담당
- 실제 배포 환경을 고려한 구조 및 인프라 설계

<br>

## 🛠 Tech Stack

### Backend
- Java 17  
- Spring Boot  
- Gradle  
- Spring Data JPA, QueryDSL  
- Spring Security, JWT  

### Database & Cache
- MySQL 8  
- Redis  

### DevOps
- Docker, Docker Compose  
- NGINX  
- AWS (ECS, ECR, RDS, ELB, S3, CodeDeploy)  
- GitHub Actions  

### Test & Tools
- JUnit5, Mockito, TestContainers  
- IntelliJ IDEA, Notion, Slack  

<br>

## 📂 Project Structure

```text
Meetdan
 ├─ .github
 │   ├─ workflows
 │   └─ ISSUE_TEMPLATE
 │
 ├─ docker
 │   └─ docker-compose.yml
 │
 ├─ frontend
 │   ├─ dist/...
 │   ├─ public/...
 │   └─ src
 │       └─ components/...
 │
 ├─ backend
 │   └─ src
 │       ├─ main
 │       │   └─ java/com/meetdan/backend
 │       │       ├─ domain
 │       │       │   └─ api
 │       │       │       ├─ controller
 │       │       │       │   ├─ request
 │       │       │       │   └─ response
 │       │       │       ├─ persistence
 │       │       │       │   ├─ entity
 │       │       │       │   ├─ type
 │       │       │       │   └─ repository
 │       │       │       └─ service
 │       │       │
 │       │       └─ global
 │       │           ├─ config
 │       │           ├─ exception
 │       │           ├─ security
 │       │           ├─ redis
 │       │           ├─ aop
 │       │           ├─ utils
 │       │           └─ ...
 │       └─ test
 │
 └─ README.md
```

<br>

## ⚙️ Key Features & Technical Decisions

### 1. Docker Compose 기반 로컬 테스트 환경 구축

모든 서비스는 `docker-compose.yml`로 정의되어 있으며,  
컨테이너 간 네트워크를 통해 실제 배포 환경과 유사한 구조로 연동된다.

<br>

### 2. JWT + Refresh Token Rotation(RTR) 인증 구조

**목표**  
Refresh Token 탈취로 인한 보안 위협을 줄이고 인증 안정성을 강화한다.

**토큰 정책**
- Access Token TTL: 30분
- Refresh Token TTL: 7일

**동작 방식**
- Access Token 만료 시 Redis에 저장된 Refresh Token 검증
- 새로운 Access Token 발급
- 기존 Refresh Token 삭제 후 새 Refresh Token 발급 및 저장

**RTR 적용**
- Refresh Token은 매 재발급 시 교체
- Redis에 저장된 토큰과 클라이언트 토큰을 비교 검증
- 불일치 또는 폐기된 토큰 사용 시 요청 차단

<br>

### 3. MySQL Full Text Search 기반 검색 기능

**목표**  
두 글자 이상의 한글 검색을 지원하여 유연한 검색 기능을 제공한다.

**구현**
- `innodb_ft_min_token_size = 2` 설정
- Hibernate `FunctionContributor`를 활용해  
  `MATCH ... AGAINST` 구문을 JPA / QueryDSL에서 사용할 수 있도록 확장

<br>

### 4. TestContainers를 활용한 테스트 환경 구성

**목표**  
테스트 환경과 운영 환경 간의 차이로 발생하는 오류에서 최소화를 목표한다.

**구현 방식**
- H2 대신 MySQL, Redis 컨테이너 사용
- 테스트 실행 시 실제 DB와 동일한 환경 구성
- 모든 테스트에서 하나의 컨테이너 인스턴스를 공유하여 자원 낭비 최소화





