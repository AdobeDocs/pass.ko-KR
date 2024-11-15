---
title: REST API V2 FAQ
description: REST API V2 FAQ
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '7304'
ht-degree: 0%

---

# REST API V2 FAQ {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 문서에서는 Adobe Pass 인증 REST API V2 채택에 대한 FAQ에 대한 개요 답변을 제공합니다.

REST API V2에 대한 자세한 내용은 [REST API V2 개요](./rest-api-v2-overview.md) 설명서를 참조하십시오.

## 일반 FAQ {#general-faqs}

[REST API V1](#migrate-rest-api-v1-to-rest-api-v2) 또는 [SDK](#migrate-sdk-to-rest-api-v2)에서 마이그레이션하는 기존 응용 프로그램이든 새 응용 프로그램이든 REST API V2를 통합해야 하는 응용 프로그램에서 작업하는 경우 이 섹션으로 시작하십시오.

마이그레이션 세부 정보 및 단계에 대한 자세한 내용은 다음 섹션도 참조하십시오.

+++등록 단계 FAQ

### 1. 등록 단계의 목적은 무엇입니까? {#registration-phase-faq1}

등록 단계의 목적은 [DCR(Dynamic Client Registration)](./rest-api-v2-glossary.md#dcr) 프로세스를 통해 Adobe Pass 인증에 대해 클라이언트 응용 프로그램을 등록하는 것입니다.

DCR(Dynamic Client Registration) 프로세스를 진행하려면 클라이언트 애플리케이션이 클라이언트 자격 증명 쌍을 가져오고 등록 단계의 최종 목표로 액세스 토큰을 검색해야 합니다.

자세한 내용은 [동적 클라이언트 등록 개요](/help/authentication/dcr-api/dynamic-client-registration-overview.md) 설명서를 참조하십시오.

### 2. 등록 단계가 필수입니까? {#registration-phase-faq2}

등록 단계는 필수이지만, 클라이언트 응용 프로그램은 여전히 유효한 클라이언트 자격 증명 및 액세스 토큰의 캐시된 쌍이 있는 경우 이 단계를 건너뛸 수 있습니다.

### 3. 소프트웨어 설명서는 무엇이며 유효기간은 얼마나 됩니까? {#registration-phase-faq3}

소프트웨어 문은 [용어집](./rest-api-v2-glossary.md#software-statement) 설명서에 정의된 용어입니다.

소프트웨어 문은 조직 관리자 중 한 사람이나 사용자를 대신하여 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](./rest-api-v2-glossary.md#tve-dashboard)에서 생성 및 다운로드할 수 있는 JSON 웹 토큰(JWT)으로 구성됩니다.

소프트웨어 문은 무제한 유효하지만 언제든지 Adobe Pass 인증 담당자에게 취소를 요청하도록 선택할 수 있습니다.

클라이언트 응용 프로그램은 클라이언트 자격 증명을 검색해야 할 때 소프트웨어 문을 저장하고 사용해야 합니다.

자세한 내용은 [동적 클라이언트 등록 개요](/help/authentication/dcr-api/dynamic-client-registration-overview.md) 설명서를 참조하십시오.

### 4. 소프트웨어 명세서를 생성하고 다운로드하는 방법 {#registration-phase-faq4}

이 작업은 조직 관리자 중 한 사람이나 사용자를 대신하여 활동하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](./rest-api-v2-glossary.md#tve-dashboard)를 통해 완료할 수 있습니다.

자세한 내용은 [TVE 대시보드 채널 사용 안내서](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) 또는 [TVE 대시보드 프로그래머 사용 안내서](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications) 설명서를 참조하십시오.

### 5. 소프트웨어 문이 취소되면 어떻게 됩니까? {#registration-phase-faq5}

소프트웨어 명령문이 취소되는 경우 고려해야 할 중요한 결과 중 하나가 있습니다.

* 해지된 소프트웨어 문을 사용하는 클라이언트 응용 프로그램에서는 더 이상 [권한](./rest-api-v2-glossary.md#entitlement) 흐름을 진행할 수 없습니다. 즉, 사용자가 콘텐츠를 재생할 수 없게 됩니다.

### 6. 클라이언트 자격 증명은 무엇이며 얼마나 오래 유효합니까? {#registration-phase-faq6}

클라이언트 자격 증명은 [용어집](./rest-api-v2-glossary.md#client-credentials) 설명서에 정의된 용어입니다.

클라이언트 자격 증명은 클라이언트 등록 끝점에서 검색할 수 있는 클라이언트 식별자 및 클라이언트 암호 쌍으로 구성됩니다.

클라이언트 자격 증명은 제한 없이 유효합니다.

클라이언트 애플리케이션은 클라이언트 자격 증명을 무기한 저장하고 액세스 토큰을 검색해야 할 때 사용해야 합니다.

자세한 내용은 [클라이언트 자격 증명 검색](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) 설명서를 참조하십시오.

### 7. 클라이언트 자격 증명을 관리하는 방법 {#registration-phase-faq7}

Adobe Pass 인증과 클라이언트-서버 및 서버-서버 통합 모두를 수행하는 경우 각 사용자 애플리케이션 인스턴스에 대해 고유한 클라이언트 자격 증명 쌍을 관리하는 것이 좋습니다.

클라이언트 애플리케이션은 클라이언트 자격 증명을 무기한 저장하고 액세스 토큰을 검색해야 할 때 사용해야 합니다.

### 8. 캐시된 클라이언트 자격 증명이 손실되면 어떻게 됩니까? {#registration-phase-faq8}

캐시된 클라이언트 자격 증명이 손실될 때 고려해야 할 세 가지 중요한 결과가 있습니다.

* 클라이언트 애플리케이션은 새로운 클라이언트 자격 증명 쌍을 획득해야 합니다.
* 클라이언트 애플리케이션은 새로운 클라이언트 자격 증명 쌍을 사용하여 새로운 액세스 토큰을 얻어야 합니다.
* 클라이언트 애플리케이션은 이전에 획득한 인증된 프로필에 대한 액세스 권한을 잃게 되므로 사용자에게 재인증을 요청해야 합니다.

### 9. 액세스 토큰은 무엇이며 유효기간은 얼마입니까? {#registration-phase-faq9}

액세스 토큰은 [용어집](./rest-api-v2-glossary.md#access-token) 설명서에 정의된 용어입니다.

액세스 토큰은 클라이언트 토큰 끝점에서 검색할 수 있는 [전달자 토큰](./appendix/headers/rest-api-v2-appendix-headers-authorization.md)(으)로 구성됩니다.

액세스 토큰은 문제 발생 시 지정된 제한되고 짧은 기간 동안 유효합니다.

클라이언트 애플리케이션은 액세스 토큰을 저장하고 REST API V2를 타깃팅할 때 만료될 때까지 사용해야 합니다.

클라이언트 애플리케이션은 현재 액세스 토큰이 만료되기 전에 새 액세스 토큰을 받아야 승인되지 않은 요청을 방지할 수 있습니다.

자세한 내용은 [액세스 토큰 검색](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) 설명서를 참조하십시오.

### 10. 클라이언트 응용 프로그램은 액세스 토큰을 새로 고침하려면 어떻게 합니까? {#registration-phase-faq10}

클라이언트 애플리케이션은 새 액세스 토큰을 검색하는 것과 동일한 방식으로 액세스 토큰을 새로 고쳐야 하지만 캐시된 클라이언트 자격 증명을 사용해야 합니다.

클라이언트 응용 프로그램은 액세스 토큰을 새로 고치려면 재등록해서는 안 됩니다. 대신 저장된 클라이언트 자격 증명을 사용해야 합니다. 그렇지 않으면 사용자가 재인증해야 합니다.

자세한 내용은 [액세스 토큰 검색](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) 설명서를 참조하십시오.

+++

+++구성 단계 FAQ

### 1. 구성 단계의 목적은 무엇입니까? {#configuration-phase-faq1}

구성 단계의 목적은 각 MVPD에 대해 Adobe Pass 인증이 저장한 구성 세부 정보와 함께 현재 통합되고 있는 MVPD 목록을 클라이언트 응용 프로그램에 제공하는 것입니다.

구성 단계는 클라이언트 애플리케이션이 사용자에게 TV 공급자를 선택하라고 요청해야 할 때 인증 단계에 대한 필수 단계 역할을 합니다.

### 2. 구성 단계가 필수입니까? {#configuration-phase-faq2}

구성 단계는 필수가 아닙니다. 클라이언트 애플리케이션은 다음 시나리오에서 이 단계를 건너뛸 수 있습니다.

* 사용자가 이미 인증되었습니다.
* 사용자는 기본 또는 프로모션 TempPass 기능을 통해 임시 액세스를 제공합니다.
* 사용자 인증이 만료되었지만 클라이언트 애플리케이션은 이전에 선택한 MVPD를 사용자 경험에 대한 동기 선택으로 캐시하고 사용자에게 해당 MVPD의 구독자임을 확인하라는 메시지를 표시합니다.

### 3. 구성은 무엇이며 얼마나 오래 유효합니까? {#configuration-phase-faq3}

구성은 [용어집](./rest-api-v2-glossary.md#configuration) 설명서에 정의된 용어입니다.

구성은 구성 끝점에서 검색할 수 있는 MVPD 목록으로 구성됩니다.

클라이언트 애플리케이션은 사용자가 MVPD를 선택해야 할 때 이 구성을 사용하여 &quot;선택기&quot;라는 UI 구성 요소를 표시할 수 있습니다.

사용자가 인증 단계를 거치기 전에 클라이언트 응용 프로그램에서 구성을 새로 고쳐야 합니다.

자세한 내용은 [구성 검색](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) 설명서를 참조하세요.

### 4. 클라이언트 애플리케이션이 자체 MVPD 목록을 관리할 수 있습니까? {#configuration-phase-faq4}

클라이언트 애플리케이션은 자체 MVPD 목록을 관리할 수 있지만, 목록이 최신 상태이고 정확한지 확인하려면 Adobe Pass 인증에서 제공하는 구성을 사용하는 것이 좋습니다.

선택한 MVPD에 지정된 [서비스 공급자](./rest-api-v2-glossary.md#service-provider)와의 활성 통합이 없는 경우 클라이언트 응용 프로그램은 Adobe Pass 인증 REST API V2에서 [오류](/help/authentication/enhanced-error-codes.md)을(를) 받습니다.

### 5. 클라이언트 애플리케이션이 MVPD 목록을 필터링할 수 있습니까? {#configuration-phase-faq5}

클라이언트 애플리케이션은 자신의 비즈니스 로직 및 이전 선택의 사용자 위치 또는 사용자 내역과 같은 요구 사항을 기반으로 사용자 지정 메커니즘을 구현함으로써 구성 응답에 제공된 MVPD의 목록을 필터링할 수 있다.

### 6. MVPD와의 통합이 비활성화되고 비활성 상태로 표시되면 어떻게 됩니까? {#configuration-phase-faq6}

MVPD와의 통합이 비활성화되고 비활성 상태로 표시되면, MVPD는 추가 구성 응답에서 제공되는 MVPD 목록에서 제거되며, 고려해야 할 두 가지 중요한 결과가 있습니다.

* 해당 MVPD의 인증되지 않은 사용자는 더 이상 해당 MVPD를 사용하여 인증 단계를 완료할 수 없습니다.
* 해당 MVPD의 인증된 사용자는 더 이상 해당 MVPD를 사용하여 사전 인증, 권한 부여 또는 로그아웃 단계를 완료할 수 없습니다.

선택한 MVPD에 지정된 [서비스 공급자](./rest-api-v2-glossary.md#service-provider)와의 활성 통합이 더 이상 없는 경우 클라이언트 응용 프로그램은 Adobe Pass 인증 REST API V2에서 [오류](/help/authentication/enhanced-error-codes.md)을(를) 받습니다.

### 7. MVPD와의 통합이 다시 활성화되어 활성으로 표시되면 어떻게 됩니까? {#configuration-phase-faq7}

MVPD와의 통합이 다시 활성화되어 활성으로 표시되면, MVPD는 추가 구성 응답에 제공된 MVPD 목록에 다시 포함되고 고려해야 할 두 가지 중요한 결과가 있습니다.

* 해당 MVPD의 인증되지 않은 사용자는 해당 MVPD를 사용하여 인증 단계를 다시 완료할 수 있습니다.
* 해당 MVPD의 인증된 사용자는 해당 MVPD를 사용하여 사전 권한 부여, 권한 부여 또는 로그아웃 단계를 다시 완료할 수 있습니다.

### 8. MVPD와의 통합을 활성화하거나 비활성화하는 방법 {#configuration-phase-faq8}

이 작업은 조직 관리자 중 한 사람이나 사용자를 대신하여 활동하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](./rest-api-v2-glossary.md#tve-dashboard)를 통해 완료할 수 있습니다.

자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#disable-integration) 설명서를 참조하십시오.

+++

+++인증 단계 FAQ

### 1. 인증 단계의 목적은 무엇입니까? {#authentication-phase-faq1}

인증 단계의 목적은 클라이언트 응용 프로그램에 사용자의 ID를 확인하고 사용자 메타데이터 정보를 얻을 수 있는 기능을 제공하는 것입니다.

인증 단계는 클라이언트 응용 프로그램에서 컨텐츠를 재생해야 할 때 사전 인증 단계 또는 인증 단계에 대한 필수 단계 역할을 합니다.

### 2. 사용자가 이미 인증되었는지 클라이언트 애플리케이션이 어떻게 알 수 있습니까? {#authentication-phase-faq2}

클라이언트 애플리케이션은 사용자가 이미 인증되었는지 확인할 수 있는 다음 끝점 중 하나를 쿼리하고 프로필 정보를 반환할 수 있습니다.

* [프로필 엔드포인트 API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 MVPD API에 대한 프로필 엔드포인트](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정(인증) 코드 API의 프로필 끝점](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

자세한 내용은 다음 문서를 참조하십시오.

* [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 3. 클라이언트 애플리케이션은 어떻게 사용자의 메타데이터 정보를 얻을 수 있습니까? {#authentication-phase-faq3}

클라이언트 응용 프로그램은 [사용자 메타데이터](/help/authentication/user-metadata-feature.md) 정보를 프로필 정보의 일부로 반환할 수 있는 다음 끝점 중 하나를 쿼리할 수 있습니다.

* [프로필 엔드포인트 API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 MVPD API에 대한 프로필 엔드포인트](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정(인증) 코드 API의 프로필 끝점](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

자세한 내용은 다음 문서를 참조하십시오.

* [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 4. 인증 세션은 무엇이며 얼마나 오래 유효합니까? {#authentication-phase-faq4}

인증 세션은 [용어집](./rest-api-v2-glossary.md#session) 설명서에 정의된 용어입니다.

인증 세션은 세션 끝점에서 검색할 수 있는 시작된 인증 프로세스에 대한 정보를 저장합니다.

인증 세션은 문제 발생 시점에 지정된 제한적이고 짧은 기간 동안 유효하며, 이는 사용자가 흐름을 다시 시작해야 하기 전에 인증 프로세스를 완료해야 하는 시간을 나타냅니다.

클라이언트 애플리케이션은 인증 프로세스를 진행하는 방법을 알기 위해 인증 세션 응답을 사용할 수 있다. 임시 액세스, 액세스 성능 저하 또는 사용자가 이미 인증된 경우와 같이 사용자를 인증할 필요가 없는 경우가 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [인증 세션 API 만들기](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [인증 세션 API 다시 시작](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 5. 인증 코드는 무엇이며 얼마나 오래 유효합니까? {#authentication-phase-faq5}

인증 코드는 [용어집](./rest-api-v2-glossary.md#code) 설명서에 정의된 용어입니다.

인증 코드는 사용자가 인증 프로세스를 시작할 때 생성되는 고유한 값을 저장하며, 프로세스가 완료되거나 인증 세션이 만료될 때까지 사용자의 인증 세션을 고유하게 식별합니다.

인증 코드는 인증 세션을 시작하는 시점에 지정된 제한되고 짧은 기간 동안 유효하며, 이는 흐름을 다시 시작해야 하기 전에 사용자가 인증 프로세스를 완료해야 하는 시간을 나타냅니다.

클라이언트 애플리케이션은 인증 세션이 만료되지 않은 것을 고려하여 사용자가 동일한 장치에서 또는 두 번째 장치에서 인증 프로세스를 완료하거나 다시 시작할 수 있도록 인증 코드를 사용할 수 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [인증 세션 API 만들기](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [인증 세션 API 다시 시작](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 6. 클라이언트 애플리케이션은 사용자가 유효한 인증 코드를 입력했는지, 인증 세션이 아직 만료되지 않았는지 어떻게 알 수 있습니까? {#authentication-phase-faq6}

클라이언트 애플리케이션은 인증 코드와 연관된 인증 세션 정보를 검색할 책임이 있는 세션 엔드포인트에 요청을 전송하여 보조(화면) 애플리케이션에서 사용자가 입력한 인증 코드의 유효성을 검사할 수 있습니다.

입력한 인증 코드가 잘못되었거나 인증 세션이 만료된 경우 클라이언트 응용 프로그램에서 [오류](/help/authentication/enhanced-error-codes.md)을(를) 받습니다.

자세한 내용은 [인증 세션 검색](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) 설명서를 참조하세요.

### 7. 프로필은 무엇이며 얼마나 오래 유효합니까? {#authentication-phase-faq7}

프로필은 [용어집](./rest-api-v2-glossary.md#profile) 설명서에 정의된 용어입니다.

프로필에는 사용자의 인증 유효성, 메타데이터 정보 및 프로필 끝점에서 검색할 수 있는 더 많은 정보가 저장됩니다.

클라이언트 애플리케이션은 프로파일을 사용하여 사용자의 인증 상태를 알거나, 사용자 메타데이터 정보에 액세스하거나, 인증에 사용되는 방법을 찾을 수 있다.

자세한 내용은 다음 문서를 참조하십시오.

* [프로필 엔드포인트 API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 MVPD API에 대한 프로필 엔드포인트](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정(인증) 코드 API의 프로필 끝점](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

프로필은 쿼리할 때 지정된 제한된 기간 동안 유효하며 사용자가 유효한 인증을 받은 후 인증 단계를 다시 수행해야 함을 나타냅니다.

인증(authN) [TTL](./rest-api-v2-glossary.md#ttl)이라고 하는 이 제한된 기간은 조직 관리자 중 한 사람이나 사용자를 대신하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](./rest-api-v2-glossary.md#tve-dashboard)를 통해 보고 변경할 수 있습니다.

자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) 설명서를 참조하십시오.

+++

+++사전 인증 단계 FAQ

### 1. 사전 승인 단계의 목적은 무엇입니까? {#preauthorization-phase-faq1}

사전 인증 단계의 목적은 사용자가 액세스할 수 있는 카탈로그의 리소스 하위 집합을 클라이언트 응용 프로그램에 제공하는 것입니다.

사전 인증 단계는 사용자가 클라이언트 애플리케이션을 처음 열거나 새 섹션으로 이동할 때 사용자 경험을 향상시킬 수 있습니다.

### 2. 사전 인증 단계가 필수입니까? {#preauthorization-phase-faq2}

사전 인증 단계는 필수가 아닙니다. 클라이언트 애플리케이션이 사용자의 권한에 따라 먼저 필터링하지 않고 리소스 카탈로그를 표시하려는 경우 이 단계를 건너뛸 수 있습니다.

### 3. 사전 승인 결정이 무엇입니까? {#preauthorization-phase-faq3}

사전 인증은 [용어집](./rest-api-v2-glossary.md#preauthorization) 설명서에 정의된 용어이며, 결정 용어는 [용어집](./rest-api-v2-glossary.md#decision)에서도 찾을 수 있습니다.

사전 권한 부여 결정은 사전 권한 부여 결정 끝점에서 검색할 수 있는 MVPD 사전 권한 부여 프로세스 조회 결과에 대한 정보를 저장합니다.

클라이언트 애플리케이션은 사전 인가 결정들을 사용하여 TV 공급자(정보) 결정들이 사용자가 액세스하도록 허용할 리소스들의 서브세트를 제시할 수 있다.

자세한 내용은 다음 문서를 참조하십시오.

* [사전 인증 결정 API 검색](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [기본 애플리케이션 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

### 4. 사전 승인 결정에 미디어 토큰이 누락된 이유는 무엇입니까? {#preauthorization-phase-faq4}

권한 부여 단계의 목적이므로 리소스 재생에 사전 권한 부여 단계를 사용할 수 없기 때문에 사전 권한 부여 결정에 미디어 토큰이 없습니다.

### 5. 리소스란 무엇이며, 지원되는 형식은 무엇입니까? {#preauthorization-phase-faq5}

리소스는 [용어집](./rest-api-v2-glossary.md#resource) 설명서에 정의된 용어입니다.

리소스는 MVPD와 합의되고 클라이언트 애플리케이션이 스트리밍할 수 있는 콘텐츠와 연결된 고유 식별자입니다.

리소스 고유 식별자는 두 가지 형식을 가질 수 있습니다.

* 채널(브랜드)의 고유 식별자와 같은 간단한 문자열 형식입니다.
* 제목, 등급 및 자녀 보호 메타데이터와 같은 추가 정보가 포함된 미디어 RSS(MRSS) 형식입니다.

자세한 내용은 [보호된 리소스 식별](/help/authentication/identify-protected-resources.md) 설명서를 참조하세요.

### 6. 클라이언트 애플리케이션이 한 번에 사전 권한 부여 결정을 받을 수 있는 리소스가 몇 개입니까? {#preauthorization-phase-faq6}

클라이언트 애플리케이션은 MVPD에 의해 부과된 조건으로 인해 단일 API 요청에서 제한된 수의 리소스(일반적으로 최대 5개)에 대한 사전 인가 결정을 얻을 수 있다.

이 최대 리소스 수는 조직 관리자 중 한 사람이나 사용자를 대신하여 활동하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](./rest-api-v2-glossary.md#tve-dashboard)를 통해 MVPD와 동의한 후 보고 변경할 수 있습니다.

자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) 설명서를 참조하십시오.

+++

+++인증 단계 FAQ

### 1. 승인 단계의 목적은 무엇입니까? {#authorization-phase-faq1}

인증 단계의 목적은 MVPD로 권한을 확인한 후 사용자가 요청하는 리소스를 재생할 수 있는 기능을 클라이언트 애플리케이션에 제공하는 것입니다.

### 2. 인증 단계가 필수입니까? {#authorization-phase-faq2}

인증 단계는 필수입니다. 스트림을 릴리스하기 전에 사용자에게 권한이 있는지 MVPD와 확인해야 하므로 클라이언트 응용 프로그램에서 사용자가 요청하는 리소스를 재생하려면 이 단계를 건너뛸 수 없습니다.

### 3. 승인 결정은 무엇이며 얼마나 오래 유효합니까? {#authorization-phase-faq3}

인증은 [용어집](./rest-api-v2-glossary.md#authorization) 설명서에 정의된 용어이며, 결정 용어는 [용어집](./rest-api-v2-glossary.md#decision)에서도 찾을 수 있습니다.

승인 결정은 승인 승인 끝점에서 검색할 수 있는 MVPD 승인 프로세스 조회 결과에 대한 정보를 저장합니다.

클라이언트 애플리케이션은 TV 공급자(신뢰할 수 있는) 결정이 사용자가 리소스 스트림에 액세스하도록 허용하는 경우 미디어 토큰을 포함하는 인증 결정을 사용하여 리소스 스트림을 재생할 수 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [인증 결정 API 검색](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

권한 부여 결정은 문제 발생 시점에 지정된 제한적이고 짧은 기간에 유효하며, 이는 MVPD를 다시 쿼리하기 전에 Adobe Pass 인증에 의해 캐시되는 시간을 나타냅니다.

인증(authZ) [TTL](./rest-api-v2-glossary.md#ttl)이라고 하는 이 제한된 기간은 조직 관리자 중 한 사람이나 사용자를 대신하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](./rest-api-v2-glossary.md#tve-dashboard)를 통해 보고 변경할 수 있습니다.

자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) 설명서를 참조하십시오.

### 4. 미디어 토큰은 무엇이며 유효기간은 얼마입니까? {#authorization-phase-faq4}

미디어 토큰은 [용어집](./rest-api-v2-glossary.md#media-token) 설명서에 정의된 용어입니다.

미디어 토큰은 의사 결정 권한 부여 끝점에서 검색할 수 있는 일반 텍스트로 전송된 서명된 문자열로 구성됩니다.

자세한 내용은 [미디어 토큰 검증기 통합](/help/authentication/media-token-verifier-int.md) 설명서를 참조하십시오.

미디어 토큰은 문제 발생 시 지정된 제한적이고 짧은 기간에 유효하며, 이는 의사 결정 승인 끝점을 다시 쿼리하기 전에 클라이언트 애플리케이션에서 사용해야 하는 시간을 나타냅니다.

TV 공급자(신뢰할 수 있는) 결정에서 사용자가 액세스할 수 있는 경우 클라이언트 애플리케이션은 미디어 토큰을 사용하여 리소스 스트림을 재생할 수 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [인증 결정 API 검색](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

### 5. 리소스란 무엇이며, 지원되는 형식은 무엇입니까? {#authorization-phase-faq5}

리소스는 [용어집](./rest-api-v2-glossary.md#resource) 설명서에 정의된 용어입니다.

리소스는 MVPD와 합의되고 클라이언트 애플리케이션이 스트리밍할 수 있는 콘텐츠와 연결된 고유 식별자입니다.

리소스 고유 식별자는 두 가지 형식을 가질 수 있습니다.

* 채널(브랜드)의 고유 식별자와 같은 간단한 문자열 형식입니다.
* 제목, 등급 및 자녀 보호 메타데이터와 같은 추가 정보가 포함된 미디어 RSS(MRSS) 형식입니다.

자세한 내용은 [보호된 리소스 식별](/help/authentication/identify-protected-resources.md) 설명서를 참조하세요.

### 6. 클라이언트 애플리케이션이 한 번에 몇 개의 리소스를 승인할 수 있습니까? {#authorization-phase-faq6}

클라이언트 애플리케이션은 MVPD에 의해 부과된 조건으로 인해 단일 API 요청에서 제한된 수의 리소스(일반적으로 최대 1)에 대한 인증 결정을 얻을 수 있습니다.

+++

+++으로그아웃 단계 FAQ

### 1. 로그아웃 단계의 목적은 무엇입니까? {#logout-phase-faq1}

로그아웃 단계의 목적은 사용자 요청 시 Adobe Pass 인증 내에서 사용자의 인증된 프로필을 종료할 수 있는 기능을 클라이언트 애플리케이션에 제공하는 것입니다.

+++

+++헤더 FAQ

### 1. 인증 헤더의 값을 계산하는 방법 {#headers-faq1}

[인증](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) 요청 헤더에 클라이언트 응용 프로그램에서 Adobe Pass으로 보호된 API에 액세스하는 데 필요한 `Bearer` 액세스 토큰이 포함되어 있습니다.

인증 헤더 값은 등록 단계에서 Adobe Pass 인증에서 가져와야 합니다.

자세한 내용은 다음 문서를 참조하십시오.

* [동적 클라이언트 등록 개요](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [클라이언트 자격 증명 API 검색](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [액세스 토큰 API 검색](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [동적 클라이언트 등록 흐름](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

클라이언트 응용 프로그램이 REST API V1에서 REST API V2로 마이그레이션하는 경우 클라이언트 응용 프로그램은 이전과 동일한 방법을 사용하여 `Bearer` 액세스 토큰을 가져올 수 있습니다.

### 2. AP-Device-Identifier 헤더의 값을 계산하는 방법 {#headers-faq2}

[AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) 요청 헤더에 클라이언트 응용 프로그램에서 만든 스트리밍 장치 식별자가 포함되어 있습니다.

[AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) 헤더 설명서는 다른 플랫폼에 대한 값을 계산하는 방법에 대한 몇 가지 예를 제공하지만, 클라이언트 애플리케이션은 자체 비즈니스 논리 및 요구 사항에 따라 다른 메서드를 사용하도록 선택할 수 있습니다.

클라이언트 애플리케이션이 REST API V1에서 REST API V2로 마이그레이션하는 경우 클라이언트 애플리케이션은 이전과 동일한 방법을 사용하여 디바이스 식별자를 계속 계산할 수 있습니다.

### 3. X-Device-Info 헤더에 대한 값을 계산하는 방법 {#headers-faq3}

[X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) 요청 헤더에 실제 스트리밍 장치와 관련된 클라이언트 정보(장치, 연결 및 응용 프로그램)가 포함되어 있습니다.

[X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) 헤더 설명서는 다양한 플랫폼에 대한 값을 계산하는 방법에 대한 몇 가지 예를 제공하지만, 클라이언트 애플리케이션은 자체 비즈니스 논리 및 요구 사항에 따라 다른 메서드를 사용하도록 선택할 수 있습니다.

클라이언트 애플리케이션이 REST API V1에서 REST API V2로 마이그레이션하는 경우 클라이언트 애플리케이션은 이전과 동일한 방법을 사용하여 디바이스 정보를 계속 계산할 수 있습니다.

+++

## 마이그레이션 FAQ {#migration-faqs}

기존 애플리케이션을 REST API V2로 마이그레이션해야 하는 애플리케이션에서 작업하는 경우 이 섹션을 계속합니다.

+++일반 마이그레이션 FAQ

### 1. REST API V2로 마이그레이션된 새 클라이언트 애플리케이션을 모든 사용자에게 한 번에 롤아웃해야 합니까? {#migration-faq1}

아니.

클라이언트 애플리케이션은 REST API V2를 통합하는 새 버전을 모든 사용자에게 동시에 롤아웃할 필요가 없습니다.

Adobe Pass 인증은 2025년 말까지 REST API V1 또는 SDK를 통합하는 이전 클라이언트 애플리케이션 버전을 계속 지원할 예정입니다.

### 2. 모든 API 및 플로우에서 REST API V2로 마이그레이션된 새 클라이언트 애플리케이션을 한 번에 롤아웃해야 합니까? {#migration-faq2}

예.

클라이언트 애플리케이션은 모든 Adobe Pass 인증 API 및 플로우에 걸쳐 REST API V2를 통합하는 새 버전을 동시에 롤아웃해야 합니다.

&#39;두 번째 화면 인증&#39; 흐름의 경우 클라이언트 응용 프로그램은 [기본](./rest-api-v2-glossary.md#primary-application) 및 [보조](./rest-api-v2-glossary.md#secondary-application) 응용 프로그램 모두에 대해 REST API V2를 통합하는 새 버전을 동시에 롤아웃해야 합니다.

Adobe Pass 인증은 API와 흐름 간에 REST API V2 및 REST API V1/SDK를 모두 통합하는 &#39;하이브리드&#39; 구현을 지원하지 않습니다.

### 3. REST API V2로 마이그레이션된 새 클라이언트 애플리케이션으로 업데이트할 때 사용자 인증이 유지됩니까? {#migration-faq3}

아니.

REST API V1 또는 SDK를 통합하는 이전 클라이언트 애플리케이션 버전 내에서 받은 사용자 인증은 유지되지 않습니다.

따라서 REST API V2로 마이그레이션된 새 클라이언트 애플리케이션 내에서 다시 인증해야 합니다.

### 4. 클라이언트 애플리케이션이 기존의 등록된 애플리케이션(소프트웨어 명령문)을 사용할 수 있습니까? {#migration-faq4}

클라이언트 애플리케이션은 기존 등록 애플리케이션(소프트웨어 명령문)을 다시 사용할 수 없으므로 REST API V2 사용 전용의 새로운 등록 애플리케이션(소프트웨어 명령문)을 생성하고 다운로드해야 합니다.

이 작업은 조직 관리자 중 한 사람이나 사용자를 대신하여 활동하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](./rest-api-v2-glossary.md#tve-dashboard)를 통해 완료할 수 있습니다.

자세한 내용은 [TVE 대시보드 채널 사용 안내서](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) 또는 [TVE 대시보드 프로그래머 사용 안내서](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications) 설명서를 참조하십시오.

이 작업의 자체 관리를 허용하도록 Adobe Pass [TVE 대시보드](./rest-api-v2-glossary.md#tve-dashboard)가 업데이트될 때까지 Adobe Pass 인증 담당자에게 새 등록된 응용 프로그램(소프트웨어 문)에 대한 REST API V2 사용을 활성화하도록 요청해야 합니다.

REST API V2를 사용하는 클라이언트 응용 프로그램에 사용되는 등록된 응용 프로그램(소프트웨어 문)을 구분하려면 등록된 응용 프로그램 이름에 &quot;RESV2&quot;와 같은 특정 접미사를 추가해야 합니다.

### 5. 클라이언트 애플리케이션이 기존 사용자 정의 체계를 사용할 수 있습니까? {#migration-faq5}

클라이언트 응용 프로그램은 Adobe Pass [TVE 대시보드](./rest-api-v2-glossary.md#tve-dashboard)를 통해 생성된 기존 사용자 지정 구성표를 다시 사용할 수 있습니다.

자세한 내용은 [TVE 대시보드 채널 사용 안내서](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#custom-schemes) 또는 [TVE 대시보드 프로그래머 사용 안내서](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#custom-schemes) 설명서를 참조하십시오.

### 6. 향상된 오류 코드가 REST API V2에서 기본적으로 활성화되어 있습니까? {#migration-faq6}

예.

REST API V2를 통합하는 클라이언트 애플리케이션은 기본적으로 활성화된 향상된 오류 코드 기능을 활용할 수 있습니다.

자세한 내용은 [향상된 오류 코드](/help/authentication/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) 설명서를 참조하세요.

+++

### REST API V1에서 REST API V2로 마이그레이션 {#migration-rest-api-v1-to-rest-api-v2-faqs}

REST API V1에서 REST API V2로 마이그레이션해야 하는 응용 프로그램에서 작업하는 경우 이 하위 섹션을 계속합니다.

+++등록 단계 FAQ

#### 1. 등록 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#registration-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션하는 경우 등록 단계와 관련된 높은 수준의 변경 사항이 없습니다.

클라이언트 응용 프로그램은 계속해서 동일한 흐름을 사용하여 [DCR(Dynamic Client Registration)](./rest-api-v2-glossary.md#dcr) 프로세스를 통해 Adobe Pass 인증에 등록하고 액세스 토큰을 가져올 수 있습니다.

자세한 내용은 다음 문서를 참조하십시오.

* [동적 클라이언트 등록 개요](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [클라이언트 자격 증명 API 검색](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [액세스 토큰 API 검색](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [동적 클라이언트 등록 흐름](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

+++

+++구성 단계 FAQ

#### 1. 구성 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#configuration-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|------------------------------------------------|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 활성 통합이 있는 MVPD 목록 검색 | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++인증 단계 FAQ

#### 1. 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#authentication-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 등록 코드 검색(인증 코드) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 등록 코드(인증 코드) 확인 | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 두 번째 화면(애플리케이션)에서 MVPD(인증 다시 시작 | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD) 인증 시작 | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 사용자 인증 상태 확인 | [GET <br/> /api/v1/checkauthn(첫 번째 화면)](/help/authentication/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn(두 번째 화면)](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 인증 토큰(프로필) 검색 | [GET <br/> /api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 메타데이터 정보 검색 | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++사전 인증 단계 FAQ

#### 1. 사전 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#preauthorization-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사전 승인된 리소스 검색(사전 인증 결정) | [GET <br/> /api/v1/사전 승인(첫 번째 화면)](/help/authentication/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/사전 승인(두 번째 화면)](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++인증 단계 FAQ

#### 1. 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#authorization-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|-------------------------------------------------------|----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD) 인증 시작 | [GET <br/> /api/v1/authorize](/help/authentication/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 인증 토큰 검색(인증 결정) | [GET <br/> /api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 짧은 인증 토큰 검색(미디어 토큰) | [GET <br/> /api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++으로그아웃 단계 FAQ

#### 1. 로그아웃 단계에서 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#logout-phase-v1-to-v2-faq1}

REST API V1에서 REST API V2로 마이그레이션할 때 다음 표에 나타나는 고려해야 할 높은 수준의 변경 사항이 있습니다.

| 범위 | REST API V1 | REST API V2 | 관찰 |
|-----------------|---------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 로그아웃 시작 | [GET <br/> /api/v1/logout](/help/authentication/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### SDK에서 REST API V2로 마이그레이션 {#migrate-sdk-to-rest-api-v2}

SDK에서 REST API V2로 마이그레이션해야 하는 애플리케이션에서 작업 중인 경우 이 하위 섹션을 계속합니다.

+++등록 단계 FAQ

#### 1. 등록 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#registration-phase-sdk-to-v2-faq1}

SDK에서 REST API V2로 마이그레이션할 때 다음 표에 고려해야 할 높은 수준의 변경 사항이 나와 있습니다.

##### AccessEnabler JavaScript SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DCR(Dynamic Client Registration) 완료 | 생성자에 소프트웨어 구문 제공 | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[동적 클라이언트 등록 개요](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[동적 클라이언트 등록 흐름](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DCR(Dynamic Client Registration) 완료 | 생성자에 소프트웨어 구문 제공 | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[동적 클라이언트 등록 개요](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[동적 클라이언트 등록 흐름](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DCR(Dynamic Client Registration) 완료 | 생성자에 소프트웨어 구문 제공 | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[동적 클라이언트 등록 개요](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[동적 클라이언트 등록 흐름](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler Fireos SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DCR(Dynamic Client Registration) 완료 | 생성자에 소프트웨어 구문 제공 | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[동적 클라이언트 등록 개요](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[동적 클라이언트 등록 흐름](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

+++구성 단계 FAQ

#### 1. 구성 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#configuration-phase-sdk-to-v2-faq1}

SDK에서 REST API V2로 마이그레이션할 때 다음 표에 고려해야 할 높은 수준의 변경 사항이 나와 있습니다.

##### AccessEnabler JavaScript SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------------------|--------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 활성 통합이 있는 MVPD 목록 검색 | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler iOS/tvOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 활성 통합이 있는 MVPD 목록 검색 | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler Android SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 활성 통합이 있는 MVPD 목록 검색 | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler Fireos SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 활성 통합이 있는 MVPD 목록 검색 | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++인증 단계 FAQ

#### 1. 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#authentication-phase-sdk-to-v2-faq1}

SDK에서 REST API V2로 마이그레이션할 때 다음 표에 고려해야 할 높은 수준의 변경 사항이 나와 있습니다.

##### AccessEnabler JavaScript SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD) 인증 시작 | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 사용자 인증 상태 확인 | [AccessEnabler.checkAuthentication](/help/authentication/javascript-sdk-api-reference.md#checkAuthN) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 메타데이터 정보 검색 | [AccessEnabler.getMetadata](/help/authentication/javascript-sdk-api-reference.md#getMeta) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler iOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD) 인증 시작 | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 사용자 인증 상태 확인 | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 메타데이터 정보 검색 | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler tvOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 등록 코드 검색(인증 코드) | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 등록 코드(인증 코드) 확인 | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 두 번째 화면(애플리케이션)에서 MVPD(인증 다시 시작 | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD) 인증 시작 | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 사용자 인증 상태 확인 | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 메타데이터 정보 검색 | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 범위 | SDK | REST API V2 | 관찰 |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD) 인증 시작 | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 사용자 인증 상태 확인 | [AccessEnabler.checkAuthentication](/help/authentication/android-sdk-api-reference.md#checkAuthN) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 메타데이터 정보 검색 | [AccessEnabler.getMetadata](/help/authentication/android-sdk-api-reference.md#getMetadata) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler Fireos SDK

| 범위 | SDK | REST API V2 | 관찰 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 등록 코드 검색(인증 코드) | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 등록 코드(인증 코드) 확인 | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 두 번째 화면(애플리케이션)에서 MVPD(인증 다시 시작 | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD) 인증 시작 | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/세션](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 사용자 인증 상태 확인 | [AccessEnabler.checkAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthN) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 사용자 메타데이터 정보 검색 | [AccessEnabler.getMetadata](/help/authentication/amazon-fireos-native-client-api-reference.md#getMetadata) | 다음 중 하나를 사용하십시오. <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 클라이언트 응용 프로그램은 이러한 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>사용자 인증 상태 확인</li><li>사용자 프로필 검색</li><li>사용자 메타데이터 정보 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[보조 응용 프로그램 내에서 수행되는 기본 프로필 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++사전 인증 단계 FAQ

#### 1. 사전 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#preauthorization-phase-sdk-to-v2-faq1}

SDK에서 REST API V2로 마이그레이션할 때 다음 표에 고려해야 할 높은 수준의 변경 사항이 나와 있습니다.

##### AccessEnabler JavaScript SDK

| 범위 | SDK | REST API V2 | 관찰 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사전 승인된 리소스 검색(사전 인증 결정) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사전 승인된 리소스 검색(사전 인증 결정) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 범위 | SDK | REST API V2 | 관찰 |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사전 승인된 리소스 검색(사전 인증 결정) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| 범위 | SDK | REST API V2 | 관찰 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사전 승인된 리소스 검색(사전 인증 결정) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++인증 단계 FAQ

#### 1. 인증 단계에 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#authorization-phase-sdk-to-v2-faq1}

SDK에서 REST API V2로 마이그레이션할 때 다음 표에 고려해야 할 높은 수준의 변경 사항이 나와 있습니다.

##### AccessEnabler JavaScript SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 짧은 인증 토큰 검색(미디어 토큰) | [AccessEnabler.checkAuthorization](/help/authentication/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 짧은 인증 토큰 검색(미디어 토큰) | [AccessEnabler.checkAuthorization](/help/authentication/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 짧은 인증 토큰 검색(미디어 토큰) | [AccessEnabler.checkAuthorization](/help/authentication/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Fireos SDK

| 범위 | SDK | REST API V2 | 관찰 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 짧은 인증 토큰 검색(미디어 토큰) | [AccessEnabler.checkAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | 클라이언트 응용 프로그램에서 이 API의 응답을 한 번에 여러 용도로 사용할 수 있습니다. <br/> <ul><li>(MVPD) 인증 시작</li><li>인증 결정 검색</li><li>짧은 미디어 토큰 검색</li></ul> <br/> 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 인증 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++으로그아웃 단계 FAQ

#### 1. 로그아웃 단계에서 필요한 높은 수준의 API 마이그레이션은 무엇입니까? {#logout-phase-sdk-to-v2-faq1}

SDK에서 REST API V2로 마이그레이션할 때 다음 표에 고려해야 할 높은 수준의 변경 사항이 나와 있습니다.

##### AccessEnabler JavaScript SDK

| 범위 | SDK | REST API V2 | 관찰 |
|-----------------|-------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 로그아웃 시작 | [AccessEnabler.logout](/help/authentication/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 범위 | SDK | REST API V2 | 관찰 |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 로그아웃 시작 | [AccessEnabler.logout](/help/authentication/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 범위 | SDK | REST API V2 | 관찰 |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 로그아웃 시작 | [AccessEnabler.logout](/help/authentication/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler Fireos SDK

| 범위 | SDK | REST API V2 | 관찰 |
|-----------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 로그아웃 시작 | [AccessEnabler.logout](/help/authentication/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | 자세한 내용은 다음 문서를 참조하십시오. <br/> <ul><li>[기본 응용 프로그램 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
