---
title: Preflight 인증
description: Preflight 인증
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 0%

---

# Preflight 인증 {#preflight-authorization}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>

## 개요 {#overview}

이 기능은 여러 리소스에 대한 간단한 인증 검사를 제공합니다. 이 간단한 검사의 목적은 UI를 장식하는 것입니다(예: 잠금 및 잠금 해제 아이콘으로 액세스 상태를 표시). Preflight 인증은 가능한 가볍고 효율적이므로 단일 API 호출이 리소스 목록에 대한 인증 상태를 산출할 수 있습니다. 이 기능은 리소스 인증에 대한 권한이 없습니다.

재생을 허용하기 전에 `getAuthorization(resource)` 또는 `checkAuthorization(resource)` 호출을 수행해야 합니다.

Preflight 인증은 다른 사용 사례에 대한 지원도 제공하며, 이 경우 프로그래머는 하나의 미디어 콘텐츠 항목을 재생하기 위해 여러 리소스 ID에 대한 인증을 요청해야 합니다. 프로그래머는 필요한 리소스에 대한 초기 프리플라이트 검사를 수행할 수 있으며 응답에 따라 비즈니스 조건이 충족되지 않을 경우 조기에 실패할 수 있습니다.

Preflight 인증을 지원하는 MVPD 목록은 [MVPD Preflight 인증](/help/authentication/mvpd-preflight-authz.md#preflight_support_list) 페이지를 참조하십시오.

>[!NOTE]
>
> 전체 프리플라이트 권한 부여가 지원되지 않는 MVPD에 대한 이 기능의 사용은 Adobe 및 MVPD와 사전에 동의해야 합니다. 이러한 MVPD에서 Preflight 권한 부여를 사용하면 [여기](/help/authentication/mvpd-preflight-authz.md#intro)에서 설명한 &quot;최악의 시나리오&quot;에 해당되며 성능 문제가 발생하고 응답 시간이 느려질 수 있습니다. </br>
> 또한 **5개 이상의 리소스와 함께 Preflight 권한 부여의 사용은 Adobe**&#x200B;에 의해 명시적으로 동의해야 합니다.

## AccessEnabler Preflight API {#AE_pre_api}

AccessEnabler는 API/콜백 함수 쌍을 노출하여 실행 전 인증을 구현합니다. API 호출은 리소스 목록으로 구성된 단일 매개 변수를 사용합니다. 콜백 함수는 실제 승인된 리소스를 나타내는 단일 매개 변수를 사용합니다. 다음 예는 ActionScript 상태이지만 호출은 모든 AccessEnabler 버전에서 사용할 수 있습니다.

### checkPreauthorizedResources(Array:resources):void {#checkPreauthRes}

리소스 목록에 대한 인증 상태를 요청하려면 AccessEnabler 개체에서 이 함수를 호출합니다.

리소스 매개 변수는 권한 부여를 확인해야 하는 리소스 목록입니다. 목록의 각 요소는 리소스 ID를 나타내는 문자열이어야 합니다. 리소스 ID는 `getAuthorization()` 호출에서 리소스 ID와 동일한 제한을 받습니다. 즉, 프로그래머와 MVPD 간에 설정된 값 또는 미디어 RSS 조각에 동의됩니다. Adobe Pass 인증은 MVPD가 실제로 지원하는 내용에 따라 리소스 형식을 변환할 수 있는 얇은 중재 계층을 제외하고 어떤 방식으로도 리소스를 관리하지 않습니다.

### preauthorizedResources(배열:authorizedResources) {#preauthRes}

이것은 프로그래머의 상위 계층 응용 프로그램에서 구현해야 하는 콜백 함수입니다. AccessEnabler는 승인된 리소스 목록을 계산한 후 이 기능을 호출합니다.


### JS 예

```javascript
    checkPreauthorizedResources(["CNBC","MSNBC"]);
    ...
    preauthorizedResources() {
        var resource = arguments[0];
        for(i in resources) {
            // Do things with resource list
            // such as decorate UI
            ...
        }
    }
```

## 구현 정보 {#details}

- [ChannelID를 사용한 Preflight](#preflight_using_channelID)
- [POST/사전 승인](#post)
- [스토리지](#storage)

API 호출은 클라이언트의 로컬 스토리지에서 현재 사용자에 대해 캐시된 승인된 리소스 목록을 찾으려고 합니다. 캐시된 목록이 없으면 AdobePass 서버에 HTTPS 호출을 수행하여 목록을 검색합니다.

캐싱 메커니즘은 네트워크 호출을 모두 건너뛰어 후속 호출에서의 성능 시간을 향상시킵니다. 또한, 캐시된 목록은 인증 프로세스의 일부로 미리 채워질 수 있습니다.  (이 시나리오 설정에 대한 자세한 내용은 MVPD 통합 안내서의 권한 부여 섹션에서 [Preflight 권한 부여 통합](/help/authentication/authz-usecase.md#preflight_authz_int)을(를) 참조하십시오.)

또한 캐시된 리소스 목록이 있으면 `checkAuthorization()`에서 네트워크 호출을 수행하기 전에 이를 확인할 수 있으므로 캐시된 리소스 목록을 사용하여 권한 부여 흐름을 최적화할 수 있습니다. 리소스가 사전 승인된 리소스 목록에 없는 경우 Adobe Pass 인증 서버를 호출하지 않아도 검사가 실패할 수 있습니다.


### ChannelID를 사용한 Preflight {#preflight_using_channelID}

Adobe Pass 인증 2.4.1 릴리스부터 Preflight 흐름은 다음과 같이 작동합니다.

1. 인증 중에 Adobe Pass 인증은 MVPD의 SAML 응답에서 `channelIID` 요소를 읽고 이 값을 사용하여 인증 토큰에서 `authorizedResources` 요소를 설정합니다.
1. `checkPreauthorizedResources()` API 함수 내에서 Adobe Pass 인증은 `authorizedResources` 요소가 설정되어 있는지 확인합니다.
1. `authorizedResources` 요소가 설정되면 Adobe Pass 인증은 해당 값을 읽고 `authorizedResources` 요소의 리소스 목록과 `checkPreauthorizedResources()` 매개 변수에서 받은 리소스 목록 간에 교차를 수행합니다.  이 교차의 결과는 사전 승인된 리소스의 최종 목록입니다.
1. `authorizedResources` 요소가 설정되지 않은 경우 `checkPreauthorizedResources()` 매개 변수에서 받은 리소스 목록이 PreAuthorizationServlet으로 전달되는 이전에 구현된 흐름을 실행하십시오. 이 서블릿은 MVPD 끝점에 대한 권한 부여 호출을 수행하고 사전 권한 부여 리소스 목록을 반환합니다.

### ChannelID가 있는 Preflight의 예

아래 예제는 샘플 채널 라인업을 보여줍니다. 채널 목록을 전송하는 데 사용되는 특성의 이름은 MVPD와 다를 수 있습니다.

```XML
    <saml:Attribute Name="visible_channels" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
        <saml:AttributeValue xsi:type="xs:string">MSNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FBN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FNC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TNT</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TBS</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TRUTV</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TOON</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">HBO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">MAX</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">EPIXHD</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">BTN-BTN2GO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">SPEED-SPEED2</saml:AttributeValue>
    </saml:Attribute>
```


인증 요소의 `authorizedResources` 요소는 다음과 같이 표시됩니다.

```JSON
    <authorizedResources>
        <authorizedResource resourceID="MSNBC"/>
        <authorizedResource resourceID="CNBC"/>
        <authorizedResource resourceID="FBN"/>
        <authorizedResource resourceID="FNC"/>
        <authorizedResource resourceID="TNT"/>
        <authorizedResource resourceID="TBS"/>
        <authorizedResource resourceID="CNN"/>
        <authorizedResource resourceID="TRUTV"/>
        <authorizedResource resourceID="TOON"/>
        <authorizedResource resourceID="HBO"/>
        <authorizedResource resourceID="MAX"/>
        <authorizedResource resourceID="EPIXHD"/>
        <authorizedResource resourceID="BTN-BTN2GO"/>
        <authorizedResource resourceID="SPEED-SPEED2"/>
    </authorizedResources>
```

프로그래머는 `checkPreauthorizedResources()` API 호출을 실행하여 다음 매개 변수 목록을 전달합니다.</span>

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- TruTV
- &quot;fbc-fox&quot;

현재 Preflight 구현은 `authorizedResources` 요소의 리소스 목록과 교차를 수행하고 이 목록을 반환합니다.

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- TruTV



**참고:** 교차는 대/소문자를 구분하지 않습니다.



### POST/사전 승인 {#post}

캐시된 승인된 리소스 목록이 없으면 AccessEnabler에서 자동으로 이 호출을 수행합니다.


#### 요청 {#req}

| 매개 변수 | 유형 | 필수 | 설명 |
| --- | --- | --- | --- |
| `authentication_token` | 문자열 | 예 | 인증 토큰입니다. |
| `resource_id` | 문자열 | 예 | 단일 리소스. `checkPreauthorizedResources()` API 호출에 제공된 리소스 배열의 각 요소에 대해 한 번씩 여러 번 지정할 수 있습니다. |


**참고:** 요청한 리소스의 최대 개수를 구성할 수 있습니다.
**최대 기본값은 5입니다.**


#### 응답 {#resp}

사전 권한 부여 서블릿이 다시 보내는 응답은 다음 형식을 갖습니다.

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <resources>
      <resource>
        <id>TNT</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>TBS</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>CNN</id>
        <authorized>false</authorized>
      </resource>
    </resources>
```

### 스토리지 {#storage}

AccessEnabler가 Service Provider에서 가져오는 사전 승인된 리소스 목록입니다. 이 리소스 목록:

- AuthN 및 AuthZ 토큰과 함께 저장됩니다.
- 사용자가 동일한 웹 사이트에 있는 한 또는
AuthN 토큰 만료
- 사용자가 새 Adobe Pass에 도착할 때마다 다시 검색됩니다
인증 통합 웹 사이트

각 항목에는 사용자에게 사전 승인된 리소스 ID가 포함됩니다.

For example:


| 리소스 ID | 인증됨 |
| ----------- | ---------- |
| CNN | true |
| TNT | false |
| ... | ... |


이 목록의 이름은 &quot;사전 인증 캐시&quot;입니다.

#### 플로우 {#flow}

1. 프로그래머의 앱/사이트에서 `checkPreauthorizedResources(resourceList)` 호출을 실행합니다.
1. AccessEnabler는 인증된 리소스에 대한 인증 토큰을 확인합니다.
   1. 인증 토큰에 승인된 리소스가 포함된 경우 - 이 목록은 권한이 있으므로 이 정보를 가져오기 위해 호출해서는 안 됩니다. resourceList의 리소스는 인증 토큰의 승인된 리소스 목록 내에서 검색되며, 검색된 리소스만 `preauthorizedResources()` 콜백에 의해 반환됩니다.
   1. 인증 토큰에 권한이 부여된 리소스가 포함되지 않은 경우 `resourceList`이(가) 사전 권한 부여 캐시의 리소스 목록과 비교됩니다.
      1. 목록에 동일한 리소스가 포함된 경우 - 서버에 대한 호출이 이미 수행되었으며 응답이 이미 사전 인증 캐시에 있음을 의미합니다. 승인된 리소스만 `preauthorizedResources()` 콜백에서 반환됩니다.
      1. 목록에 동일한 리소스가 포함되지 않은 경우 - 클라이언트가 resourceList의 리소스에 대한 인증 상태를 얻기 위해 서버를 호출해야 합니다. 응답을 가져와서 사전 인증 캐시에 저장하면 이전 리소스가 완전히 교체됩니다. 승인된 리소스만 `preauthorizedResources()` 콜백에서 반환됩니다.


#### 목록 검색 {#listRetrieve}

`checkPreauthorizedResources()`이(가) 호출될 때마다 권한 부여를 확인하기 위한 리소스 목록이 사전 권한 부여 캐시에 대해 검사됩니다. 목록에 동일한 리소스 집합이 포함되어 있으면 `preauthorizedResources()` 콜백을 트리거하는 데 필요한 모든 리소스가 이미 캐시에 있으므로 서비스 공급자에 대한 호출이 수행되지 않습니다.


#### logout() {#logout}

로그아웃 시 사전 인증 캐시가 비워집니다.


## 종속성 {#depends}

Preflight API의 성능은 특정 MVPD 구현에 따라 달라집니다.  구현 옵션은 MVPD 통합 안내서의 권한 부여 섹션에서 [Preflight 권한 부여 통합 실행](/help/authentication/authz-usecase.md#preflight_authz_int)을 참조하십시오.


## 보안 {#security}

클라이언트 API는 모든 프로그래머가 사용할 수 있습니다.

이 구현에서는 HTTPS를 전송으로 사용하지만 더 가벼운 통화를 하기 위해 추가 보안 수단을 사용하지 않습니다(서명 없음, FAXS 없음).

**주의:** 이 API를 신뢰할 수 있는 방법으로 사용하여 사용자에게 보호된 리소스에 대한 액세스 권한이 부여되어야 하는지 여부를 결정하지 마십시오. 이 API의 목적은 UI 장식 및 / 또는 비즈니스 의사 결정을 선행하는 것입니다. 재생을 허용하기 전에 항상 `getAuthorization()` 및 `checkAuthorization()` 호출을 수행해야 합니다.


## 호환성 {#compat}

이 기능은 AS, JS, AIR, iOS, Android, Xbox(두 번째 화면 AuthN 흐름)와 같은 AccessEnabler의 모든 버전에서 지원됩니다.

Preflight 권한 부여는 CDATA 섹션이 포함된 사전 권한 부여 리소스를 지원하지 않습니다. 현재 프리플라이트 시스템의 초점은 채널 레벨 필터링을 지원하는 것이다. CDATA 섹션이 있는 리소스는 에셋 수준 리소스일 수 있습니다. Preflight는 CDATA가 포함되지 않는 한 채널 수준 사전 인증에 간단한 `mrss` 리소스를 지원합니다.

## 다른 기능과 통합 {#integ_w_other_features}

리소스가 사전 승인된 리소스 목록에 없는 경우 빠르게 실패하기 위해 인증 플로우에서 가능한 최적화가 발생할 수 있습니다.

이 최적화에서 서버 프리플라이트 끝점은 성능 저하 메커니즘과 통합됩니다.

MVPD 및 요청자에 대해 &quot;AuthN All&quot; 규칙이 있는 경우, Preflight 끝점은 요청에서 받은 모든 리소스를 간단하게 미러백합니다.

요청에 있는 리소스 중 하나 이상에 대해 &quot;AuthZ All&quot;이 활성화된 경우에도 마찬가지입니다.

<!--
## Related Information

  - `checkPreauthorizedResources()`
      - [iOS](#checkPreauth)
      - [Android](#checkPreauth)
      - [JavaScript](#checkPreauthRes)
      - [ActionScript](#checkPreauthRes)
  - [Xbox](#2nd%20Screen%20Authentication)
  - [MVPD Integration Guide: Preflight AuthZ](#)
-->
