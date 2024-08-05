---
title: 기본 사전 인증 - 기본 애플리케이션 - 플로우
description: REST API V2 - 기본 사전 인증 - 기본 애플리케이션 - 흐름
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---


# 기본 애플리케이션 내에서 수행되는 기본 사전 인증 흐름 {#basic-preauthorization-flow-performed-within-primary-application}

>[!NOTE]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

Adobe Pass 인증 권한 내의 **사전 인증 흐름**&#x200B;을 사용하면 스트리밍 응용 프로그램에서 MVPD가 리소스 목록에 대한 사용자의 액세스를 허용 또는 거부할 수 있는지 여부를 확인할 수 있습니다. 이러한 확인은 애플리케이션이 보기에 적합할 수 있는 콘텐츠에 대한 정확한 정보를 사용자에게 제시할 수 있도록 합니다.

## 특정 mvpd를 사용하여 사전 인증 결정 검색 {#retrieve-preauthorization-decisions-using-specific-mvpd}

### 전제 조건 {#prerequisites-retrieve-preauthorization-decisions-using-specific-mvpd}

특정 MVPD를 사용하여 사전 권한 부여 결정을 검색하기 전에 다음 사전 요구 사항이 충족되는지 확인하십시오.

* 스트리밍 애플리케이션에는 기본 인증 흐름 중 하나를 사용하여 MVPD에 대해 성공적으로 생성된 올바른 일반 프로필이 있어야 합니다.
   * [기본 응용 프로그램 내에서 인증 수행](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [사전 선택된 mvpd로 보조 응용 프로그램 내에서 인증 수행](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [미리 선택된 mvpd 없이 보조 응용 프로그램 내에서 인증 수행](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* 스트리밍 애플리케이션은 리소스 목록과 함께 관련 상태를 표시하기 위해 사전 인증 결정을 검색하려고 합니다.

### 워크플로 {#workflow-retrieve-preauthorization-decisions-using-specific-mvpd}

다음 다이어그램과 같이 기본 응용 프로그램 내에서 수행되는 특정 MVPD를 사용하여 기본 사전 권한 부여 흐름을 구현하려면 주어진 단계를 따르십시오.

![특정 mvpd를 사용하여 사전 권한 부여 결정 검색](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-preauthorization-decisions-within-primary-application-using-specific-mvpd.png)

*특정 mvpd를 사용하여 사전 권한 부여 결정 검색*

1. **사전 권한 부여 결정 검색:** 스트리밍 응용 프로그램은 의사 결정 사전 권한 부여 끝점을 호출하여 리소스 목록에 대한 사전 권한 부여 결정을 얻는 데 필요한 모든 데이터를 수집합니다.

   자세한 내용은 [특정 mvpd를 사용하여 사전 권한 부여 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   * `serviceProvider`, `mvpd` 및 `resources`과(와) 같은 모든 _필수_ 매개 변수
   * `Authorization` 및 `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   * 모든 _선택적_ 매개 변수 및 헤더

1. **일반 프로필 찾기:** Adobe Pass 서버는 받은 매개 변수와 헤더를 기반으로 올바른 프로필을 식별합니다.

1. **요청된 리소스에 대한 MVPD 결정 검색:** Adobe Pass 서버가 MVPD 사전 인증 끝점을 호출하여 스트리밍 응용 프로그램에서 받은 각 리소스에 대한 `Permit` 또는 `Deny` 결정을 가져옵니다.

1. **미리 권한 부여 결정 반환:** 미리 권한 부여 결정 끝점 응답에 각 리소스에 대한 `Permit` 또는 `Deny` 결정이 포함되어 있습니다.
   * `Permit` 결정은 리소스를 재생할 수 있음을 의미합니다. 사전 인증 흐름은 리소스를 재생하는 데 사용되면 안 되므로 응답에는 미디어 토큰이 포함되지 않습니다.
   * `Deny` 결정은 리소스를 재생할 수 없음을 의미합니다. 응답에는 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 오류 페이로드가 포함되어 있습니다.

   의사 결정 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd를 사용하여 사전 인증 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.

   >[!IMPORTANT]
   >
   > 결정 사전 인증 끝점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **사전 권한 부여 결정 처리:** 스트리밍 응용 프로그램에서 응답을 처리하고, 이 응답을 사용하여 사용자 인터페이스에 각 리소스에 대한 적절한 상태를 선택적으로 표시할 수 있습니다.
