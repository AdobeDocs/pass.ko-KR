---
title: REST API V2 - API - 개요
description: REST API V2 - API - 개요
source-git-commit: c849882286c88d16a5652717d381700287c53277
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 1%

---


# REST API V2 - API - 개요 {#rest-api-v2-apis-overview}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> REST API V2 구현은 [조절 메커니즘](/help/authentication/throttling-mechanism.md) 설명서에 의해 제한됩니다.

## REST API V2를 사용하시겠습니까?

이제 [Adobe Developer](https://developer.adobe.com/adobe-pass/) 웹 사이트에서 제품 전용 페이지를 통해 Adobe Pass 인증 REST API V2를 살펴볼 수 있습니다.

또한 전담 지원 팀이 귀하에게 필요한 질문이나 기술 지원을 도와드릴 수 있습니다.

## REST API V2 - API {#rest-api-v2-apis}

시작하려면 [REST API V2 - API](./rest-api-v2-apis-overview.md) 및 [REST API V2 - 흐름](../flows/rest-api-v2-flows-overview.md)의 각 항목에 대한 공개 설명서를 참조하세요.

### 구성 {#rest-api-v2-apis-configuration}

* [특정 서비스 공급자에 대한 구성 검색](configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

### 세션 {#rest-api-v2-apis-sessions}

* [인증 세션 만들기](sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [인증 세션 다시 시작](sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [인증 세션 검색](sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)

### 프로필 {#rest-api-v2-apis-profiles}

* [프로필 검색](profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 mvpd에 대한 프로필 검색](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정 코드에 대한 프로필 검색](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

### 결정 {#rest-api-v2-apis-decisions}

* [특정 mvpd를 사용하여 권한 부여 결정 검색](decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [특정 mvpd를 사용하여 사전 인증 결정 검색](decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

### 로그아웃 {#rest-api-v2-apis-logout}

* [특정 mvpd에 대한 로그아웃 시작](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

### Partner Single Sign-On {#rest-api-v2-apis-partner-single-sign-on}

* [파트너 인증 요청 검색](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [파트너 인증 응답을 사용하여 프로필 검색](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
