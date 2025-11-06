---
title: REST API V2 FAQ
description: REST API V2 FAQ
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '9682'
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

#### &#x200B;1. 구성 단계의 목적은 무엇입니까? {#configuration-phase-faq1}

구성 단계의 목적은 각 MVPD에 대해 Adobe Pass 인증으로 저장된 구성 세부 사항(예: `id`, `displayName`, `logoUrl` 등)과 함께 현재 통합되는 MVPD 목록을 클라이언트 응용 프로그램에 제공하는 것입니다.

구성 단계는 클라이언트 애플리케이션이 사용자에게 TV 공급자를 선택하라고 요청해야 할 때 인증 단계에 대한 필수 단계 역할을 합니다.

#### &#x200B;2. 구성 단계가 필수입니까? {#configuration-phase-faq2}

구성 단계는 필수가 아니며, 클라이언트 애플리케이션은 사용자가 인증 또는 재인증할 MVPD을 선택해야 하는 경우에만 구성을 검색해야 합니다.

클라이언트 애플리케이션은 다음 시나리오에서 이 단계를 건너뛸 수 있습니다.

* 사용자가 이미 인증되었습니다.
* 기본 또는 프로모션 [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) 기능을 통해 사용자에게 임시 액세스 권한이 제공됩니다.
* 사용자 인증이 만료되었지만 클라이언트 애플리케이션이 이전에 선택한 MVPD을 사용자 경험에 동기된 선택으로 캐시했으며 사용자에게 아직 해당 MVPD의 구독자인지 확인하라는 메시지를 표시했습니다.

#### &#x200B;3. 구성은 무엇이며 얼마나 오래 유효합니까? {#configuration-phase-faq3}

구성은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration) 설명서에 정의된 용어입니다.

구성에 `id`Configuration`displayName` 끝점에서 검색할 수 있는 `logoUrl`, [, ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) 특성 등으로 정의된 MVPD 목록이 포함되어 있습니다.

사용자가 인증 또는 재인증하기 위해 MVPD을 선택해야 하는 경우에만 클라이언트 애플리케이션이 구성을 검색해야 합니다.

클라이언트 애플리케이션은 사용자가 TV 공급자를 선택해야 할 때마다 구성 응답을 사용하여 사용 가능한 MVPD 옵션이 있는 UI 선택기를 제공할 수 있습니다.

인증, 사전 권한 부여, 권한 부여 또는 로그아웃 단계를 계속하려면 클라이언트 응용 프로그램에서 MVPD의 구성 수준 `id` 특성에 지정된 대로 사용자가 선택한 MVPD 식별자를 저장해야 합니다.

자세한 내용은 [구성 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) 설명서를 참조하세요.

#### &#x200B;4. 구성이 서비스 공급자, 플랫폼 또는 사용자에게만 적용됩니까? {#configuration-phase-faq4}

구성은 [서비스 공급자](rest-api-v2-glossary.md#service-provider)에만 적용됩니다.

구성은 플랫폼 유형에 따라 다릅니다.

사용자별로 구성이 다릅니다.

서버 간 아키텍처를 사용하는 클라이언트 애플리케이션의 경우 서버측 메모리 스토리지에 각 플랫폼 유형에 대한 구성 응답(예: 2분 TTL 포함)을 캐시하는 것이 좋습니다. 이렇게 하면 각 사용자에 대한 불필요한 요청이 줄어들고 전반적인 사용자 경험이 향상됩니다.

#### &#x200B;5. 클라이언트 애플리케이션이 구성 응답 정보를 영구 저장소에 캐시해야 합니까? {#configuration-phase-faq5}

>[!IMPORTANT]
> 
> 서버 간 아키텍처를 사용하는 클라이언트 애플리케이션의 경우 서버측 메모리 스토리지에 각 플랫폼 유형에 대한 구성 응답(예: 2분 TTL 포함)을 캐시하는 것이 좋습니다. 이렇게 하면 각 사용자에 대한 불필요한 요청이 줄어들고 전반적인 사용자 경험이 향상됩니다.

사용자가 인증 또는 재인증하기 위해 MVPD을 선택해야 하는 경우에만 클라이언트 애플리케이션이 구성을 검색해야 합니다.

클라이언트 애플리케이션은 불필요한 요청을 방지하고 다음과 같은 경우 사용자 경험을 개선하기 위해 구성 응답 정보를 메모리 저장소에 캐시해야 합니다.

* 사용자가 이미 인증되었습니다.
* 기본 또는 프로모션 [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) 기능을 통해 사용자에게 임시 액세스 권한이 제공됩니다.
* 사용자 인증이 만료되었지만 클라이언트 애플리케이션이 이전에 선택한 MVPD을 사용자 경험에 동기된 선택으로 캐시했으며 사용자에게 아직 해당 MVPD의 구독자인지 확인하라는 메시지를 표시했습니다.

#### &#x200B;6. 클라이언트 애플리케이션이 자체 MVPD 목록을 관리할 수 있습니까? {#configuration-phase-faq6}

클라이언트 애플리케이션은 자체 MVPD 목록을 관리할 수 있지만, MVPD 식별자를 Adobe Pass 인증과 동기화해야 합니다. 따라서 목록이 최신 상태이고 정확한지 확인하려면 Adobe Pass 인증에서 제공하는 구성을 사용하는 것이 좋습니다.

제공된 MVPD 식별자가 잘못되었거나 지정된 [서비스 공급자](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)와 활성 통합이 없는 경우 클라이언트 응용 프로그램은 Adobe Pass 인증 REST API V2에서 [오류](rest-api-v2-glossary.md#service-provider)을(를) 받습니다.

#### &#x200B;7. 클라이언트 애플리케이션이 MVPD 목록을 필터링할 수 있습니까? {#configuration-phase-faq7}

클라이언트 애플리케이션은 자신의 비즈니스 로직 및 이전 선택의 사용자 위치 또는 사용자 내역과 같은 요구 사항을 기반으로 사용자 지정 메커니즘을 구현함으로써 구성 응답에 제공된 MVPD의 목록을 필터링할 수 있다.

클라이언트 응용 프로그램은 [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) MVPD 또는 아직 개발 또는 테스트 중인 통합이 있는 MVPD의 목록을 필터링할 수 있습니다.

#### &#x200B;8. MVPD과의 통합이 비활성화되고 비활성화로 표시되면 어떻게 됩니까? {#configuration-phase-faq8}

MVPD과의 통합이 비활성화되고 비활성화로 표시되면 추가 구성 응답에 제공된 MVPD 목록에서 MVPD이 제거되며 고려해야 할 두 가지 중요한 결과가 있습니다.

* 해당 MVPD의 인증되지 않은 사용자는 더 이상 해당 MVPD을 사용하여 인증 단계를 완료할 수 없습니다.
* 해당 MVPD의 인증된 사용자는 더 이상 해당 MVPD을 사용하여 사전 인증, 권한 부여 또는 로그아웃 단계를 완료할 수 없습니다.

선택한 MVPD에 지정된 [서비스 공급자](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)와의 활성 통합이 더 이상 없는 경우 클라이언트 응용 프로그램이 Adobe Pass 인증 REST API V2에서 [오류](rest-api-v2-glossary.md#service-provider)을(를) 받습니다.

#### &#x200B;9. MVPD과의 통합이 다시 활성화되어 활성으로 표시되면 어떻게 됩니까? {#configuration-phase-faq9}

MVPD과의 통합이 다시 활성화되어 활성으로 표시되면 MVPD은 추가 구성 응답에 제공된 MVPD 목록에 다시 포함되며 다음과 같은 두 가지 중요한 결과가 있습니다.

* 해당 MVPD의 인증되지 않은 사용자는 해당 MVPD을 사용하여 인증 단계를 다시 완료할 수 있습니다.
* 해당 MVPD의 인증된 사용자는 해당 MVPD을 사용하여 사전 인증, 권한 부여 또는 로그아웃 단계를 다시 완료할 수 있습니다.

#### &#x200B;10. MVPD과의 통합을 활성화하거나 비활성화하는 방법 {#configuration-phase-faq10}

이 작업은 조직 관리자 중 한 사람이나 사용자를 대신하여 활동하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)를 통해 완료할 수 있습니다.

자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration) 설명서를 참조하십시오.

+++

### 인증 단계 FAQ {#authentication-phase-faqs-general}

+++인증 단계 FAQ

#### &#x200B;1. 인증 단계의 목적은 무엇입니까? {#authentication-phase-faq1}

인증 단계의 목적은 클라이언트 응용 프로그램에 사용자의 ID를 확인하고 사용자 메타데이터 정보를 얻을 수 있는 기능을 제공하는 것입니다.

인증 단계는 클라이언트 응용 프로그램에서 컨텐츠를 재생해야 할 때 사전 인증 단계 또는 인증 단계에 대한 필수 단계 역할을 합니다.

#### &#x200B;2. 인증 단계가 필수입니까? {#authentication-phase-faq2}

인증 단계는 필수입니다. Adobe Pass 인증 내에 유효한 프로필이 없는 경우 클라이언트 애플리케이션이 사용자를 인증해야 합니다.

클라이언트 애플리케이션은 다음 시나리오에서 이 단계를 건너뛸 수 있습니다.

* 사용자가 이미 인증되었으며 프로필이 여전히 유효합니다.
* 기본 또는 프로모션 [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) 기능을 통해 사용자에게 임시 액세스 권한이 제공됩니다.

클라이언트 응용 프로그램 오류를 처리하려면 클라이언트 응용 프로그램에서 사용자를 인증해야 함을 나타내는 [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) 코드(예: `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated` 등)를 처리해야 합니다.

#### &#x200B;3. 인증 세션은 무엇이며 얼마나 오래 유효합니까? {#authentication-phase-faq3}

인증 세션은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session) 설명서에 정의된 용어입니다.

인증 세션은 세션 끝점에서 검색할 수 있는 시작된 인증 프로세스에 대한 정보를 저장합니다.

인증 세션은 `notAfter` 타임스탬프가 문제 발생 시 지정한 제한적이고 짧은 시간 동안 유효하며, 이는 사용자가 흐름을 다시 시작해야 하기 전에 인증 프로세스를 완료해야 하는 시간을 나타냅니다.

클라이언트 애플리케이션은 인증 프로세스를 진행하는 방법을 알기 위해 인증 세션 응답을 사용할 수 있다. 임시 액세스, 액세스 성능 저하 또는 사용자가 이미 인증된 경우와 같이 사용자를 인증할 필요가 없는 경우가 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [인증 세션 API 만들기](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [인증 세션 API 다시 시작](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;4. 인증 코드는 무엇이며 얼마나 오래 유효합니까? {#authentication-phase-faq4}

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

#### &#x200B;5. 클라이언트 애플리케이션은 사용자가 유효한 인증 코드를 입력했는지, 인증 세션이 아직 만료되지 않았는지 어떻게 알 수 있습니까? {#authentication-phase-faq5}

클라이언트 애플리케이션은 인증 세션을 재개하거나 인증 코드와 연관된 인증 세션 정보를 검색할 책임이 있는 세션 엔드포인트 중 하나에 요청을 전송하여 보조(화면) 애플리케이션에서 사용자가 입력한 인증 코드의 유효성을 검사할 수 있습니다.

입력한 인증 코드가 잘못되었거나 인증 세션이 만료된 경우 클라이언트 응용 프로그램에서 [오류](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)을(를) 받습니다.

자세한 내용은 [인증 세션 다시 시작](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) 및 [인증 세션 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) 문서를 참조하십시오.

#### &#x200B;6. 사용자가 이미 인증되었는지 클라이언트 애플리케이션이 어떻게 알 수 있습니까? {#authentication-phase-faq6}

클라이언트 애플리케이션은 사용자가 이미 인증되었는지 확인할 수 있는 다음 끝점 중 하나를 쿼리하고 프로필 정보를 반환할 수 있습니다.

* [프로필 엔드포인트 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 MVPD API의 프로필 엔드포인트](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정(인증) 코드 API의 프로필 끝점](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

자세한 내용은 다음 문서를 참조하십시오.

* [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### &#x200B;7. 프로필은 무엇이며 얼마나 오래 유효합니까? {#authentication-phase-faq7}

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

#### &#x200B;8. 클라이언트 애플리케이션이 사용자의 프로필 정보를 영구 저장소에 캐시해야 합니까? {#authentication-phase-faq8}

클라이언트 애플리케이션은 불필요한 요청을 방지하고 다음 측면을 고려하여 사용자 경험을 개선하기 위해 사용자 프로필 정보의 일부를 영구 저장소에 캐시해야 합니다.

| 속성 | 사용자 경험 |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `mvpd` | 클라이언트 애플리케이션은 이를 사용하여 사용자가 선택한 TV 공급자를 추적하고 사전 승인 또는 승인 단계 동안 계속 사용할 수 있습니다.<br/><br/>현재 사용자 프로필이 만료되면 클라이언트 응용 프로그램에서 기억된 MVPD 선택을 사용하여 사용자에게 확인을 요청할 수 있습니다. |
| `attributes` | 클라이언트 애플리케이션은 이를 사용하여 다양한 [사용자 메타데이터](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) 키(예: `zip`, `maxRating` 등)를 기반으로 사용자 경험을 개인화할 수 있습니다.<br/><br/>사용자 메타데이터는 인증 흐름이 완료된 후에 사용할 수 있으므로 [사용자 메타데이터](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) 정보가 이미 프로필 정보에 포함되어 있으므로 클라이언트 응용 프로그램에서 개별 끝점을 쿼리하여 검색할 필요가 없습니다.<br/><br/>MVPD 및 특정 메타데이터 특성에 따라 인증 흐름 중에 특정 메타데이터 특성이 업데이트될 수 있습니다. 따라서 클라이언트 애플리케이션은 최신 사용자 메타데이터를 검색하기 위해 프로필 API를 다시 쿼리해야 할 수 있습니다. |
| `notAfter` | 클라이언트 애플리케이션은 이를 사용하여 사용자 프로필 만료 날짜를 추적할 수 있습니다.<br/><br/>클라이언트 응용 프로그램 오류를 처리하려면 [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) 코드(예: `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated` 등)를 처리해야 합니다. 이는 클라이언트 응용 프로그램에서 사용자를 인증해야 함을 나타냅니다. |

#### &#x200B;9. 클라이언트 애플리케이션이 재인증 없이 사용자의 프로필을 확장할 수 있습니까? {#authentication-phase-faq9}

아니.

사용자 프로필의 만료는 MVPD와 함께 설정된 인증 TTL에 의해 결정되므로 사용자 상호 작용 없이 사용자 프로필을 유효성 이상으로 확장할 수 없습니다.

따라서 클라이언트 애플리케이션은 사용자에게 다시 인증하고 MVPD 로그인 페이지와 상호 작용하여 시스템에 대한 프로필을 새로 고치도록 요청해야 합니다.

그러나 HBA([홈 기반 인증](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md))를 지원하는 MVPD의 경우 사용자는 자격 증명을 입력할 필요가 없습니다.

#### &#x200B;10. 사용 가능한 각 프로필 끝점의 사용 사례는 무엇입니까? {#authentication-phase-faq10}

기본 프로필 끝점은 클라이언트 응용 프로그램에 사용자의 인증 상태를 알고, 사용자 메타데이터 정보에 액세스하고, 인증에 사용되는 방법 또는 ID를 제공하는 데 사용되는 엔티티를 찾을 수 있는 기능을 제공하도록 설계되었습니다.

각 끝점은 다음과 같이 특정 사용 사례에 적합합니다.

| API | 설명 | 사용 사례 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [프로필 끝점 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | 모든 사용자 프로필을 검색합니다. | **사용자가 처음으로 클라이언트 응용 프로그램을 엽니다**<br/><br/>&#x200B;이 시나리오에서는 클라이언트 응용 프로그램에 사용자가 선택한 MVPD 식별자가 영구 저장소에 캐시되지 않습니다.<br/><br/>따라서 사용 가능한 모든 사용자 프로필을 검색하도록 단일 요청을 보냅니다. |
| [특정 MVPD API에 대한 프로필 끝점](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | 특정 MVPD과 연결된 사용자 프로필을 검색합니다. | **사용자가 이전 방문에서 인증한 후 클라이언트 응용 프로그램으로 돌아갑니다**<br/><br/>&#x200B;이 경우 클라이언트 응용 프로그램에는 이전에 선택한 사용자의 MVPD 식별자가 영구 저장소에 캐시되어 있어야 합니다.<br/><br/>따라서 특정 MVPD에 대한 사용자 프로필을 검색하기 위한 단일 요청을 보냅니다. |
| [특정(인증) 코드 API에 대한 프로필 끝점](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 특정 인증 코드와 연관된 사용자 프로필을 검색합니다. | **사용자가 인증 프로세스를 시작합니다**<br/><br/>&#x200B;이 시나리오에서는 클라이언트 응용 프로그램이 사용자가 인증을 완료했는지 확인하고 프로필 정보를 검색해야 합니다.<br/><br/>따라서 인증 코드와 연결된 사용자 프로필을 검색하는 폴링 메커니즘이 시작됩니다. |

자세한 내용은 [기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md) 및 [보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md) 문서를 참조하십시오.

Profiles SSO 끝점은 다른 용도로 사용되며 클라이언트 애플리케이션에 파트너 인증 응답을 사용하여 사용자 프로필을 만들고 이를 한 번의 작업으로 검색하는 기능을 제공합니다.

| API | 설명 | 사용 사례 |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [프로필 SSO 끝점 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) | 파트너 인증 응답을 사용하여 사용자 프로필을 만들고 검색합니다. | **사용자는 응용 프로그램에서 파트너 SSO(Single Sign-On)를 사용하여 인증하도록 허용합니다**<br/><br/>&#x200B;이 시나리오에서는 클라이언트 응용 프로그램이 파트너 인증 응답을 받은 후 사용자 프로필을 만들어 한 번의 작업으로 검색해야 합니다. |

이후 쿼리의 경우 기본 프로필 끝점을 사용하여 사용자의 인증 상태를 결정하고, 사용자 메타데이터 정보에 액세스하고, 인증에 사용되는 방법 또는 ID를 제공하는 데 사용되는 엔티티를 찾아야 합니다.

자세한 내용은 [파트너 흐름을 사용한 Single Sign-On](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) 및 [Apple SSO Cookbook(REST API V2)](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md) 문서를 참조하십시오.

#### &#x200B;11. 사용자에게 여러 MVPD 프로필이 있는 경우 클라이언트 애플리케이션은 어떻게 해야 합니까? {#authentication-phase-faq11}

여러 프로필을 지원할지 여부는 클라이언트 애플리케이션의 비즈니스 요구 사항에 따라 다릅니다.

대부분의 사용자는 하나의 프로필만 갖게 됩니다. 그러나 여러 프로필이 존재하는 경우(아래에 자세히 설명 참조) 클라이언트 애플리케이션은 프로필 선택을 위한 최상의 사용자 경험을 결정할 책임이 있습니다.

클라이언트 애플리케이션은 원하는 MVPD 프로필을 선택하라는 메시지를 사용자에게 보내거나, 응답에서 첫 번째 사용자 프로필을 선택하거나 유효 기간이 가장 긴 사용자 프로필을 선택하는 것과 같이 자동으로 선택하도록 선택할 수 있다.

REST API v2는 다음을 수용할 수 있도록 여러 프로필을 지원합니다.

* 일반 MVPD 프로필과 SSO(Single Sign-On)를 통해 얻은 프로필 중에서 선택해야 할 수 있는 사용자.
* 일반 MVPD에서 로그아웃할 필요 없이 임시 액세스 권한을 제공 받는 사용자.
* MVPD 구독이 DTC(직접 소비자) 서비스와 결합된 사용자입니다.
* 여러 MVPD 구독이 있는 사용자.

#### &#x200B;12. 사용자 프로필이 만료되면 어떻게 됩니까? {#authentication-phase-faq12}

사용자 프로필이 만료되면 프로필 끝점의 응답에 더 이상 포함되지 않습니다.

Profiles 엔드포인트가 빈 프로필 맵 응답을 반환하는 경우 클라이언트 애플리케이션이 새 인증 세션을 만들고 사용자에게 다시 인증하라는 메시지를 표시해야 합니다.

자세한 내용은 [인증 세션 API 만들기](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 설명서를 참조하세요.

#### &#x200B;13. 언제 사용자 프로필이 유효하지 않게 됩니까? {#authentication-phase-faq13}

사용자 프로필은 다음 시나리오에서 유효하지 않게 됩니다.

* 프로필 끝점 응답의 `notAfter` 타임스탬프에 표시된 대로 인증 TTL이 만료되는 경우.
* 클라이언트 응용 프로그램에서 [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) 헤더 값을 변경하는 경우.
* 클라이언트 응용 프로그램이 [인증](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) 헤더 값을 검색하는 데 사용되는 클라이언트 자격 증명을 업데이트할 때.
* 클라이언트 응용 프로그램이 클라이언트 자격 증명을 얻는 데 사용되는 소프트웨어 문을 취소하거나 업데이트할 때.

#### &#x200B;14. 클라이언트 애플리케이션이 언제 폴링 메커니즘을 시작해야 합니까? {#authentication-phase-faq14}

효율성을 보장하고 불필요한 요청을 방지하기 위해 클라이언트 애플리케이션은 다음 조건에서 폴링 메커니즘을 시작해야 합니다.

**기본(화면) 응용 프로그램 내에서 수행된 인증**

브라우저 구성 요소가 `redirectUrl`세션[ 끝점 요청에서 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 매개 변수에 대해 지정된 URL을 로드한 후 사용자가 최종 대상 페이지에 도달하면 기본(스트리밍) 응용 프로그램에서 폴링을 시작해야 합니다.

**보조(화면) 응용 프로그램 내에서 수행된 인증**

기본(스트리밍) 응용 프로그램은 사용자가 인증 프로세스를 시작하는 즉시([세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 끝점 응답을 받고 인증 코드를 사용자에게 표시한 후) 폴링을 시작해야 합니다.

#### &#x200B;15. 클라이언트 애플리케이션이 폴링 메커니즘을 언제 중지해야 합니까? {#authentication-phase-faq15}

효율성을 보장하고 불필요한 요청을 방지하기 위해 클라이언트 애플리케이션은 다음 조건에서 폴링 메커니즘을 중지해야 합니다.

**인증 성공**

사용자의 프로필 정보가 성공적으로 검색되어 인증 상태가 확인됩니다. 이 시점에서는 더 이상 폴링이 필요하지 않습니다.

**인증 세션 및 코드 만료**

`notAfter`세션[ 끝점 응답에서 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 타임스탬프(예: 30분)에 표시된 대로 인증 세션 및 코드가 만료됩니다. 이 경우 사용자는 인증 프로세스를 다시 시작해야 하며 이전 인증 코드를 사용한 폴링을 즉시 중지해야 합니다.

**새 인증 코드가 생성됨**

사용자가 기본(화면) 장치에서 새 인증 코드를 요청하면 기존 세션이 더 이상 유효하지 않으며 이전 인증 코드를 사용한 폴링을 즉시 중지해야 합니다.

#### &#x200B;16. 클라이언트 애플리케이션이 폴링 메커니즘에 사용해야 하는 호출 사이의 간격은 얼마입니까? {#authentication-phase-faq16}

효율성을 보장하고 불필요한 요청을 방지하기 위해 클라이언트 애플리케이션은 다음 조건에서 폴링 메커니즘 빈도를 구성해야 합니다.

| **기본(화면) 응용 프로그램 내에서 수행된 인증** | **보조(화면) 응용 프로그램 내에서 수행된 인증** |
|----------------------------------------------------------------------------|----------------------------------------------------------------------------|
| 기본(스트리밍) 애플리케이션은 3~5초 이상 간격으로 폴링해야 합니다. | 기본(스트리밍) 애플리케이션은 3~5초 이상 간격으로 폴링해야 합니다. |

#### &#x200B;17. 클라이언트 응용 프로그램에서 보낼 수 있는 최대 폴링 요청 수는 얼마입니까? {#authentication-phase-faq17}

클라이언트 응용 프로그램은 Adobe Pass 인증 [제한 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits)에 정의된 현재 제한을 준수해야 합니다.

클라이언트 응용 프로그램 오류 처리는 클라이언트 응용 프로그램이 허용된 최대 요청 수를 초과했음을 나타내는 [429 요청이 너무 많음](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response) 오류 코드를 처리할 수 있어야 합니다.

자세한 내용은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md) 설명서를 참조하십시오.

#### &#x200B;18. 클라이언트 애플리케이션은 어떻게 사용자의 메타데이터 정보를 얻을 수 있습니까? {#authentication-phase-faq18}

클라이언트 응용 프로그램은 [사용자 메타데이터](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) 정보를 프로필 정보의 일부로 반환할 수 있는 다음 끝점 중 하나를 쿼리할 수 있습니다.

* [프로필 엔드포인트 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 MVPD API의 프로필 엔드포인트](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정(인증) 코드 API의 프로필 끝점](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

사용자 메타데이터는 인증 흐름이 완료된 후에 사용할 수 있으므로 [사용자 메타데이터](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) 정보를 검색하기 위해 클라이언트 응용 프로그램이 별도의 끝점을 쿼리할 필요가 없습니다. 이미 프로필 정보에 포함되어 있기 때문입니다.

자세한 내용은 다음 문서를 참조하십시오.

* [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

특정 메타데이터 속성은 MVPD 및 특정 메타데이터 속성에 따라 인증 흐름 중에 업데이트될 수 있습니다. 그 결과 클라이언트 애플리케이션은 최신 사용자 메타데이터를 검색하기 위해 위의 API를 다시 쿼리해야 할 수 있습니다.

#### &#x200B;19. 클라이언트 애플리케이션은 성능 저하된 액세스를 어떻게 관리해야 합니까? {#authentication-phase-faq19}

[성능 저하 기능](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)을 사용하면 MVPD의 인증 또는 권한 부여 서비스에 문제가 발생하는 경우에도 클라이언트 응용 프로그램에서 사용자를 위한 원활한 스트리밍 환경을 유지할 수 있습니다.

요약하면, MVPD의 일시적인 서비스 중단에도 불구하고 콘텐츠에 중단 없이 액세스할 수 있습니다.

조직에서 프리미엄 저하 기능을 사용하려는 경우 클라이언트 애플리케이션이 그러한 시나리오에서 REST API v2 종단점이 작동하는 방식을 개략적으로 설명하는 저하된 액세스 흐름을 처리해야 합니다.

자세한 내용은 [액세스 흐름이 저하됨](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md) 설명서를 참조하세요.

#### &#x200B;20. 클라이언트 애플리케이션은 임시 액세스를 어떻게 관리해야 합니까? {#authentication-phase-faq20}

[TempPass 기능](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)을 사용하면 클라이언트 응용 프로그램에서 사용자에게 임시 액세스를 제공할 수 있습니다.

요약하면, 지정된 기간 동안 콘텐츠 또는 사전 정의된 VOD 제목 수에 대한 제한 시간 액세스를 제공하여 사용자를 참여시킬 수 있습니다.

조직에서 프리미엄 TempPass 기능을 사용하려는 경우 클라이언트 애플리케이션이 이러한 시나리오에서 REST API v2 종단점이 작동하는 방식에 대한 개요를 제공하는 임시 액세스 흐름을 처리해야 합니다.

이전 API 버전에서 클라이언트 애플리케이션은 임시 액세스를 제공하기 위해 일반 MVPD으로 인증된 사용자를 로그아웃해야 했습니다.

REST API v2를 사용하면 클라이언트 애플리케이션이 스트림 인증 시 일반 MVPD과 TempPass MVPD 간을 원활하게 전환할 수 있으므로 사용자가 로그아웃할 필요가 없습니다.

자세한 내용은 [임시 액세스 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) 설명서를 참조하세요.

#### &#x200B;21. 클라이언트 애플리케이션은 교차 장치 단일 사인온 액세스를 어떻게 관리해야 합니까? {#authentication-phase-faq21}

클라이언트 애플리케이션이 여러 장치에서 일관된 고유 사용자 식별자를 제공하는 경우 REST API v2에서 장치 간 SSO(Single Sign-On)를 활성화할 수 있습니다.

서비스 토큰이라고 하는 이 식별자는 선택한 외부 ID 서비스의 구현 또는 통합을 통해 클라이언트 애플리케이션에서 생성해야 합니다.

자세한 내용은 [서비스 토큰 흐름을 사용하는 Single Sign-On](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) 설명서를 참조하세요.

+++

### 사전 인증 단계 FAQ {#preauthorization-phase-faqs-general}

+++사전 인증 단계 FAQ

#### &#x200B;1. 사전 승인 단계의 목적은 무엇입니까? {#preauthorization-phase-faq1}

사전 인증 단계의 목적은 사용자가 액세스할 수 있는 카탈로그의 리소스 하위 집합을 클라이언트 응용 프로그램에 제공하는 것입니다.

사전 인증 단계는 사용자가 클라이언트 애플리케이션을 처음 열거나 새 섹션으로 이동할 때 사용자 경험을 향상시킬 수 있습니다.

#### &#x200B;2. 사전 인증 단계가 필수입니까? {#preauthorization-phase-faq2}

사전 인증 단계는 필수가 아닙니다. 클라이언트 애플리케이션이 사용자의 권한에 따라 먼저 필터링하지 않고 리소스 카탈로그를 표시하려는 경우 이 단계를 건너뛸 수 있습니다.

#### &#x200B;3. 사전 승인 결정이 무엇입니까? {#preauthorization-phase-faq3}

사전 인증은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization) 설명서에 정의된 용어이며, 결정 용어는 [용어집](rest-api-v2-glossary.md#decision)에서도 찾을 수 있습니다.

사전 권한 부여 결정은 사전 권한 부여 결정 끝점에서 검색할 수 있는 MVPD 사전 권한 부여 프로세스 조회 결과에 대한 정보를 저장합니다.

클라이언트 애플리케이션은 사전 인가 결정들을 사용하여 TV 공급자(정보) 결정들이 사용자가 액세스하도록 허용할 리소스들의 서브세트를 제시할 수 있다.

자세한 내용은 다음 문서를 참조하십시오.

* [사전 인증 결정 API 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [기본 애플리케이션 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### &#x200B;4. 클라이언트 애플리케이션이 영구 저장소에 사전 인증 결정을 캐시해야 합니까? {#preauthorization-phase-faq4}

클라이언트 애플리케이션은 영구 스토리지에 사전 인증 결정을 저장할 필요가 없습니다. 그러나 사용자 경험을 개선하기 위해 허용 결정을 메모리에 캐시하는 것이 좋습니다. 이렇게 하면 이미 사전 승인된 리소스에 대한 의사 결정 사전 권한 부여 종단점에 대한 불필요한 호출을 방지하여 지연을 줄이고 성능을 개선할 수 있습니다.

#### &#x200B;5. 클라이언트 애플리케이션은 사전 승인 결정이 거부된 이유를 어떻게 확인할 수 있습니까? {#preauthorization-phase-faq5}

클라이언트 애플리케이션은 의사 결정 사전 인증 끝점의 응답에 포함된 [오류 코드 및 메시지](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)를 검사하여 사전 인증 거부 이유를 확인할 수 있습니다. 이러한 세부 사항은 insight에 사전 인증 요청이 거부된 특정 이유를 제공하여 사용자 경험을 알리거나 애플리케이션에서 필요한 처리를 트리거하는 데 도움이 됩니다.

사전 권한 부여 결정을 검색하기 위해 구현된 모든 다시 시도 메커니즘이 사전 권한 부여 결정이 거부되는 경우 무한 루프가 발생하지 않도록 합니다.

적절한 횟수로 재시도 횟수를 제한하고 명확한 피드백을 사용자에게 표시하여 부인들을 우아하게 처리하는 것이 좋습니다.

#### &#x200B;6. 사전 인증 결정에 미디어 토큰이 누락된 이유는 무엇입니까? {#preauthorization-phase-faq6}

권한 부여 단계의 목적이므로 리소스 재생에 사전 권한 부여 단계를 사용할 수 없기 때문에 사전 권한 부여 결정에 미디어 토큰이 없습니다.

#### &#x200B;7. 사전 승인 결정이 이미 있는 경우 승인 단계를 건너뛸 수 있습니까? {#preauthorization-phase-faq7}

아니.

사전 권한 부여 결정을 사용할 수 있는 경우에도 권한 부여 단계를 건너뛸 수 없습니다. 사전 권한 부여 결정은 정보 제공용으로만 사용되며 실제 재생 권한을 부여하지 않습니다. 사전 인증 단계는 초기 지침을 제공하기 위한 것이지만, 콘텐츠를 재생하기 전에 인증 단계가 계속 필요합니다.

#### &#x200B;8. 리소스란 무엇이며, 지원되는 형식은 무엇입니까? {#preauthorization-phase-faq8}

리소스는 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) 설명서에 정의된 용어입니다.

리소스는 MVPD와 합의되고 클라이언트 애플리케이션이 스트리밍할 수 있는 콘텐츠와 연결된 고유 식별자입니다.

리소스 고유 식별자는 두 가지 형식을 가질 수 있습니다.

* 채널(브랜드)의 고유 식별자와 같은 간단한 문자열 형식입니다.
* 제목, 등급 및 자녀 보호 메타데이터와 같은 추가 정보가 포함된 미디어 RSS(MRSS) 형식입니다.

자세한 내용은 [보호된 리소스](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources) 설명서를 참조하세요.

#### &#x200B;9. 클라이언트 애플리케이션이 한 번에 사전 권한 부여 결정을 받을 수 있는 리소스가 몇 개입니까? {#preauthorization-phase-faq9}

클라이언트 애플리케이션은 MVPD에 의해 부과된 조건으로 인해 단일 API 요청에서 제한된 수의 리소스(일반적으로 최대 5개)에 대한 사전 인가 결정을 얻을 수 있다.

이 최대 리소스 수는 조직 관리자 중 한 사람이나 사용자를 대신하여 활동하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)를 통해 MVPD와 동의한 후 보고 변경할 수 있습니다.

자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) 설명서를 참조하십시오.

+++

### 인증 단계 FAQ {#authorization-phase-faqs-general}

+++인증 단계 FAQ

#### &#x200B;1. 승인 단계의 목적은 무엇입니까? {#authorization-phase-faq1}

인증 단계의 목적은 MVPD을 사용하여 권한을 확인한 후 사용자가 요청하는 리소스를 재생하는 기능을 클라이언트 애플리케이션에 제공하는 것입니다.

#### &#x200B;2. 인증 단계가 필수입니까? {#authorization-phase-faq2}

인증 단계는 필수입니다. 스트림을 릴리스하기 전에 사용자에게 권한이 있는지 MVPD을 통해 확인해야 하므로 클라이언트 애플리케이션은 사용자가 요청하는 리소스를 재생하려는 경우 이 단계를 건너뛸 수 없습니다.

#### &#x200B;3. 승인 결정은 무엇이며 얼마나 오래 유효합니까? {#authorization-phase-faq3}

인증은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization) 설명서에 정의된 용어이며, 결정 용어는 [용어집](rest-api-v2-glossary.md#decision)에서도 찾을 수 있습니다.

승인 결정은 승인 승인 결정 엔드포인트에서 검색할 수 있는 MVPD 승인 프로세스 조회 결과에 대한 정보를 저장합니다.

클라이언트 애플리케이션은 TV 공급자(신뢰할 수 있는) 결정이 사용자가 리소스 스트림에 액세스하도록 허용하는 경우 미디어 토큰을 포함하는 인증 결정을 사용하여 리소스 스트림을 재생할 수 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [인증 결정 API 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

권한 부여 결정은 문제 발생 시 지정된 제한되고 짧은 기간에 유효하며, 이는 MVPD을 다시 쿼리하기 전에 Adobe Pass 인증에 의해 캐시되는 시간을 나타냅니다.

인증(authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl)이라고 하는 이 제한된 기간은 조직 관리자 중 한 사람이나 사용자를 대신하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](rest-api-v2-glossary.md#tve-dashboard)를 통해 보고 변경할 수 있습니다.

자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) 설명서를 참조하십시오.

#### &#x200B;4. 클라이언트 애플리케이션이 영구 저장소에서 승인 결정을 캐시해야 합니까? {#authorization-phase-faq4}

클라이언트 애플리케이션은 영구 스토리지에 권한 부여 결정을 저장할 필요가 없습니다.

#### &#x200B;5. 클라이언트 애플리케이션은 승인 결정이 거부된 이유를 어떻게 확인할 수 있습니까? {#authorization-phase-faq5}

클라이언트 응용 프로그램은 권한 부여 결정 끝점의 응답에 포함된 [오류 코드 및 메시지](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)를 검사하여 거부된 권한 부여 결정의 이유를 확인할 수 있습니다. 이러한 세부 사항은 insight에 인증 요청이 거부된 구체적인 이유를 제공하여 사용자 경험을 알리거나 애플리케이션에서 필요한 처리를 트리거하는 데 도움이 됩니다.

인증 결정이 거부된 경우 인증 결정을 검색하기 위해 구현된 재시도 메커니즘이 무한 루프가 되지 않도록 하십시오.

적절한 횟수로 재시도 횟수를 제한하고 명확한 피드백을 사용자에게 표시하여 부인들을 우아하게 처리하는 것이 좋습니다.

#### &#x200B;6. 미디어 토큰은 무엇이며 유효기간은 얼마입니까? {#authorization-phase-faq6}

미디어 토큰은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token) 설명서에 정의된 용어입니다.

미디어 토큰은 의사 결정 권한 부여 끝점에서 검색할 수 있는 일반 텍스트로 전송된 서명된 문자열로 구성됩니다.

자세한 내용은 [미디어 토큰 검증기](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) 설명서를 참조하십시오.

미디어 토큰은 문제가 발생한 시점에 지정된 제한되고 짧은 기간에 유효하며, 이는 클라이언트 애플리케이션에서 확인하고 사용해야 하기 전에 시간 제한을 나타냅니다.

TV 공급자(신뢰할 수 있는) 결정에서 사용자가 액세스할 수 있는 경우 클라이언트 애플리케이션은 미디어 토큰을 사용하여 리소스 스트림을 재생할 수 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [인증 결정 API 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### &#x200B;7. 클라이언트 애플리케이션이 리소스 스트림을 재생하기 전에 미디어 토큰의 유효성을 검사해야 합니까? {#authorization-phase-faq7}

예.

리소스 스트림의 재생을 시작하기 전에 클라이언트 애플리케이션이 미디어 토큰의 유효성을 검사해야 합니다. 이 유효성 검사는 [미디어 토큰 검증기](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)를 사용하여 수행해야 합니다. 클라이언트 응용 프로그램은 반환된 `serializedToken`에서 `token`을(를) 확인함으로써 스트림 리핑과 같은 권한 없는 액세스를 방지하고 적절하게 권한이 부여된 사용자만 콘텐츠를 재생할 수 있도록 합니다.

#### &#x200B;8. 재생 중에 클라이언트 애플리케이션이 만료된 미디어 토큰을 새로 고쳐야 합니까? {#authorization-phase-faq8}

아니.

스트림이 재생되는 동안 클라이언트 응용 프로그램에서 만료된 미디어 토큰을 새로 고칠 필요가 없습니다. 재생 중에 미디어 토큰이 만료되면 스트림은 중단 없이 계속 진행할 수 있도록 허용해야 합니다. 그러나 클라이언트는 다음에 사용자가 리소스를 재생하려고 할 때 새로운 인증 결정을 요청하고 새 미디어 토큰을 받아야 합니다.

#### &#x200B;9. 권한 부여 결정에 있는 각 타임스탬프 속성의 목적은 무엇입니까? {#authorization-phase-faq9}

승인 결정은 승인 자체의 유효 기간 및 연관된 미디어 토큰에 대한 필수 컨텍스트를 제공하는 여러 타임스탬프 속성을 포함한다. 이러한 타임스탬프는 권한 부여 결정과 관련되는지 또는 미디어 토큰과 관련되는지에 따라 다른 용도로 사용됩니다.

**의사 결정 수준 타임스탬프**

다음 타임스탬프는 전체 권한 부여 결정의 유효 기간을 설명합니다.

| 속성 | 설명 | 메모 |
|-------------|----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | 인증 결정이 내려진 시간(밀리초)입니다. | 인증 유효성 창의 시작을 표시합니다. |
| `notAfter` | 인증 결정이 만료되는 시간(밀리초)입니다. | [인증 TTL(Time-to-Live)](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#authorization-ttl-management)은 다시 인증을 요구하기 전까지 인증이 유효한 기간을 결정합니다. 이 TTL은 MVPD 담당자와 협상합니다. |

**토큰 수준 타임스탬프**

다음 타임스탬프는 권한 부여 결정에 연결된 미디어 토큰의 유효 기간을 설명합니다.

| 속성 | 설명 | 메모 |
|-------------|-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | 미디어 토큰을 발급한 시간(밀리초). | 토큰이 재생에 유효하게 되면 표시됩니다. |
| `notAfter` | 미디어 토큰이 만료되는 시간(밀리초)입니다. | 미디어 토큰은 오용 위험을 최소화하고 토큰 생성 서버와 토큰 검증 서버 간의 잠재적인 클럭 차이를 설명하기 위해 고의적으로 짧은 수명(일반적으로 7분)을 갖습니다. |

#### &#x200B;10. 리소스란 무엇이며, 지원되는 형식은 무엇입니까? {#authorization-phase-faq10}

리소스는 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) 설명서에 정의된 용어입니다.

리소스는 MVPD와 합의되고 클라이언트 애플리케이션이 스트리밍할 수 있는 콘텐츠와 연결된 고유 식별자입니다.

리소스 고유 식별자는 두 가지 형식을 가질 수 있습니다.

* 채널(브랜드)의 고유 식별자와 같은 간단한 문자열 형식입니다.
* 제목, 등급 및 자녀 보호 메타데이터와 같은 추가 정보가 포함된 미디어 RSS(MRSS) 형식입니다.

자세한 내용은 [보호된 리소스](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources) 설명서를 참조하세요.

#### &#x200B;11. 클라이언트 애플리케이션이 한 번에 몇 개의 리소스를 승인할 수 있습니까? {#authorization-phase-faq11}

클라이언트 애플리케이션은 MVPD에 의해 부과된 조건으로 인해 단일 API 요청에서 제한된 수의 리소스(일반적으로 최대 1)에 대한 인증 결정을 얻을 수 있습니다.

+++

### 로그아웃 단계 FAQ {#logout-phase-faqs-general}

+++로그아웃 단계 FAQ

#### &#x200B;1. 로그아웃 단계의 목적은 무엇입니까? {#logout-phase-faq1}

로그아웃 단계의 목적은 사용자 요청 시 Adobe Pass 인증 내에서 사용자의 인증된 프로필을 종료할 수 있는 기능을 클라이언트 애플리케이션에 제공하는 것입니다.

#### &#x200B;2. 로그아웃 단계가 필수입니까? {#logout-phase-faq2}

로그아웃 단계는 필수입니다. 클라이언트 애플리케이션은 사용자에게 로그아웃 기능을 제공해야 합니다.

+++

### 헤더 FAQ {#headers-faqs-general}

+++헤더 FAQ

#### &#x200B;1. 인증 헤더의 값을 계산하는 방법 {#headers-faq1}

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

#### &#x200B;2. AP-Device-Identifier 헤더의 값을 계산하는 방법 {#headers-faq2}

>[!IMPORTANT]
>
> 클라이언트 애플리케이션이 REST API V1에서 REST API V2로 마이그레이션하는 경우 클라이언트 애플리케이션은 이전과 동일한 방법을 사용하여 디바이스 식별자 값을 계속 계산할 수 있습니다.

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) 요청 헤더에 클라이언트 응용 프로그램에서 만든 스트리밍 장치 식별자가 포함되어 있습니다.

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) 헤더 설명서는 값을 계산하는 방법에 대한 주요 플랫폼의 예를 제공하지만, 클라이언트 애플리케이션은 자체 비즈니스 논리 및 요구 사항에 따라 다른 메서드를 사용하도록 선택할 수 있습니다.

#### &#x200B;3. X-Device-Info 헤더에 대한 값을 계산하는 방법 {#headers-faq3}

>[!IMPORTANT]
>
> 클라이언트 애플리케이션이 REST API V1에서 REST API V2로 마이그레이션하는 경우 클라이언트 애플리케이션은 이전과 동일한 방법을 사용하여 디바이스 정보 값을 계속 계산할 수 있습니다.

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) 요청 헤더에는 실제 스트리밍 장치와 관련된 클라이언트 정보(장치, 연결 및 응용 프로그램)가 포함되어 있으며 MVPD가 적용할 수 있는 플랫폼별 규칙을 결정하는 데 사용됩니다.

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) 헤더 설명서는 값을 계산하는 방법에 대한 주요 플랫폼에 대한 예를 제공하지만, 클라이언트 애플리케이션은 자체 비즈니스 논리 및 요구 사항에 따라 다른 메서드를 사용하도록 선택할 수 있습니다.

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) 헤더가 없거나 잘못된 값을 포함하는 경우 요청이 `unknown` 플랫폼에서 시작된 것으로 분류될 수 있습니다.

이로 인해 요청이 안전하지 않은 것으로 처리되고 인증 TTL의 단축과 같은 보다 제한적인 규칙이 적용될 수 있습니다. 또한 스트리밍 장치 `connectionIp` 및 `connectionPort`과(와) 같은 일부 필드는 Spectrum의 [홈 기본 인증](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md)과(와) 같은 기능에 필수입니다.

요청을 장치 대신 서버에서 보내는 경우에도 [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) 헤더 값은 실제 스트리밍 장치 정보를 반영해야 합니다.

+++

### 기타 FAQ {#misc-faqs-general}

+++기타 FAQ

#### &#x200B;1. REST API V2 요청 및 응답을 탐색하고 API를 테스트할 수 있습니까? {#misc-faq1}

예.

전용 [Adobe Developer](https://developer.adobe.com/adobe-pass/) 웹 사이트를 통해 REST API V2를 살펴볼 수 있습니다. Adobe Developer 웹 사이트에서는 다음에 대한 무제한 액세스를 제공합니다.

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)과(와) 상호 작용하려면 [DCR API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)을(를) 통해 얻은 `Bearer` 액세스 토큰과 함께 [Authorization](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) 헤더를 포함해야 합니다.

[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)을(를) 사용하려면 REST API V2 범위가 포함된 소프트웨어 문이 필요합니다. 자세한 내용은 [DCR(동적 클라이언트 등록) FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md) 문서를 참조하십시오.

#### &#x200B;2. OpenAPI 지원을 통한 API 개발 도구를 사용하여 REST API V2 요청 및 응답을 탐색할 수 있습니까? {#misc-faq2}

예.

[Adobe Developer](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) 웹 사이트에서 [DCR API](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) 및 [REST API V2](https://developer.adobe.com/adobe-pass/)에 대한 OpenAPI 사양 파일을 다운로드할 수 있습니다.

OpenAPI 사양 파일을 다운로드하려면 다운로드 버튼을 클릭하여 다음 파일을 로컬 시스템에 저장합니다.

* [DCR API JSON](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [REST API V2 JSON](https://developer.adobe.com/adobe-pass/restApiV2.json)

그런 다음 이러한 파일을 기본 API 개발 도구로 가져와서 REST API V2 요청 및 응답을 탐색하고 API를 테스트할 수 있습니다.

#### &#x200B;3. https://sp.auth-staging.adobe.com/apitest/api.html에 호스팅된 기존 API 테스트 도구를 계속 사용할 수 있습니까? {#misc-faq3}

아니.

REST API V2로 마이그레이션하는 클라이언트 애플리케이션은 https://developer.adobe.com/adobe-pass/에 호스팅된 새 테스트 도구를 사용해야 합니다. Adobe Developer 웹 사이트에서는 다음에 대한 무제한 액세스를 제공합니다.

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)과(와) 상호 작용하려면 [DCR API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)을(를) 통해 얻은 `Bearer` 액세스 토큰과 함께 [Authorization](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) 헤더를 포함해야 합니다.

[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)을(를) 사용하려면 REST API V2 범위가 포함된 소프트웨어 문이 필요합니다. 자세한 내용은 [DCR(동적 클라이언트 등록) FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md) 문서를 참조하십시오.

+++

## 마이그레이션 FAQ {#migration-faqs}

기존 애플리케이션을 REST API V2로 마이그레이션해야 하는 애플리케이션에서 작업하는 경우 이 섹션을 계속합니다.

>[!MORELIKETHIS]
>
> * [DCR(Dynamic Client Registration) FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### 일반 마이그레이션 FAQ {#general-migration-faqs}

+++일반 마이그레이션 FAQ

#### &#x200B;1. REST API V2로 마이그레이션된 새 클라이언트 애플리케이션을 모든 사용자에게 한 번에 롤아웃해야 합니까? {#migration-faq1}

아니.

클라이언트 애플리케이션은 REST API V2를 통합하는 새 버전을 모든 사용자에게 동시에 롤아웃할 필요가 없습니다.

Adobe Pass 인증은 2025년 말까지 REST API V1 또는 SDK을 통합하는 이전 클라이언트 애플리케이션 버전을 계속 지원할 예정입니다.

#### &#x200B;2. 모든 API 및 플로우에서 REST API V2로 마이그레이션된 새 클라이언트 애플리케이션을 한 번에 롤아웃해야 합니까? {#migration-faq2}

예.

클라이언트 애플리케이션은 모든 Adobe Pass 인증 API 및 플로우에 걸쳐 REST API V2를 통합하는 새 버전을 동시에 롤아웃해야 합니다.

&#39;두 번째 화면 인증&#39; 흐름의 경우 클라이언트 응용 프로그램은 [기본](rest-api-v2-glossary.md#primary-application) 및 [보조](rest-api-v2-glossary.md#secondary-application) 응용 프로그램 모두에 대해 REST API V2를 통합하는 새 버전을 동시에 롤아웃해야 합니다.

Adobe Pass 인증은 API와 흐름 간에 REST API V2 및 REST API V1/SDK을 모두 통합하는 &#39;하이브리드&#39; 구현을 지원하지 않습니다.

#### &#x200B;3. REST API V2로 마이그레이션된 새 클라이언트 애플리케이션으로 업데이트할 때 사용자 인증이 유지됩니까? {#migration-faq3}

아니.

REST API V1 또는 SDK을 통합하는 이전 클라이언트 애플리케이션 버전 내에서 얻은 사용자 인증은 유지되지 않습니다.

따라서 REST API V2로 마이그레이션된 새 클라이언트 애플리케이션 내에서 다시 인증해야 합니다.

#### &#x200B;4. 향상된 오류 코드가 REST API V2에서 기본적으로 활성화되어 있습니까? {#migration-faq4}

예.

REST API V2로 마이그레이션하는 클라이언트 애플리케이션은 기본적으로 이 기능을 통해 보다 상세하고 정확한 오류 정보를 제공합니다.

자세한 내용은 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) 설명서를 참조하세요.

#### &#x200B;5. REST API V2로 마이그레이션할 때 기존 통합에서 구성을 변경해야 합니까? {#migration-faq5}

아니.

REST API V2로 마이그레이션하는 클라이언트 애플리케이션은 기존 MVPD 통합에 대한 구성 변경이 필요하지 않습니다. 또한 기존 [서비스 공급자](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider) 및 [MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd)에 대해 동일한 식별자를 계속 사용합니다.

또한 Adobe Pass 인증에서 MVPD 종단점과 통신하는 데 사용되는 프로토콜은 변경되지 않습니다.

+++

### REST API V1에서 REST API V2로 마이그레이션 {#migration-rest-api-v1-to-rest-api-v2-faqs}

REST API V1에서 REST API V2로 마이그레이션해야 하는 응용 프로그램에서 작업하는 경우 이 하위 섹션을 계속합니다.

#### 등록 단계 FAQ {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++등록 단계 FAQ

##### &#x200B;1. 등록 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#registration-phase-v1-to-v2-faq1}

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

##### &#x200B;1. 구성 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#configuration-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 활성 통합이 있는 MVPD 목록 검색 | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 인증 단계 FAQ {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++인증 단계 FAQ

##### &#x200B;1. 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#authentication-phase-v1-to-v2-faq1}

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

##### &#x200B;1. 사전 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#preauthorization-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사전 승인된 리소스 검색(사전 인증 결정) | [GET <br/> /api/v1/사전 승인(첫 번째 화면)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/사전 승인(두 번째 화면)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 인증 단계 FAQ {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++인증 단계 FAQ

##### &#x200B;1. 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#authorization-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD) 인증 시작 | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 인증 토큰 검색(인증 결정) | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 짧은 인증 토큰 검색(미디어 토큰) | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### 로그아웃 단계 FAQ {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++로그아웃 단계 FAQ

##### &#x200B;1. 로그아웃 단계에서 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#logout-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 로그아웃 시작 | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### SDK에서 REST API V2로 마이그레이션 {#migration-sdk-to-rest-api-v2}

SDK에서 REST API V2로 마이그레이션해야 하는 애플리케이션에서 작업하는 경우 이 하위 섹션을 계속합니다.

#### 등록 단계 FAQ {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++등록 단계 FAQ

##### &#x200B;1. 등록 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#registration-phase-sdk-to-v2-faq1}

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

##### &#x200B;1. 구성 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#configuration-phase-sdk-to-v2-faq1}

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

##### &#x200B;1. 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#authentication-phase-sdk-to-v2-faq1}

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

##### &#x200B;1. 사전 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#preauthorization-phase-sdk-to-v2-faq1}

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

##### &#x200B;1. 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#authorization-phase-sdk-to-v2-faq1}

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

+++로그아웃 단계 FAQ

##### &#x200B;1. 로그아웃 단계에서 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#logout-phase-sdk-to-v2-faq1}

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
