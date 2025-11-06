---
title: 동적 클라이언트 등록 흐름
description: 동적 클라이언트 등록 흐름
exl-id: d881cf0a-de09-4b1d-a094-d5490f944796
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# 동적 클라이언트 등록 흐름 {#dynamic-client-registration-flow}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> 동적 클라이언트 등록 API 구현은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md) 설명서에 의해 제한됩니다.

## Adobe Pass으로 보호된 API 액세스 {#access-adobe-pass-protected-apis}

### 사전 요구 사항 {#prerequisites-access-adobe-pass-protected-apis}

Adobe Pass으로 보호된 API에 액세스하려면 다음 전제 조건이 충족되는지 확인하십시오.

* 클라이언트 담당자는 [등록된 응용 프로그램 관리](../dynamic-client-registration-overview.md#manage-registered-applications) 섹션에 설명된 대로 등록된 응용 프로그램을 만들어야 합니다.
* 클라이언트 담당자는 [소프트웨어 문 관리](../dynamic-client-registration-overview.md#manage-software-statements) 섹션에 설명된 대로 소프트웨어 문을 다운로드하여 포함해야 합니다.

>[!IMPORTANT]
>
> Adobe Pass 인증 SDK는 클라이언트 애플리케이션을 대신하여 클라이언트 자격 증명 및 액세스 토큰을 가져오고 새로 고치는 역할을 합니다.
> 
> 다른 모든 Adobe Pass으로 보호된 API의 경우 클라이언트 애플리케이션은 아래 워크플로우를 따라야 합니다.

### 워크플로 {#workflow-access-adobe-pass-protected-apis}

다음 다이어그램에 표시된 대로 지정된 단계에 따라 Adobe Pass으로 보호된 API에 액세스합니다.

![Adobe Pass으로 보호된 API에 액세스](../../../../assets/dcr-api/dcr-api-access-adobe-pass-protected-apis.png)

*Adobe Pass으로 보호된 API에 액세스*

1. **클라이언트 자격 증명 검색:** 클라이언트 응용 프로그램은 클라이언트 등록 끝점을 호출하여 클라이언트 자격 증명을 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [클라이언트 자격 증명 검색](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) API 설명서를 참조하십시오.
   >
   > * _과(와) 같은 모든_&#x200B;필수`software_statement` 매개 변수
   > * _,_&#x200B;과(와) 같은 모든 `Content-Type`required`X-Device-Info` 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **클라이언트 자격 증명 반환:** 클라이언트 등록 끝점 응답에 수신한 매개 변수 및 헤더와 관련된 클라이언트 자격 증명에 대한 정보가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 클라이언트 자격 증명 응답에 제공된 정보에 대한 자세한 내용은 [클라이언트 자격 증명 검색](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) API 설명서를 참조하십시오.
   >
   > <br/>
   >
   > 클라이언트 레지스터는 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   >
   > <br/>
   >
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [클라이언트 자격 증명 검색](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) API 설명서를 준수하는 추가 정보가 제공됩니다.

   >[!TIP]
   >
   > 클라이언트 자격 증명은 캐시되어 무기한으로 사용되어야 합니다.

1. **액세스 토큰 검색:** 클라이언트 응용 프로그램은 클라이언트 토큰 끝점을 호출하여 액세스 토큰을 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [액세스 토큰 검색](../apis/dynamic-client-registration-apis-retrieve-access-token.md#request) API 설명서를 참조하십시오.
   >
   > * _,_ 및 `client_id`과(와) 같은 모든 `client_secret`필수`grant_type` 매개 변수
   > * _,_&#x200B;과(와) 같은 모든 `Content-Type`required`X-Device-Info` 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **액세스 토큰 반환:** 클라이언트 토큰 끝점 응답에 수신된 매개 변수 및 헤더와 연결된 액세스 토큰에 대한 정보가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 액세스 토큰 응답에 제공된 정보에 대한 자세한 내용은 [액세스 토큰 검색](../apis/dynamic-client-registration-apis-retrieve-access-token.md#success) API 설명서를 참조하십시오.
   >
   > <br/>
   >
   > 클라이언트 토큰은 기본 조건이 충족되는지 확인하기 위해 요청 데이터의 유효성을 검사합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   >
   > <br/>
   >
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [액세스 토큰 검색](../apis/dynamic-client-registration-apis-retrieve-access-token.md#error) API 설명서를 준수하는 추가 정보가 제공됩니다.

   >[!TIP]
   >
   > 액세스 토큰은 지정된 기간(예: 24시간 TTL(Time-to-Live)) 내에서만 캐시되고 사용되어야 합니다. 만료되면 클라이언트 애플리케이션이 새 액세스 토큰을 요청해야 합니다.

1. **보호된 API 액세스 계속:** 클라이언트 응용 프로그램은 액세스 토큰을 사용하여 다른 Adobe Pass의 보호된 API에 액세스합니다. 클라이언트 응용 프로그램은 `Authorization` 인증 체계(예: `Bearer`)를 사용하여 `Authorization: Bearer <access_token>` 요청 헤더에 액세스 토큰을 포함해야 합니다.

   >[!IMPORTANT]
   >
   > Adobe Pass 보호 API는 액세스 토큰의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   >
   > * _access_token_&#x200B;은(는) 유효해야 합니다.
   > * _access_token_&#x200B;은(는) 올바른 _client_id_ 및 _client_secret_&#x200B;과(와) 연결되어 있어야 합니다.
   > * _access_token_&#x200B;은(는) 올바른 _software_statement_&#x200B;와 연결되어 있어야 합니다.
   >
   > <br/>
   >
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
