# 🍽️ Meetdan Backend

**Meetdan**은 대학생을 위한 미팅(과팅) 매칭 서비스입니다.  
본 레포지토리는 Meetdan 서비스의 **백엔드 서버**를 담당합니다.

소규모 사용자(100명 미만)를 대상으로 하며,  
**무료 인프라(Render, Firebase 등)**를 활용해 운영되는 프로젝트입니다.

---

## 🧩 프로젝트 개요

- 대학생 미팅 / 과팅 매칭 서비스
- 사용자 프로필 기반 매칭
- 매칭 결과 및 알림 제공
- 소규모 트래픽을 고려한 경량 백엔드 구조

---

## 🛠️ 기술 스택

### Backend
- **Java 21 (JDK 21)**
- **Spring Boot**
- **Spring Web**
- **Spring Data JPA**
- **Spring Security**
- **JWT (Json Web Token)**

### Database
- **MySQL**
  - JPA / Hibernate 기반 ORM 사용
  - 개발 및 배포 환경 분리

### Infra / Deployment
- **Render (Free Plan)**
  - Spring Boot API 서버 배포
  - GitHub 연동 자동 배포
- **GitHub**
  - 소스 코드 관리 및 협업

### Notification
- **Firebase Cloud Messaging (FCM)**
  - 매칭 성사 및 주요 이벤트 알림
  - Android / iOS 푸시 알림 지원
  - 무료 플랜 사용

---

## 📦 시스템 아키텍처
```
Client (Android / iOS)
        ↓
Spring Boot Backend (Render)
        ↓
MySQL Database
        ↓
Firebase Cloud Messaging
```

---

## 🔐 인증 방식

- **JWT 기반 인증**
- Access Token 사용
- 로그인 / 회원가입 API 분리
- 인증이 필요한 API에 대해 Spring Security 필터 적용

---

## 🚀 배포 환경

| 항목 | 내용 |
|----|----|
| Backend Server | Render (Free Plan) |
| Database | MySQL |
| Notification | Firebase Cloud Messaging |
| 예상 사용자 수 | 100명 미만 |

> 본 프로젝트는 비용 발생 없이 운영 가능한 구조를 목표로 설계되었습니다.

---

## ⚙️ 환경 변수 설정

Render 배포 시 아래 환경 변수를 설정해야 합니다.
```env
SPRING_PROFILES_ACTIVE=prod

DB_URL=jdbc:mysql://...
DB_USERNAME=...
DB_PASSWORD=...

JWT_SECRET_KEY=...

FIREBASE_PROJECT_ID=...
FIREBASE_PRIVATE_KEY=...
FIREBASE_CLIENT_EMAIL=...
```

---

## 📂 패키지 구조 (예시)
```
com.meetdan.backend
 ┣ global
 ┃ ┣ config
 ┃ ┣ security
 ┃ ┣ apiPayload
 ┣ domain
 ┃ ┣ user
 ┃ ┣ meeting
 ┃ ┣ matching
 ┣ controller
 ┣ service
 ┣ repository
 ┗ MeetdanApplication.java
```

---

## 🔔 알림 기능 (Firebase)

- 매칭 성사 시 푸시 알림 전송
- 주요 이벤트 발생 시 FCM 기반 알림 처리
- Firebase Admin SDK 사용

---

## 🧪 테스트

- Postman을 활용한 API 테스트
- 핵심 비즈니스 로직 위주 단위 테스트 진행

---

## 📌 기타

- 본 프로젝트는 학습 및 포트폴리오 목적으로 제작되었습니다.
- 대규모 트래픽 및 상용 환경은 고려하지 않았습니다.

