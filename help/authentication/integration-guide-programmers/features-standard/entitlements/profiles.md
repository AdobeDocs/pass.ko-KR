---
title: 프로필
description: 프로필
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# 프로필 {#profiles}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

사용자가 유료 TV 공급자(MVPD)를 통해 인증하면 Adobe Pass 인증 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)에서 프로필을 만듭니다.

프로필 유형은 사용되는 인증 방법에 따라 다릅니다.

* **보통**

  기본 인증을 통해 만들어집니다.

* **Apple SSO**

  Apple의 비디오 구독자 계정 프레임워크를 사용하여 SSO(Single Sign-On)를 통해 생성합니다.

* **플랫폼 SSO**

  플랫폼 ID를 사용하여 SSO(Single Sign-On)를 통해 생성합니다.

* **서비스 토큰 SSO**

  서비스 토큰을 사용하여 SSO(단일 인증)를 통해 생성됨

프로필은 클라이언트 애플리케이션에서 다음을 수행할 수 있도록 하는 주요 데이터를 저장합니다.

* 사용자의 인증 상태를 확인합니다.
* 사용된 인증 방법을 식별합니다.
* ID 공급자를 식별합니다.
* [사용자 메타데이터](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)에 액세스합니다.

프로필은 Adobe Pass 인증의 백엔드에 안전하게 저장되고 요청하는 애플리케이션, 디바이스 및 서비스 공급자 식별자에 연결됩니다. [TTL(Authentication Time-to-Live)](#authentication-ttl-management)에 정의된 대로 제한된 기간 동안 유효합니다.

## 인증 TTL(Time-to-Live) 관리 {#authentication-ttl-management}

TTL(Authentication Time-to-Live)은 사용자가 다시 인증해야 하기 전까지 사용자가 인증된 상태를 유지하는 기간을 정의합니다. 이 기간은 제한되어 있으며 MVPD 담당자와 합의해야 합니다. TTL 값은 다음에 따라 달라질 수 있습니다.

* 플랫폼 카테고리(예: 데스크탑, 모바일, TV 연결 장치)
* 특정 플랫폼(예: iOS, Android, tvOS, Roku, FireTV)

인증(authN) TTL은 조직 관리자 중 한 사람이나 사용자를 대신하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)를 통해 보고 변경할 수 있습니다.

자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) 설명서를 참조하십시오.

## REST API V2 {#rest-api-v2}

다음 API를 사용하여 프로필을 검색할 수 있습니다.

* [프로필 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 mvpd에 대한 프로필 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정 코드에 대한 프로필 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

프로필의 구조를 이해하려면 위의 API의 **응답** 및 **샘플** 섹션을 참조하십시오.

위의 API를 통합하는 방법 및 시기에 대한 자세한 내용은 다음 문서를 참조하십시오.

* [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

>[!MORELIKETHIS]
>
> [인증 단계 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
