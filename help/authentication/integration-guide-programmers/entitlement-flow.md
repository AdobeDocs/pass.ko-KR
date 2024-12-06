---
title: 프로그래머 권한 흐름
description: 프로그래머 권한 흐름
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1823'
ht-degree: 0%

---

# 프로그래머 권한 흐름 {#prog-entitlement-flow}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

이 문서에서는 프로그래머의 관점에서 기본 자격 흐름에 대해 설명합니다.  기본 TVE 통합 이외의 기능 및 사용 사례에 대한 자세한 내용은 [프로그래머 사용 사례](/help/authentication/integration-guide-programmers/programmer-use-cases.md)를 참조하십시오.

Adobe Pass 인증은 양측에 안전하고 일관된 인터페이스를 제공하여 프로그래머와 MVPD 간의 권한 흐름을 중재합니다.  프로그래머 측에서는 Adobe Pass 인증이 두 가지 일반적인 유형의 권한 인터페이스를 제공합니다.

1. AccessEnabler - 웹 페이지를 렌더링할 수 있는 디바이스(예: 웹 앱, 스마트폰/태블릿 앱)의 앱에 대한 API 라이브러리를 제공하는 클라이언트 구성 요소입니다.
2. Clientless API - 웹 페이지를 렌더링할 수 없는 장치(예: 셋톱 박스, 게임 콘솔, 스마트 TV)에 대한 RESTful 웹 서비스. 웹 페이지를 렌더링하기 위한 요구사항은 사용자가 MVPD의 웹 사이트에서 인증하도록 하는 MVPD의 요구사항에서 비롯됩니다.

여기에 제시된 플랫폼 중립적 개요 외에도, Clientless API 설명서인 Clientless API별 개요가 있습니다. AccessEnabler는 기본적으로 지원되는 플랫폼(웹의 AS / JS, iOS의 Objective-C 및 Android의 Java)에서 실행됩니다. AccessEnabler API는 지원되는 플랫폼에서 일관성을 유지합니다. AccessEnabler를 지원하지 않는 모든 플랫폼은 동일한 Clientless API를 사용합니다.

두 유형의 인터페이스에서 Adobe Pass 인증은 프로그래머 앱과 사용자의 MVPD 간의 권한 흐름을 안전하게 중재합니다.

![](../assets/prog-entitlement-flow.png)


*그림: Adobe Pass 인증 환경*

>[!IMPORTANT]
>
>위의 다이어그램에는 Adobe Pass 인증 서버를 거치지 않는 자격 흐름의 한 부분인 MVPD 로그인이 있습니다. 사용자는 MVPD의 로그인 페이지에 로그온해야 합니다. 이 요구 사항 때문에 웹 페이지를 렌더링할 수 없는 장치에서 프로그래머 앱은 사용자가 MVPD로 로그인할 수 있는 웹 가능 장치로 전환하도록 지시한 후 나머지 권한 흐름 동안 원래 장치로 돌아가야 합니다.

## 권한 흐름 {#entitlement-flow}

기본 권한을 구성하는 4개의 개별 하위 흐름이 있습니다
흐름:

1. [시작 흐름](/help/authentication/integration-guide-programmers/entitlement-flow.md#startup)
1. [인증 흐름](/help/authentication/integration-guide-programmers/entitlement-flow.md#authentication)
1. [인증 흐름](/help/authentication/integration-guide-programmers/entitlement-flow.md#authorization)
1. [로그아웃](/help/authentication/integration-guide-programmers/entitlement-flow.md#logout)

사용자가 프로그래머 사이트를 처음 방문할 때 권한 흐름이 위의 순서로 진행됩니다. 그러나 후속 방문 시 인증 및 권한 부여 토큰의 만료 여부에 따라 또는 정책 보기에 따라 사용자는 하위 흐름 중 하나 또는 두 개만 진행할 수 있습니다.

### 시작 흐름 {#startup}

프로그래머 및 장치의 ID를 설정하고 초기화 작업을 수행합니다. 이는 모든 후속 권한 부여 호출에 대한 사전 요구 사항입니다.

**AccessEnabler**

* **`setRequestor()`** - AccessEnalber 및 확장에 따라 Adobe Pass 인증 서버로 ID를 설정합니다. 이 호출은 권한 흐름의 나머지 부분에 대한 전조입니다. 예를 들어 JavaScript에서:

  ```JavaScript
    /* Define the requestor ID (Programmer/aggregator ID). */
      var requestorID = "sample_requestor_Id";
      ...
      // Callback indicating that the AccessEnabler swf has initialized
      function swfLoaded() {
          // AccessEnabler is loaded so we can use the API function it provides
          accessEnablerObject.setRequestor(requestorID); 
      ...
      }
  ```

**클라이언트 없는 API**

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`** - 플랫폼에 따라 앱 호출 regcode 전에 완료해야 하는 사전 요구 작업이 있을 수 있습니다. 자세한 내용은 **클라이언트 없는 API 설명서**&#x200B;를 참조하세요. 예를 들어 Xbox 플랫폼은 regcode를 호출하기 전에 미리 지정된 보안 단계를 완료해야 합니다.

### 인증 흐름 {#authentication}

인증에 성공하면 장치 및 요청자에 연결된 AuthN 토큰이 생성됩니다. 인증에 성공하려면 인증이 필요합니다.

**AccessEnabler**

* `checkAuthentication()` - 전체 인증 흐름을 실제로 트리거하지 않고 캐시된 올바른 인증 토큰이 로컬 토큰 캐시에 있는지 확인합니다. `setAuthenticationStatus()` 콜백 함수를 트리거합니다.
* `getAuthentication()` - 전체 인증 흐름을 시작합니다. 성공하면 Adobe Pass 인증이 AuthN 토큰을 생성하고 클라이언트에서 캐시합니다. 사용자는 플랫폼에 따라 iFrame, 팝업 창 또는 웹 보기로 표시되는 선택한 MVPD 사이트에 로그인합니다. 그러면 displayProviderDialog()가 트리거됩니다.

**클라이언트 없는 API**

* `<FQDN>/.../checkauthn` - 위의 `checkAuthentication()`의 웹 서비스 버전입니다.
* `<FQDN>/.../config` - MVPD 목록을 두 번째 화면 앱으로 반환합니다.
* `<FQDN>/.../authenticate` - 두 번째 화면 앱에서 인증 흐름을 시작하여 로그인을 위해 선택한 MVPD로 사용자를 리디렉션합니다. 성공하면 Adobe Pass 인증이 AuthN 토큰을 생성하여 서버에 저장하고 사용자가 원래 장치로 돌아가서 권한 흐름을 완료합니다.

다음 두 가지 사항이 참인 경우 AuthN 토큰은 유효한 것으로 간주됩니다.

* AuthN 토큰이 만료되지 않음
* AuthN 토큰과 연결된 MVPD가 현재 요청자 ID에 대해 허용되는 MVPD 목록에 있습니다.

#### 일반 AccessEnabler 초기 인증 워크플로 {#generic-ae-initial-authn-flow}

1. 앱에서 `getAuthentication()`을(를) 호출하여 캐시된 올바른 인증 토큰을 확인하는 인증 워크플로를 시작합니다. 이 메서드에는 선택적 `redirectURL` 매개 변수가 있습니다. `redirectURL`에 대한 값을 제공하지 않으면 인증이 성공하면 사용자는 인증이 초기화된 URL로 반환됩니다.
1. AccessEnabler가 현재 인증 상태를 확인합니다. 사용자가 현재 인증되면 AccessEnabler가 `setAuthenticationStatus()` 콜백 함수를 호출하여 성공을 나타내는 인증 상태를 전달합니다.
1. 사용자가 인증되지 않은 경우 AccessEnabler는 지정된 MVPD로 사용자의 마지막 인증 시도가 성공했는지 여부를 확인하여 인증 흐름을 계속합니다. MVPD ID가 캐시되고 `canAuthenticate` 플래그가 true이거나 `setSelectedProvider()`을(를) 사용하여 MVPD를 선택한 경우 사용자에게 MVPD 선택 대화 상자가 표시되지 않습니다. 인증 흐름은 MVPD의 캐시된 값(즉, 마지막으로 성공한 인증 중에 사용된 동일한 MVPD)을 사용하여 계속됩니다. 백엔드 서버에 대한 네트워크 호출이 수행되며 사용자가 MVPD 로그인 페이지로 리디렉션됩니다.

1. 캐시된 MVPD ID가 없고 `setSelectedProvider()`을(를) 사용하여 선택한 MVPD가 없거나 `canAuthenticate` 플래그가 false로 설정되어 있으면 `displayProviderDialog()` 콜백이 호출됩니다. 이 콜백은 앱에서 사용자에게 선택할 MVPD 목록을 제공하는 UI를 만들도록 지시합니다. MVPD 선택기를 만드는 데 필요한 정보가 들어 있는 MVPD 개체 배열이 제공됩니다. 각 MVPD 개체는 MVPD 엔터티를 설명하고 MVPD의 ID 및 MVPD 로고를 찾을 수 있는 URL과 같은 정보를 포함합니다.

1. MVPD를 선택한 후에는 앱에서 사용자가 선택한 내용을 AccessEnabler에 알려야 합니다. Flash이 아닌 클라이언트의 경우 사용자가 원하는 MVPD를 선택하면 `setSelectedProvider()` 메서드를 호출하여 AccessEnabler에 사용자 선택을 알립니다. Flash 클라이언트가 대신 &quot;`mvpdSelection`&quot; 유형의 공유 `MVPDEvent`을(를) 디스패치하여 선택한 공급자를 전달합니다.

1. AccessEnabler에서 사용자의 MVPD 선택을 받으면 백엔드 서버에 대한 네트워크 호출이 수행되며 사용자는 MVPD 로그인 페이지로 리디렉션됩니다.

1. 인증 워크플로 내에서 AccessEnabler는 Adobe Pass 인증 및 선택한 MVPD와 통신하여 사용자의 자격 증명(사용자 ID 및 암호)을 요청하고 ID를 확인합니다. 일부 MVPD는 로그인을 위해 자체 사이트로 리디렉션하지만 다른 MVPD에서는 iFrame 내에 로그인 페이지를 표시해야 합니다. 고객이 그러한 MVPD.<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)--> 중 하나를 선택하는 경우 페이지에 iFrame을 만드는 콜백이 포함되어야 합니다.

1. 사용자가 성공적으로 로그인하면 AccessEnabler가 인증 토큰을 검색하여 인증 흐름이 완료되었음을 앱에 알립니다. AccessEnabler가 상태 코드가 1인 `setAuthenticationStatus()` 콜백을 호출하여 성공을 나타냅니다. 이 단계를 실행하는 동안 오류가 발생하면 `setAuthenticationStatus()` 콜백이 인증 실패와 해당 오류 코드를 나타내는 상태 코드 0으로 트리거됩니다.

>[!IMPORTANT]
>Comcast는 현재 로고에 대한 정적 URL을 제공하지 않는 유일한 MVPD입니다. 프로그래머는 [XFINITY 개발자 포털](https://developers.xfinity.com/products/tv-everywhere)에서 최신 로고를 가져와야 합니다.
>

### 인증 흐름 {#authorization}

권한이 보호된 콘텐츠를 보기 위한 필수 조건입니다. 인증이 성공하면 보안을 위해 프로그래머 앱에 제공되는 단기 미디어 토큰과 함께 AuthZ 토큰이 생성됩니다. 인증 워크플로를 지원하려면 필요한 요청자 설정을 이전에 수행하고 [미디어 토큰 검증기](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md)를 통합해야 합니다. 이를 완료하면 인증을 시작할 수 있습니다.

사용자가 보호된 리소스에 대한 액세스를 요청하면 앱에서 권한 부여를 시작합니다. 요청한 리소스(예: 채널, 에피소드 등)를 지정하는 리소스 ID를 전달합니다. 앱은 먼저 저장된 인증 토큰을 확인합니다. 하나를 찾을 수 없으면 인증 프로세스를 시작합니다.

**AccessEnabler**

* `checkAuthorization()` - 전체 인증 흐름을 시작하지 않고 인증을 확인합니다. 프로그래머 앱의 UI에 표시되는 상태 정보를 업데이트하는 데 종종 사용됩니다.

* `getAuthorization()` - 전체 인증 흐름을 시작합니다.

다음 콜백 함수를 제공하여 의 결과를 처리합니다.
인증 호출:

* `setToken()` - 이전에 인증이 성공하고 인증에 성공하면 AccessEnabler가 `setToken()` 콜백 함수를 호출하여 수명이 짧은 미디어 토큰을 전달하여 Adobe Pass 인증 권한 부여 흐름에 대한 성공적인 결론을 나타냅니다. (사용자가 보호된 콘텐츠를 볼 수 있도록 허용하기 전에 프로그래머 앱은 미디어 토큰 검증기를 사용하여 미디어 토큰의 유효성을 검사합니다.

* `tokenRequestFailed()` - 사용자가 요청된 리소스에 대해 권한이 없는 경우(또는 다른 이유로 쿼리에 실패한 경우) AccessEnabler는 이 콜백 함수(자체 오류 보고 함수 포함)를 호출하여 실패에 대한 세부 정보를 전달합니다.

**클라이언트 없는 API**

* `\<FQDN\>/.../authorize` - 전체 인증 흐름을 시작합니다.

#### 일반 AccessEnabler 인증 워크플로 {#generic-ae-authr-wf}

1. `setReqestor()`을(를) 사용하여 할당된 프로그래머 GUID를 Access Enabler에 등록하는 콜백 함수를 제공합니다. 이 콜백 함수는 AccessEnabler가
정상적으로 다운로드되었습니다.

1. 사용자가 보호된 리소스에 대한 액세스를 요청하면 `getAuthorization()`을(를) 호출합니다. `getAuthorization()`을(를) 사용하여 요청된 리소스(예: 채널, 에피소드 등)를 지정하는 리소스 ID를 전달합니다. AccessEnabler는 인증 요청과 함께 전달할 캐시된 인증 토큰을 찾습니다. 이 중 하나가 없으면 인증 흐름이 시작됩니다.
1. 인증 결과를 처리할 콜백 함수를 제공합니다.

   * `setToken()` - 인증이 성공하거나 사용자가 이전에 인증을 받은 경우 Access Enabler는 `setToken()` 콜백 함수를 호출하여 단기 인증 토큰을 전달하여 인증 프로세스를 계속합니다.

   * `tokenRequestFailed()` - 사용자가 요청된 리소스에 대해 권한이 없는 경우(또는 다른 이유로 인해 쿼리에 실패한 경우) AccessEnabler가 사용자가 등록한 오류 보고 함수와 `tokenRequestFailed()` 콜백을 호출하여 실패에 대한 세부 정보를 전달합니다.

### 로그아웃 흐름 {#logout}

현재 사용자의 자격 흐름과 연결된 토큰 및 기타 데이터를 지웁니다.

**AccessEnabler**

* `logout()`

**클라이언트 없는 API**

* `\<FQDN\>/.../logout`

## AccessEnabler 동작 이해 {#ae-behavior}

모든 AccessEnabler API 호출은 API 참조에 명시된 한 가지 예외를 제외하고 비동기적으로 수행됩니다. API를 임의의 횟수로 호출할 수 있지만 작업이 트리거되었다는 확실한 보장은 없습니다
에 의한 호출은 호출이 수행된 순서와 동일한 순서로 완료됩니다. (현재 Flash Player 런타임은 예외입니다. 다중 스레드가 아닌 경우 *do* 호출이 순서대로 완료됩니다.
호출됩니다.)

응답을 구분하고 응답을 호출과 연결할 수 있도록 모든 콜백은 입력 매개 변수를 다시 에코합니다. 여기에는 `checkAuthorization()`에 의해 트리거되는 `setToken()` 및`tokenRequestFailed()`이(가) 포함됩니다. `checkAuthorization()` 콜백의 경우 사용된 리소스가 다시 에코됩니다. 이 기능을 활용하여 어떤 응답이 어떤 호출에 해당하는지 구분할 수 있습니다. 이 기능을 사용하려면 다음과 같은 코드를 작성할 수 있습니다.

```JavaScript
    for each (resource in ["TNT", "CNN", "TBS", "AdultSwim"] ) {
         ae.checkAuthorization(resource);
    }
    
    // Success callback
    function setToken(resource, token) {
         // Use "resource" to figure 
         // out which checkAuthorization
         // call triggered this response
    }
    
    // Old error callback
    function tokenRequestFailed(resource, error, details) {
         // use "resource" to figure
         // out in response to which
         // checkAuthorization call
         // this was triggered
    }
    
    // Error callback using new error api
    ae.bind("errorEvent',"errorHandler");
    
    function errorHandler(error) {
         if(error.resource) {        
              // Use error.resource to figure
              // which checkAuthorization call
              // triggered this response
         }
    }
```

### AE 동작 FAQ {#ae-beh-faq}

**질문. 첫 번째 호출이 완료되기 전에 두 번째 AccessEnabler 호출을 수행하면 어떻게 됩니까?**

제1 호출은 제2 호출이 실행될 때(비동기 통신) 계속 실행된다.

**질문. AccessEnabler에서 지원할 수 있는 최대 동시 호출 수가 있습니까?**

AccessEnabler 코드에는 제한이 명시적으로 설정되어 있지 않으므로 사용 가능한 시스템 리소스와 MVPD의 용량에 의해서만 제한됩니다.

<!--

>[!MORELIKETHIS]
>
>*   [Programmer use cases](/help/authentication/programmer-use-cases.md)
>*   Error Reporting
>**Platform Cookbooks:**
>*   Clientless integration cookbook
>*   iOS Integration Cookbook
>*   Android Integration Cookbook
>*   JavaScript Integration Cookbook
>*   ActionScript Integration Cookbook
>*   Windows 8 Integration Cookbook
>*   AIR Native Extension Overview
-->
