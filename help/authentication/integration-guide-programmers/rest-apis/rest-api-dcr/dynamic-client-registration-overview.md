---
title: 동적 클라이언트 등록 개요
description: 동적 클라이언트 등록 개요
exl-id: 9f98dfcd-4375-48c3-beff-259dfb1d3a26
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '809'
ht-degree: 0%

---

# 동적 클라이언트 등록 개요 {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

동적 클라이언트 등록은 [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591)에서 정의한 인증 메커니즘을 나타내며, [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)에서 설명한 OAuth 2.0 인증 프레임워크를 기반으로 합니다.

Adobe Pass은 다음과 같은 보호된 API에 액세스할 수 있도록 하는 동적 클라이언트 등록 서비스를 제공합니다.

* Adobe Pass 인증 관리 API:
   * [임시 패스 API 재설정](../../features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
   * [저하 API](../../features-premium/degraded-access/degradation-feature.md#degradation-api-access)
   * [프록시 MVPD API](../../../integration-guide-mvpds/proxy-mvpd-webserv.md)
   * [권한 부여 서비스 모니터링 API](../../features-premium/esm/entitlement-service-monitoring-api.md)
* Adobe Pass 인증 REST API:
   * [REST API V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [(기존) REST API V1](../../legacy/rest-api-v1/rest-api-reference.md)
* Adobe Pass 인증 SDK:
   * [(기존) JavaScript SDK](../../legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
   * [(기존) iOS/tvOS SDK](../../legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
   * [(기존) Android SDK](../../legacy/sdks/android-sdk/android-sdk-api-reference.md)
   * [(레거시) FireOS SDK](../../legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> 동적 클라이언트 등록 인증 메커니즘은 중단 대상인 이전 Adobe Pass 인증 솔루션을 대체합니다.
>
> * 서명된 요청자 ID 메커니즘.
> * 도메인 나열 메커니즘.
> * API 키 메커니즘.

동적 클라이언트 등록이 채택되면 다음과 같은 이점이 있습니다.

* 향상된 보안.
* 플랫폼 간에 통합된 모델.
* 애플리케이션 라이프사이클을 세밀하게 제어할 수 있습니다.

Dynamic Client Registration을 관리하고 사용하는 방법에 대한 자세한 내용은 다음 섹션을 참조하십시오.

## 동적 클라이언트 등록 관리 {#dynamic-client-registration-management}

동적 클라이언트 등록 관리 프로세스를 사용하면 특정 플랫폼에서 실행되고 특정 Adobe Pass 인증 API에 액세스해야 하는 클라이언트 응용 프로그램이 [Adobe Pass TVE 대시보드](https://experience.adobe.com/#/pass/authentication)를 통해 등록할 수 있습니다.

Adobe Pass TVE Dashboard는 Adobe Pass 인증 고객(프로그래머)이 구성 및 데이터를 관리하는 도구입니다. 이 셀프 서비스 대시보드를 사용하면 [Adobe Pass TVE 대시보드 사용 안내서](../../../user-guide-tve-dashboard/tve-dashboard-overview.md) 설명서에 설명된 다양한 기능을 사용할 수 있습니다.

[Adobe Pass TVE 대시보드](https://experience.adobe.com/#/pass/authentication)에 액세스할 수 있는 경우 아래 섹션의 단계에 따라 등록된 응용 프로그램을 만들고 소프트웨어 문을 다운로드하십시오.

### 등록된 응용 프로그램 관리 {#manage-registered-applications}

>[!IMPORTANT]
>
> Adobe Pass TVE 대시보드에 액세스할 수 없는 경우 [Zendesk](https://adobeprimetime.zendesk.com)를 통해 티켓을 만든 다음 기술 계정 관리자(TAM)에게 등록된 응용 프로그램을 만들고 소프트웨어 설명을 공유하도록 요청하십시오.

등록된 응용 프로그램을 만드는 방법에는 두 가지가 있습니다.

* **프로그래머 수준**

  프로그래머 수준의 등록 프로세스를 사용하면 사용 가능한 모든 채널 또는 선택한 채널 하위 집합에 연결된 등록된 애플리케이션을 만들 수 있습니다. 자세한 내용은 [프로그래머를 위한 TVE 대시보드 사용 안내서](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md) 설명서를 참조하십시오.


* **채널 수준**

  채널 수준 등록 프로세스를 사용하면 현재 선택한 채널에만 연결된 등록된 애플리케이션을 만들 수 있습니다. 자세한 내용은 [채널에 대한 TVE 대시보드 사용 안내서](../../../user-guide-tve-dashboard/tve-dashboard-channels.md) 설명서를 참조하십시오.

>[!IMPORTANT]
>
> 보안을 강화하고 무단 액세스를 방지하기 위해 보다 구체적이고 제한된 권한으로 등록된 애플리케이션을 만드는 것이 좋습니다. 따라서 등록된 응용 프로그램을 만들 때는 할당된 `channels`, `platforms` 및 `scopes`에 대해 더 좁은 옵션을 사용하는 것이 좋습니다.
>
> 클라이언트 애플리케이션의 각 주요 업데이트에 대해 등록된 애플리케이션을 새로 만들어 수명 주기와 사용량을 관리하는 것이 좋습니다. 필요한 경우 [Zendesk](https://adobeprimetime.zendesk.com)를 통해 티켓을 만들고 특정 클라이언트 응용 프로그램 버전의 기능을 차단하기 위해 등록된 응용 프로그램을 취소하도록 기술 계정 관리자(TAM)에게 요청하십시오.

### 소프트웨어 구문 관리 {#manage-software-statements}

>[!IMPORTANT]
>
> Adobe Pass TVE 대시보드에 액세스할 수 없는 경우 [Zendesk](https://adobeprimetime.zendesk.com)를 통해 티켓을 만든 다음 기술 계정 관리자(TAM)에게 등록된 응용 프로그램을 만들고 소프트웨어 설명을 공유하도록 요청하십시오.

소프트웨어 문을 다운로드하기 전에 클라이언트 응용 프로그램 요구 사항을 충족하는 [등록된 응용 프로그램 관리](#manage-registered-applications) 섹션에 설명된 대로 등록된 응용 프로그램을 만들었는지 확인하십시오.

등록된 응용 프로그램이 만들어진 수준에 따라 소프트웨어 문을 다운로드할 수 있는 두 가지 방법이 있습니다.

* **프로그래머 수준**

  자세한 내용은 [프로그래머를 위한 TVE 대시보드 사용 안내서](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md) 설명서를 참조하십시오.

* **채널 수준**

  자세한 내용은 [채널에 대한 TVE 대시보드 사용 안내서](../../../user-guide-tve-dashboard/tve-dashboard-channels.md) 설명서를 참조하십시오.

Software 문은 클라이언트 응용 프로그램 소프트웨어에 대한 정보를 번들로 포함하는 JSON 웹 토큰(`JWT`)입니다. [클라이언트 자격 증명 검색](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API에 제공되면 소프트웨어 문은 JSON 웹 서명(`JWS`)을 사용하여 디지털 서명됩니다.

소프트웨어 구문의 정의와 작동 방식에 대한 자세한 설명은 [RFC 7591](https://tools.ietf.org/html/rfc7591) 설명서를 참조하십시오.

## 동적 클라이언트 등록 흐름 {#dynamic-client-registration-flow}

요약하면, 동적 클라이언트 등록 인증 메커니즘에는 다음과 같은 몇 가지 단계가 포함됩니다.

**관리**

* 클라이언트 담당자는 [등록된 응용 프로그램 관리](#manage-registered-applications) 섹션에 설명된 대로 등록된 응용 프로그램을 만들어야 합니다.
* 클라이언트 담당자는 [소프트웨어 문 관리](#manage-software-statements) 섹션에 설명된 대로 소프트웨어 문을 다운로드하여 포함해야 합니다.

**흐름**

* 클라이언트 응용 프로그램은 [클라이언트 자격 증명 검색](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API 설명서에 설명된 대로 클라이언트 자격 증명을 가져와야 합니다.
* 클라이언트 응용 프로그램은 [액세스 토큰 검색](apis/dynamic-client-registration-apis-retrieve-access-token.md) API 설명서에 설명된 대로 액세스 토큰을 가져와야 합니다.

Adobe Pass으로 보호된 API에 액세스하는 방법을 더 잘 이해하려면 [동적 클라이언트 등록 흐름](flows/dynamic-client-registration-flow.md) 설명서를 참조하십시오. 또한 더 많은 컨텍스트를 제공하고 데모를 포함하는 이 [웨비나](https://my.adobeconnect.com/pzkp8ujrigg1/) 녹화를 볼 수도 있습니다.
