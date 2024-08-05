---
title: 기본 인증 - 기본 애플리케이션 - 플로우
description: REST API V2 - 기본 권한 부여 - 기본 애플리케이션 - 흐름
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '566'
ht-degree: 0%

---


# 기본 애플리케이션 내에서 수행되는 기본 인증 흐름 {#basic-authorization-flow-performed-within-primary-application}

Adobe Pass 인증 권한 내의 **인증 흐름**&#x200B;을 사용하면 스트리밍 응용 프로그램에서 MVPD가 사용자의 콘텐츠 스트리밍 요청을 허용할지 또는 거부할지 여부를 확인할 수 있습니다. 결정이 `Permit`인 경우 응답에 미디어 토큰이 포함됩니다. Adobe Pass 서버는 미디어 토큰에 서명하고 스트리밍 애플리케이션이 미디어 토큰 검증기 라이브러리를 사용하여 스트림이 릴리스되기 전에 그 진위를 확인할 수 있도록 합니다.

미디어 토큰 검증자 라이브러리를 사용한 확인은 CDN에서 스트림을 릴리스하기 위해 권한 체인에 연결된 스트리밍 애플리케이션 백엔드 서비스에서 수행해야 합니다.

## 특정 mvpd를 사용하여 권한 부여 결정 검색 {#retrieve-authorization-decisions-using-specific-mvpd}

### 전제 조건 {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

특정 MVPD를 사용하여 권한 부여 결정을 검색하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* 스트리밍 애플리케이션에는 기본 인증 흐름 중 하나를 사용하여 MVPD에 대해 성공적으로 생성된 올바른 일반 프로필이 있어야 합니다.
   * [기본 응용 프로그램 내에서 인증 수행](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [사전 선택된 mvpd로 보조 응용 프로그램 내에서 인증 수행](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [미리 선택된 mvpd 없이 보조 응용 프로그램 내에서 인증 수행](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* 스트리밍 애플리케이션은 사용자가 선택한 리소스를 재생하기 전에 인증 결정을 검색해야 합니다.

### 워크플로 {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

다음 다이어그램과 같이 기본 애플리케이션 내에서 수행된 특정 MVPD를 사용하여 기본 권한 흐름을 구현하려면 주어진 단계를 따르십시오.

![특정 mvpd를 사용하여 권한 부여 결정 검색](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png)

*특정 mvpd를 사용하여 권한 부여 결정 검색*

1. **권한 부여 결정 검색:** 스트리밍 응용 프로그램은 결정 권한 부여 끝점을 호출하여 특정 리소스에 대한 권한 부여 결정을 얻는 데 필요한 모든 데이터를 수집합니다.

   자세한 내용은 [특정 mvpd를 사용하여 권한 부여 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   * `serviceProvider`, `mvpd` 및 `resources`과(와) 같은 모든 _필수_ 매개 변수
   * `Authorization` 및 `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   * 모든 _선택적_ 매개 변수 및 헤더

1. **일반 프로필 찾기:** Adobe Pass 서버는 받은 매개 변수와 헤더를 기반으로 올바른 프로필을 식별합니다.

1. **요청된 리소스에 대한 MVPD 결정 검색:** Adobe Pass 서버가 MVPD 권한 부여 끝점을 호출하여 스트리밍 응용 프로그램에서 받은 특정 리소스에 대한 `Permit` 또는 `Deny` 결정을 가져옵니다.

1. **미디어 토큰이 있는 `Permit` 결정을 반환합니다.** Decisions Authorize 끝점 응답에 `Permit` 결정 및 미디어 토큰이 포함되어 있습니다.

   의사 결정 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd를 사용하여 인증 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.

   >[!IMPORTANT]
   >
   > 결정 권한 부여 끝점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **미디어 토큰으로 스트림 시작:** 스트리밍 응용 프로그램에서 미디어 토큰을 사용하여 콘텐츠를 재생합니다.

1. **세부 정보가 포함된 `Deny` 결정 반환:** Decisions Authorize Endpoint 응답에 `Deny` 결정 및 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 오류 페이로드가 포함되어 있습니다.

   의사 결정 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd를 사용하여 인증 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.

   >[!IMPORTANT]
   >
   > 결정 권한 부여 끝점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **`Deny` 결정 정보 처리:** 스트리밍 응용 프로그램에서 응답의 오류 정보를 처리하고 이를 사용하여 사용자 인터페이스에 특정 메시지를 선택적으로 표시할 수 있습니다.
