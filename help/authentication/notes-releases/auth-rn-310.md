---
title: Adobe Pass Authentication 3.1.0 릴리스 노트
description: Adobe Pass Authentication 3.1.0 릴리스 노트
exl-id: cf9fc8e2-4b37-4b0a-a6ed-cda1b6738e76
source-git-commit: 0c6aec04ae9df410228730b5bce6ced1aeecd312
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Adobe Pass Authentication 3.1.0 릴리스 노트 {#authn-310-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-310}

* [빌드 번호](#build-number-310)
* [릴리스 개요](#release-overview-310)

### 빌드 번호 {#build-number-310}

Adobe Pass 인증: adobe-pass-**3.1.0**

릴리스 날짜: **2/25/2025 - 02/27/2025**

### 릴리스 개요 {#release-overview-310}

#### REST API v2

* REST API v2 `partner_logout`로그아웃 API`partner_interactive` 응답에서 일반 로그아웃과 파트너 SSO(Single Sign-On) 로그아웃을 구별하기 위한 새 [&#x200B; 작업 이름 및 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) 작업 유형입니다.
* REST API v2 `reason`세션 API[&#x200B; 및 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)세션 SSO API[&#x200B; 응답에서 작업 이름에 대한 자세한 정보를 제공하는 새 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) 필드.

#### 버그 수정

* Spectrum 구독자가 REST API v2 [Authenticate API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)를 통해 인증하지 못하는 문제를 해결했습니다.
* REST API V2 [인증 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)를 통해 생성된 이벤트가 ESM에서 제대로 집계되지 않는 문제를 해결했습니다.
* REST API v2 `notBefore`프로필 API[&#x200B; 응답에서 사용자 프로필에 대해 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) 타임스탬프를 잘못 계산하던 문제를 수정했습니다.

#### JavaScript SDK

* [AccessEnabler JavaScript SDK](authn-rn-javascript-471.md)의 버전 3.5.0을 제거했습니다.
