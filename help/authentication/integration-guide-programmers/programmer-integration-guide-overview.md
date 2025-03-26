---
title: 프로그래머 통합 안내서
description: 프로그래머 통합 안내서
exl-id: 51461caf-08ef-459e-b284-8f317f45e7b1
source-git-commit: 07bb12f7983f39b58e1b9795fdaa1bec4f68e674
workflow-type: tm+mt
source-wordcount: '2119'
ht-degree: 0%

---

# 프로그래머 통합 안내서 {#programmer-integration-guide}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 통합 안내서는 Adobe® 인증 전달과 통합하려는 콘텐츠 공급자(프로그래머)를 위한 것입니다.

오늘날의 디지털 환경에서 뷰어는 언제 어디서나 인터넷에 액세스할 수 있으며 보호된 콘텐츠에 대한 액세스를 요청할 수 있습니다. 그들은 한번의 행사를 보거나 당신이 방영하는 전체 텔레비전 시리즈를 방영할 수 있는 권리를 찾으려 할지도 모른다.

보호된 콘텐츠에 대한 액세스 권한을 부여하기 전에 뷰어에 권한이 있는지 여부를 결정해야 합니다. 주요 질문은 다음과 같습니다.

* **뷰어에 다중 채널 비디오 프로그래밍 배포자(MVPD)를 사용하는 활성 구독이 있습니까?**
* **해당 구독에 프로그램이 포함되어 있습니까?**

## TV Everywhere용 Adobe Pass 인증 {#adobe-pass-authentication-for-tv-everywhere}

프로그래머들에게 자격을 결정하는 것이 항상 간단한 것은 아니다. MVPD는 고객의 식별 데이터 및 액세스 권한을 관리하는 관리자입니다. 더욱 복잡한 문제는 프로그래머 뷰어가 각각 고유한 시스템으로 작동하는 매우 다양한 MVPD에 가입할 수 있다는 것입니다. 이러한 복잡성으로 인해 자격 검증은 기술적으로 어렵고 리소스 집약적입니다.

![프로그래머가 직접 결정한 사용자 권한](../assets/user-ent-by-progr.png){align="center"}

*프로그래머가 직접 결정한 사용자 권한*

Adobe Pass 인증은 프로그래머와 MVPD 간의 권한 거래를 안전하게 지원하므로 적합한 뷰어에게 보호된 콘텐츠를 빠르고 쉽고 안전하게 제공할 수 있습니다.

![Adobe Pass 인증에 의해 중재된 사용자 권한](../assets/user-ent-mediatedby-authn.png){align="center"}

*Adobe Pass 인증에 의해 중재된 사용자 권한*

Adobe Pass 인증은 프록시 역할을 하며 양측에 안전하고 일관된 인터페이스를 제공하여 프로그래머와 MVPD 간의 권한 흐름을 촉진합니다.

프로그래머의 경우 Adobe Pass 인증은 **Standard** 또는 **Premium** 계층의 일부로 API를 제공합니다.

* 표준 Adobe Pass 인증 API:
   * [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* Premium Adobe Pass 인증 API:
   * [임시 패스 API 재설정](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [TempPass 기능](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [저하 API](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [성능 저하 기능](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [권한 부여 서비스 모니터링 API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

### 사용 사례 {#use-cases}

이 섹션에서는 Adobe Pass 인증에서 지원하는 프로그래머 통합 사용 사례에 대해 간략히 설명합니다.

* 단일 채널 네트워크를 사용하는 프로그래머(TVE) 애플리케이션

  이를 통해 프로그래머는 TVE 애플리케이션 내의 단일 브랜드 채널 네트워크에서 콘텐츠에 대한 액세스를 뷰어에게 제공할 수 있습니다.

* 다중 채널 네트워크를 가진 프로그래머 (TVE) 응용 프로그램

  이를 통해 프로그래머는 단일 TVE 애플리케이션 내에서 여러 채널 네트워크에서 콘텐츠에 액세스할 수 있는 권한을 시청자에게 제공할 수 있습니다.

* 특수 이벤트에 대한 프로그래머 (TVE) 응용 프로그램

  이를 통해 프로그래머는 일반 채널과 같은 MVPD 권한 데이터베이스에 있는 리소스가 아닐 수 있는 특수 이벤트의 콘텐츠에 대한 액세스 권한을 뷰어에게 제공할 수 있습니다.

| **단계** | **우선 순위** | **사용 사례** | **문서** |
|----------------------|--------------|-------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **인증** | **높음** | 인증 | 자세한 내용은 [인증 단계](#authentication-phase) 섹션에서 집계된 문서를 참조하십시오. |
|                      | **높음** | 홈 기반 인증(HBA) | 자세한 내용은 [홈 기반 인증](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md)을 참조하세요. |
|                      | **높음** | SSO(단일 인증) | 자세한 내용은 [SSO(Single Sign-On)](#sso) 섹션에서 집계된 문서를 참조하십시오. |
|                      | **높음** | MVPD 선택 | 자세한 내용은 [구성 단계](#configuration-phase) 섹션에서 집계된 문서를 참조하십시오. |
|                      | **Medium** | 브랜드 MVPD 로그인 페이지 | MVPD가 기본 언어 환경 설정 지원을 포함하여 프로그래머 또는 서비스 공급자와 관련된 브랜딩을 로그인 페이지에 제공할 수 있습니다. |
|                      | **높음** | 플랫폼별 TTL(Time-To-Live) 값 구성 | 자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)를 참조하세요. |
| **사전 인증** | **낮음** | 사전 승인(사전 승인) | 자세한 내용은 [사전 인증 단계](#preauthorization-phase) 섹션에서 집계된 문서를 참조하십시오. |
|                      | **Medium** | 향상된 오류 코드 | 자세한 내용은 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)를 참조하세요. |
| **인증** | **높음** | 인증 | 자세한 내용은 [인증 단계](#authorization-phase) 섹션에서 집계된 문서를 참조하십시오. |
|                      | **높음** | 개별 채널 인증 | 사용자가 단일 TVE 애플리케이션 내에서 여러 채널 네트워크의 콘텐츠에 액세스할 수 있습니다. 프로그래머는 자격을 확인하기 위해 채널별 권한 부여 호출을 수행할 수 있습니다. |
|                      | **낮음** | 자산 수준 인증 | MVPD가 인증 중에 개별 콘텐츠 자산에 대한 세부 분석을 수집할 수 있습니다. |
|                      | **Medium** | 향상된 오류 코드 | 자세한 내용은 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)를 참조하세요. |
|                      | **높음** | 프로그래머 Federated Player - 페이지 수준 권한 있음 | 자세한 내용은 [미디어 토큰](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)을 참조하세요. |
|                      | **Medium** | 프로그래머 Federated Player - 내부 플레이어 권한 있음 | 자세한 내용은 [미디어 토큰](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)을 참조하세요. |
|                      | **높음** | 신디케이트된 플레이어 - 페이지 수준 권한으로 MVPD 포털에 호스팅 | 자세한 내용은 [미디어 토큰](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)을 참조하세요. |
|                      | **낮음** | 자녀 보호 - 승인 요청의 콘텐츠 등급 | 프로그래머는 자산 수준 인증에 유용한 MVPD에 대한 인증 요청의 일부로 콘텐츠 등급을 포함할 수 있습니다. |
|                      | **낮음** | 자녀 보호 - 사용자 속성에 따라 콘텐츠 필터링 | 프로그래머가 사용자에게 허용된 최대 콘텐츠 등급을 확인하고 그에 따라 사용 가능한 콘텐츠를 필터링할 수 있습니다. |
| **로그아웃** | **Medium** | 로그아웃 | 자세한 내용은 [로그아웃 단계](#logout-phase) 섹션에서 집계된 문서를 참조하십시오. |

## 권한 흐름 {#entitlement-flow}

권한 흐름은 프로그래머(TVE) 애플리케이션이 보호된 콘텐츠를 스트리밍하기 위해 완료해야 하는 일련의 단계입니다. 플로우는 다음 단계로 구성됩니다.

* [등록 단계](#registration-phase)
* [구성 단계](#configuration-phase)
* [인증 단계](#authentication-phase)
* [(선택 사항) 사전 인증 단계](#preauthorization-phase)
* [인증 단계](#authorization-phase)
* [로그아웃 단계](#logout-phase)

프로그래머(TVE) 애플리케이션에 대한 사용자의 초기 방문 시 자격 흐름은 요약된 시퀀스를 따릅니다. 그러나, 후속 방문에서, 애플리케이션은 등록 또는 인증의 상태 및 적용 가능한 시청 정책에 기초하여 특정 단계를 우회할 수 있다.

권한 흐름 및 해당 단계에 대한 자세한 내용을 보려면 이 문서를 계속 읽고 관련 Cookbook 안내서에서 추가 인사이트를 참조하십시오.

* [REST API V2 Cookbook(클라이언트-서버)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
* [REST API V2 Cookbook(서버 간)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)

>[!NOTE]
>
> 이 문서에서는 Adobe Pass 인증이 지원하는 다양한 플랫폼(브라우저, 모바일 장치, TV 연결 장치 등)에서 실행되는 애플리케이션 유형을 포괄적으로 지칭하는 데 TVE(프로그래머) 애플리케이션이 사용됩니다.

### 등록 단계 {#registration-phase}

등록 단계의 목적은 [DCR(Dynamic Client Registration)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) 프로세스를 통해 Adobe Pass 인증에 대해 클라이언트 응용 프로그램을 등록하는 것입니다.

DCR(Dynamic Client Registration) 프로세스를 진행하려면 클라이언트 애플리케이션이 클라이언트 자격 증명 쌍을 가져오고 등록 단계의 최종 목표로 액세스 토큰을 검색해야 합니다.

**API**

* [클라이언트 자격 증명 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [액세스 토큰 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**흐름**

* [동적 클라이언트 등록 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**FAQ**

* [등록 단계 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#registration-phase-faqs-general).

### 구성 단계 {#configuration-phase}

구성 단계의 목적은 각 MVPD에 대해 Adobe Pass 인증으로 저장된 구성 세부 정보와 함께 현재 통합되는 MVPD 목록을 클라이언트 애플리케이션에 제공하는 것입니다.

구성 단계는 클라이언트 애플리케이션이 사용자에게 TV 공급자를 선택하라고 요청해야 할 때 인증 단계에 대한 필수 단계 역할을 합니다.

**API**

* [특정 서비스 공급자에 대한 구성 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

**FAQ**

* [구성 단계 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#configuration-phase-faqs-general).

>[!TIP]
>
> TVE 애플리케이션에는 사용자가 자신의 TV 공급자를 쉽게 식별하고 선택할 수 있는 MVPD 선택 인터페이스가 포함되어야 합니다.

### 인증 단계 {#authentication-phase}

인증 단계의 목적은 MVPD에서 사용자의 ID를 확인하고 사용자 메타데이터 정보를 얻을 수 있는 기능을 클라이언트 애플리케이션에 제공하는 것입니다.

인증 단계는 클라이언트 응용 프로그램에서 컨텐츠를 재생해야 할 때 사전 인증 단계 또는 인증 단계에 대한 필수 단계 역할을 합니다.

인증에 성공하면 사용자 메타데이터 정보도 포함하는 애플리케이션, 디바이스 및 서비스 공급자에 연결된 프로필이 생성됩니다.

**높은 수준의 단계**

다음 단계는 SAML 통합의 경우 높은 수준의 단계에 대해 간략히 설명합니다.

1. **프로그래머 응용 프로그램(웹 사이트) 로드**\
   사용자가 Adobe Pass 인증 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)을(를) 통합하는 프로그래머 애플리케이션(웹 사이트)으로 이동합니다.

1. **보호된 콘텐츠 요청**\
   사용자가 보호된 콘텐츠에 액세스하려고 하면 프로그래머 애플리케이션은 사용자가 선택할 수 있는 MVPD 목록을 표시합니다.

1. **인증 요청 초기화**\
   MVPD 선택 시 사용자는 Adobe Pass 인증 서버로 리디렉션됩니다. 여기서, 선택된 MVPD에 대한 암호화된 SAML 인증 요청은 SAML 통합의 경우에 생성된다. 이 요청은 프로그래머를 대신하여 MVPD으로 전송됩니다. MVPD 시스템에 따라 사용자의 브라우저가 MVPD의 로그인 페이지로 리디렉션되거나 로그인 iFrame이 프로그래머 애플리케이션 내에 포함됩니다.

1. **MVPD 로그인**\
   MVPD은 요청을 수락하고 리디렉션 또는 iFrame을 통해 로그인 인터페이스를 제공합니다.

1. **사용자 로그인 및 유효성 검사**\
   사용자가 MVPD 자격 증명으로 로그인합니다. MVPD은 사용자의 구독 상태를 확인하고 자체 HTTP 세션을 설정합니다.

1. **Adobe Pass 인증에 대한 MVPD 응답**\
   유효성 검사가 완료되면 MVPD은 SAML 응답(암호화됨)을 생성하여 Adobe Pass 인증으로 다시 보냅니다.

1. **프로필 생성**\
   Adobe Pass 인증은 SAML 응답을 확인하고, 캐시되는 사용자 프로필을 생성하고, 사용자를 프로그래머 애플리케이션(웹 사이트)으로 다시 리디렉션합니다.

**API**

* [인증 세션 만들기](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [인증 세션 다시 시작](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [인증 세션 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [사용자 에이전트에서 인증 수행](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [프로필 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 mvpd에 대한 프로필 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정 코드에 대한 프로필 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**흐름**

* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**FAQ**

* [인증 단계 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

>[!TIP]
>
> TVE 애플리케이션은 사용자의 인증 상태를 명확하게 전달해야 합니다. 예를 들어, &quot;잠김&quot; 또는 &quot;잠금 해제&quot; 아이콘과 함께 MVPD 로고를 표시하여 보호된 콘텐츠의 접근성을 나타냅니다.

#### SSO(단일 인증) {#single-sign-on}

**API**

* [파트너 인증 요청 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [파트너 인증 응답을 사용하여 프로필 만들기 및 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

**흐름**

* [파트너 흐름을 사용한 SSO(Single Sign-On)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
* [플랫폼 ID 흐름을 사용한 SSO(Single Sign-On)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
* [서비스 토큰 흐름을 사용한 SSO(Single Sign-On)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)

### (선택 사항) 사전 인증 단계 {#preauthorization-phase}

사전 인증 단계의 목적은 사용자가 액세스할 수 있는 카탈로그의 리소스 하위 집합을 클라이언트 응용 프로그램에 제공하는 것입니다.

사전 인증 단계는 사용자가 클라이언트 애플리케이션을 처음 열거나 새 섹션으로 이동할 때 사용자 경험을 향상시킬 수 있습니다.

**API**

* [사전 인증 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**흐름**

* [기본 애플리케이션 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**FAQ**

* [사전 인증 단계 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general).

>[!TIP]
>
> TVE 애플리케이션은 제한된 콘텐츠의 &quot;잠금&quot; 아이콘 및 승인된 콘텐츠의 &quot;잠금 해제&quot; 아이콘과 같은 시각적 표시기를 사용하여 제한된 콘텐츠와 승인된 콘텐츠를 명확히 구별해야 합니다.

### 인증 단계 {#authorization-phase}

인증 단계의 목적은 MVPD을 사용하여 권한을 확인한 후 사용자가 요청하는 리소스를 재생하는 기능을 클라이언트 애플리케이션에 제공하는 것입니다.

성공적인 인증은 보안 목적으로 프로그래머(TVE) 애플리케이션에 제공되는 미디어 토큰도 포함하는 결정을 생성합니다.

**높은 수준의 단계**

다음 단계는 상위 레벨 단계에 대한 개요입니다.

1. **리소스 식별자 처리**\
   보호된 콘텐츠는 [리소스 식별자](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)로 식별됩니다. 이 식별자는 간단한 문자열이거나 더 복잡한 구조일 수 있습니다. 이 식별자는 사전 정의되어 있으며 프로그래머와 MVPD이 동의합니다. 프로그래머 응용 프로그램에서 Adobe Pass 인증 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)에 리소스 식별자를 보냅니다.

1. **MVPD 인증 확인**\
   Adobe Pass 인증 서버는 표준화된 프로토콜을 사용하여 MVPD의 인증 엔드포인트와 통신합니다.

1. **Adobe Pass 인증에 대한 MVPD 응답**\
   유효성 검사가 완료되면 MVPD은 사용자에게 콘텐츠에 액세스할 권한이 있는지 여부를 확인하고 응답을 다시 Adobe Pass 인증으로 보냅니다.

1. **결정 및 미디어 토큰 생성**\
   Adobe Pass 인증은 응답을 확인하고 캐시되는 [결정](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)을 생성하며 미디어 토큰이 포함된 결정을 프로그래머 애플리케이션(웹 사이트)으로 다시 반환합니다.

1. **콘텐츠 액세스 확인**\
   프로그래머 응용 프로그램은 [미디어 토큰 검증기](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)를 사용하여 올바른 사용자가 올바른 콘텐츠에 액세스하고 있는지 확인합니다. 유효성을 검사하면 사용자에게 보호된 콘텐츠를 볼 수 있는 액세스 권한이 부여됩니다.

**API**

* [인증 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**흐름**

* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**FAQ**

* [인증 단계 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general).

>[!TIP]
>
> TVE 애플리케이션은 제한된 콘텐츠의 &quot;잠금&quot; 아이콘 및 승인된 콘텐츠의 &quot;잠금 해제&quot; 아이콘과 같은 시각적 표시기를 사용하여 제한된 콘텐츠와 승인된 콘텐츠를 명확히 구별해야 합니다.

### 로그아웃 단계 {#logout-phase}

로그아웃 단계의 목적은 사용자 요청 시 Adobe Pass 인증 내에서 사용자의 인증된 프로필을 종료할 수 있는 기능을 클라이언트 애플리케이션에 제공하는 것입니다.

**API**

* [특정 mvpd에 대한 로그아웃 시작](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**흐름**

* [기본 애플리케이션 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**FAQ**

* [로그아웃 단계 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general).

#### SLO(단일 로그아웃) {#single-logout}

**흐름**

* [단일 로그아웃 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)

## 권한 이해 {#understanding-entitlements}

Adobe Pass 인증 솔루션은 인증 및 권한 부여 워크플로의 성공적인 완료 시 생성되는 특정 데이터인 권한 생성을 중심으로 다룹니다. 이러한 권한은 보호된 콘텐츠에 대한 액세스 권한을 부여하지만 사용 기간은 제한됩니다. 권한이 만료되면 인증 또는 권한 부여 프로세스를 다시 시작하여 갱신해야 합니다.

권한에 대한 자세한 내용은 다음 문서를 참조하십시오.

* **프로필**

  인증에 성공하면 Adobe Pass 인증은 요청하는 애플리케이션, 장치 및 서비스 공급자 식별자(요청자 식별자)와 연결된 인증된 프로필(&quot;long-lived&quot;)을 만듭니다.

* **[사용자 메타데이터](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  인증에 성공하면(그리고 경우에 따라 인증 후에도) Adobe Pass 인증은 MVPD에서 사용자 메타데이터를 수신하여 이를 요청하는 애플리케이션에 노출할 수 있습니다.

* **[결정](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  승인이 성공하면 Adobe Pass 인증은 요청하는 애플리케이션, 디바이스, 서비스 공급자 식별자(요청자 식별자) 및 특정 보호된 리소스(리소스 식별자)와 관련된 권한 부여 결정(&quot;long-lived&quot;)을 만듭니다.

* **[미디어 토큰](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  인증에 성공하면 Adobe Pass 인증은 성공적인 재생 요청과 연결된 미디어 토큰(&quot;단기&quot;)을 만들고 사기(예: 스트림 복사)를 줄이기 위한 업계 모범 사례에 대한 지원을 제공합니다.

프로필 및 의사 결정에 대한 TTL(time-to-live) 값은 프로그래머 및 유료 TV 공급자 간의 합의에 따라 설정되며, 프로그래머는 관련 있는 모든 사람에게 가장 적합한 값에 동의합니다.
