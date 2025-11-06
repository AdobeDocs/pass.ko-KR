---
title: REST API V2 Cookbook(클라이언트-서버)
description: REST API V2 Cookbook(클라이언트-서버)
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1833'
ht-degree: 0%

---

# REST API V2 Cookbook(클라이언트-서버) {#rest-api-v2-cookbook-client-to-server}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> REST API V2 구현은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md) 설명서에 의해 제한됩니다.

이 문서는 C2S(클라이언트-서버) 아키텍처를 가진 스트리밍 애플리케이션에 [Adobe Pass 인증 REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)을(를) 통합하는 개발자를 위한 것입니다.

## 사전 요구 사항 {#prerequisites}

용어와 정의는 [REST API V2 용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md) 설명서를 참조하세요.

필수 요구 사항 및 권장 사례를 보려면 [REST API V2 검사 목록](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md) 설명서를 참조하십시오.

FAQ에 대해서는 [REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md) 설명서를 참조하십시오.

## A. 등록 단계 {#registration-phase}

등록 단계의 목적은 DCR(Dynamic Client Registration) 프로세스를 통해 Adobe Pass 인증에 대해 스트리밍 애플리케이션을 등록하는 것입니다.

DCR(Dynamic Client Registration) 프로세스를 진행하려면 스트리밍 애플리케이션이 클라이언트 자격 증명 쌍을 가져오고 등록 단계의 최종 목표로 액세스 토큰을 검색해야 합니다.

등록 단계는 필수이지만, 스트리밍 애플리케이션은 여전히 유효한 클라이언트 자격 증명 및 액세스 토큰의 캐시된 쌍이 있는 경우 이 단계를 건너뛸 수 있습니다.

+++관련 문서

**API:**

* [클라이언트 자격 증명 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [액세스 토큰 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**흐름:**

* [동적 클라이언트 등록 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**FAQ:**

* [등록 단계 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### 1단계: 애플리케이션 등록 {#step-1-register-your-application}

* **클라이언트 자격 증명 검색:** 스트리밍 응용 프로그램은 [**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) 끝점을 호출하여 클라이언트 자격 증명을 검색합니다.

   * 스트리밍 애플리케이션은 클라이언트 자격 증명을 저장하고 액세스 토큰을 검색해야 할 때 무기한 사용해야 합니다.


* **액세스 토큰 검색:** 스트리밍 응용 프로그램에서 [**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) 끝점을 호출하여 액세스 토큰을 검색합니다.

   * 스트리밍 애플리케이션은 만료될 때까지 액세스 토큰을 저장 및 사용한 다음 삭제하고 새 액세스 토큰을 얻어야 합니다.

## B. 인증 단계 {#authentication-phase}

인증 단계의 목적은 스트리밍 애플리케이션에 사용자의 ID를 확인하고 사용자 메타데이터 정보를 얻을 수 있는 기능을 제공하는 것입니다.

인증 단계는 스트리밍 애플리케이션에서 콘텐츠를 재생해야 하는 경우 사전 인증 단계 또는 인증 단계에 대한 사전 요구 단계로 작동합니다.

+++관련 문서

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

* [인증 단계 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### 2단계: 인증된 기존 프로필 확인 {#step-2-check-for-existing-authenticated-profiles}

* **프로필 검색:** 스트리밍 응용 프로그램에서 [**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) 끝점을 호출하여 기존 프로필을 확인합니다.


* **시나리오 1:** 기존 프로필이 있습니다. 스트리밍 응용 프로그램은 [사전 권한 부여 단계](#preauthorization-phase) 또는 [권한 부여 단계](#authorization-phase)로 진행할 수 있습니다.


* **시나리오 2:** 기존 프로필이 없습니다. 스트리밍 응용 프로그램은 [사용자 인증](#step-3-authenticate-the-user)을 위해 다음 단계로 진행할 수 있습니다.


* **시나리오 3:** 기존 프로필이 없습니다. 스트리밍 응용 프로그램에서 [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) 기능을 통해 사용자에게 임시 액세스 권한을 제공할 수 있습니다.

   * 이 시나리오는 이 문서의 범위를 벗어납니다. 자세한 내용은 [임시 액세스 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) 설명서를 참조하세요.

### 3단계: 사용자 인증 {#step-3-authenticate-the-user}

* **구성 검색:** 스트리밍 응용 프로그램은 [**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) 끝점을 호출하여 사용 가능한 MVPD 목록을 검색합니다.

   * 스트리밍 애플리케이션은 구성 응답에서 MVPD의 목록을 세분화하기 위한 맞춤형 필터링 메커니즘을 구현하여 다른 공급자(예: 개발 중인 MVPD, 테스트 MVPD, TempPass)만 숨길 수 있습니다. 이렇게 하면 사용자가 TV 공급자를 선택할 때 조정된 선택 사항이 표시됩니다.


* **인증 세션 만들기:** 스트리밍 응용 프로그램에서 [**/api/v2/{serviceProvider}/sessions**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 끝점을 호출하여 인증 세션을 시작합니다.


* **시나리오 1:** 스트리밍 응용 프로그램에서 브라우저나 웹 보기를 열 수 있으므로 `url` 인증을 로드해야 합니다.

   * 사용자는 MVPD 로그인 페이지 내에서 사용자 이름과 암호를 제출합니다. 인증에 성공하면 최종 리디렉션에 성공 페이지가 표시됩니다.


* **시나리오 2:** 스트리밍 응용 프로그램에서 브라우저를 열 수 없으므로 `code` 인증을 표시해야 합니다. 사용자에게 `code`을(를) 입력하고, `url` 인증을 만들고, 다음을 열려면 별도의 웹 응용 프로그램이 필요합니다. [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

   * 사용자는 MVPD 로그인 페이지 내에서 사용자 이름과 암호를 제출합니다. 인증에 성공하면 최종 리디렉션에 성공 페이지가 표시됩니다.

### 4단계: 인증된 프로필 확인 {#step-4-check-for-authenticated-profiles}

* **특정 코드에 대한 프로필 검색:** 스트리밍 응용 프로그램은 `code`/api/v2/[**/profiles/code/{serviceProvider} 끝점을 호출하여 프로필이 성공적으로 생성 및 저장되었는지 확인하려면 {code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)을(를) 사용하여 폴링 메커니즘을 구현해야 합니다.

   * 스트리밍 응용 프로그램은 다음 조건에서 **폴링을 시작** 메커니즘을 시작해야 합니다.

      * **기본(화면) 응용 프로그램 내에서 수행되는 인증:** 브라우저 구성 요소가 `redirectUrl`세션[&#x200B; 끝점 요청에서 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 매개 변수에 대해 지정된 URL을 로드한 후 사용자가 최종 대상 페이지에 도달하면 기본(스트리밍) 응용 프로그램에서 폴링을 시작해야 합니다.

      * **보조(화면) 응용 프로그램 내에서 수행되는 인증:** 기본(스트리밍) 응용 프로그램은 사용자가 인증 프로세스를 시작하는 즉시([세션](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 끝점 응답을 받고 인증 코드를 사용자에게 표시한 후) 폴링을 시작해야 합니다.

   * 스트리밍 응용 프로그램은 다음 조건에서 **폴링을 중지** 메커니즘을 수행해야 합니다.

      * **인증 성공:** 사용자의 프로필 정보를 성공적으로 검색하여 인증 상태를 확인합니다. 이 시점에서는 더 이상 폴링이 필요하지 않습니다.

      * **인증 세션 및 코드 만료:** `notAfter`세션[&#x200B; 끝점 응답에서 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 타임스탬프(예: 30분)에 표시된 대로 인증 세션 및 코드가 만료됩니다. 이 경우 사용자는 인증 프로세스를 다시 시작해야 하며 이전 인증 코드를 사용한 폴링을 즉시 중지해야 합니다.

      * **새 인증 코드 생성됨:** 사용자가 기본(화면) 장치에서 새 인증 코드를 요청하면 기존 세션이 더 이상 유효하지 않으며 이전 인증 코드를 사용한 폴링을 즉시 중지해야 합니다.

   * 다음 조건에서 스트리밍 응용 프로그램은 **폴링** 메커니즘 빈도를 구성해야 합니다.

      * **기본(화면) 응용 프로그램 내에서 수행되는 인증:** 기본(스트리밍) 응용 프로그램은 3-5초 이상 간격으로 폴링해야 합니다.

      * **보조(화면) 응용 프로그램 내에서 수행되는 인증:** 기본(스트리밍) 응용 프로그램은 3-5초 이상 간격으로 폴링해야 합니다.

   * 스트리밍 애플리케이션은 불필요한 요청을 방지하고 사용자 경험을 개선하기 위해 사용자 프로필 정보의 일부를 영구 저장소에 캐싱해야 합니다.

## C. (선택 사항) 사전 인증 단계 {#preauthorization-phase}

사전 인증 단계의 목적은 사용자가 액세스할 수 있는 카탈로그의 리소스 하위 집합을 스트리밍 애플리케이션에 제공하는 것입니다.

사전 인증 단계는 사용자가 스트리밍 애플리케이션을 처음 열거나 새 섹션으로 이동할 때 사용자 경험을 향상시킬 수 있습니다.

사전 인증 단계는 필수가 아닙니다. 스트리밍 애플리케이션은 사용자의 권한에 따라 먼저 필터링하지 않고 리소스 카탈로그를 표시하려는 경우 이 단계를 건너뛸 수 있습니다.

+++관련 문서

**API**

* [사전 인증 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**흐름**

* [기본 애플리케이션 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**FAQ**

* [사전 인증 단계 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### 5단계: 사전 승인된 리소스 확인 {#step-5-check-for-preauthorized-resources}

* **사전 권한 부여 결정 검색:** 스트리밍 응용 프로그램은 [**/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) 끝점을 호출하여 리소스 목록에 대한 사전 권한 부여 결정을 검색합니다.

   * 스트리밍 애플리케이션은 영구 스토리지에 사전 인증 결정을 저장할 필요가 없습니다. 그러나 사용자 경험을 개선하기 위해 허용 결정을 메모리에 캐시하는 것이 좋습니다. 이렇게 하면 이미 사전 승인된 리소스에 대한 불필요한 호출을 방지하여 지연을 줄이고 성능을 개선하는 데 도움이 됩니다.

   * 스트리밍 애플리케이션은 의사 결정 사전 인증 끝점의 응답에 포함된 [오류 코드 및 메시지](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)를 검사하여 사전 인증 거부 이유를 확인할 수 있습니다. 이러한 세부 사항은 insight에 사전 인증 요청이 거부된 특정 이유를 제공하여 사용자 경험을 알리거나 애플리케이션에서 필요한 처리를 트리거하는 데 도움이 됩니다. 사전 권한 부여 결정을 검색하기 위해 구현된 모든 다시 시도 메커니즘이 사전 권한 부여 결정이 거부되는 경우 무한 루프가 발생하지 않도록 합니다. 적절한 횟수로 재시도 횟수를 제한하고 명확한 피드백을 사용자에게 표시하여 부인들을 우아하게 처리하는 것이 좋습니다.

   * 스트리밍 애플리케이션은 MVPD들에 의해 부과된 조건들로 인해, 단일 API 요청에서 제한된 수의 리소스, 일반적으로 최대 5개에 대한 사전 인가 결정을 획득할 수 있다. 이 최대 리소스 수는 조직 관리자 중 한 사람이나 사용자를 대신하여 활동하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)를 통해 MVPD와 동의한 후 보고 변경할 수 있습니다.


## D. 인증 단계 {#authorization-phase}

인증 단계의 목적은 MVPD에 대한 권한을 확인한 후 사용자가 요청하는 리소스를 스트리밍 애플리케이션에서 재생하는 기능을 제공하는 것입니다.

인증 단계는 필수 단계이며, 스트리밍 애플리케이션은 스트림을 릴리스하기 전에 사용자에게 권한이 있는지 MVPD을 통해 확인해야 하므로 사용자가 요청하는 리소스를 재생하려는 경우 이 단계를 건너뛸 수 없습니다.

+++관련 문서

**API**

* [인증 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**흐름**

* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**FAQ**

* [인증 단계 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### 6단계: 승인된 리소스 확인 {#step-6-check-for-authorized-resources}

* **권한 부여 결정 검색:** 스트리밍 응용 프로그램은 [**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) 끝점을 호출하여 특정 리소스에 대한 권한 부여 결정을 검색합니다.

   * 스트리밍 애플리케이션은 영구 스토리지에 권한 부여 결정을 저장할 필요가 없습니다.

   * 스트리밍 애플리케이션은 의사 결정 권한 부여 끝점의 응답에 포함된 [오류 코드 및 메시지](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)를 검사하여 거부된 인증 결정의 이유를 확인할 수 있습니다. 이러한 세부 사항은 insight에 인증 요청이 거부된 구체적인 이유를 제공하여 사용자 경험을 알리거나 애플리케이션에서 필요한 처리를 트리거하는 데 도움이 됩니다. 인증 결정이 거부된 경우 인증 결정을 검색하기 위해 구현된 재시도 메커니즘이 무한 루프가 되지 않도록 하십시오. 적절한 횟수로 재시도 횟수를 제한하고 명확한 피드백을 사용자에게 표시하여 부인들을 우아하게 처리하는 것이 좋습니다.

   * 스트림이 재생되는 동안 스트리밍 애플리케이션은 만료된 미디어 토큰을 새로 고칠 필요가 없습니다. 재생 중에 미디어 토큰이 만료되면 스트림은 중단 없이 계속 진행할 수 있도록 허용해야 합니다. 그러나 클라이언트는 다음에 사용자가 리소스를 재생하려고 할 때 새로운 인증 결정을 요청하고 새 미디어 토큰을 받아야 합니다.

   * 스트리밍 애플리케이션은 MVPD들에 의해 부과된 조건들로 인해, 단일 API 요청에서 제한된 수의 리소스, 일반적으로 최대 1에 대한 인가 결정을 획득할 수 있다.

## E. 로그아웃 단계 {#logout-phase}

로그아웃 단계의 목적은 사용자 요청 시 Adobe Pass 인증 내에서 사용자의 인증된 프로필을 종료할 수 있는 기능을 스트리밍 애플리케이션에 제공하는 것입니다.

로그아웃 단계는 필수입니다. 스트리밍 애플리케이션은 사용자에게 로그아웃 기능을 제공해야 합니다.

+++관련 문서

**API**

* [특정 mvpd에 대한 로그아웃 시작](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**흐름**

* [기본 애플리케이션 내에서 수행되는 기본 로그아웃 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**FAQ**

* [로그아웃 단계 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### 7단계: 로그아웃 {#step-7-logout}

* **Adobe Pass 로그아웃 시작:** 스트리밍 응용 프로그램에서 [**/api/v2/{serviceProvider}/logout/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) 끝점을 호출하여 로그아웃 흐름을 시작합니다.

   * 로그아웃 프로세스가 올바르게 완료되었는지 확인하려면 스트리밍 응용 프로그램이 로그아웃 끝점 응답의 `actionName` 및 `actionType` 특성에 제공된 지침을 따라야 합니다.

      * 응답의 `actionType` 특성이 &quot;interactive&quot;로 설정된 경우:

         * **시나리오 1:** 스트리밍 응용 프로그램에서 브라우저나 웹 보기를 열 수 있으므로 로그아웃 `url`을(를) 로드해야 합니다.

         * **시나리오 2:** 스트리밍 응용 프로그램에서 브라우저를 열 수 없으므로 MVPD 세션이 스트리밍 장치 브라우저 캐시에서 지속되지 않으므로 로그아웃 프로세스를 중지할 수 있습니다.
