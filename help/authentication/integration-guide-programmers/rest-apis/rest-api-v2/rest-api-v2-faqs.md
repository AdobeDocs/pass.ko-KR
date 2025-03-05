---
title: REST API V2 FAQ
description: REST API V2 FAQ
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '8198'
ht-degree: 0%

---

# REST API V2 FAQ {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 문서에서는 Adobe Pass 인증 REST API V2 채택에 대한 FAQ에 대한 개요 답변을 제공합니다.

REST API V2에 대한 자세한 내용은 [REST API V2 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) 설명서를 참조하십시오.

## 일반 FAQ {#general-faqs}

[REST API V1](#migration-rest-api-v1-to-rest-api-v2) 또는 [SDK](#migration-sdk-to-rest-api-v2)에서 마이그레이션하는 기존 응용 프로그램이든 새 응용 프로그램이든 REST API V2를 통합해야 하는 응용 프로그램에서 작업하는 경우 이 섹션으로 시작하십시오.

마이그레이션 세부 정보 및 단계에 대한 자세한 내용은 다음 섹션도 참조하십시오.

### 등록 단계 FAQ {#registration-phase-faqs-general}

+++등록 단계 FAQ

[DCR(동적 클라이언트 등록) FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs) 설명서를 참조하십시오.

+++

### 구성 단계 FAQ {#configuration-phase-faqs-general}

+++구성 단계 FAQ

#### 1. 구성 단계의 목적은 무엇입니까? {#configuration-phase-faq1}

구성 단계의 목적은 각 MVPD에 대해 Adobe Pass 인증으로 저장된 구성 세부 사항(예: `id`, `displayName`, `logoUrl` 등)과 함께 현재 통합되는 MVPD 목록을 클라이언트 응용 프로그램에 제공하는 것입니다.

구성 단계는 클라이언트 애플리케이션이 사용자에게 TV 공급자를 선택하라고 요청해야 할 때 인증 단계에 대한 필수 단계 역할을 합니다.

#### 2. 구성 단계가 필수입니까? {#configuration-phase-faq2}

구성 단계는 필수가 아니며, 클라이언트 애플리케이션은 사용자가 인증 또는 재인증할 MVPD을 선택해야 하는 경우에만 구성을 검색해야 합니다.

클라이언트 애플리케이션은 다음 시나리오에서 이 단계를 건너뛸 수 있습니다.

* 사용자가 이미 인증되었습니다.
* 기본 또는 프로모션 [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) 기능을 통해 사용자에게 임시 액세스 권한이 제공됩니다.
* 사용자 인증이 만료되었지만 클라이언트 애플리케이션이 이전에 선택한 MVPD을 사용자 경험에 동기된 선택으로 캐시했으며 사용자에게 아직 해당 MVPD의 구독자인지 확인하라는 메시지를 표시했습니다.

#### 3. 구성은 무엇이며 얼마나 오래 유효합니까? {#configuration-phase-faq3}

구성은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration) 설명서에 정의된 용어입니다.

구성에 [Configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) 끝점에서 검색할 수 있는 `id`, `displayName`, `logoUrl` 특성 등으로 정의된 MVPD 목록이 포함되어 있습니다.

사용자가 인증 또는 재인증하기 위해 MVPD을 선택해야 하는 경우에만 클라이언트 애플리케이션이 구성을 검색해야 합니다.

클라이언트 애플리케이션은 사용자가 TV 공급자를 선택해야 할 때마다 구성 응답을 사용하여 사용 가능한 MVPD 옵션이 있는 UI 선택기를 제공할 수 있습니다.

인증, 사전 권한 부여, 권한 부여 또는 로그아웃 단계를 계속하려면 클라이언트 응용 프로그램에서 MVPD의 구성 수준 `id` 특성에 지정된 대로 사용자가 선택한 MVPD 식별자를 저장해야 합니다.

자세한 내용은 [구성 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) 설명서를 참조하세요.

#### 4. 클라이언트 애플리케이션이 구성 응답 정보를 영구 저장소에 캐시해야 합니까? {#configuration-phase-faq4}

사용자가 인증 또는 재인증하기 위해 MVPD을 선택해야 하는 경우에만 클라이언트 애플리케이션이 구성을 검색해야 합니다.

클라이언트 애플리케이션은 불필요한 요청을 방지하고 다음과 같은 경우 사용자 경험을 개선하기 위해 구성 응답 정보를 메모리 저장소에 캐시해야 합니다.

* 사용자가 이미 인증되었습니다.
* 기본 또는 프로모션 [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) 기능을 통해 사용자에게 임시 액세스 권한이 제공됩니다.
* 사용자 인증이 만료되었지만 클라이언트 애플리케이션이 이전에 선택한 MVPD을 사용자 경험에 동기된 선택으로 캐시했으며 사용자에게 아직 해당 MVPD의 구독자인지 확인하라는 메시지를 표시했습니다.

#### 5. 클라이언트 애플리케이션이 자체 MVPD 목록을 관리할 수 있습니까? {#configuration-phase-faq5}

클라이언트 애플리케이션은 자체 MVPD 목록을 관리할 수 있지만, MVPD 식별자를 Adobe Pass 인증과 동기화해야 합니다. 따라서 목록이 최신 상태이고 정확한지 확인하려면 Adobe Pass 인증에서 제공하는 구성을 사용하는 것이 좋습니다.

제공된 MVPD 식별자가 잘못되었거나 지정된 [서비스 공급자](rest-api-v2-glossary.md#service-provider)와 활성 통합이 없는 경우 클라이언트 응용 프로그램은 Adobe Pass 인증 REST API V2에서 [오류](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)을(를) 받습니다.

#### 6. 클라이언트 애플리케이션이 MVPD 목록을 필터링할 수 있습니까? {#configuration-phase-faq6}

클라이언트 애플리케이션은 자신의 비즈니스 로직 및 이전 선택의 사용자 위치 또는 사용자 내역과 같은 요구 사항을 기반으로 사용자 지정 메커니즘을 구현함으로써 구성 응답에 제공된 MVPD의 목록을 필터링할 수 있다.

클라이언트 응용 프로그램은 [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) MVPD 또는 아직 개발 또는 테스트 중인 통합이 있는 MVPD의 목록을 필터링할 수 있습니다.

#### 7. MVPD과의 통합이 비활성화되고 비활성화로 표시되면 어떻게 됩니까? {#configuration-phase-faq7}

MVPD과의 통합이 비활성화되고 비활성화로 표시되면 추가 구성 응답에 제공된 MVPD 목록에서 MVPD이 제거되며 고려해야 할 두 가지 중요한 결과가 있습니다.

* 해당 MVPD의 인증되지 않은 사용자는 더 이상 해당 MVPD을 사용하여 인증 단계를 완료할 수 없습니다.
* 해당 MVPD의 인증된 사용자는 더 이상 해당 MVPD을 사용하여 사전 인증, 권한 부여 또는 로그아웃 단계를 완료할 수 없습니다.

선택한 MVPD에 지정된 [서비스 공급자](rest-api-v2-glossary.md#service-provider)와의 활성 통합이 더 이상 없는 경우 클라이언트 응용 프로그램이 Adobe Pass 인증 REST API V2에서 [오류](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)을(를) 받습니다.

#### 8. MVPD과의 통합이 다시 활성화되어 활성으로 표시되면 어떻게 됩니까? {#configuration-phase-faq8}

MVPD과의 통합이 다시 활성화되어 활성으로 표시되면 MVPD은 추가 구성 응답에 제공된 MVPD 목록에 다시 포함되며 다음과 같은 두 가지 중요한 결과가 있습니다.

* 해당 MVPD의 인증되지 않은 사용자는 해당 MVPD을 사용하여 인증 단계를 다시 완료할 수 있습니다.
* 해당 MVPD의 인증된 사용자는 해당 MVPD을 사용하여 사전 인증, 권한 부여 또는 로그아웃 단계를 다시 완료할 수 있습니다.

#### 9. MVPD과의 통합을 활성화하거나 비활성화하는 방법 {#configuration-phase-faq9}

이 작업은 조직 관리자 중 한 사람이나 사용자를 대신하여 활동하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)를 통해 완료할 수 있습니다.

자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration) 설명서를 참조하십시오.

+++

### 인증 단계 FAQ {#authentication-phase-faqs-general}

+++인증 단계 FAQ

#### 1. 인증 단계의 목적은 무엇입니까? {#authentication-phase-faq1}

인증 단계의 목적은 클라이언트 응용 프로그램에 사용자의 ID를 확인하고 사용자 메타데이터 정보를 얻을 수 있는 기능을 제공하는 것입니다.

인증 단계는 클라이언트 응용 프로그램에서 컨텐츠를 재생해야 할 때 사전 인증 단계 또는 인증 단계에 대한 필수 단계 역할을 합니다.

#### 2. 인증 세션은 무엇이며 얼마나 오래 유효합니까? {#authentication-phase-faq2}

인증 세션은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session) 설명서에 정의된 용어입니다.

인증 세션은 세션 끝점에서 검색할 수 있는 시작된 인증 프로세스에 대한 정보를 저장합니다.

인증 세션은 `notAfter` 타임스탬프가 문제 발생 시 지정한 제한적이고 짧은 시간 동안 유효하며, 이는 사용자가 흐름을 다시 시작해야 하기 전에 인증 프로세스를 완료해야 하는 시간을 나타냅니다.

클라이언트 애플리케이션은 인증 프로세스를 진행하는 방법을 알기 위해 인증 세션 응답을 사용할 수 있다. 임시 액세스, 액세스 성능 저하 또는 사용자가 이미 인증된 경우와 같이 사용자를 인증할 필요가 없는 경우가 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [인증 세션 API 만들기](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [인증 세션 API 다시 시작](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 3. 인증 코드는 무엇이며 얼마나 오래 유효합니까? {#authentication-phase-faq3}

인증 코드는 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code) 설명서에 정의된 용어입니다.

인증 코드는 사용자가 인증 프로세스를 시작할 때 생성되는 고유한 값을 저장하며, 프로세스가 완료되거나 인증 세션이 만료될 때까지 사용자의 인증 세션을 고유하게 식별합니다.

인증 코드는 `notAfter` 타임스탬프가 인증 세션을 시작하는 시점에 지정된 제한되고 짧은 시간 동안 유효하며, 이는 사용자가 흐름을 다시 시작해야 하기 전에 인증 프로세스를 완료해야 하는 시간을 나타냅니다.

클라이언트 애플리케이션은 인증 코드를 사용하여 사용자가 성공적으로 인증을 완료했는지 여부를 확인하고 일반적으로 폴링 메커니즘을 통해 사용자의 프로필 정보를 검색할 수 있습니다.

또한 클라이언트 애플리케이션은 인증 세션이 만료되지 않은 것을 고려하여 사용자가 동일한 장치 또는 두 번째(화면) 장치에서 인증 프로세스를 완료하거나 다시 시작할 수 있도록 인증 코드를 사용할 수 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [인증 세션 API 만들기](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [인증 세션 API 다시 시작](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 4. 클라이언트 애플리케이션은 사용자가 유효한 인증 코드를 입력했는지, 인증 세션이 아직 만료되지 않았는지 어떻게 알 수 있습니까? {#authentication-phase-faq4}

클라이언트 애플리케이션은 인증 세션을 재개하거나 인증 코드와 연관된 인증 세션 정보를 검색할 책임이 있는 세션 엔드포인트 중 하나에 요청을 전송하여 보조(화면) 애플리케이션에서 사용자가 입력한 인증 코드의 유효성을 검사할 수 있습니다.

입력한 인증 코드가 잘못되었거나 인증 세션이 만료된 경우 클라이언트 응용 프로그램에서 [오류](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)을(를) 받습니다.

자세한 내용은 [인증 세션 다시 시작](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) 및 [인증 세션 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) 문서를 참조하십시오.

#### 5. 사용자가 이미 인증되었는지 클라이언트 애플리케이션이 어떻게 알 수 있습니까? {#authentication-phase-faq5}

클라이언트 애플리케이션은 사용자가 이미 인증되었는지 확인할 수 있는 다음 끝점 중 하나를 쿼리하고 프로필 정보를 반환할 수 있습니다.

* [프로필 엔드포인트 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 MVPD API의 프로필 엔드포인트](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정(인증) 코드 API의 프로필 끝점](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

자세한 내용은 다음 문서를 참조하십시오.

* [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 6. 프로필은 무엇이며 얼마나 오래 유효합니까? {#authentication-phase-faq6}

프로필은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile) 설명서에 정의된 용어입니다.

프로필에는 사용자의 인증 유효성, 메타데이터 정보 및 프로필 끝점에서 검색할 수 있는 더 많은 정보가 저장됩니다.

클라이언트 애플리케이션은 프로파일을 사용하여 사용자의 인증 상태를 알고, 사용자 메타데이터 정보에 액세스하고, 인증에 사용되는 방법 또는 ID를 제공하는 데 사용되는 엔티티를 찾을 수 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [프로필 엔드포인트 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 MVPD API의 프로필 엔드포인트](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정(인증) 코드 API의 프로필 끝점](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

`notAfter` 타임스탬프에서 쿼리할 때 지정된 제한된 기간 동안 프로필이 유효합니다. 이는 사용자가 인증 단계를 다시 거치기 전에 유효한 인증을 받는 시간을 나타냅니다.

인증(authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl)이라고 하는 이 제한된 기간은 조직 관리자 중 한 사람이나 사용자를 대신하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)를 통해 보고 변경할 수 있습니다.

자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) 설명서를 참조하십시오.

#### 7. 클라이언트 애플리케이션이 사용자의 프로필 정보를 영구 저장소에 캐시해야 합니까? {#authentication-phase-faq7}

클라이언트 애플리케이션은 불필요한 요청을 방지하고 다음 측면을 고려하여 사용자 경험을 개선하기 위해 사용자의 프로필 정보를 영구 저장소에 캐시해야 합니다.

| 속성 | 사용자 경험 |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `attributes` | 클라이언트 애플리케이션은 이를 사용하여 다양한 [사용자 메타데이터](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) 키(예: `zip`, `maxRating` 등)를 기반으로 사용자 경험을 개인화할 수 있습니다. |
| `mvpd` | 클라이언트 애플리케이션은 이를 사용하여 사용자의 선택된 TV 공급자를 추적할 수 있다.<br/><br/>현재 사용자 프로필이 만료되면 클라이언트 응용 프로그램에서 기억된 MVPD 선택을 사용하여 사용자에게 확인을 요청할 수 있습니다. |
| `notAfter` | 클라이언트 애플리케이션은 이를 사용하여 사용자 프로필 만료 날짜를 추적하고 만료되면 재인증 프로세스를 트리거하여 사전 인증 또는 인증 단계 중 오류를 방지할 수 있습니다.<br/><br/>클라이언트 응용 프로그램 오류 처리에서 [authenticated_profile_expired](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) 오류 코드를 처리할 수 있어야 합니다. 이는 클라이언트 응용 프로그램에서 사용자를 다시 인증해야 함을 나타냅니다. |

#### 8. 클라이언트 애플리케이션이 재인증 없이 사용자의 프로필을 확장할 수 있습니까? {#authentication-phase-faq8}

아니.

사용자 프로필의 만료는 MVPD와 함께 설정된 인증 TTL에 의해 결정되므로 사용자 상호 작용 없이 사용자 프로필을 유효성 이상으로 확장할 수 없습니다.

따라서 클라이언트 애플리케이션은 사용자에게 다시 인증하고 MVPD 로그인 페이지와 상호 작용하여 시스템에 대한 프로필을 새로 고치도록 요청해야 합니다.

그러나 HBA([홈 기반 인증](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md))를 지원하는 MVPD의 경우 사용자는 자격 증명을 입력할 필요가 없습니다.

#### 9. 사용 가능한 각 프로필 끝점의 사용 사례는 무엇입니까? {#authentication-phase-faq9}

프로필 엔드포인트는 클라이언트 애플리케이션이 사용자의 인증 상태를 알고, 사용자 메타데이터 정보에 액세스하고, 인증에 사용되는 방법 또는 ID를 제공하는 데 사용되는 엔티티를 찾을 수 있도록 설계되었습니다.

각 끝점은 다음과 같이 특정 사용 사례에 적합합니다.

| API | 설명 | 사용 사례 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [프로필 끝점 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | 모든 사용자 프로필을 검색합니다. | **사용자가 처음으로 클라이언트 응용 프로그램을 엽니다**<br/><br/>&#x200B;이 시나리오에서는 클라이언트 응용 프로그램에 사용자가 선택한 MVPD 식별자가 영구 저장소에 캐시되지 않습니다.<br/><br/>따라서 사용 가능한 모든 사용자 프로필을 검색하도록 단일 요청을 보냅니다. |
| [특정 MVPD API에 대한 프로필 끝점](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | 특정 MVPD과 연결된 사용자 프로필을 검색합니다. | **사용자가 이전 방문에서 인증한 후 클라이언트 응용 프로그램으로 돌아갑니다**<br/><br/>&#x200B;이 경우 클라이언트 응용 프로그램에는 이전에 선택한 사용자의 MVPD 식별자가 영구 저장소에 캐시되어 있어야 합니다.<br/><br/>따라서 특정 MVPD에 대한 사용자 프로필을 검색하기 위한 단일 요청을 보냅니다. |
| [특정(인증) 코드 API에 대한 프로필 끝점](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 특정 인증 코드와 연관된 사용자 프로필을 검색합니다. | **사용자가 인증 프로세스를 시작합니다**<br/><br/>&#x200B;이 시나리오에서는 클라이언트 응용 프로그램이 사용자가 인증을 완료했는지 확인하고 프로필 정보를 검색해야 합니다.<br/><br/>따라서 인증 코드와 연결된 사용자 프로필을 검색하는 폴링 메커니즘이 시작됩니다. |

#### 10. 사용자에게 여러 MVPD 프로필이 있는 경우 클라이언트 애플리케이션은 어떻게 해야 합니까? {#authentication-phase-faq10}

사용자에게 여러 MVPD 프로필이 있는 경우 클라이언트 애플리케이션은 이 시나리오를 처리하기 위한 최상의 접근 방식을 결정해야 합니다.

클라이언트 애플리케이션은 원하는 MVPD 프로필을 선택하라는 메시지를 사용자에게 보내거나, 응답에서 첫 번째 사용자 프로필을 선택하거나 유효 기간이 가장 긴 사용자 프로필을 선택하는 것과 같이 자동으로 선택하도록 선택할 수 있다.

#### 11. 사용자 프로필이 만료되면 어떻게 됩니까? {#authentication-phase-faq11}

사용자 프로필이 만료되면 프로필 끝점의 응답에 더 이상 포함되지 않습니다.

Profiles 엔드포인트가 빈 프로필 맵 응답을 반환하는 경우 클라이언트 애플리케이션이 새 인증 세션을 만들고 사용자에게 다시 인증하라는 메시지를 표시해야 합니다.

자세한 내용은 [인증 세션 API 만들기](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 설명서를 참조하세요.

#### 12. 언제 사용자 프로필이 유효하지 않게 됩니까? {#authentication-phase-faq12}

사용자 프로필은 다음 시나리오에서 유효하지 않게 됩니다.

* 프로필 끝점 응답의 `notAfter` 타임스탬프에 표시된 대로 인증 TTL이 만료되는 경우.
* 클라이언트 응용 프로그램에서 [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) 헤더 값을 변경하는 경우.
* 클라이언트 응용 프로그램이 [인증](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) 헤더 값을 검색하는 데 사용되는 클라이언트 자격 증명을 업데이트할 때.
* 클라이언트 응용 프로그램이 클라이언트 자격 증명을 얻는 데 사용되는 소프트웨어 문을 취소하거나 업데이트할 때.

#### 13. 클라이언트 애플리케이션이 언제 폴링 메커니즘을 시작해야 합니까? {#authentication-phase-faq13}

효율성을 보장하고 불필요한 요청을 방지하기 위해 클라이언트 애플리케이션은 다음 조건에서 폴링 메커니즘을 시작해야 합니다.

**기본(화면) 응용 프로그램 내에서 수행된 인증**

브라우저 구성 요소가 [세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 끝점 요청에서 `redirectUrl` 매개 변수에 대해 지정된 URL을 로드한 후 사용자가 최종 대상 페이지에 도달하면 기본(스트리밍) 응용 프로그램에서 폴링을 시작해야 합니다.

**보조(화면) 응용 프로그램 내에서 수행된 인증**

기본(스트리밍) 응용 프로그램은 사용자가 인증 프로세스를 시작하는 즉시([세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 끝점 응답을 받고 인증 코드를 사용자에게 표시한 후) 폴링을 시작해야 합니다.

#### 14. 클라이언트 애플리케이션이 폴링 메커니즘을 언제 중지해야 합니까? {#authentication-phase-faq14}

효율성을 보장하고 불필요한 요청을 방지하기 위해 클라이언트 애플리케이션은 다음 조건에서 폴링 메커니즘을 중지해야 합니다.

**인증 성공**

사용자의 프로필 정보가 성공적으로 검색되어 인증 상태가 확인됩니다. 이 시점에서는 더 이상 폴링이 필요하지 않습니다.

**인증 세션 및 코드 만료**

인증 세션 및 코드가 [세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 끝점 응답의 `notAfter` 타임스탬프에 표시된 대로 만료됩니다. 이 경우 사용자는 인증 프로세스를 다시 시작해야 하며 이전 인증 코드를 사용한 폴링을 즉시 중지해야 합니다.

**새 인증 코드가 생성됨**

사용자가 기본(화면) 장치에서 새 인증 코드를 요청하면 기존 세션이 더 이상 유효하지 않으며 이전 인증 코드를 사용한 폴링을 즉시 중지해야 합니다.

#### 15. 클라이언트 애플리케이션이 폴링 메커니즘에 사용해야 하는 호출 사이의 간격은 얼마입니까? {#authentication-phase-faq15}

효율성을 보장하고 불필요한 요청을 방지하기 위해 클라이언트 애플리케이션은 다음 조건에서 폴링 메커니즘 빈도를 구성해야 합니다.

| **기본(화면) 응용 프로그램 내에서 수행된 인증** | **보조(화면) 응용 프로그램 내에서 수행된 인증** |
|----------------------------------------------------------------------|----------------------------------------------------------------------|
| 기본(스트리밍) 애플리케이션은 1~5초마다 폴링해야 합니다. | 기본(스트리밍) 애플리케이션은 3~5초마다 폴링해야 합니다. |

#### 16. 클라이언트 응용 프로그램에서 보낼 수 있는 최대 폴링 요청 수는 얼마입니까? {#authentication-phase-faq16}

클라이언트 응용 프로그램은 Adobe Pass 인증 [제한 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits)에 정의된 현재 제한을 준수해야 합니다.

클라이언트 응용 프로그램 오류 처리는 클라이언트 응용 프로그램이 허용된 최대 요청 수를 초과했음을 나타내는 [429 요청이 너무 많음](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response) 오류 코드를 처리할 수 있어야 합니다.

자세한 내용은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md) 설명서를 참조하십시오.

#### 17. 클라이언트 애플리케이션은 어떻게 사용자의 메타데이터 정보를 얻을 수 있습니까? {#authentication-phase-faq17}

클라이언트 응용 프로그램은 [사용자 메타데이터](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) 정보를 프로필 정보의 일부로 반환할 수 있는 다음 끝점 중 하나를 쿼리할 수 있습니다.

* [프로필 엔드포인트 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 MVPD API의 프로필 엔드포인트](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정(인증) 코드 API의 프로필 끝점](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

사용자가 인증되었는지 확인할 때 얻은 프로필 정보에 이미 포함되어 있으므로 클라이언트 애플리케이션은 사용자의 메타데이터 정보를 검색하기 위해 별도의 엔드포인트를 쿼리할 필요가 없습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 18. 클라이언트 애플리케이션은 성능 저하된 액세스를 어떻게 관리해야 합니까? {#authentication-phase-faq18}

조직에서 [degradation](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md) 기능을 사용하려는 경우 클라이언트 응용 프로그램은 이러한 시나리오에서 REST API v2 끝점이 작동하는 방식에 대한 개요인 저하된 액세스 흐름을 처리해야 합니다.

자세한 내용은 [액세스 흐름이 저하됨](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md) 설명서를 참조하세요.

+++

### 사전 인증 단계 FAQ {#preauthorization-phase-faqs-general}

+++사전 인증 단계 FAQ

#### 1. 사전 승인 단계의 목적은 무엇입니까? {#preauthorization-phase-faq1}

사전 인증 단계의 목적은 사용자가 액세스할 수 있는 카탈로그의 리소스 하위 집합을 클라이언트 응용 프로그램에 제공하는 것입니다.

사전 인증 단계는 사용자가 클라이언트 애플리케이션을 처음 열거나 새 섹션으로 이동할 때 사용자 경험을 향상시킬 수 있습니다.

#### 2. 사전 인증 단계가 필수입니까? {#preauthorization-phase-faq2}

사전 인증 단계는 필수가 아닙니다. 클라이언트 애플리케이션이 사용자의 권한에 따라 먼저 필터링하지 않고 리소스 카탈로그를 표시하려는 경우 이 단계를 건너뛸 수 있습니다.

#### 3. 사전 승인 결정이 무엇입니까? {#preauthorization-phase-faq3}

사전 인증은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization) 설명서에 정의된 용어이며, 결정 용어는 [용어집](rest-api-v2-glossary.md#decision)에서도 찾을 수 있습니다.

사전 권한 부여 결정은 사전 권한 부여 결정 끝점에서 검색할 수 있는 MVPD 사전 권한 부여 프로세스 조회 결과에 대한 정보를 저장합니다.

클라이언트 애플리케이션은 사전 인가 결정들을 사용하여 TV 공급자(정보) 결정들이 사용자가 액세스하도록 허용할 리소스들의 서브세트를 제시할 수 있다.

자세한 내용은 다음 문서를 참조하십시오.

* [사전 인증 결정 API 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [기본 애플리케이션 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### 4. 사전 승인 결정에 미디어 토큰이 누락된 이유는 무엇입니까? {#preauthorization-phase-faq4}

권한 부여 단계의 목적이므로 리소스 재생에 사전 권한 부여 단계를 사용할 수 없기 때문에 사전 권한 부여 결정에 미디어 토큰이 없습니다.

#### 5. 리소스란 무엇이며, 지원되는 형식은 무엇입니까? {#preauthorization-phase-faq5}

리소스는 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) 설명서에 정의된 용어입니다.

리소스는 MVPD와 합의되고 클라이언트 애플리케이션이 스트리밍할 수 있는 콘텐츠와 연결된 고유 식별자입니다.

리소스 고유 식별자는 두 가지 형식을 가질 수 있습니다.

* 채널(브랜드)의 고유 식별자와 같은 간단한 문자열 형식입니다.
* 제목, 등급 및 자녀 보호 메타데이터와 같은 추가 정보가 포함된 미디어 RSS(MRSS) 형식입니다.

자세한 내용은 [보호된 리소스](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources) 설명서를 참조하세요.

#### 6. 클라이언트 애플리케이션이 한 번에 사전 권한 부여 결정을 받을 수 있는 리소스가 몇 개입니까? {#preauthorization-phase-faq6}

클라이언트 애플리케이션은 MVPD에 의해 부과된 조건으로 인해 단일 API 요청에서 제한된 수의 리소스(일반적으로 최대 5개)에 대한 사전 인가 결정을 얻을 수 있다.

이 최대 리소스 수는 조직 관리자 중 한 사람이나 사용자를 대신하여 활동하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)를 통해 MVPD와 동의한 후 보고 변경할 수 있습니다.

자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) 설명서를 참조하십시오.

+++

### 인증 단계 FAQ {#authorization-phase-faqs-general}

+++인증 단계 FAQ

#### 1. 승인 단계의 목적은 무엇입니까? {#authorization-phase-faq1}

인증 단계의 목적은 MVPD을 사용하여 권한을 확인한 후 사용자가 요청하는 리소스를 재생하는 기능을 클라이언트 애플리케이션에 제공하는 것입니다.

#### 2. 인증 단계가 필수입니까? {#authorization-phase-faq2}

인증 단계는 필수입니다. 스트림을 릴리스하기 전에 사용자에게 권한이 있는지 MVPD을 통해 확인해야 하므로 클라이언트 애플리케이션은 사용자가 요청하는 리소스를 재생하려는 경우 이 단계를 건너뛸 수 없습니다.

#### 3. 승인 결정은 무엇이며 얼마나 오래 유효합니까? {#authorization-phase-faq3}

인증은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization) 설명서에 정의된 용어이며, 결정 용어는 [용어집](rest-api-v2-glossary.md#decision)에서도 찾을 수 있습니다.

승인 결정은 승인 승인 결정 엔드포인트에서 검색할 수 있는 MVPD 승인 프로세스 조회 결과에 대한 정보를 저장합니다.

클라이언트 애플리케이션은 TV 공급자(신뢰할 수 있는) 결정이 사용자가 리소스 스트림에 액세스하도록 허용하는 경우 미디어 토큰을 포함하는 인증 결정을 사용하여 리소스 스트림을 재생할 수 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [인증 결정 API 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

권한 부여 결정은 문제 발생 시 지정된 제한되고 짧은 기간에 유효하며, 이는 MVPD을 다시 쿼리하기 전에 Adobe Pass 인증에 의해 캐시되는 시간을 나타냅니다.

인증(authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl)이라고 하는 이 제한된 기간은 조직 관리자 중 한 사람이나 사용자를 대신하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](rest-api-v2-glossary.md#tve-dashboard)를 통해 보고 변경할 수 있습니다.

자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) 설명서를 참조하십시오.

#### 4. 미디어 토큰은 무엇이며 유효기간은 얼마입니까? {#authorization-phase-faq4}

미디어 토큰은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token) 설명서에 정의된 용어입니다.

미디어 토큰은 의사 결정 권한 부여 끝점에서 검색할 수 있는 일반 텍스트로 전송된 서명된 문자열로 구성됩니다.

자세한 내용은 [미디어 토큰 검증기](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) 설명서를 참조하십시오.

미디어 토큰은 문제 발생 시 지정된 제한적이고 짧은 기간에 유효하며, 이는 의사 결정 승인 끝점을 다시 쿼리하기 전에 클라이언트 애플리케이션에서 사용해야 하는 시간을 나타냅니다.

TV 공급자(신뢰할 수 있는) 결정에서 사용자가 액세스할 수 있는 경우 클라이언트 애플리케이션은 미디어 토큰을 사용하여 리소스 스트림을 재생할 수 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [인증 결정 API 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### 5. 리소스란 무엇이며, 지원되는 형식은 무엇입니까? {#authorization-phase-faq5}

리소스는 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) 설명서에 정의된 용어입니다.

리소스는 MVPD와 합의되고 클라이언트 애플리케이션이 스트리밍할 수 있는 콘텐츠와 연결된 고유 식별자입니다.

리소스 고유 식별자는 두 가지 형식을 가질 수 있습니다.

* 채널(브랜드)의 고유 식별자와 같은 간단한 문자열 형식입니다.
* 제목, 등급 및 자녀 보호 메타데이터와 같은 추가 정보가 포함된 미디어 RSS(MRSS) 형식입니다.

자세한 내용은 [보호된 리소스](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources) 설명서를 참조하세요.

#### 6. 클라이언트 애플리케이션이 한 번에 몇 개의 리소스를 승인할 수 있습니까? {#authorization-phase-faq6}

클라이언트 애플리케이션은 MVPD에 의해 부과된 조건으로 인해 단일 API 요청에서 제한된 수의 리소스(일반적으로 최대 1)에 대한 인증 결정을 얻을 수 있습니다.

+++

### 로그아웃 단계 FAQ {#logout-phase-faqs-general}

+++으로그아웃 단계 FAQ

#### 1. 로그아웃 단계의 목적은 무엇입니까? {#logout-phase-faq1}

로그아웃 단계의 목적은 사용자 요청 시 Adobe Pass 인증 내에서 사용자의 인증된 프로필을 종료할 수 있는 기능을 클라이언트 애플리케이션에 제공하는 것입니다.

+++

### 헤더 FAQ {#headers-faqs-general}

+++헤더 FAQ

#### 1. 인증 헤더의 값을 계산하는 방법 {#headers-faq1}

>[!IMPORTANT]
>
> 클라이언트 응용 프로그램이 REST API V1에서 REST API V2로 마이그레이션하는 경우 클라이언트 응용 프로그램은 이전과 동일한 방법을 사용하여 `Bearer` 액세스 토큰 값을 계속 얻을 수 있습니다.

[인증](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) 요청 헤더에 클라이언트 응용 프로그램에서 Adobe Pass으로 보호된 API에 액세스하는 데 필요한 `Bearer` 액세스 토큰이 포함되어 있습니다.

인증 헤더 값은 등록 단계에서 Adobe Pass 인증에서 가져와야 합니다.

자세한 내용은 다음 문서를 참조하십시오.

* [동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [클라이언트 자격 증명 API 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [액세스 토큰 API 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [동적 클라이언트 등록 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

#### 2. AP-Device-Identifier 헤더의 값을 계산하는 방법 {#headers-faq2}

>[!IMPORTANT]
>
> 클라이언트 애플리케이션이 REST API V1에서 REST API V2로 마이그레이션하는 경우 클라이언트 애플리케이션은 이전과 동일한 방법을 사용하여 디바이스 식별자 값을 계속 계산할 수 있습니다.

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) 요청 헤더에 클라이언트 응용 프로그램에서 만든 스트리밍 장치 식별자가 포함되어 있습니다.

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) 헤더 설명서는 값을 계산하는 방법에 대한 주요 플랫폼의 예를 제공하지만, 클라이언트 애플리케이션은 자체 비즈니스 논리 및 요구 사항에 따라 다른 메서드를 사용하도록 선택할 수 있습니다.

#### 3. X-Device-Info 헤더에 대한 값을 계산하는 방법 {#headers-faq3}

>[!IMPORTANT]
>
> 클라이언트 애플리케이션이 REST API V1에서 REST API V2로 마이그레이션하는 경우 클라이언트 애플리케이션은 이전과 동일한 방법을 사용하여 디바이스 정보 값을 계속 계산할 수 있습니다.

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) 요청 헤더에 실제 스트리밍 장치와 관련된 클라이언트 정보(장치, 연결 및 응용 프로그램)가 포함되어 있습니다.

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) 헤더 설명서는 값을 계산하는 방법에 대한 주요 플랫폼에 대한 예를 제공하지만, 클라이언트 애플리케이션은 자체 비즈니스 논리 및 요구 사항에 따라 다른 메서드를 사용하도록 선택할 수 있습니다.

+++

### 기타 FAQ {#misc-faqs-general}

+++기타 FAQ

#### 1. REST API V2 요청 및 응답을 탐색하고 API를 테스트할 수 있습니까? {#misc-faq1}

예.

전용 [Adobe Developer](https://developer.adobe.com/adobe-pass/) 웹 사이트를 통해 REST API V2를 살펴볼 수 있습니다. Adobe Developer 웹 사이트에서는 다음에 대한 무제한 액세스를 제공합니다.

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)과(와) 상호 작용하려면 [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)을(를) 통해 얻은 `Bearer` 액세스 토큰과 함께 [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) 헤더를 포함해야 합니다.

[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)을(를) 사용하려면 REST API V2 범위가 포함된 소프트웨어 문이 필요합니다. 자세한 내용은 [DCR(동적 클라이언트 등록) FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md) 문서를 참조하십시오.

#### 2. OpenAPI 지원을 통한 API 개발 도구를 사용하여 REST API V2 요청 및 응답을 탐색할 수 있습니까? {#misc-faq2}

예.

[Adobe Developer](https://developer.adobe.com/adobe-pass/) 웹 사이트에서 [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) 및 [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)에 대한 OpenAPI 사양 파일을 다운로드할 수 있습니다.

OpenAPI 사양 파일을 다운로드하려면 다운로드 버튼을 클릭하여 다음 파일을 로컬 시스템에 저장합니다.

* [DCR API JSON](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [REST API V2 JSON](https://developer.adobe.com/adobe-pass/restApiV2.json)

그런 다음 이러한 파일을 기본 API 개발 도구로 가져와서 REST API V2 요청 및 응답을 탐색하고 API를 테스트할 수 있습니다.

#### 3. https://sp.auth-staging.adobe.com/apitest/api.html에 호스팅된 기존 API 테스트 도구를 계속 사용할 수 있습니까? {#misc-faq3}

아니.

REST API V2로 마이그레이션하는 클라이언트 애플리케이션은 https://developer.adobe.com/adobe-pass/에 호스팅된 새 테스트 도구를 사용해야 합니다. Adobe Developer 웹 사이트에서는 다음에 대한 무제한 액세스를 제공합니다.

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)과(와) 상호 작용하려면 [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)을(를) 통해 얻은 `Bearer` 액세스 토큰과 함께 [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) 헤더를 포함해야 합니다.

[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)을(를) 사용하려면 REST API V2 범위가 포함된 소프트웨어 문이 필요합니다. 자세한 내용은 [DCR(동적 클라이언트 등록) FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md) 문서를 참조하십시오.

+++

## 마이그레이션 FAQ {#migration-faqs}

기존 애플리케이션을 REST API V2로 마이그레이션해야 하는 애플리케이션에서 작업하는 경우 이 섹션을 계속합니다.

>[!MORELIKETHIS]
>
> * [DCR(Dynamic Client Registration) FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### 일반 마이그레이션 FAQ {#general-migration-faqs}

+++일반 마이그레이션 FAQ

#### 1. REST API V2로 마이그레이션된 새 클라이언트 애플리케이션을 모든 사용자에게 한 번에 롤아웃해야 합니까? {#migration-faq1}

아니.

클라이언트 애플리케이션은 REST API V2를 통합하는 새 버전을 모든 사용자에게 동시에 롤아웃할 필요가 없습니다.

Adobe Pass 인증은 2025년 말까지 REST API V1 또는 SDK을 통합하는 이전 클라이언트 애플리케이션 버전을 계속 지원할 예정입니다.

#### 2. 모든 API 및 플로우에서 REST API V2로 마이그레이션된 새 클라이언트 애플리케이션을 한 번에 롤아웃해야 합니까? {#migration-faq2}

예.

클라이언트 애플리케이션은 모든 Adobe Pass 인증 API 및 플로우에 걸쳐 REST API V2를 통합하는 새 버전을 동시에 롤아웃해야 합니다.

&#39;두 번째 화면 인증&#39; 흐름의 경우 클라이언트 응용 프로그램은 [기본](rest-api-v2-glossary.md#primary-application) 및 [보조](rest-api-v2-glossary.md#secondary-application) 응용 프로그램 모두에 대해 REST API V2를 통합하는 새 버전을 동시에 롤아웃해야 합니다.

Adobe Pass 인증은 API와 흐름 간에 REST API V2 및 REST API V1/SDK을 모두 통합하는 &#39;하이브리드&#39; 구현을 지원하지 않습니다.

#### 3. REST API V2로 마이그레이션된 새 클라이언트 애플리케이션으로 업데이트할 때 사용자 인증이 유지됩니까? {#migration-faq3}

아니.

REST API V1 또는 SDK을 통합하는 이전 클라이언트 애플리케이션 버전 내에서 얻은 사용자 인증은 유지되지 않습니다.

따라서 REST API V2로 마이그레이션된 새 클라이언트 애플리케이션 내에서 다시 인증해야 합니다.

#### 4. 향상된 오류 코드가 REST API V2에서 기본적으로 활성화되어 있습니까? {#migration-faq4}

예.

REST API V2로 마이그레이션하는 클라이언트 애플리케이션은 기본적으로 이 기능을 통해 보다 상세하고 정확한 오류 정보를 제공합니다.

자세한 내용은 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) 설명서를 참조하세요.

#### 5. REST API V2로 마이그레이션할 때 기존 통합에서 구성을 변경해야 합니까? {#migration-faq5}

아니.

REST API V2로 마이그레이션하는 클라이언트 애플리케이션은 기존 MVPD 통합에 대한 구성 변경이 필요하지 않습니다. 또한 기존 [서비스 공급자](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider) 및 [MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd)에 대해 동일한 식별자를 계속 사용합니다.

또한 Adobe Pass 인증에서 MVPD 종단점과 통신하는 데 사용되는 프로토콜은 변경되지 않습니다.

+++

### REST API V1에서 REST API V2로 마이그레이션 {#migration-rest-api-v1-to-rest-api-v2-faqs}

REST API V1에서 REST API V2로 마이그레이션해야 하는 응용 프로그램에서 작업하는 경우 이 하위 섹션을 계속합니다.

#### 등록 단계 FAQ {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++등록 단계 FAQ

##### 1. 등록 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#registration-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션하는 경우 등록 단계와 관련된 높은 수준의 변경 사항이 없습니다.

클라이언트 응용 프로그램은 계속해서 동일한 흐름을 사용하여 [DCR(Dynamic Client Registration)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) 프로세스를 통해 Adobe Pass 인증에 등록하고 액세스 토큰을 가져올 수 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [클라이언트 자격 증명 API 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [액세스 토큰 API 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [동적 클라이언트 등록 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

#### 구성 단계 FAQ {#configuration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++구성 단계 FAQ

##### 1. 구성 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#configuration-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 활성 통합이 있는 MVPD 목록 검색 | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 인증 단계 FAQ {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++인증 단계 FAQ

##### 1. 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#authentication-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 등록 코드 검색(인증 코드) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 등록 코드(인증 코드) 확인 | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 두 번째 화면(애플리케이션)에서 (MVPD) 인증 다시 시작 | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD) 인증 시작 | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 사용자 인증 상태 확인 | [GET <br/> /api/v1/checkauthn(첫 번째 화면)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn(두 번째 화면)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 인증 토큰(프로필) 검색 | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 메타데이터 정보 검색 | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### 사전 인증 단계 FAQ {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++사전 인증 단계 FAQ

##### 1. 사전 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#preauthorization-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사전 승인된 리소스 검색(사전 인증 결정) | [GET <br/> /api/v1/사전 승인(첫 번째 화면)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/사전 승인(두 번째 화면)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 인증 단계 FAQ {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++인증 단계 FAQ

##### 1. 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#authorization-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD) 인증 시작 | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 인증 토큰 검색(인증 결정) | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 짧은 인증 토큰 검색(미디어 토큰) | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### 로그아웃 단계 FAQ {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++으로그아웃 단계 FAQ

##### 1. 로그아웃 단계에서 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#logout-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 로그아웃 시작 | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### SDK에서 REST API V2로 마이그레이션 {#migration-sdk-to-rest-api-v2}

SDK에서 REST API V2로 마이그레이션해야 하는 애플리케이션에서 작업하는 경우 이 하위 섹션을 계속합니다.

#### 등록 단계 FAQ {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++등록 단계 FAQ

##### 1. 등록 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#registration-phase-sdk-to-v2-faq1}

SDK에서 REST API V2로 마이그레이션할 때 다음 표에 고려해야 할 높은 수준의 변경 사항이 나와 있습니다.

###### AccessEnabler JavaScript SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DCR(Dynamic Client Registration) 완료 | 생성자에 소프트웨어 구문 제공 | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[동적 클라이언트 등록 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DCR(Dynamic Client Registration) 완료 | 생성자에 소프트웨어 구문 제공 | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[동적 클라이언트 등록 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DCR(Dynamic Client Registration) 완료 | 생성자에 소프트웨어 구문 제공 | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[동적 클라이언트 등록 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DCR(Dynamic Client Registration) 완료 | 생성자에 소프트웨어 구문 제공 | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[동적 클라이언트 등록 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### 구성 단계 FAQ {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++구성 단계 FAQ

##### 1. 구성 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#configuration-phase-sdk-to-v2-faq1}

SDK에서 REST API V2로 마이그레이션할 때 다음 표에 고려해야 할 높은 수준의 변경 사항이 나와 있습니다.

###### AccessEnabler JavaScript SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 활성 통합이 있는 MVPD 목록 검색 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler iOS/tvOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 활성 통합이 있는 MVPD 목록 검색 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler Android SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 활성 통합이 있는 MVPD 목록 검색 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler FireOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 활성 통합이 있는 MVPD 목록 검색 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 인증 단계 FAQ {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++인증 단계 FAQ

##### 1. 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#authentication-phase-sdk-to-v2-faq1}

SDK에서 REST API V2로 마이그레이션할 때 다음 표에 고려해야 할 높은 수준의 변경 사항이 나와 있습니다.

###### AccessEnabler JavaScript SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD) 인증 시작 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 사용자 인증 상태 확인 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 메타데이터 정보 검색 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD) 인증 시작 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 사용자 인증 상태 확인 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 메타데이터 정보 검색 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 등록 코드 검색(인증 코드) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 등록 코드(인증 코드) 확인 | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 두 번째 화면(애플리케이션)에서 (MVPD) 인증 다시 시작 | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD) 인증 시작 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 사용자 인증 상태 확인 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 메타데이터 정보 검색 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD) 인증 시작 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 사용자 인증 상태 확인 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 메타데이터 정보 검색 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 등록 코드 검색(인증 코드) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 등록 코드(인증 코드) 확인 | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 두 번째 화면(애플리케이션)에서 (MVPD) 인증 다시 시작 | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD) 인증 시작 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 사용자 인증 상태 확인 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 메타데이터 정보 검색 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### 사전 인증 단계 FAQ {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++사전 인증 단계 FAQ

##### 1. 사전 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#preauthorization-phase-sdk-to-v2-faq1}

SDK에서 REST API V2로 마이그레이션할 때 다음 표에 고려해야 할 높은 수준의 변경 사항이 나와 있습니다.

###### AccessEnabler JavaScript SDK

| 범위 | SDK | REST API V2 | 관찰 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사전 승인된 리소스 검색(사전 인증 결정) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사전 승인된 리소스 검색(사전 인증 결정) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 범위 | SDK | REST API V2 | 관찰 |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사전 승인된 리소스 검색(사전 인증 결정) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| 범위 | SDK | REST API V2 | 관찰 |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사전 승인된 리소스 검색(사전 인증 결정) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 인증 단계 FAQ {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++인증 단계 FAQ

##### 1. 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#authorization-phase-sdk-to-v2-faq1}

SDK에서 REST API V2로 마이그레이션할 때 다음 표에 고려해야 할 높은 수준의 변경 사항이 나와 있습니다.

###### AccessEnabler JavaScript SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 짧은 인증 토큰 검색(미디어 토큰) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 짧은 인증 토큰 검색(미디어 토큰) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 짧은 인증 토큰 검색(미디어 토큰) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 짧은 인증 토큰 검색(미디어 토큰) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### 로그아웃 단계 FAQ {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++으로그아웃 단계 FAQ

##### 1. 로그아웃 단계에서 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#logout-phase-sdk-to-v2-faq1}

SDK에서 REST API V2로 마이그레이션할 때 다음 표에 고려해야 할 높은 수준의 변경 사항이 나와 있습니다.

###### AccessEnabler JavaScript SDK

| 범위 | SDK | REST API V2 | 관찰 |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 로그아웃 시작 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 로그아웃 시작 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 범위 | SDK | REST API V2 | 관찰 |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 로그아웃 시작 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 로그아웃 시작 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
