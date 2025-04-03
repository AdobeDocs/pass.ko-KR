---
title: 향상된 오류 코드
description: 향상된 오류 코드
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 27aaa0d3351577e60970a4035b02d814f0a17e2f
workflow-type: tm+mt
source-wordcount: '2649'
ht-degree: 3%

---

# 향상된 오류 코드 {#enhanced-error-codes}

>[!IMPORTANT]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

향상된 오류 코드는 다음과 통합된 클라이언트 애플리케이션에 추가 오류 정보를 제공하는 Adobe Pass 인증 기능을 나타냅니다.

* Adobe Pass 인증 REST API:
   * [REST API v2](../../rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [(기존) REST API v1](../../legacy/rest-api-v1/rest-api-overview.md)
* Adobe Pass 인증 SDK가 API 사전 권한을 부여합니다.
   * [(기존) JavaScript SDK(API 사전 인증)](../../legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
   * [(기존) iOS/tvOS SDK(API 사전 승인)](../../legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
   * [(기존) Android SDK(API 사전 인증)](../../legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)

  _(*) 사전 인증 API는 향상된 오류 코드를 지원하는 유일한 Adobe Pass 인증 SDK API입니다._

>[!IMPORTANT]
>
> Adobe Pass 인증 REST API v2를 통합하는 애플리케이션은 추가 구성 없이 기본적으로 향상된 오류 코드의 혜택을 받게 됩니다.
>
> <br/>
>
> Adobe Pass 인증 REST API v1 또는 SDK 사전 인증 API를 통합하는 애플리케이션은 기능이 명시적으로 활성화된 경우에만 향상된 오류 코드의 이점을 얻을 수 있습니다.
>
> <br/>
>
> 이 기능을 명시적으로 사용하려면 [Zendesk](https://adobeprimetime.zendesk.com)을(를) 통해 티켓을 만들고 기술 계정 관리자(TAM)에게 도움을 요청하십시오.

## 표시 {#enhanced-error-codes-representation}

향상된 오류 코드는 통합 Adobe Pass 인증 API 및 사용된 &quot;Accept&quot; 헤더 값(예: `application/json` 또는 `application/xml`)에 따라 `JSON` 또는 `XML` 형식으로 나타낼 수 있습니다.

| Adobe Pass 인증 API | JSON | XML |
|-------------------------------|---------|---------|
| REST API v2 | &amp;check; |         |
| REST API v1 | &amp;check; | &amp;check; |
| SDK 사전 인증 API | &amp;check; |         |

>[!IMPORTANT]
>
> Adobe Pass 인증은 다음과 같은 두 가지 형식으로 클라이언트 애플리케이션에 향상된 오류 코드를 전달할 수 있습니다.
>
> <br/>
>
> * **최상위 오류 정보**: 이 경우 ***&quot;error&quot;*** 개체가 최상위 수준에 있으므로 응답 본문에는 ***&quot;error&quot;*** 개체만 포함될 수 있습니다.
> * **항목 수준 오류 정보**: 이 경우 ***&quot;error&quot;*** 개체가 항목 수준에 있으므로 응답 본문에는 서비스 중에 오류가 발생한 모든 항목에 대한 ***&quot;error&quot;*** 개체가 포함될 수 있습니다.
>
> <br/>
>
> 각 통합 Adobe Pass 인증 API에 대한 공개 설명서를 확인하여 향상된 오류 코드 표시 세부 사항을 확인하십시오.

**REST API v2**

REST API v2에 적용 가능한 향상된 오류 코드 예 `JSON`이(가) 포함된 다음 HTTP 응답을 참조하십시오.

>[!BEGINTABS]

>[!TAB REST API v2 - 항목 수준 오류 정보(JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "decisions": [
    {
      "resource": "REF30",
      "serviceProvider": "REF30",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": true,
      "token": {
        "issuedAt": 1697094207324,
        "notBefore": 1697094207324,
        "notAfter": 1697094802367,
        "serializedToken": "PHNpZ25hdHVyZUluZm8..."
      }
    },
    {
      "resource": "REF40",
      "serviceProvider": "REF40",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": false,
      "error" : {
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v2 - 최상위 JSON(오류 정보)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "action": "none",
  "status": 400,
  "code": "invalid_parameter_service_provider",
  "message": "The service provider parameter value is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!ENDTABS]

**REST API v1**

REST API v1에 적용할 수 있는 향상된 오류 코드 예 `JSON` 또는 `XML`이(가) 포함된 다음 HTTP 응답을 참조하세요.

>[!BEGINTABS]

>[!TAB REST API v1 - 항목 수준 오류 정보(JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resources": [
    {
      "id": "TestStream1",
      "authorized": true
    },
    {
      "id": "TestStream2",
      "authorized": false,
      "error": {
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v1 - 최상위 JSON(오류 정보)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "8bcb17f9-b172-47d2-86d9-3eb146eba85e"
}
```

>[!TAB REST API v1 - 최상위 오류 정보(XML)]

```XML
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<error>
  <action>none</action>
  <status>400</status>
  <code>invalid_requestor</code>
  <message>The requestor parameter is missing or invalid.</message>
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!ENDTABS]

### 구조 {#enhanced-error-codes-representation-structure}

향상된 오류 코드에는 다음 `JSON` 필드 또는 예가 있는 `XML` 특성이 포함되어 있습니다.

| 이름 | 유형 | 예 | 제한됨 | 설명 |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *작업* | *문자열* | *없음* | &amp;check; | 이 문서에 정의된 대로 상황을 수정할 수 있는 Adobe Pass 인증 권장 작업입니다. <br/><br/> 자세한 내용은 [작업](#enhanced-error-codes-action) 섹션을 참조하세요. |
| *상태* | *정수* | *403* | &amp;check; | [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6) 문서에 정의된 HTTP 응답 상태 코드입니다. <br/><br/> 자세한 내용은 [상태](#enhanced-error-codes-status) 섹션을 참조하세요. |
| *코드* | *문자열* | *authorization_denied_by_mvpd* | &amp;check; | 이 문서에 정의된 대로 오류와 연결된 Adobe Pass 인증 고유 식별자 코드. <br/><br/> 자세한 내용은 [코드](#enhanced-error-codes-code) 섹션을 참조하세요. |
| *메시지* | *문자열* | *지정된 리소스에 대한 권한 부여를 요청할 때 MVPD에서 &quot;거부&quot; 결정을 반환했습니다.* |            | 사람이 인식할 수 있는 메시지로서, 경우에 따라 최종 사용자에게 표시될 수 있습니다. <br/><br/> 자세한 내용은 [응답 처리](#enhanced-error-codes-response-handling) 섹션을 참조하십시오. |
| *세부 정보* | *문자열* | *구독 패키지에 &quot;Live&quot; 채널이 포함되어 있지 않습니다* |            | 서비스 파트너가 제공할 수 있는 자세한 메시지(경우에 따라 <br/><br/>) 서비스 파트너가 사용자 지정 메시지를 제공하지 않는 경우 이 필드가 없을 수 있습니다. |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html* |            | 이 오류가 발생한 이유 및 가능한 해결 방법에 대한 자세한 정보로 연결되는 Adobe Pass 인증 공개 설명서 URL. <br/><br/> 이 필드에는 절대 URL이 있으며 다른 URL을 제공할 수 있는 오류 컨텍스트에 따라 오류 코드에서 유추해서는 안 됩니다. |
| *추적* | *문자열* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | 특정 문제를 해결하기 위해 Adobe Pass 인증 지원에 문의할 때 사용할 수 있는 응답의 고유 식별자입니다. |

>[!IMPORTANT]
>
> **Restricted** 열은 각 필드에 유한 집합의 값이 있는지 여부를 나타내며, 제한되지 않은 필드에는 데이터가 포함될 수 있습니다.
>
> <br/>
>
> 이 문서에 대한 향후 업데이트는 유한 집합에 값을 추가할 수 있지만 기존 집합을 제거하거나 변경하지 않습니다.

### 작업 {#enhanced-error-codes-representation-action}

향상된 오류 코드에는 상황을 수정할 수 있는 권장 작업을 제공하는 &quot;작업&quot; 필드가 포함되어 있습니다.

&quot;작업&quot; 필드에 사용할 수 있는 값은 다음과 같습니다.

| 작업 | 설명 | 범주 |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| 없음 | 이 문제를 해결하기 위한 사전 정의된 작업이 없지만, 경우에 따라 API의 부적절한 호출을 나타낼 수 있습니다. | 요청 컨텍스트를 수정합니다. |
| 구성 | 클라이언트 애플리케이션은 대부분의 경우 Adobe Pass TVE Dashboard를 통해 수행되는 구성 변경이 필요합니다. | 통합 구성 컨텍스트를 수정합니다. |
| application-registration | 클라이언트 응용 프로그램을 다시 등록해야 합니다. | 클라이언트 애플리케이션 컨텍스트를 수정합니다. |
| 인증 | 클라이언트 응용 프로그램에서는 사용자를 인증하거나 다시 인증해야 합니다. | 클라이언트 애플리케이션 컨텍스트를 수정합니다. |
| authorization | 클라이언트 응용 프로그램에서는 지정된 리소스에 대한 인증을 받아야 합니다. | 클라이언트 애플리케이션 컨텍스트를 수정합니다. |
| 다시 시도 | 클라이언트 응용 프로그램에서 요청을 다시 시도해야 합니다. | 요청 컨텍스트를 수정합니다. |

_(*) 일부 오류의 경우 여러 작업이 가능한 해결 방법일 수 있지만, &quot;작업&quot; 필드는 오류를 수정할 가능성이 가장 높은 작업을 나타냅니다._

### 상태 {#enhanced-error-codes-representation-status}

향상된 오류 코드에는 오류와 연관된 HTTP 상태 코드를 나타내는 &quot;status&quot; 필드가 포함되어 있습니다.

&quot;상태&quot; 필드에 사용할 수 있는 값은 다음과 같습니다.

| 코드 | Reason-Phrase |
|------|-----------------------|
| 400 | 잘못된 요청 |
| 401 | 승인되지 않음 |
| 403 | 금지됨 |
| 404 | 찾을 수 없음 |
| 405 | 메서드가 허용되지 않음 |
| 410 | 없어짐 |
| 412 | 사전 조건 실패 |
| 500 | 내부 서버 오류 |

4xx &quot;status&quot;가 있는 향상된 오류 코드는 일반적으로 클라이언트가 오류를 생성할 때 나타나며 대부분의 경우 클라이언트가 이를 해결하기 위해 추가 작업이 필요함을 나타냅니다.

5xx &quot;status&quot;가 있는 향상된 오류 코드는 일반적으로 서버에서 오류가 생성될 때 표시되며 대부분의 경우 서버에서 오류를 해결하기 위해 추가 작업이 필요하다는 의미입니다.

>[!IMPORTANT]
>
> HTTP 응답 상태 코드가 향상된 오류 코드 &quot;상태&quot; 필드와 다른 경우가 있습니다. 특히 향상된 오류 코드를 항목 수준 오류 정보로 전달하는 Adobe Pass 인증 API와 상호 작용할 때 그렇습니다.

### 코드 {#enhanced-error-codes-representation-code}

향상된 오류 코드에는 오류와 관련된 Adobe Pass 인증 고유 식별자를 제공하는 &quot;코드&quot; 필드가 포함됩니다.

&quot;code&quot; 필드에 대해 가능한 값은 통합된 Adobe Pass 인증 API를 기반으로 두 목록에서 [아래](#enhanced-error-codes-list)로 집계됩니다.

## 목록 {#enhanced-error-codes-lists}

### REST API v2 {#enhanced-error-codes-lists-rest-api-v2}

아래 표에는 Adobe Pass 인증 REST API v2와 통합할 때 클라이언트 애플리케이션에서 발생할 수 있는 향상된 오류 코드가 나와 있습니다.

| 작업 | 코드 | 상태 | 메시지 |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **없음** | *invalid_parameter_service_provider* | 400 | 서비스 공급자 매개 변수 값이 없거나 잘못되었습니다. |
|                              | *invalid_parameter_mvpd* | 400 | mvpd 매개 변수 값이 없거나 잘못되었습니다. |
|                              | *invalid_parameter_code* | 400 | 코드 매개 변수 값이 누락되었거나 잘못되었습니다. |
|                              | *invalid_parameter_resources* | 400 | 리소스 매개 변수 값이 누락되었거나 잘못되었습니다. |
|                              | *invalid_parameter_redirect_url* | 400 | 리디렉션 URL 매개 변수 값이 없거나 잘못되었습니다. |
|                              | *invalid_parameter_partner* | 400 | 파트너 매개 변수 값이 누락되었거나 잘못되었습니다. |
|                              | *invalid_parameter_saml_response* | 400 | SAML 응답 매개 변수 값이 없거나 잘못되었습니다. |
|                              | *invalid_header_device_info* | 400 | 디바이스 정보 헤더 값이 누락되었거나 잘못되었습니다. |
|                              | *invalid_header_device_identifier* | 400 | 장치 식별자 헤더 값이 누락되었거나 잘못되었습니다. |
|                              | *invalid_header_identity_for_temporary_access* | 400 | 임시 액세스 헤더 값에 대한 ID가 누락되었거나 잘못되었습니다. |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | 파트너 프레임워크 상태 헤더의 권한 액세스 상태 값이 없습니다. |
|                              | *invalid_header_pfs_permission_access_not_determined* | 400 | 파트너 프레임워크 상태 헤더의 권한 액세스 상태 값이 확인되지 않았습니다. |
|                              | *invalid_header_pfs_permission_access_not_granted* | 400 | 파트너 프레임워크 상태 헤더의 권한 액세스 상태 값이 부여되지 않았습니다. |
|                              | *invalid_header_pfs_provider_id_not_determined* | 400 | 파트너 프레임워크 상태 헤더의 공급자 ID 값이 알려진 mvpd와 연결되어 있지 않습니다. |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | 파트너 프레임워크 상태 헤더의 공급자 ID 값이 매개 변수로 전송된 mvpd와 일치하지 않습니다. |
|                              | *invalid_header_pfs_provider_info_expired* | 400 | 파트너 프레임워크 상태 헤더의 공급자 정보가 만료되었습니다. |
|                              | *invalid_integration* | 400 | 지정한 서비스 공급자와 mvpd 간의 통합이 없거나 비활성화되었습니다. |
|                              | *invalid_authentication_session* | 400 | 이 요청과 연결된 인증 세션이 없거나 잘못되었습니다. |
|                              | *preauthorization_denied_by_mvpd* | 403 | MVPD이 지정된 리소스에 대한 사전 승인을 요청할 때 &quot;거부&quot; 결정을 반환했습니다. |
|                              | *authorization_denied_by_mvpd* | 403 | MVPD이 지정된 리소스에 대한 권한 부여를 요청할 때 &quot;거부&quot; 결정을 반환했습니다. |
|                              | *authorization_denied_by_parental_controls* | 403 | MVPD이 지정된 리소스에 대한 자녀 보호 설정에 따라 &quot;거부&quot; 결정을 반환했습니다. |
|                              | *authorization_denied_by_degradation_rule* | 403 | 지정한 서비스 공급자와 mvpd 간의 통합에 요청된 리소스에 대한 인증을 거부하는 성능 저하 규칙이 적용되었습니다. |
|                              | *internal_server_error* | 500 | 내부 서버 오류로 인해 요청에 실패했습니다. |
| **구성** | *too_many_resources* | 403 | 너무 많은 리소스를 쿼리하여 권한 부여 또는 사전 권한 부여 요청에 실패했습니다. 권한 부여 및 사전 권한 부여 제한을 올바르게 구성하려면 지원 팀에 문의하십시오. |
|                              | *invalid_configuration_user_metadata_certificate* | 500 | 사용자 메타데이터 인증서 구성이 누락되었거나 잘못되었습니다. |
|                              | *invalid_configuration_temporary_access* | 500 | 임시 액세스 구성이 잘못되었습니다. |
|                              | *invalid_configuration_platform* | 500 | 플랫폼 구성이 누락되었거나 통합에 적합하지 않습니다. |
|                              | *invalid_configuration_platform_id* | 500 | 플랫폼 ID 구성이 누락되었거나 잘못되었습니다. |
|                              | *invalid_configuration_platform_trait* | 500 | 플랫폼 트레이트 구성이 누락되었거나 잘못되었습니다. |
|                              | *invalid_configuration_platform_category_trait* | 500 | 플랫폼 범주 트레이트 구성이 누락되었거나 잘못되었습니다. |
|                              | *invalid_configuration_platform_services* | 500 | 플랫폼 서비스 구성이 누락되었거나 통합에 적합하지 않습니다. |
|                              | *invalid_configuration_mvpd_platform* | 500 | mvpd 및 플랫폼에 대한 mvpd 플랫폼 구성이 없거나 잘못되었습니다. |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | mvpd 및 플랫폼에 대한 mvpd 플랫폼 탑승 상태 구성이 누락되었거나 잘못되었습니다. |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | mvpd 및 플랫폼에 대한 mvpd 플랫폼 프로필 교환 구성이 없거나 잘못되었습니다. |
| **application-registration** | *invalid_access_token_service_provider* | 401 | 잘못된 서비스 공급자로 인해 액세스 토큰이 잘못되었습니다. |
|                              | *invalid_access_token_client_application* | 401 | 잘못된 클라이언트 응용 프로그램으로 인해 액세스 토큰이 잘못되었습니다. |
| **인증** | *authenticated_profile_missing* | 403 | 이 요청과 연결된 인증된 프로필이 누락되었습니다. |
|                              | *authenticated_profile_expired* | 403 | 이 요청과 연결된 인증된 프로필이 만료되었습니다. |
|                              | *authenticated_profile_invalidated* | 403 | 이 요청과 연결된 인증된 프로필이 무효화되었습니다. |
|                              | *temporary_access_duration_limit_exceeded* | 403 | 임시 액세스 기간 제한이 초과되었습니다. |
|                              | *temporary_access_resources_limit_exceeded* | 403 | 임시 액세스 리소스 제한을 초과했습니다. |
|                              | *authorization_denied_by_hba_policies* | 403 | MVPD이 홈 기반 인증 정책으로 인해 &quot;거부&quot; 결정을 반환했습니다. 현재 인증은 홈 기반 인증 플로우를 통해 받았지만 지정된 리소스에 대한 권한 부여를 요청할 때 장치가 더 이상 홈에 있지 않습니다. 계속하려면 지원되는 MVPD으로 다시 인증해야 합니다. |
|                              | *authorization_denied_by_session_invalidated* | 403 | ID 공급자에 의해 인증 세션이 무효화되었습니다. 계속하려면 지원되는 MVPD으로 다시 인증해야 합니다. |
|                              | *identity_not_recognized_by_mvpd* | 403 | MVPD에서 사용자 ID를 인식하지 못하여 인증 요청에 실패했습니다. |
| **다시 시도** | *network_received_error* | 403 | 연결된 파트너 서비스에서 응답을 검색하는 도중 읽기 오류가 발생했습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다. |
|                              | *network_connection_timeout* | 403 | 연결된 파트너 서비스와의 연결 시간 제한이 초과되었습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다. |
|                              | *maximum_execution_time_exceeded* | 403 | 최대 허용 시간 내에 요청이 완료되지 않았습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다. |

### REST API v1 {#enhanced-error-codes-lists-rest-api-v1}

아래 표에는 Adobe Pass 인증 REST API v1과 통합할 때 클라이언트 애플리케이션에서 발생할 수 있는 향상된 오류 코드가 나와 있습니다.

| 작업 | 코드 | 상태 | 메시지 |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **없음** | *invalid_requestor* | 400 | 요청자 매개변수가 누락되었거나 잘못되었습니다. |
|                    | *invalid_device_info* | 400 | 디바이스 정보가 누락되었거나 잘못되었습니다. |
|                    | *invalid_device_id* | 400 | 디바이스 식별자가 없거나 잘못되었습니다. |
|                    | *missing_resource* | 400, 412 | 리소스 매개 변수가 누락되었습니다. |
|                    | *잘못된 형식_authz_request* | 400, 412 | 인증 요청이 Null이거나 잘못되었습니다. |
|                    | *preauthorization_denied_by_mvpd* | 403 | MVPD이 지정된 리소스에 대한 사전 승인을 요청할 때 &quot;거부&quot; 결정을 반환했습니다. |
|                    | *authorization_denied_by_mvpd* | 403 | MVPD이 지정된 리소스에 대한 권한 부여를 요청할 때 &quot;거부&quot; 결정을 반환했습니다. |
|                    | *authorization_denied_by_parental_controls* | 403 | MVPD이 지정된 리소스에 대한 자녀 보호 설정에 따라 &quot;거부&quot; 결정을 반환했습니다. |
|                    | *internal_error* | 400, 405, 500 | 내부 서버 오류로 인해 요청에 실패했습니다. |
| **구성** | *unknown_integration* | 400, 412 | 지정한 프로그래머와 ID 공급자 간의 통합이 없습니다. TVE 대시보드 를 사용하여 필요한 통합을 만듭니다. |
|                    | *too_many_resources* | 403 | 너무 많은 리소스를 쿼리하여 권한 부여 또는 사전 권한 부여 요청에 실패했습니다. 권한 부여 및 사전 권한 부여 제한을 올바르게 구성하려면 지원 팀에 문의하십시오. |
| **인증** | *authentication_session_issuer_mismatch* | 400 | 인증 흐름에 대해 표시된 MVPD이 인증 세션을 발급한 사용자와 다르기 때문에 인증 요청에 실패했습니다. 계속하려면 원하는 MVPD으로 다시 인증해야 합니다. |
|                    | *authorization_denied_by_hba_policies* | 403 | MVPD이 홈 기반 인증 정책으로 인해 &quot;거부&quot; 결정을 반환했습니다. 홈 기반 인증 흐름(HBA)을 사용하여 현재 인증을 받았지만 지정한 리소스에 대한 권한 부여를 요청할 때 장치가 더 이상 홈에 있지 않습니다. 계속하려면 지원되는 MVPD으로 다시 인증해야 합니다. |
|                    | *authorization_denied_by_session_invalidated* | 403 | ID 공급자에 의해 인증 세션이 무효화되었습니다. 계속하려면 지원되는 MVPD으로 다시 인증해야 합니다. |
|                    | *identity_not_recognized_by_mvpd* | 403 | MVPD에서 사용자 ID를 인식하지 못하여 인증 요청에 실패했습니다. |
|                    | *authentication_session_invalidated* | 403 | ID 공급자에 의해 인증 세션이 무효화되었습니다. 계속하려면 지원되는 MVPD으로 다시 인증해야 합니다. |
|                    | *authentication_session_missing* | 403,412 | 이 요청과 연결된 인증 세션을 검색할 수 없습니다. 계속하려면 지원되는 MVPD으로 다시 인증해야 합니다. |
|                    | *authentication_session_expired* | 403,412 | 현재 인증 세션이 만료되었습니다. 계속하려면 지원되는 MVPD으로 다시 인증해야 합니다. |
|                    | *preauthorization_authentication_session_missing* | 412 | 이 요청과 연결된 인증 세션을 검색할 수 없습니다. 계속하려면 지원되는 MVPD으로 다시 인증해야 합니다. |
|                    | *preauthorization_authentication_session_expired* | 412 | 현재 인증 세션이 만료되었습니다. 계속하려면 지원되는 MVPD으로 다시 인증해야 합니다. |
| **인증** | *authorization_not_found* | 403,404 | 지정한 리소스에 대한 권한 부여를 찾을 수 없습니다. 계속하려면 사용자가 새 인증을 받아야 합니다. |
|                    | *authorization_expired* | 410 | 지정한 리소스에 대한 이전 권한 부여가 만료되었습니다. 계속하려면 사용자가 새 인증을 받아야 합니다. |
| **다시 시도** | *network_received_error* | 403 | 연결된 파트너 서비스에서 응답을 검색하는 도중 읽기 오류가 발생했습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다. |
|                    | *network_connection_timeout* | 403 | 연결된 파트너 서비스와의 연결 시간 제한이 초과되었습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다. |
|                    | *maximum_execution_time_exceeded* | 403 | 최대 허용 시간 내에 요청이 완료되지 않았습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다. |

### SDK 사전 인증 API {#enhanced-error-codes-lists-sdks-preauthorize-api}

Adobe Pass 인증 SDK 사전 권한 부여 API와 통합할 때 클라이언트 응용 프로그램에 발생할 수 있는 향상된 오류 코드에 대해 이전 [섹션](#enhanced-error-codes-list-rest-api-v1)을 참조하세요.

## 응답 처리 {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> 네트워크 시간 초과 시 권한 부여 요청을 다시 시도하거나, 세션이 만료된 경우 사용자를 다시 인증해야 하는 것과 같이 클라이언트 애플리케이션 코드에서 자동으로 처리할 수 있는 향상된 오류 코드가 있지만, 다른 유형에는 구성 변경 또는 Adobe Pass 인증 고객 지원 팀 상호 작용이 필요할 수 있습니다.
>
> <br/>
>
> 따라서 [Zendesk](https://adobeprimetime.zendesk.com)를 통해 티켓을 만들 때 새 응용 프로그램이나 새 기능을 시작하기 전에 필요한 변경 사항을 적용할 수 있도록 전체 오류 정보를 수집하고 제공하는 것이 중요합니다.

요약하면 향상된 오류 코드가 포함된 응답을 처리할 때는 다음 사항을 고려해야 합니다.

1. **상태 값을 모두 확인**: 항상 HTTP 응답 상태 코드와 향상된 오류 코드 &quot;상태&quot; 필드를 모두 확인합니다. 둘 다 중요한 정보를 제공하므로 서로 다를 수 있습니다.

1. **최상위 오류 정보와 항목 수준 오류 정보 불가지론자**: 전달되는 방식에 따라 최상위 및 항목 수준 오류 정보를 불가지론자로 처리하고 향상된 오류 코드를 전송하는 두 가지 형식을 모두 처리할 수 있습니다.

1. **다시 시도 논리**: 다시 시도해야 하는 오류의 경우 서버를 압도하지 않도록 지수 백오프를 사용하여 다시 시도해야 합니다. 또한 여러 항목을 한 번에 처리하는 Adobe Pass 인증 API(예: 사전 인증 API)의 경우, 전체 목록이 아니라 &quot;다시 시도&quot;로 표시된 항목만 반복된 요청에 포함해야 합니다.

1. **구성 변경**: 구성을 변경해야 하는 오류의 경우 새 응용 프로그램이나 새 기능을 시작하기 전에 필요한 변경이 수행되었는지 확인하십시오.

1. **인증 및 권한 부여**: 인증 및 권한 부여와 관련된 오류의 경우 사용자에게 다시 인증하거나 필요에 따라 새 권한을 부여하도록 요청해야 합니다.

1. **사용자 피드백**: 사용자가 읽을 수 있는 &quot;메시지&quot; 및 (잠재적인) &quot;세부 정보&quot; 필드를 사용하여 사용자에게 문제에 대해 알립니다(선택 사항). &quot;세부 정보&quot; 텍스트 메시지는 MVPD 사전 인증 또는 권한 부여 종단점 또는 성능 저하 규칙을 적용할 때 프로그래머로부터 전달될 수 있습니다.
