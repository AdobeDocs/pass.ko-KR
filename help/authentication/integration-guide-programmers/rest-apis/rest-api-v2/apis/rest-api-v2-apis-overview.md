---
title: REST API V2 API 개요
description: REST API V2 API 개요
exl-id: 62b48bf0-d200-4949-b268-8f8ea2daabfa
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 297578d3b311fbc2a5702f1bcc947e94463fb884
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%
---
# REST API V2 API 개요 {#rest-api-v2-apis-overview}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

시작하려면 아래 목록에 있는 API의 공개 설명서를 참조하십시오.

## 기본 URL {#rest-api-v2-apis-base-urls}

REST API V2 요청에 대해 아래 환경별 기본 URL을 사용하십시오. 각 API에 대해 문서화된 상대 `path`을(를) 이 기본 URL에 추가합니다.

일부 API 응답은 스트리밍 장치가 브라우저 또는 사용자 에이전트에서 열어야 하는 대화형 URL을 반환합니다. 예를 들어 [특정 mvpd에 대한 로그아웃 시작](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)에서 반환된 `url` 특성과 [사용자 에이전트에서 인증 수행](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)에서 반환된 `Location` 리디렉션이 있습니다.

이러한 대화형 URL은 동일한 Adobe Pass SP(서비스 공급자) 호스트를 사용합니다. 반환된 그대로 절대 대화형 URL을 엽니다. Adobe Pass에서 `/adobe-services/`(으)로 시작하는 상대 대화형 URL을 반환하는 경우 대상 환경의 SP 호스트에 대해 해당 URL을 확인합니다.

| 환경 | REST API V2 및 대화형 기본 URL |
| --- |--------------------------------------|
| 프로덕션 | `https://sp.auth.adobe.com` |
| 스테이징 | `https://sp.auth-staging.adobe.com` |

## 구성 {#rest-api-v2-apis-configuration}

* [특정 서비스 공급자에 대한 구성 검색](configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

## 세션 {#rest-api-v2-apis-sessions}

* [인증 세션 만들기](sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [인증 세션 다시 시작](sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [인증 세션 검색](sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [사용자 에이전트에서 인증 수행](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)

## 프로필 {#rest-api-v2-apis-profiles}

* [프로필 검색](profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 mvpd에 대한 프로필 검색](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정 코드에 대한 프로필 검색](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

## 결정 {#rest-api-v2-apis-decisions}

* [특정 mvpd를 사용하여 권한 부여 결정 검색](decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [특정 mvpd를 사용하여 사전 인증 결정 검색](decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

## 로그아웃 {#rest-api-v2-apis-logout}

* [특정 mvpd에 대한 로그아웃 시작](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

## Partner Single Sign-On {#rest-api-v2-apis-partner-single-sign-on}

* [파트너 인증 요청 검색](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [파트너 인증 응답을 사용하여 프로필 만들기 및 검색](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

## REST API V2를 사용하시겠습니까?

이제 [Adobe Developer](https://developer.adobe.com/adobe-pass/) 웹 사이트에서 제품 전용 페이지를 통해 REST API V2를 살펴볼 수 있습니다.
