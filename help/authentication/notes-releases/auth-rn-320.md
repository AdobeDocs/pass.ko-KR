---
title: Adobe Pass Authentication 3.2.0 릴리스 노트
description: Adobe Pass Authentication 3.2.0 릴리스 노트
exl-id: 43aee317-dbac-4000-893e-839ee3e9f6ba
source-git-commit: fcdf50b2caad20deef15fceeb3e23f4195c0078d
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Adobe Pass Authentication 3.2.0 릴리스 노트 {#authn-320-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-320}

* [빌드 번호](#build-number-320)
* [릴리스 개요](#release-overview-320)

### 빌드 번호 {#build-number-320}

Adobe Pass 인증: adobe-pass-**3.2.0**

릴리스 날짜: **06/10/2025 - 06/12/2025**

### 릴리스 개요 {#release-overview-320}

#### REST API v2

* `missing_parameters_fallback`세션 API[ 응답에 매개 변수가 없는 경우에 대해 새 이유 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)이(가) 추가되었습니다.
* 새 필드 &quot;장치&quot;를 [세션 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) 응답에 추가했습니다.

#### 새로운 기능

* 저하 API를 확장하여 한 번의 호출로 여러 MVPD에 대한 저하 규칙을 적용하는 기능을 추가합니다.

#### 버그 수정

* 사용자 메타데이터 처리에 실패한 경우 인증 흐름이 정상적으로 완료되지 않는 문제를 해결했습니다.
* 인증 사유가 올바르게 계산되지 않는 문제가 해결되었습니다.
* upstreamUserID가 프로필 응답에 없는 문제가 해결되었습니다.
* 인증 요청에 REST API V2 시작 인증 요청에 대한 범위 정보가 포함되지 않는 문제를 해결했습니다.
