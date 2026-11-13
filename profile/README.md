# 내 곁에 (위급 상황 실시간 위치 공유 안심 서비스)

> "위급한 순간, 원터치로 내 위치를 가장 믿을 수 있는 사람에게"

---

## 목차

1. [프로젝트 개요](#프로젝트-개요)
2. [팀원 소개](#팀원-소개)
3. [주요 기능](#주요-기능)
4. [App](#app)
5. [Backend](#backend)
6. [시스템 아키텍처](#시스템-아키텍처)
7. [Repositories](#repositories)

---

## 프로젝트 개요

폭력, 납치, 급박한 신체 위협 같은 위기 상황에서는 피해자가 직접 112에 전화를 걸거나 상황을 말로 설명하기 어렵습니다. 반대로 기존 위치 공유 앱은 평상시에도 위치를 계속 수집해 사생활 침해 우려와 배터리 소모가 크고, 실제 위기 상황에서 나를 도와줄 가족, 친구와의 신뢰 기반 연결망도 부족합니다.

**내 곁에**는 도움 요청 버튼 한 번으로 미리 연결해 둔 지인에게 긴급 알림과 현재 위치를 즉시 전달하고, 호출이 진행되는 동안 실시간 위치를 공유해 지인이 신고 등 대리 대응을 빠르게 할 수 있도록 돕는 실시간 안심 보호 서비스입니다.

### 주요 특징

- **필요할 때만 위치 공유**: 평상시에는 위치를 수집하지 않고, 긴급 호출 시에만 위치를 전송
- **원터치 긴급 전파**: 호출 즉시 연결된 지인에게 고우선순위 푸시와 주소 정보 전달
- **실시간 위치 추적**: 호출이 진행되는 동안 발신자의 위치를 수신자에게 실시간 전달
- **신뢰 기반 지인 네트워크**: 초대와 수락으로 연결된 지인끼리만 호출과 위치를 주고받음
- **개인정보 보호**: 전화번호와 디바이스 토큰을 암호화해 저장하고, 만료된 호출과 위치 데이터를 자동 정리

---

## 팀원 소개

| 역할          | 이름   |
| ------------- | ------ |
| Frontend / PM | 오영일 |
| Backend       | 손세영 |

---

## 주요 기능

| 기능             | 설명                                                                 |
| ---------------- | -------------------------------------------------------------------- |
| 회원가입 / 로그인 | 이메일 인증 기반 일반 회원가입, 비밀번호 재설정                      |
| 소셜 로그인      | Kakao, Google, Apple 로그인과 신규 회원 온보딩                       |
| 지인 연결        | 전화번호 기반 사용자 검색, 지인 초대(수락/거절/취소), 사용자 차단    |
| 긴급 호출        | 원터치 호출 생성과 종료, 최초 위치와 주소 전달                       |
| 실시간 위치 공유 | 호출 중 발신자 위치 실시간 전송, 사용자별 공유 ON/OFF 설정           |
| 수신 알림 / 지도 | 받은 긴급 알림 목록과 상세, 지도 마커와 이동 경로 조회               |
| 푸시 알림        | 긴급 호출 시 지인에게 푸시 알림 발송                                 |
| 마이페이지       | 프로필 조회/수정, 프로필 이미지 업로드, 회원 탈퇴                    |

---

## App

### 기술 스택

| 항목             | 사용 기술 / 라이브러리                 |
| ---------------- | -------------------------------------- |
| Framework        | React Native, Expo (Expo Router)       |
| Language         | TypeScript                             |
| Server State     | TanStack Query, Axios                  |
| Form / Validation | React Hook Form, Zod                  |
| 지도 / 위치      | React Native Maps, Expo Location       |
| Platform         | Android / iOS                          |

### 앱 프로젝트 구조

```
src/
├── api/                # API 클라이언트
├── app/                # Expo Router 기반 라우팅
│   └── (tabs)/         # 탭 화면
├── components/         # 공통 컴포넌트
│   ├── icons/
│   └── ui/
├── constants/          # 상수
├── hooks/              # 커스텀 훅
└── lib/                # 유틸리티
```

---

## Backend

### 기술 스택

| 항목            | 사용 기술 / 라이브러리                                         |
| --------------- | -------------------------------------------------------------- |
| Framework       | Spring Boot 4.1 (Java 17)                                      |
| 실시간 통신     | Spring WebSocket (STOMP)                                       |
| Security        | Spring Security, JWT, OAuth2/OIDC 토큰 검증                    |
| Database        | MySQL 8.4, Spring Data JPA, Flyway                             |
| Cache / Session | Redis 7                                                        |
| 외부 연동       | Firebase Cloud Messaging, Azure Blob Storage, Kakao 로컬 API   |
| API 문서        | Springdoc OpenAPI (Swagger UI), AsyncAPI                       |

### 백엔드 프로젝트 구조

```
src/main/java/com/gyeote/gyeote_api
├── domain/             # 비즈니스 도메인
│   ├── auth/           # 회원가입, 로그인, 소셜 로그인
│   ├── user/           # 프로필, 위치 공유 설정
│   ├── contact/        # 지인 연결, 차단
│   ├── emergency/      # 긴급 호출, 위치 갱신
│   ├── alert/          # 수신 긴급 알림
│   ├── location/       # 지도 마커, 이동 경로
│   ├── device/         # 디바이스 토큰
│   └── notification/   # 푸시 알림 발송
└── global/             # 공통 인프라 (보안, 설정, 예외, 스토리지, WebSocket)
```

### 배포

- **배포 환경**: Azure VM (Docker)
- **CI/CD**: GitHub Actions, Azure Container Registry
- **무중단 배포**: Nginx 기반 Blue-Green 배포

---

## 시스템 아키텍처

```
┌──────────────────────────┐
│   Mobile App              │  React Native (Expo)
└────────────┬─────────────┘
             │  REST (HTTPS) / STOMP over WebSocket
┌────────────▼─────────────┐
│   Nginx                   │  리버스 프록시, Blue-Green 전환
└────────────┬─────────────┘
             │
┌────────────▼─────────────┐
│   Spring Boot API         │  Azure VM (Docker)
└────────────┬─────────────┘
             │
┌────────────▼──────────────────────────────┐
│   Data / External Services                  │
│  MySQL / Redis                              │
│  FCM / Azure Blob Storage / Kakao 로컬 API  │
│  Kakao·Google·Apple 로그인                  │
└─────────────────────────────────────────────┘
```

---

## Repositories

| Repo                                                                       | 설명                           |
| -------------------------------------------------------------------------- | ------------------------------ |
| [By-My-Side-App](https://github.com/By-My-Side-Dev/By-My-Side-App)         | 모바일 앱 (React Native, Expo) |
| [By-My-Side-Backend](https://github.com/By-My-Side-Dev/By-My-Side-Backend) | 백엔드 API 서버 (Spring Boot)  |

---

내 곁에 © 2026
