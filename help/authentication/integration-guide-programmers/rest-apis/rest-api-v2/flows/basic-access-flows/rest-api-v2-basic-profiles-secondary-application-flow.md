---
title: 기본 프로필 - 보조 애플리케이션 - 흐름
description: REST API V2 - 기본 프로필 - 보조 애플리케이션 - 흐름
exl-id: 1fcefcfa-7534-4b85-b3b5-df513685d66b
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 0%

---

# 보조 애플리케이션 내에서 수행되는 기본 프로필 흐름 {#basic-profiles-flow-secondary-application}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> REST API V2 구현은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md) 설명서에 의해 제한됩니다.

>[!MORELIKETHIS]
>
> [REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)도 방문하십시오.

Adobe Pass 인증 권한 내의 **프로필 흐름**&#x200B;을 사용하면 보조 응용 프로그램에서 활성 사용자 로그인에 대한 정보에 액세스할 수 있습니다.

기본 프로필 흐름을 사용하면 다음 시나리오를 쿼리할 수 있습니다.

* [특정 코드에 대한 프로필 검색](#retrieve-profile-for-specific-code)

## 특정 코드에 대한 프로필 검색 {#retrieve-profile-for-specific-code}

### 사전 요구 사항 {#prerequisites-retrieve-profile-for-specific-code}

특정 인증 코드에 대한 프로필을 검색하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* MVPD과의 대화형 인증을 수행하는 데 사용되는 `code`이(가) 있는 보조 응용 프로그램에서 특정 인증 코드에 대한 프로필을 검색하려고 합니다.

### 워크플로 {#workflow-retrieve-profile-for-specific-code}

다음 다이어그램과 같이 보조 응용 프로그램 내에서 수행되는 특정 인증 코드에 대한 기본 프로필 검색 플로우를 구현하려면 주어진 단계를 따르십시오.

![특정 코드에 대한 프로필 검색](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-secondary-application-for-specific-code.png)

*특정 코드에 대한 프로필 검색*

1. **특정 코드에 대한 프로필 검색:** 보조 응용 프로그램은 프로필 끝점에 요청을 보내 해당 특정 인증 코드에 대한 프로필 정보를 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 다음에 대한 자세한 내용은 [특정 코드에 대한 프로필 검색](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API 설명서를 참조하십시오.
   >
   > * _및_&#x200B;과(와) 같은 모든 `serviceProvider`필수`code` 매개 변수
   > * _과(와) 같은 모든_ required`Authorization` 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **일반 프로필 찾기:** Adobe Pass 서버는 받은 매개 변수와 헤더를 기반으로 올바른 프로필을 식별합니다.

1. **일반 프로필에 대한 정보를 반환합니다.** Profiles 끝점 응답에는 받은 매개 변수 및 헤더와 연결된 찾은 프로필에 대한 정보가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 프로필 응답에 제공된 정보에 대한 자세한 내용은 [특정 코드에 대한 프로필 검색](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API 설명서를 참조하십시오.
   > 
   > <br/>
   > 
   > 프로필 끝점은 요청 데이터의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   >
   > <br/>
   > 
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../../features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **인증 흐름이 성공으로 완료되었음을 나타냅니다.** Profiles 끝점 응답에 프로필이 포함되어 있으면 보조 응용 프로그램이 응답을 처리하고 해당 응답을 사용하여 사용자 인터페이스에 특정 메시지를 선택적으로 표시할 수 있습니다.

1. **인증 흐름에 문제가 발생했음을 나타냅니다.** 프로필 끝점 응답에 프로필이 포함되어 있지 않으면 보조 응용 프로그램이 응답을 처리하고 해당 응답을 사용하여 사용자 인터페이스에 특정 메시지를 선택적으로 표시할 수 있습니다.
