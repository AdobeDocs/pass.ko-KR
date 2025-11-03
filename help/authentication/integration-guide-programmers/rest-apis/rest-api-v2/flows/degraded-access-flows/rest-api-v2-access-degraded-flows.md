---
title: 액세스 흐름이 저하됨
description: REST API V2 - 액세스 흐름 성능 저하
exl-id: 9276f5d9-8b1a-4282-8458-0c1e1e06bcf5
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '1615'
ht-degree: 0%

---

# 액세스 흐름 성능 저하 {#degraded-access-flows}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> REST API V2 구현은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md) 설명서에 의해 제한됩니다.

>[!MORELIKETHIS]
>
> [REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)도 방문하십시오.

성능 저하를 통해 특정 MVPD 인증 및 권한 부여 끝점을 일시적으로 우회할 수 있습니다. 일반적으로 프로그래머는 이 작업을 시작하지만 누가 열화 이벤트를 트리거하는지에 관계없이, 작업은 영향을 받는 MVPD와 함께 만들어진 사전 배열에 따라 달라집니다.

성능 저하 기능에 대한 자세한 내용은 [성능 저하](/help/premium-workflow/degraded-access/degradation-feature.md) 설명서를 참조하십시오.

액세스 흐름이 저하되면 다음 시나리오를 쿼리할 수 있습니다.

* [저하가 적용되는 동안 인증 수행](#perform-authentication-while-degradation-is-applied)
* [저하가 적용되는 동안 인증 결정 검색](#retrieve-authorization-decisions-while-degradation-is-applied)
* [저하가 적용되는 동안 사전 인증 결정 검색](#retrieve-preauthorization-decisions-while-degradation-is-applied)
* [저하가 적용되는 동안 프로필 검색](#retrieve-profile-while-degradation-is-applied)

## 저하가 적용되는 동안 인증 수행 {#perform-authentication-while-degradation-is-applied}

### 사전 요구 사항 {#prerequisites-perform-authentication-while-degradation-is-applied}

저하가 적용되는 동안 인증 흐름을 수행하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* 스트리밍 애플리케이션은 MVPD에 로그인해야 하는 경우 인증 세션을 시작해야 합니다.

>[!IMPORTANT]
> 
> 가정
> 
> <br/>
> 
> * 스트리밍 애플리케이션에 Adobe Pass 백엔드에 저장된 특정 MVPD에 대한 유효한 프로필이 없습니다.
> * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합에 AuthNAll 저하 규칙이 적용되었습니다.

### 워크플로 {#workflow-perform-authentication-while-degradation-is-applied}

다음 다이어그램에 표시된 대로 성능 저하가 적용되는 동안 인증 흐름을 구현하려면 주어진 단계를 따르십시오.

![저하가 적용되는 동안 인증 수행](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-perform-authentication-while-degradation-is-applied-flow.png)

*저하가 적용되는 동안 인증 수행*

1. **인증 세션 만들기:** 스트리밍 응용 프로그램은 세션 끝점을 호출하여 인증 세션을 시작하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [인증 세션 만들기](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API 설명서를 참조하십시오.
   > 
   > * _,_, `serviceProvider` 및 `mvpd`과(와) 같은 모든 `domainName`필수`redirectUrl` 매개 변수
   > * _및_&#x200B;과(와) 같은 모든 `Authorization`required`AP-Device-Identifier` 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **성능 저하 규칙 확인:** Adobe Pass 서버는 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 적용된 AuthNaLl 성능 저하 규칙이 있는지 확인합니다.

1. **다음 작업을 나타냅니다.** 세션 끝점 응답에는 다음 작업에 대해 스트리밍 응용 프로그램을 안내하는 데 필요한 데이터가 포함됩니다.
   * `actionName` 특성이 &quot;authorize&quot;로 설정되어 있습니다.
   * `actionType` 특성이 &quot;direct&quot;로 설정되어 있습니다.

   >[!IMPORTANT]
   >
   > 세션 응답에 제공된 정보에 대한 자세한 내용은 [인증 세션 만들기](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API 설명서를 참조하십시오.
   > 
   > <br/>
   > 
   > 세션 끝점은 요청 데이터를 확인하여 기본 조건이 충족되는지 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../../features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   > 
   > 세션 끝점은 요청 데이터를 사용하여 성능 저하된 액세스 조건이 충족되는지 확인합니다.
   >
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합에는 AuthNaLl 저하 규칙이 적용되어 있어야 합니다.
   >
   > <br/>
   > 
   > 성능이 저하된 액세스 유효성 검사에 실패하면 이 응답은 기본적으로 기본 인증 플로우로 설정됩니다.

1. **결정 흐름을 진행합니다.** 스트리밍 응용 프로그램은 후속 결정 흐름을 계속할 수 있습니다.

## 저하가 적용되는 동안 인증 결정 검색 {#retrieve-authorization-decisions-while-degradation-is-applied}

### 사전 요구 사항 {#prerequisites-retrieve-authorization-decisions-while-degradation-is-applied}

저하가 적용되는 동안 인증 결정을 검색하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* 스트리밍 애플리케이션은 사용자가 선택한 리소스를 재생하기 전에 인증 결정을 검색해야 합니다.

>[!IMPORTANT]
>
> 가정
> 
> <br/>
> 
> * 스트리밍 애플리케이션에 해당 특정 MVPD에 대한 유효한 프로필이 없습니다.
> * 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 AuthZAll 또는 AuthNAll 저하 규칙이 적용되었습니다.

### 워크플로 {#workflow-retrieve-authorization-decisions-while-degradation-is-applied}

다음 다이어그램과 같이 성능 저하가 적용되는 동안 인증 흐름을 구현하려면 주어진 단계를 따르십시오.

![저하가 적용되는 동안 권한 부여 결정 검색](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-retrieve-authorization-decisions-while-degradation-is-applied-flow.png)

*저하가 적용되는 동안 권한 부여 결정 검색*

1. **권한 부여 결정 검색:** 스트리밍 응용 프로그램은 결정 권한 부여 끝점을 호출하여 특정 리소스에 대한 권한 부여 결정을 얻는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   > 
   > 자세한 내용은 [특정 mvpd를 사용하여 권한 부여 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   >
   > * _,_ 및 `serviceProvider`과(와) 같은 모든 `mvpd`필수`resources` 매개 변수
   > * _및_&#x200B;과(와) 같은 모든 `Authorization`required`AP-Device-Identifier` 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **성능 저하 규칙 확인:** Adobe Pass 서버는 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 AuthZAll 또는 AuthNAll 성능 저하 규칙이 적용되었는지 확인합니다.

1. **미디어 토큰이 있는 `Permit` 결정을 반환합니다.** Decisions Authorize 끝점 응답에 `Permit` 결정 및 미디어 토큰이 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 의사 결정 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd를 사용하여 인증 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   >
   > <br/>
   > 
   > 결정 권한 부여 끝점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../../features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   >
   > 승인 결정 끝점은 요청 데이터를 사용하여 저하된 액세스 조건이 충족되는지 확인합니다.
   >
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합에는 AuthZAll 또는 AuthNAll 열화 규칙이 적용되어야 합니다.
   >
   > <br/>
   > 
   > 성능이 저하된 액세스 유효성 검사에 실패하면 이 응답은 기본적으로 기본 인증 플로우로 설정됩니다.

1. **미디어 토큰으로 스트림 시작:** 스트리밍 응용 프로그램에서 미디어 토큰을 사용하여 콘텐츠를 재생합니다.

## 저하가 적용되는 동안 사전 인증 결정 검색 {#retrieve-preauthorization-decisions-while-degradation-is-applied}

### 사전 요구 사항 {#prerequisites-retrieve-preauthorization-decisions-while-degradation-is-applied}

저하가 적용되는 동안 사전 인증 결정을 검색하기 전에 다음 사전 요구 사항이 충족되는지 확인하십시오.

* 스트리밍 애플리케이션은 리소스 목록과 함께 관련 상태를 표시하기 위해 사전 인증 결정을 검색하려고 합니다.

>[!IMPORTANT]
>
> 가정
>
> <br/>
> 
> * 스트리밍 애플리케이션에 해당 특정 MVPD에 대한 유효한 프로필이 없습니다.
> * 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 AuthZAll 또는 AuthNAll 저하 규칙이 적용되었습니다.

### 워크플로 {#workflow-retrieve-preauthorization-decisions-while-degradation-is-applied}

다음 다이어그램과 같이 성능 저하가 적용되는 동안 사전 인증 흐름을 구현하려면 주어진 단계를 따르십시오.

![저하가 적용되는 동안 사전 인증 결정을 검색합니다](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-retrieve-preauthorization-decisions-while-degradation-is-applied-flow.png)

*저하가 적용되는 동안 사전 인증 결정을 검색합니다*

1. **사전 권한 부여 결정 검색:** 스트리밍 응용 프로그램은 의사 결정 사전 권한 부여 끝점을 호출하여 리소스 목록에 대한 사전 권한 부여 결정을 얻는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 mvpd를 사용하여 사전 권한 부여 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   >
   > * _,_ 및 `serviceProvider`과(와) 같은 모든 `mvpd`필수`resources` 매개 변수
   > * _및_&#x200B;과(와) 같은 모든 `Authorization`required`AP-Device-Identifier` 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **성능 저하 규칙 확인:** Adobe Pass 서버는 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 AuthZAll 또는 AuthNAll 성능 저하 규칙이 적용되었는지 확인합니다.

1. **미리 권한 부여 결정 반환:** 미리 권한 부여 결정 끝점 응답에 각 리소스에 대한 `Permit` 결정이 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 의사 결정 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd를 사용하여 사전 인증 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   >
   > <br/>
   >
   > 결정 사전 인증 끝점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../../features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   >
   > 사전 권한 부여 결정 끝점은 요청 데이터를 사용하여 저하된 액세스 조건이 충족되는지 확인합니다.
   >
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합에는 AuthZAll 또는 AuthNAll 열화 규칙이 적용되어야 합니다.
   >
   > <br/>
   > 
   > 성능이 저하된 액세스 유효성 검사에 실패하면 응답은 기본적으로 기본 사전 인증 플로우로 설정됩니다.

1. **사전 권한 부여 결정 처리:** 스트리밍 응용 프로그램에서 응답을 처리하고, 이 응답을 사용하여 사용자 인터페이스에 각 리소스에 대한 적절한 상태를 선택적으로 표시할 수 있습니다.

## 저하가 적용되는 동안 프로필 검색 {#retrieve-profile-while-degradation-is-applied}

>[!IMPORTANT]
>
> 성능 저하가 적용되는 동안 프로필 끝점 쿼리는 선택 사항입니다.
>
> <br/>
> 
> 세션 끝점 응답은 성능 저하가 적용되는 동안 애플리케이션이 의사 결정 흐름을 진행하도록 지시합니다. 자세한 내용은 [저하가 적용되는 동안 인증 수행](#perform-authentication-while-degradation-is-applied) 섹션을 참조하십시오.

### 사전 요구 사항 {#prerequisites-retrieve-profile-while-degradation-is-applied}

저하가 적용되는 동안 특정 MVPD에 대한 프로필을 검색하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* `mvpd` 식별자가 선택되었거나 캐시된 스트리밍 응용 프로그램에서 특정 MVPD에 대한 프로필을 검색하려고 합니다.

>[!IMPORTANT]
>
> 가정
>
> <br/>
> 
> * 스트리밍 애플리케이션에 해당 특정 MVPD에 대한 유효한 프로필이 없습니다.
> * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합에 AuthNAll 저하 규칙이 적용되었습니다.

### 워크플로 {#workflow-retrieve-profile-while-degradation-is-applied}

다음 다이어그램과 같이 저하가 적용되는 동안 특정 MVPD에 대한 프로필 검색 흐름을 구현하려면 주어진 단계를 따르십시오.

![저하가 적용되는 동안 프로필 검색](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-retrieve-profile-while-degradation-is-applied-flow.png)

*저하가 적용되는 동안 프로필 검색*

1. **특정 mvpd에 대한 프로필 검색:** 스트리밍 응용 프로그램은 프로필 끝점에 요청을 보내 해당 특정 MVPD에 대한 프로필 정보를 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 mvpd에 대한 프로필 검색](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) API 설명서를 참조하십시오.
   >
   > * _및_&#x200B;과(와) 같은 모든 `serviceProvider`필수`mvpd` 매개 변수
   > * _및_&#x200B;과(와) 같은 모든 `Authorization`required`AP-Device-Identifier` 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **성능 저하 규칙 확인:** Adobe Pass 서버는 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 적용된 AuthNaLl 성능 저하 규칙이 있는지 확인합니다.

1. **성능이 저하된 프로필에 대한 정보를 반환합니다.** Profiles 끝점 응답에 `type` 특성을 &quot;성능이 저하됨&quot;으로 설정하는 등 성능이 저하된 프로필에 대한 정보가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 프로필 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd에 대한 프로필 검색](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) API 설명서를 참조하십시오.
   >
   > <br/>
   >
   > 프로필 끝점은 요청 데이터의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../../features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   > 
   > 프로필 끝점은 요청 데이터를 사용하여 저하된 액세스 조건이 충족되는지 확인합니다.
   >
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합에는 AuthNaLl 저하 규칙이 적용되어 있어야 합니다.
   >
   > <br/>
   > 
   > 성능이 저하된 액세스 유효성 검사에 실패하면 응답은 기본적으로 기본 프로필 검색 플로우로 설정됩니다.

1. **의사 결정 흐름을 진행합니다.** 프로필 끝점 응답에 프로필이 포함된 경우 스트리밍 응용 프로그램에서 성능이 저하된 프로필 정보를 사용하여 후속 의사 결정 흐름을 계속합니다.

1. **새 기본 인증 흐름을 나타냅니다.** Profiles 끝점 응답에 프로필이 포함되어 있지 않으면 스트리밍 응용 프로그램에서 사용자에게 새 기본 인증 흐름을 시작하도록 지시합니다.

>[!NOTE]
>
> 특정 인증 코드에 대한 프로필 검색 흐름의 단계는 사용된 끝점이 [특정 코드에 대한 프로필 검색](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) 설명서에 설명된 끝점이라는 점을 제외하면 위와 동일합니다.
