---
title: 사전 승인
description: JavaScript 사전 권한 부여
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1466'
ht-degree: 0%

---

# (기존) 사전 인증 {#js-preauthorize}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#preauth-overview}

사전 승인 API 메서드는 애플리케이션에서 하나 이상의 리소스에 대한 사전 승인 결정을 가져오는 데 사용됩니다. API 사전 인증 요청은 UI 힌트 및/또는 콘텐츠 필터링에 사용해야 합니다. 지정된 리소스에 대한 사용자 액세스를 허용하기 전에 실제 인증 API 요청을 수행해야 합니다.

Adobe Pass 인증 서비스에서 사전 승인 API 요청을 처리할 때 예기치 않은 오류(예: 네트워크 문제 및 MVPD 인증 끝점을 사용할 수 없음)가 발생하는 경우, 영향을 받는 리소스에 대한 하나 이상의 개별 오류 정보가 사전 승인 API 응답 결과의 일부로 포함됩니다.

### public preauthorize(request: PreauthorizeRequest, callback: AccessEnablerCallback&lt;any>): void {#preauth-method}

**설명:** 이 메서드는 응용 프로그램의 UI를 장식하기 위한 기본 목적(예: 잠금 및 잠금 해제 아이콘으로 액세스 상태 표시)으로, 응용 프로그램에서 인증된 사용자의 사전 권한 부여(정보 제공) 결정을 얻기 위해 Adobe Pass Authentication Service로부터 특정 보호된 리소스를 보는 데 사용됩니다.

**가용성:** v4.4.0+

**매개 변수:**

* `PreauthorizeRequest`: 요청을 정의하는 데 사용되는 빌더 개체
* `AccessEnablerCallback`: API 응답을 반환하는 데 사용되는 콜백
* `PreauthorizeResponse`: API 응답 콘텐츠를 반환하는 데 사용되는 개체

### 클래스 PreauthorizeRequestBuilder {#preath-req-builder-class}

#### setResources(리소스: 문자열[]): PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* 사전 권한 부여 결정을 받을 리소스 목록을 설정합니다.
* 사전 권한 부여 API 사용에 대해 설정해야 합니다.
* 목록의 각 요소는 MVPD과 동의해야 하는 리소스 ID 값 또는 미디어 RSS 조각을 나타내는 문자열이어야 합니다.
* 이 메서드는 이 메서드 호출의 수신자인 현재 `PreauthorizeRequestBuilder` 개체 인스턴스의 컨텍스트에서만 정보를 설정합니다.

* 실제 `PreauthorizeRequest`을(를) 만들려면 `PreauthorizeRequestBuilder`의 메서드를 볼 수 있습니다.

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}`개 리소스. 사전 권한 부여 결정을 받을 리소스 목록입니다.
* `@returns {PreauthorizeRequestBuilder}` 메서드 호출의 수신자인 동일한 `PreauthorizeRequestBuilder` 개체 인스턴스에 대한 참조입니다.
* 이렇게 하면 메서드 체인을 만들 수 있습니다.

#### disableFeatures(...features: string[]): PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* 사전 인증 결정을 받을 때 비활성화할 기능을 설정합니다.
* 이 함수는 이 함수 호출의 수신자인 현재 `PreauthorizeRequestBuilder` 개체 인스턴스의 컨텍스트에서만 정보를 설정합니다.
* 실제 `PreauthorizeRequest`을(를) 작성하려면 `PreauthorizeRequestBuilder`의 함수를 확인합니다.

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}`개 기능. 비활성화하려는 기능 세트입니다.
* `@returns` 함수 호출의 수신자인 동일한 `PreauthorizeRequestBuilder` 개체 인스턴스에 대한 참조입니다.
* 함수 체인을 만들 수 있도록 이 작업을 수행합니다.

#### build(): PreauthorizeRequest {#preauth-req}

* 새 `PreauthorizeRequest` 개체 인스턴스의 참조를 만들고 검색합니다.
* 이 메서드는 호출할 때마다 새 `PreauthorizeRequest` 개체를 인스턴스화합니다.
* 이 메서드는 이 메서드 호출의 수신자인 현재 `PreauthorizeRequestBuilder` 개체 인스턴스의 컨텍스트에서 미리 설정된 값을 사용합니다.
* 이 방법은 어떤 부작용도 일으키지 않는다는 것을 명심해라.
* 따라서 SDK의 상태나 이 메서드 호출의 수신자인 `PreauthorizeRequestBuilder` 개체 인스턴스의 상태는 변경되지 않습니다.
* 즉, 동일한 수신자에 대해 이 메서드를 연속해서 호출하면 서로 다른 새 `PreauthorizeRequest` 개체 인스턴스가 만들어지지만, 호출 간에 수정되지 않는 `PreauthorizeRequestBuilder`(으)로 값이 설정된 경우 동일한 정보를 갖게 됩니다.
* 제공된 정보(리소스 및 캐싱)를 업데이트할 필요가 없는 경우, PreauthorizeRequest 인스턴스를 사전 승인 API의 여러 용도로 다시 사용할 수 있습니다.
* `@returns {PreauthorizeRequest}`

### 인터페이스 AccessEnablerCallback&lt;T> {#interface-access-enablr-callback}

#### onResponse(결과: T); {#on-response-result}

* API 사전 인증 요청이 이행되었을 때 SDK에서 호출한 응답 콜백입니다.
* 결과는 성공 또는 상태가 포함된 오류 결과입니다.
* `@param {T} result`

#### onFailure(결과: T); {#on-failure-result}

* API 사전 권한 부여 요청을 처리할 수 없을 때 SDK에서 호출한 실패 콜백입니다.
* 그 결과 상태가 포함된 실패 결과가 됩니다.
* `@param {T} result`

### 클래스 PreauthorizeResponse {#preauth-response-class}

#### 공개 상태: 상태; {#public-status}

* 반환: 실패 시 추가 상태(상태) 정보입니다.
* `null` 값을 보유할 수 있습니다.

#### 공용 결정: 결정[]; {#public-decisions}

* 반환: 사전 권한 부여 결정 목록입니다. 각 리소스에 대해 하나의 의사 결정.
* 실패 시 목록이 비어 있을 수 있습니다.

### 클래스 상태 {#class-status}

#### 공개 상태: 숫자; {#public-status-numbr}

* RFC 7231에 설명된 HTTP 응답 상태 코드입니다.
* `Status`이(가) Adobe Pass 인증 서비스 대신 SDK에서 온 경우 0일 수 있습니다.

#### 공개 코드: 숫자; {#public-code-numbr}

* 표준 Adobe Pass 인증 서비스 오류 코드.
* 빈 문자열 또는 `null` 값을 포함할 수 있습니다.

#### 공개 메시지: 문자열; {#public-msg-string}

* 경우에 따라 MVPD 인증 종단점 또는 프로그래머 열화 규칙에서 제공하는 자세한 메시지입니다.
* 빈 문자열 또는 `null` 값을 포함할 수 있습니다.

#### 공개 세부 정보: 문자열; {#public-details-strng}

* 일부 경우에 MVPD 인증 종단점 또는 프로그래머 열화 규칙에서 제공하는 자세한 메시지를 보유합니다.
* 빈 문자열 또는 `null` 값을 포함할 수 있습니다.


#### public helpUrl: 문자열; {#public-help-url-string}

* 이 상태/오류가 발생한 이유 및 가능한 해결 방법에 대한 자세한 정보로 연결되는 URL입니다.
* 빈 문자열 또는 `null` 값을 포함할 수 있습니다.

#### 공개 추적: 문자열; {#public-trace-string}

* 이 응답에 대한 고유 식별자로, 보다 복잡한 시나리오에서 특정 문제를 식별하기 위해 지원에 문의할 때 사용할 수 있습니다.
* 빈 문자열 또는 `null` 값을 포함할 수 있습니다.

#### 공개 작업: 문자열; {#public-action-string}

* 상황을 해결하기 위한 권장 조치입니다.
   * **없음**: 이 문제를 해결하기 위한 사전 정의된 작업이 없습니다. 이는 공용 API의 부적절한 호출을 나타낼 수 있습니다.
   * **구성**: TVE 대시보드를 통해 또는 지원 팀에 연락하여 구성을 변경해야 합니다.
   * **application-registration**: 응용 프로그램을 다시 등록해야 합니다.
   * **인증**: 사용자는 인증하거나 다시 인증해야 합니다.
   * **인증**: 사용자는 특정 리소스에 대한 인증을 받아야 합니다.
   * **저하**: 일부 형식의 저하를 적용해야 합니다.
   * **다시 시도**: 요청을 다시 시도하면 문제가 해결될 수 있습니다.
   * **다음 시간 후에 다시 시도**: 표시된 시간 후에 요청을 다시 시도하면 문제가 해결될 수 있습니다.
* 빈 문자열 또는 `null` 값을 포함할 수 있습니다.

### 클래스 결정 {#class-decision}

#### 공개 id: 문자열; {#public-id-string}

* 결정을 얻은 리소스 ID입니다.

#### public authorized: 부울; {#public-auth-boolean}

* 결정이 성공했는지 여부를 나타내는 플래그 값.

#### 공개 오류: 상태; {#public-error-status}

* 일부 오류가 발생한 경우 추가 상태(상태) 정보입니다. `null` 값을 보유할 수 있습니다.

## 클라이언트 구현 예 {#client-imp-example}

```JavaScript
let accessEnablerApi = new window.AccessEnabler.AccessEnabler("software statement");
let accessEnablerModels = window.AccessEnabler.models;



// Build request
let requestBuilder = new accessEnablerModels.PreauthorizeRequest.getBuilder();
let request = requestBuilder
    .setResources(["RES01", "RES02", "RES03"])
    .disableFeatures("LOCAL_CACHE")
    .build();



// Create callback
let callback = {
    onResponse(response) {
        // Handle onResponse
    },
    onFailure(response) {
        // Handle onFailure
    }
};

// Invoke call
accessEnablerApi.preauthorize(request, callback);
```


## 시나리오 예 {#scenario-examples}

### 시나리오 1: 요청한 모든 리소스가 승인되었습니다. {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>향상된 오류 코드 플래그</th>
    <th>응답</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>비활성화됨</td>
    <td>

```JavaScript
        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": true
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }    
```

</td>
  </tr>
</tbody>


### 시나리오 2: 일부 요청된 리소스가 승인되었습니다. {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>향상된 오류 코드 플래그</th>
    <th>응답</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>비활성화됨</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: true
    }
    ]
    }
    
    &quot;

</td>
  </tr>

<tr>
    <td>활성화됨</td>
    <td>

    &quot;JavaScript
    {
    &quot;decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;MVPD에서 지정된 리소스에 대한 사전 인증을 요청할 때 \&quot;Deny\&quot; 결정을 반환했습니다.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    ,
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: true
    },
    ]
    
    
    &quot;

</td>
  </tr>
</tbody>


### 시나리오 3: 요청된 리소스 중 어느 것도 승인되지 않았습니다. {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>향상된 오류 코드 플래그</th>
    <th>응답</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>비활성화됨</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: false14}}
    ]
    }
    
    &quot;

    
</td>
  </tr>

<tr>
    <td>활성화됨</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;MVPD에서 지정된 리소스에 대한 사전 인증을 요청할 때 \&quot;Deny\&quot; 결정을 반환했습니다.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    ,
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;MVPD에서 지정된 리소스에 대한 사전 인증을 요청할 때 \&quot;Deny\&quot; 결정을 반환했습니다.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    
    ,
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code: &quot;maximum_execution_time_exceeded&quot;,
    &quot;message&quot;: &quot;최대 허용 시간에 요청이 완료되지 않았습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    }
    
    ]
    
    
    &quot;

</td>
  </tr>
</tbody>


### 시나리오 4: 잘못된 클라이언트 요청 - 지정된 리소스가 없습니다. {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>향상된 오류 코드 플래그</th>
    <th>응답</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>사용 안 함/사용</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 400,
    &quot;code&quot;: &quot;internal_error&quot;,
    &quot;message&quot;: &quot;내부 오류로 인해 요청이 실패했습니다.&quot;,
    &quot;details&quot;: &quot;필수 문자열[] 매개 변수 &#39;resource&#39;가 없습니다.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;decisions&quot;: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### 시나리오 5: 잘못된 클라이언트 요청 - 빈 리소스가 지정되었습니다. {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>향상된 오류 코드 플래그</th>
    <th>응답</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>사용 안 함/사용</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 412,
    &quot;code&quot;: &quot;missing_resource&quot;,
    &quot;message&quot;: &quot;리소스 매개 변수가 누락되었습니다&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;decisions&quot;: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### 시나리오 6: 네트워크 오류입니다. {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>향상된 오류 코드 플래그</th>
    <th>응답</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>활성화됨</td>
    <td>

    &quot;JavaScript
    {
    &quot;decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;연결된 파트너 서비스에서 응답을 검색하는 동안 읽기 오류가 발생했습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;연결된 파트너 서비스에서 응답을 검색하는 동안 읽기 오류가 발생했습니다. 요청을 다시 시도하면 문제가 해결될 수 있습니다.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    }
    
    ]
    
    &quot;

</td>
  </tr>
</tbody>
</table>

### 시나리오 7: 유효한 AuthN 세션 없이 흐름 사전 권한 부여가 호출되었습니다.

<table>
<thead>
  <tr>
    <th>향상된 오류 코드 플래그</th>
    <th>응답</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>사용 안 함/사용</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;authentication_session_missing&quot;,
    &quot;message&quot;: &quot;이 요청과 연결된 인증 세션을 검색할 수 없습니다. 계속하려면 사용자가 지원되는 MVPD을 사용하여 다시 인증해야 합니다.&quot;,
    &quot;action&quot;: &quot;authentication&quot;
    },
    &quot;decisions&quot;: []
    }
    
    &quot;

</td>
  </tr>
</tbody>
</table>



### 시나리오 8: setRequestor 호출이 완료되기 전에 흐름 사전 권한 부여가 호출되었습니다.

<table>
<thead>
  <tr>
    <th>향상된 오류 코드 플래그</th>
    <th>응답</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>사용 안 함/사용</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;requestor_not_configured&quot;,
    &quot;message&quot;: &quot;요청자가 아직 구성되지 않았으며, 이는 setRequestor API와 다른 API를 사용하기 위한 필수 조건입니다.&quot;,
    &quot;action&quot;: &quot;retry&quot;
    },
    &quot;decisions&quot;: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>
