---
title: 향상된 오류 코드
description: 향상된 오류 코드
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 87639ad93d8749ae7b1751cd13a099ccfc2636ac
workflow-type: tm+mt
source-wordcount: '2299'
ht-degree: 2%

---

# 향상된 오류 코드 {#enhanced-error-codes}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

이 문서에서는 애플리케이션에 다시 반환되는 API 오류 코드 목록 및 추가 오류 정보에 대해 설명합니다.

프로그래머 애플리케이션에서 향상된 오류 코드를 사용하려면 지원 팀에 구성 변경과 함께 활성화하도록 요청해야 합니다.

## 응답 오류 처리 {#response-error-handling}

대부분의 시나리오에서 Adobe Pass 인증 API는 다음을 제공하기 위해 응답 본문에 추가 오류 정보를 포함합니다 **의미 있는 컨텍스트** 특정 오류가 발생한 이유 및/또는 문제를 자동으로 해결할 수 있는 가능한 해결 방법  *그러나 인증 또는 로그아웃 흐름과 관련된 일부 특정 경우에 Adobe Pass 인증 서비스가 HTML 응답 또는 빈 본문을 반환할 수 있습니다. 자세한 내용은 API 설명서를 참조하십시오.*

특정 유형의 오류는 자동으로 처리할 수 있지만(예: 네트워크 시간 초과의 경우 권한 부여 요청을 다시 시도하거나, 세션이 만료된 경우 사용자를 다시 인증해야 함), 다른 유형에는 구성 변경 또는 고객 지원 팀의 상호 작용이 필요할 수 있습니다. 프로그래머는 이러한 경우 전체 오류 정보를 수집하고 제공하는 것이 중요합니다.

Adobe Pass 인증 API는 400-500 범위의 HTTP 상태 코드를 반환하여 오류 또는 오류를 나타냅니다. 각 HTTP 상태 코드에는 다음과 같은 특정 의미가 있습니다.

- 4xx 오류 코드는 오류가 클라이언트에 의해 생성되고 클라이언트가 이를 해결하기 위해 추가 작업을 수행해야 함을 의미합니다(예: 보호된 API를 호출하기 전에 액세스 토큰을 가져오거나 필요한 매개 변수를 제공)

- 5xx 오류 코드는 서버에서 오류가 발생하여 서버가 이를 수정하기 위해 추가 작업을 해야 함을 의미합니다.

추가 오류 정보는 응답 본문 내의 &quot;error&quot; 필드에 포함됩니다.

<table>
<thead>
  <tr>
    <th>이름</th>
    <th>유형</th>
    <th>예</th>
    <th>설명</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>오류</td>
    <td><i>오브젝트</i></td>
    <td><strong>JSON</strong>
    <br>
    <code>{<br>&nbsp;&nbsp;&nbsp;&nbsp;"status" : 403,<br>&nbsp;&nbsp;&nbsp;&nbsp;"code" : "network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"message" : "Unable to contact your TV provider<br>&nbsp;&nbsp;&nbsp;&nbsp;services",<br>&nbsp;&nbsp;&nbsp;&nbsp;"helpUrl" : "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;"trace" : "12f6fef9-d2e0-422b-a9d7-60d799abe353",<br>&nbsp;&nbsp;&nbsp;&nbsp;"action" : "retry"<br>}
    </code>
    <p>
    <p>
    <strong>XML</strong>
    <br>
    <code>&lt;error&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;status&gt;403&lt;/status&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;code&gt;network_connection_failure&lt;/code&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;message&gt;Unable to contact your TV provider services&lt;/message&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;helpUrl&gt;https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html&lt;/helpUrl&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;trace>12f6fef9-d2e0-422b-a9d7-60d799abe353&lt;/trace&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;action>retry&lt;/action&gt;<br>&lt;/error&gt;
    </code>
    </td>
    <td>요청을 완료하려고 시도하는 동안 수집된 컬렉션 또는 오류 개체를 참조합니다.</td>
  </tr>
</tbody>
</table>

</br>

여러 항목(사전 인증 API 등)을 처리하는 Adobe Pass API는 항목 수준 오류 정보를 사용하여 특정 항목에 대한 처리가 실패했는지, 다른 항목에 대해 처리가 성공했는지 여부를 나타낼 수 있습니다. 이 경우 ***&quot;error&quot;*** 개체는 항목 수준에 있으며 응답 본문에는 여러 개가 포함될 수 있습니다. ***&quot;오류&quot;*** 오브젝트 - API 설명서를 참조하십시오.

</br>

**부분 성공 및 항목 수준 오류가 있는 예**

```json
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream2",
            "authorized" : false,
            "error" : {
 
               "status" : 403,
               "code" : "network_connection_failure",
               "message" : "Unable to contact your TV provider services",
               "details" : "",
               "helpUrl" : "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
               "trace" : "8bcb17f9-b172-47d2-86d9-3eb146eba85e",
               "action" : "retry"
            }
 
        }
    ]
}
```

</br>

각 오류 개체에는 다음 매개 변수가 있습니다.

| 이름 | 유형 | 예 | 제한됨 | 설명 |
|---|---|----|:---:|---|
| *상태* | *정수* | *403* | &amp;check; | RFC 7231에 설명된 응답 HTTP 상태 코드(<https://tools.ietf.org/html/rfc7231#section-6>) <ul><li>400 잘못된 요청</li><li>401 승인되지 않음</li><li>403 금지</li><li>404 찾을 수 없음</li><li>405 메서드가 허용되지 않음</li><li>충돌</li><li>410 없어짐</li><li>412 사전 조건 오류</li><li>429 요청이 너무 많음</li><li>500 간격 서버 오류</li><li>503 서비스를 사용할 수 없음</li></ul> |
| *코드* | *문자열* | *network_connection_fail* | &amp;check; | 표준 Adobe Pass 인증 오류 코드. 전체 오류 코드 목록은 아래에 나와 있습니다. |
| *메시지* | *문자열* | *TV 공급자 서비스에 연결할 수 없습니다.* | | 최종 사용자에게 표시될 수 있는 사람이 읽을 수 있는 메시지. |
| *세부 사항* | *문자열* | *구독 패키지에 &quot;라이브&quot; 채널이 포함되지 않음* | | 일부 경우에, 상세 메시지는 MVPD 인증 종단점에 의해 또는 열화 규칙을 통해 프로그래머에 의해 제공된다. <p> 파트너 서비스에서 사용자 지정 메시지를 받지 못한 경우 이 필드가 오류 필드에 표시되지 않을 수 있습니다. |
| *helpUrl* | *url* | &quot;`http://`&quot; | | 이 오류가 발생한 이유 및 가능한 해결 방법에 대한 자세한 정보로 연결되는 URL입니다. <p>URI는 절대 URL을 나타내며 오류 코드에서 유추해서는 안 됩니다. 오류 컨텍스트에 따라 다른 URL을 제공할 수 있습니다. 예를 들어 동일한 bad_request 오류 코드는 인증 및 권한 부여 서비스에 대해 서로 다른 URL을 산출합니다. |
| *추적* | *문자열* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* | | 보다 복잡한 시나리오에서 특정 문제를 식별하기 위해 지원에 문의할 때 사용할 수 있는 이 응답에 대한 고유 식별자입니다. |
| *작업* | *문자열* | *다시 시도* | &amp;check; | 상황을 해결하기 위한 권장 조치: <ul><li> *없음* - 죄송합니다. 이 문제를 해결하기 위한 사전 정의된 작업이 없습니다. 이는 공용 API의 부적절한 호출을 나타낼 수 있습니다.</li><li>*구성* - TVE 대시보드를 통해 또는 지원 센터에 문의하여 구성을 변경해야 합니다. </li><li>*application-registration* - 응용 프로그램을 다시 등록해야 합니다. </li><li>*인증* - 사용자는 인증 또는 재인증해야 합니다. </li><li>*authorization* - 사용자는 특정 리소스에 대한 인증을 받아야 합니다. </li><li>*저하* - 일부 형식의 성능 저하를 적용해야 합니다. </li><li>*다시 시도* - 요청을 다시 시도하면 문제가 해결될 수 있음</li><li>*다음 시간 이후에 다시 시도* - 표시된 기간 후에 요청을 다시 시도하면 문제가 해결될 수 있습니다.</li></ul> |

</br>

**참고:**

- ***제한됨*** 열 *해당 필드 값이 유한 집합을 나타내는지 여부를 나타냅니다.* (예: 의 기존 HTTP 상태 코드)*상태*&quot; 필드). 이 사양에 대한 향후 업데이트는 제한된 목록에 값을 추가할 수 있지만 기존 값을 제거하거나 변경하지 않습니다. 제한되지 않은 필드에는 일반적으로 모든 데이터가 포함될 수 있지만 적절한 크기를 보장하기 위해 적용되는 제한이 있을 수 있습니다.

- 각 Adobe 응답에는 HTTP 서비스 전체에서 클라이언트 요청을 식별하는 &quot;Adobe-요청-ID&quot;가 포함됩니다. &quot;**추적**&quot;필드는 와 이 함께 보고되어야 함을 보완합니다.

## HTTP 상태 코드 및 오류 코드 {#http-status-codes-and-error-codes}

다양한 오류 코드와 관련 HTTP 상태 코드 간의 불일치는 이전 sdk 및 애플리케이션과의 이전 버전과의 호환성 요구 사항(예: *알 수 없음\_application* 이(가) 400개의 잘못된 요청을 생성하는 동안 *알 수 없는\_software\_statement* yirs 401 Unauthorized). 이러한 불일치를 해결하는 것은 향후 반복에 목표가 될 것이다.

## 작업 및 오류 코드 {#actions-and-error-codes}

대부분의 오류 코드의 경우 여러 작업을 즉시 문제 해결을 위한 경로로 사용할 수 있으며 자동으로 해결하기 위해 여러 작업이 필요할 수도 있습니다. 우리는 오류를 고칠 확률이 가장 높은 것을 선택했다. 다음 **작업** 다음 세 가지 범주로 나눌 수 있습니다.

1. 요청 컨텍스트를 수정하려고 시도하는 작업(다시 시도, 다음 후 다시 시도)
1. 애플리케이션 내에서 사용자 컨텍스트를 수정하려고 시도하는 것(애플리케이션 등록, 인증, 권한 부여)
1. 응용 프로그램과 id 공급자 간의 통합 컨텍스트를 수정하려고 하는 것입니다(구성, 성능 저하).

첫 번째 카테고리(다시 시도 및 다음 다시 시도)의 경우 동일한 요청을 다시 시도하면 문제를 해결하는 데 충분할 수 있습니다. 여러 항목을 처리하는 API의 경우, 애플리케이션은 요청을 반복하고 &quot;재시도&quot; 또는 &quot;재시도 후&quot; 작업이 있는 항목만 포함해야 합니다. 의 경우&#x200B;*다음 시간 이후에 다시 시도*&quot; 작업, &quot;<u>다음 시간 이후에 다시 시도</u>&quot;헤더는 요청을 반복하기 전에 애플리케이션이 대기해야 하는 시간(초)을 나타냅니다.

두 번째 및 세 번째 카테고리의 경우 실제 작업 구현은 애플리케이션 기능에 크게 의존합니다. 예, &quot;*저하*&quot;은 &quot;사용자가 컨텐츠를 재생할 수 있도록 15분 임시 패스로 전환&quot; 또는 &quot;지정된 MVPD와의 통합을 위해 AUTHN-ALL 또는 AUTHZ-ALL 저하를 적용하는 자동 도구&quot;로 구현할 수 있습니다. 와 유사&#x200B;*인증*&quot;액션은 태블릿에 대한 수동 인증(백 채널 인증)과 연결된 TV에 대한 전체 두 번째 화면 인증 흐름을 트리거할 수 있습니다. 그래서 스키마와 모든 매개 변수가 있는 완전한 URL을 제공하도록 선택했습니다.

## 오류 코드 {#error-codes}

아래 오류 표에는 가능한 오류 코드, 관련 메시지 및 가능한 작업이 나열되어 있습니다.

| 작업 | 오류 코드 | HTTP 상태 코드 | 설명 |
|---|---|---|---|
| **없음** | *authorization_denied_by_mvpd* | 403 | MVPD가 지정된 리소스에 대한 권한 부여를 요청할 때 &quot;거부&quot; 결정을 반환했습니다. |
|  | *authorization_denied_by_parental_controls* | 403 | MVPD가 지정된 리소스에 대한 자녀 보호 설정에 따라 &quot;거부&quot; 결정을 반환했습니다. |
|  | *authorization_denied_by_programmer* | 403 | 프로그래머가 적용한 열화 규칙은 현재 사용자에 대해 &quot;거부&quot; 결정을 시행합니다. |
|  | *bad_request* | 400 | API 요청이 잘못되었거나 형식이 잘못되었습니다. API 설명서를 검토하여 요청 요구 사항을 결정하십시오. |
|  | *개별화_서비스_사용할 수 없음* | 503 | 개별화 서비스를 사용할 수 없어 요청에 실패했습니다. |
|  | *internal_error* | 500 | 내부 서버 오류로 인해 요청에 실패했습니다. |
|  | *invalid_client_time* | 400 | 클라이언트 컴퓨터 날짜/시간/시간대가 올바르게 설정되지 않았습니다. 이로 인해 인증/권한 부여 오류가 발생할 수 있습니다. |
|  | *invalid_custom_scheme* | 400 | 응용 프로그램 등록에 사용된 지정된 사용자 지정 구성표를 인식할 수 없습니다. TVE 대시보드 구성에서 올바른 사용자 지정 구성표 값을 확인하십시오. |
|  | *invalid_domain* | 400 | 요청자가 잘못된 도메인을 사용하고 있습니다. 특정 요청자 ID에 사용되는 모든 도메인은 Adobe으로 허용 목록에 추가해야 합니다. |
|  | *invalid_header* | 400 | 잘못된 헤더가 포함되어 있어 요청에 실패했습니다. API 설명서를 검토하여 요청에 유효한 헤더와 해당 값에 대한 제한 사항이 있는지 확인합니다. |
|  | *invalid_http_method* | 405 | 요청과 연결된 HTTP 메서드가 지원되지 않습니다. API 설명서를 검토하여 요청에 대해 지원되는 HTTP 메서드를 결정하십시오. |
|  | *invalid_parameter_value* | 400 | 잘못된 매개 변수 또는 매개 변수 값이 포함되어 있어 요청에 실패했습니다. API 설명서를 검토하여 요청에 유효한 매개 변수와 해당 값에 대한 제한 사항이 있는지 확인합니다. |
|  | *invalid_resource_value* | 400 | 잘못된 리소스 또는 잘못된 리소스가 사용되었기 때문에 요청에 실패했습니다. API 설명서를 검토하여 요청에 대해 복잡한 리소스를 인코딩해야 하는 방법과 해당 값 및/또는 크기에 대한 제한 사항이 있는지 확인합니다. |
|  | *invalid_registration_code* | 404 | 지정한 등록 코드가 더 이상 유효하지 않거나 만료되었습니다. |
|  | *invalid_service_configuration* | 500 | 잘못된 서비스 구성으로 인해 요청에 실패했습니다. |
|  | *missing_authentication_header* | 400 | 특정 API에 대한 필수 인증 헤더가 포함되어 있지 않아 요청에 실패했습니다. |
|  | *missing_resource_mapping* | 400 | 지정한 리소스에 해당하는 매핑이 없습니다. 필요한 매핑을 수정하려면 지원 팀에 문의하십시오. |
|  | *preauthorization_denied_by_mvpd* | 403 | 지정된 리소스에 대한 사전 승인을 요청할 때 MVPD가 &quot;거부&quot; 결정을 반환했습니다. |
|  | *preauthorization_denied_by_programmer* | 403 | 프로그래머가 적용한 열화 규칙은 현재 사용자에 대해 &quot;거부&quot; 결정을 적용합니다. |
|  | *registration_code_service_unavailable* | 503 | 등록 코드 서비스를 사용할 수 없기 때문에 요청에 실패했습니다. |
|  | *service_unavailable* | 503 | 인증 또는 권한 부여 서비스를 사용할 수 없기 때문에 요청에 실패했습니다. |
|  | *access_token_unavailable* | 400 | 액세스 토큰을 검색하는 동안 예기치 않은 오류가 발생하여 요청에 실패했습니다. TVE 대시보드 구성에서 사용 가능한 소프트웨어 구문 및 등록된 사용자 지정 체계를 확인하십시오. |
|  | *unsupported_client_version* | 400 | 이 버전의 Adobe Pass 인증 SDK는 너무 오래되었으며 더 이상 지원되지 않습니다. API 설명서에서 최신 버전으로 업그레이드하는 데 필요한 단계를 확인하십시오. |
| **구성** | *network_required_ssl* | 403 | Target 파트너 서비스에 SSL 연결 문제가 있습니다. 지원 팀에 문의하십시오. |
|  | *too_many_resource* | 403 | 너무 많은 리소스를 쿼리하여 권한 부여 또는 사전 권한 부여 요청에 실패했습니다. 권한 부여 및 사전 권한 부여 제한을 올바르게 구성하려면 지원 팀에 문의하십시오. |
|  | *unknown_programmer* | 400 | 프로그래머 또는 서비스 공급자를 인식할 수 없습니다. TVE 대시보드를 사용하여 지정된 프로그래머를 등록합니다. |
|  | *unknown_application* | 400 | 애플리케이션을 인식할 수 없습니다. TVE 대시보드를 사용하여 지정된 애플리케이션을 등록합니다. |
|  | *unknown_integration* | 400 | 지정한 프로그래머와 ID 공급자 간의 통합이 없습니다. TVE 대시보드 를 사용하여 필요한 통합을 만듭니다. |
|  | *unknown_software_statement* | 401 | 액세스 토큰과 연결된 소프트웨어 문을 인식할 수 없습니다. 소프트웨어 문의 상태를 명확히 하려면 지원 팀에 문의하십시오. |
| **application-registration** | *access_token_expired* | 401 | 액세스 토큰이 만료되었습니다. API 설명서에 표시된 대로 애플리케이션에서 액세스 토큰을 새로 고쳐야 합니다. |
|  | *invalid_access_token_signature* | 401 | 액세스 토큰 서명이 더 이상 유효하지 않습니다. API 설명서에 표시된 대로 애플리케이션에서 액세스 토큰을 새로 고쳐야 합니다. |
|  | *invalid_client_id* | 401 | 연결된 클라이언트 식별자를 인식할 수 없습니다. 애플리케이션은 API 설명서에 명시된 애플리케이션 등록 프로세스를 따라야 합니다. |
| **인증** | *authentication_session_expired* | 410 | 현재 인증 세션이 만료되었습니다. 계속하려면 지원되는 MVPD로 다시 인증해야 합니다. |
|  | *authentication_session_missing* | 401 | 이 요청과 연결된 인증 세션을 검색할 수 없습니다. 계속하려면 지원되는 MVPD로 다시 인증해야 합니다. |
|  | *authentication_session_invalidated* | 401 | ID 공급자에 의해 인증 세션이 무효화되었습니다. 계속하려면 지원되는 MVPD로 다시 인증해야 합니다. |
|  | *authentication_session_issuer_mismatch* | 400 | 인증 플로우에 대해 표시된 MVPD가 인증 세션을 발행한 MVPD와 다르므로 인증 요청이 실패했습니다. 계속하려면 원하는 MVPD로 다시 인증해야 합니다. |
|  | *authorization_denied_by_hba_policies* | 403 | MVPD가 홈 기반 인증 정책으로 인해 &quot;거부&quot; 결정을 반환했습니다. 홈 기반 인증 흐름(HBA)을 사용하여 현재 인증을 받았지만 지정한 리소스에 대한 권한 부여를 요청할 때 장치가 더 이상 홈에 있지 않습니다. 계속하려면 지원되는 MVPD로 다시 인증해야 합니다. |
|  | *identity_not_recognized_by_mvpd* | 403 | MVPD에서 사용자 ID를 인식하지 못하여 인증 요청이 실패했습니다. |
| **authorization** | *authorization_expired* | 410 | 지정한 리소스에 대한 이전 권한 부여가 만료되었습니다. 계속하려면 사용자가 새 인증을 받아야 합니다. |
|  | *authorization_not_found* | 404 | 지정한 리소스에 대한 권한 부여를 찾을 수 없습니다. 계속하려면 사용자가 새 인증을 받아야 합니다. |
|  | *device_identifier_mismatch* | 403 | 지정한 장치 식별자가 인증 장치 ID와 일치하지 않습니다. 계속하려면 사용자가 새 인증을 받아야 합니다. |
| **다시 시도** | *network_connection_fail* | 403 | 연결된 파트너 서비스에 연결 오류가 발생했습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다. |
|  | *network_connection_time* | 403 | 연결된 파트너 서비스와의 연결 시간 제한이 초과되었습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다. |
|  | *network_received_error* | 403 | 연결된 파트너 서비스에서 응답을 검색하는 도중 읽기 오류가 발생했습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다. |
|  | *maximum_execution_time_exceeded* | 403 | 최대 허용 시간 내에 요청이 완료되지 않았습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다. |
| **다음 시간 이후에 다시 시도** | *too_many_request* | 429 | 지정된 간격 내에 너무 많은 요청이 전송되었습니다. 애플리케이션은 제안된 기간 후에 요청을 재시도할 수 있습니다. |
|  | *user_rate_limit_exceeded* | 429 | 주어진 간격 내에 특정 사용자가 너무 많은 요청을 발행했습니다. 애플리케이션은 제안된 기간 후에 요청을 재시도할 수 있습니다. |
