---
title: iOS/tvOS Cookbook
description: iOS/tvOS Cookbook
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '2403'
ht-degree: 0%

---

# (기존) iOS/tvOS SDK Cookbook {#iostvos-sdk-cookbook}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 소개 {#intro}

이 문서에서는 프로그래머 상위 수준 애플리케이션이 iOS/tvOS AccessEnabler 라이브러리에서 노출된 API를 통해 구현할 수 있는 자격 부여 워크플로에 대해 설명합니다.

iOS/tvOS용 Adobe Pass 인증 권한 부여 솔루션은 궁극적으로 두 개의 도메인으로 나뉩니다.

* UI 도메인 - UI를 구현하고 AccessEnabler 라이브러리에서 제공하는 서비스를 사용하여 제한된 콘텐츠에 액세스할 수 있는 상위 레벨 애플리케이션 계층입니다.

* AccessEnabler 도메인 - 권한 부여 워크플로우는 다음과 같은 형태로 구현됩니다.

   * Adobe의 백엔드 서버에 대한 네트워크 호출
   * 인증 및 권한 부여 워크플로와 관련된 비즈니스 논리 규칙
   * 다양한 리소스 관리 및 워크플로 상태 처리(예: 토큰 캐시)

AccessEnabler 도메인의 목적은 권한 부여 워크플로의 모든 복잡성을 숨기고 AccessEnabler 라이브러리를 통해 권한 부여 워크플로를 구현하는 간단한 권한 부여 기본 세트를 상위 레이어 애플리케이션에 제공하는 것입니다.

1. 요청자 ID 설정
1. 특정 ID 공급자에 대한 인증 확인 및 가져오기
1. 특정 리소스에 대한 권한 부여 확인 및 받기
1. 로그아웃
1. Apple SSO는 Apple VSA 프레임워크를 프록싱하여 흘러갑니다

AccessEnabler의 네트워크 작업은 자체 스레드에서 수행되므로 UI 스레드가 차단되지 않습니다. 따라서 두 애플리케이션 도메인 간의 양방향 통신 채널은 완전히 비동기적인 패턴을 따라야 합니다.

* UI 애플리케이션 레이어는 AccessEnabler 라이브러리에 의해 노출된 API 호출을 통해 AccessEnabler 도메인에 메시지를 전송합니다.
* AccessEnabler는 UI 레이어가 AccessEnabler 라이브러리에 등록하는 AccessEnabler 프로토콜에 포함된 콜백 메서드를 통해 UI 레이어에 응답합니다.

## Experience Cloud ID 서비스 구성(방문자 ID) {#visitorIDSetup}

[!DNL Analytics] 관점에서 [Experience Cloud ID](https://experienceleague.adobe.com/docs/id-service/using/home.html) 값을 구성하는 것이 중요합니다. `visitorID` 값이 설정되면 SDK은 모든 네트워크 호출과 함께 이 정보를 전송하며 [!DNL Adobe Pass] 인증 서버는 이 정보를 수집합니다. Adobe Pass 인증 서비스의 분석을 다른 애플리케이션이나 웹 사이트에서 얻은 다른 분석 보고서와 상호 연관시킬 수 있습니다. visitorID 설정 방법에 대한 정보는 [여기](#setOptions)에서 찾을 수 있습니다.

## 권한 흐름 {#entitlement}

A. [필수 구성 요소](#prereqs) </br>
B. [시작 흐름](#startup_flow) </br>
C. [Apple SSO 없는 인증 흐름](#authn_flow_wo_applesso) </br>
D. [iOS에서 Apple SSO를 사용하는 인증 흐름](#authn_flow_with_applesso) </br>
E. [tvOS에서 Apple SSO를 사용한 인증 흐름](#authn_flow_with_applesso_tvOS) </br>
[인증 흐름](#authz_flow) </br>
예: [미디어 흐름 보기](#media_flow) </br>
시간: [Apple SSO가 없는 로그아웃 흐름](#logout_flow_wo_AppleSSO) </br>
I. [Apple SSO를 통한 로그아웃 흐름](#logout_flow_with_AppleSSO) </br>


### A. 사전 요구 사항 {#prereqs}

1. 콜백 함수를 만듭니다.
   * `setRequestorComplete()` </br>
   * [setRequestor()](#$setReq)에 의해 트리거되어 성공 또는 실패를 반환합니다. </br>
   * 성공 은 권한 부여 호출을 계속할 수 있음을 나타냅니다.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * 사용자가 공급자(MVPD)를 선택하지 않았고 아직 인증되지 않은 경우에만 [`getAuthentication()`](#$getAuthN)에 의해 트리거됩니다. </br>
      * `mvpds` 매개 변수는 사용자가 사용할 수 있는 공급자의 배열입니다.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * `checkAuthentication()`에 의해 매번 트리거됩니다. </br>
      * 사용자가 이미 인증되고 공급자를 선택한 경우에만 [`getAuthentication()`](#$getAuthN)에 의해 트리거됩니다. </br>
      * 반환된 상태는 성공 또는 실패이며, errorcode는 실패 유형을 설명합니다.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * 사용자가 MVPD을 선택한 후 [`getAuthentication()`](#$getAuthN)에 의해 트리거됩니다. `url` 매개 변수는 MVPD 로그인 페이지의 위치를 제공합니다.

   * `sendTrackingData(event, data)` </br>
      * `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()`에 의해 트리거됩니다.
      * `event` 매개 변수는 발생한 권한 부여 이벤트를 나타냅니다. `data` 매개 변수는 이벤트와 관련된 값 목록입니다.

   * `setToken(token, resource)`

      * 리소스 보기에 대한 권한 부여가 성공한 후 [checkAuthorization()](#checkAuthZ) 및 [getAuthorization()](#$getAuthZ)에 의해 트리거됩니다.
      * `token` 매개 변수는 수명이 짧은 미디어 토큰이고 `resource` 매개 변수는 사용자가 볼 수 있는 콘텐츠입니다.

   * `tokenRequestFailed(resource, code, description)` </br>
      * 인증에 실패한 후 [checkAuthorization()](#checkAuthZ) 및 [getAuthorization()](#$getAuthZ)에 의해 트리거됩니다.
      * `resource` 매개 변수는 사용자가 보려고 한 콘텐츠입니다. `code` 매개 변수는 오류가 발생한 유형을 나타내는 오류 코드입니다. `description` 매개 변수는 오류 코드와 관련된 오류를 설명합니다.

   * `selectedProvider(mvpd)` </br>
      * [`getSelectedProvider()`](#getSelProv)에 의해 트리거됩니다.
      * `mvpd` 매개 변수는 사용자가 선택한 공급자에 대한 정보를 제공합니다.

   * `setMetadataStatus(metadata, key, arguments)`
      * `getMetadata().`에 의해 트리거됨
      * `metadata` 매개 변수는 요청한 특정 데이터를 제공합니다. `key` 매개 변수는 [getMetadata()](#getMeta) 요청에 사용된 키이고 `arguments` 매개 변수는 [getMetadata()](#getMeta)에 전달된 동일한 사전입니다.

   * [&#39;preauthorizedResources(authorizedResources)&#39;](#preauthResources)

      * [`checkPreauthorizedResources()`](#checkPreauth)에 의해 트리거됩니다.

      * `authorizedResources` 매개 변수는 사용자가 사용하는 리소스를 나타냅니다.
은(는) 볼 수 있는 권한이 있습니다.

   * [`presentTvProviderDialog(viewController)`](#presentTvDialog)

      * 현재 요청자가 SSO를 지원하는 MVPD에서 적어도 를 지원하는 경우 [getAuthentication()](#getAuthN)에 의해 트리거됩니다.
      * viewController 매개 변수는 Apple SSO 대화 상자이며 기본 보기 컨트롤러에 표시되어야 합니다.

   * [`dismissTvProviderDialog(viewController)`](#dismissTvDialog)

      * 사용자 작업에 의해 트리거됩니다(Apple SSO 대화 상자에서 &quot;취소&quot; 또는 &quot;기타 TV 공급자&quot;를 선택하여).
      * viewController 매개 변수는 Apple SSO 대화 상자로 기본 보기 컨트롤러에서 해제해야 합니다.

![](../../../../assets/iOS-flows.png)

### B. 시작 흐름 {#startup_flow}

1. 상위 수준 응용 프로그램을 시작합니다.</br>
1. Adobe Pass 인증 </br> 시작

   a. [`init`](#$init)을(를) 호출하여 Adobe Pass 인증 AccessEnabler의 단일 인스턴스를 만듭니다.
   * **종속성:** Adobe Pass 인증 기본 iOS/tvOS 라이브러리(AccessEnabler)

   b. `setRequestor()`을(를) 호출하여 프로그래머의 ID를 설정합니다. 프로그래머의 `requestorID`을(를) 전달하고 선택적으로 Adobe Pass 인증 끝점 배열을 전달합니다. tvOS의 경우 공개 키와 암호를 제공해야 합니다. 자세한 내용은 [클라이언트 없는 설명서](#create_dev)를 참조하세요.

   * **종속성:** 올바른 Adobe Pass 인증 요청자 ID(Adobe Pass 인증 계정으로 작업)
관리자(이 항목을 정렬합니다).

   * **트리거:**
     [setRequestorComplete()](#$setReqComplete) 콜백.

   >[!NOTE]
   >
   >요청자 ID가 완전히 설정될 때까지 권한 부여 요청을 완료할 수 없습니다. 이는 [`setRequestor()`](#$setReq)이(가) 계속 실행 중인 동안 모든 후속 권한 부여 요청이 실행됨을 의미합니다. 예를 들어 [`checkAuthentication()`](#checkAuthN)이(가) 차단되었습니다.

   두 가지 구현 옵션이 있습니다. 요청자 식별 정보가 백엔드 서버로 전송되면 UI 애플리케이션 레이어가 다음 두 가지 방법 중 하나를 선택할 수 있습니다. </br>

   1. [`setRequestorComplete()`](#setReqComplete) 콜백(AccessEnabler 대리자의 일부)이 트리거될 때까지 기다립니다. 이 옵션을 사용하면 [`setRequestor()`](#$setReq)이(가) 가장 확실하게 완료되므로 대부분의 구현에 권장됩니다.

   1. [`setRequestorComplete()`](#setReqComplete) 콜백이 트리거될 때까지 기다리지 않고 계속하고 권한 부여 요청 실행을 시작합니다. 이러한 호출(checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout)은 [`setRequestor()`](#$setReq) 이후에 실제 네트워크 호출을 수행하는 AccessEnabler 라이브러리에 의해 큐에 있습니다. 예를 들어 네트워크 연결이 불안정한 경우 이 옵션이 가끔 중단될 수 있습니다.

1. 전체 인증 흐름을 시작하지 않고 기존 인증을 확인하려면 `checkAuthentication()`을(를) 호출하십시오.  이 호출이 성공하면 인증 플로우로 직접 진행할 수 있습니다. 그렇지 않으면 인증 플로우로 이동합니다.

   * **종속성:** [setRequestor()](#$setReq)에 대한 호출이 성공했습니다(이 종속성은 이후의 모든 호출에도 적용됨).

   * **트리거:** [setAuthenticationStatus()](#$setAuthNStatus) 콜백.


### C. Apple SSO가 없는 인증 흐름 {#authn_flow_wo_applesso}

1. 인증 흐름을 시작하거나 사용자가 이미 있다는 확인을 받으려면 [`getAuthentication()`](#$getAuthN)을(를) 호출하십시오
인증됨.

   **트리거:**

   * 사용자가 이미 인증된 경우 [setAuthenticationStatus()](#$setAuthNStatus) 콜백입니다. 이 경우 [인증 흐름](#authz_flow)으로 바로 진행하십시오.

   * 사용자가 아직 인증되지 않은 경우 [displayProviderDialog()](#$dispProvDialog) 콜백입니다.

1. (으)로 전송된 공급자 목록이 있는 사용자 표시
   [`displayProviderDialog()`](#dispProvDialog).

1. 사용자가 공급자를 선택한 후 `navigateToUrl:` 또는 `navigateToUrl:useSVC:` 콜백에서 사용자 MVPD의 URL을 가져온 다음 `UIWebView/WKWebView` 또는 `SFSafariViewController` 컨트롤러를 열고 해당 컨트롤러를 URL로 보냅니다.

1. 이전 단계에서 인스턴스화된 `UIWebView/WKWebView` 또는 `SFSafariViewController`을(를) 통해 사용자는 MVPD의 로그인 페이지에 도달하고 로그인 자격 증명을 입력합니다. 컨트롤러 내에서 여러 리디렉션 작업이 발생합니다.</br>

>[!NOTE]
>
>이 시점에서 사용자는 인증 흐름을 취소할 수 있습니다. 이 경우 UI 계층은 `null`을(를) 매개 변수로 사용하여 [setSelectedProvider()](#setSelProv)을(를) 호출하여 이 이벤트에 대해 AccessEnabler에 알립니다. 이렇게 하면 AccessEnabler가 내부 상태를 정리하고 인증 흐름을 재설정할 수 있습니다.

1. 사용자가 성공적으로 로그인하면 애플리케이션 레이어가 특정 사용자 지정 URL의 로드를 감지합니다. 이 특정 사용자 지정 URL은 실제로 유효하지 않으며 제어자가 실제로 로드하기 위한 것이 아닙니다. 응용 프로그램에서 인증 흐름이 완료되었으며 `UIWebView/WKWebView` 또는 `SFSafariViewController` 컨트롤러를 닫아도 안전하다는 신호로만 해석해야 합니다. `SFSafariViewController`컨트롤러를 사용해야 하는 경우 특정 사용자 지정 URL이 **`application's custom scheme`**(예: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`)에 의해 정의되며, 그렇지 않으면 이 특정 사용자 지정 URL이 **`ADOBEPASS_REDIRECT_URL`** 상수(즉, `adobepass://ios.app`)에 의해 정의됩니다.

1. UIWebView/WKWebView 또는 SFSafariViewController 컨트롤러를 닫고 AccessEnabler가 백 엔드 서버에서 인증 토큰을 검색하도록 지시하는 AccessEnabler의 `handleExternalURL:url` API 메서드를 호출합니다.

1. (선택 사항) [`checkPreauthorizedResources(resources)`](#$checkPreauth)을(를) 호출하여 사용자가 볼 수 있는 권한이 있는 리소스를 확인합니다. `resources` 매개 변수는 사용자의 인증 토큰과 연결된 보호된 리소스 배열입니다. 사용자의 MVPD에서 얻은 인증 정보의 한 가지 용도는 UI를 장식하는 것입니다(예: 보호된 콘텐츠 옆에 있는 잠긴/잠금 해제된 기호).

   * **트리거:** [`preauthorizedResources()`](#preauthResources) 콜백
   * 완료된 인증 흐름 이후의 **실행 지점:**

1. 인증이 성공하면 인증 플로우로 진행합니다.

### D. iOS에서 Apple SSO를 사용하는 인증 흐름 {#authn_flow_with_applesso}

1. 인증 흐름을 시작하거나 사용자가 이미 인증되었음을 확인하려면 [`getAuthentication()`](#$getAuthN)을(를) 호출하십시오.
   **트리거:**

   * 사용자가 인증되지 않았고 현재 요청자가 SSO를 지원하는 MVPD을 적어도 하나 이상 보유한 경우 [presentTvProviderDialog()](#presentTvDialog) 콜백입니다. SSO를 지원하는 MVPD가 없는 경우 클래식 인증 흐름이 사용됩니다.

1. 사용자가 공급업체를 선택하면 AccessEnabler 라이브러리는 Apple의 VSA 프레임워크에서 제공하는 정보가 포함된 인증 토큰을 받게 됩니다.

1. [setAuthenticatiosStatus()](#setAuthNStatus) 콜백이 트리거됩니다. 이 시점에서 사용자는 Apple SSO로 인증되어야 합니다.

1. [선택 사항] [`checkPreauthorizedResources(resources)`](#$checkPreauth)을(를) 호출하여 사용자가 볼 수 있는 권한이 있는 리소스를 확인합니다. `resources` 매개 변수는 사용자의 인증 토큰과 연결된 보호된 리소스 배열입니다. 사용자의 MVPD에서 얻은 인증 정보의 한 가지 용도는 UI를 장식하는 것입니다(예: 보호된 콘텐츠 옆에 있는 잠긴/잠금 해제된 기호).

   * **트리거:** [`preauthorizedResources()`](#preauthResources) 콜백
   * 완료된 인증 흐름 이후의 **실행 지점:**

1. 인증이 성공하면 인증 플로우로 진행합니다.

### E. tvOS에서 Apple SSO를 사용하는 인증 흐름 {#authn_flow_with_applesso_tvOS}

1. 시작하려면 [`getAuthentication()`](#$getAuthN)에 문의하십시오.
인증 흐름 또는 사용자가 이미 있다는 확인을 받기 위해
인증됨.
   **트리거:**
   * 사용자가 인증되지 않았고 현재 요청자가 SSO를 지원하는 MVPD을 적어도 하나 이상 보유한 경우 [`presentTvProviderDialog()`](#presentTvDialog) 콜백입니다. SSO를 지원하는 MVPD가 없는 경우 클래식 인증 흐름이 사용됩니다.

1. 사용자가 공급자를 선택하면 [`status()`](#status_callback_implementation) 콜백이 호출됩니다. 등록 코드가 제공되고 AccessEnabler 라이브러리가 두 번째 화면 인증을 위해 서버 폴링을 시작합니다.

1. 제공된 등록 코드가 두 번째 화면에서 성공적으로 인증에 사용된 경우 [`setAuthenticatiosStatus()`](#setAuthNStatus) 콜백이 트리거됩니다. 이 시점에서 사용자는 Apple SSO로 인증되어야 합니다.
1. [선택 사항] [`checkPreauthorizedResources(resources)`](#$checkPreauth)을(를) 호출하여 사용자가 볼 수 있는 권한이 있는 리소스를 확인합니다. `resources` 매개 변수는 사용자의 인증 토큰과 연결된 보호된 리소스 배열입니다. 사용자의 MVPD에서 얻은 인증 정보의 한 가지 용도는 UI를 장식하는 것입니다(예: 보호된 콘텐츠 옆에 있는 잠긴/잠금 해제된 기호).

   * **트리거:** [`preauthorizedResources()`](#preauthResources) 콜백

   * 완료된 인증 흐름 이후의 **실행 지점:**
1. 인증이 성공하면 인증 플로우로 진행합니다.

### F. 인증 흐름 {#authz_flow}

1. 인증 흐름을 시작하려면 [getAuthorization()](#$getAuthZ)을 호출하십시오.

   * **종속성:** MVPD과(과) 합의된 올바른 리소스 ID입니다.
   * 리소스 ID는 다른 디바이스 또는 플랫폼에서 사용되는 ID와 동일해야 하며 MVPD에서 동일합니다. 리소스 ID에 대한 자세한 내용은 [보호된 리소스 확인](/help/authentication/integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md)을 참조하세요.

1. 인증 및 권한 부여의 유효성을 검사합니다.

   * [getAuthorization()](#$getAuthZ) 호출이 성공하는 경우: 사용자에게 유효한 AuthN 및 AuthZ 토큰이 있습니다(사용자는 요청된 미디어를 시청하도록 인증 및 승인됨).

   * [getAuthorization()](#$getAuthZ)이(가) 실패하면: throw된 예외를 검사하여 해당 유형(AuthN, AuthZ 또는 기타 다른 것)을 확인하십시오.
      * 인증(AuthN) 오류인 경우 인증 흐름을 다시 시작합니다.
      * 인증(AuthZ) 오류인 경우 사용자에게 요청된 미디어를 볼 수 있는 권한이 없으며 사용자에게 일종의 오류 메시지가 표시되어야 합니다.
      * 다른 유형의 오류(연결 오류, 네트워크 오류 등)가 있는 경우 사용자에게 적절한 오류 메시지를 표시합니다.

1. 짧은 미디어 토큰의 유효성을 검사합니다.\
   Adobe Pass 인증 미디어 토큰 검증기 라이브러리를 사용하여 위의 [getAuthorization()](#$getAuthZ) 호출에서 반환된 단기 미디어 토큰을 확인하십시오.

   * 유효성 검사가 성공하는 경우: 사용자에 대해 요청된 미디어를 재생합니다.
   * 유효성 검사에 실패하는 경우: AuthZ 토큰이 잘못되었으며 미디어 요청이 거부되고 오류 메시지가 사용자에게 표시되어야 합니다.


1. 일반 애플리케이션 플로우로 돌아갑니다.

### G. 미디어 흐름 보기 {#media_flow}

1. 사용자가 보려는 미디어를 선택합니다.
1. 미디어는 보호됩니까? 응용 프로그램에서 선택한 미디어가 보호되어 있는지 확인합니다.

   * 선택한 미디어가 보호되어 있으면 응용 프로그램에서 위의 [인증 흐름](#authz_flow)을 시작합니다.

   * 선택한 미디어가 보호되지 않은 경우 다음 동안 미디어를 재생합니다.
사용자.

### H. Apple SSO가 없는 로그아웃 흐름 {#logout_flow_wo_AppleSSO}

1. 사용자를 로그아웃하려면 [`logout()`](#$logout)을(를) 호출하십시오. AccessEnabler는 캐시된 모든 값과 토큰을 지웁니다. 캐시를 지운 후 AccessEnabler가 서버 호출을 수행하여 서버측 세션을 정리합니다. 서버 호출로 인해 IdP로 SAML 리디렉션이 발생할 수 있으므로(IdP측에서 세션 정리가 허용됨) 이 호출은 모든 리디렉션을 따라야 합니다. 이러한 이유로 이 호출은 UIWebView/WKWebView 또는 SFSafariViewController 컨트롤러 내에서 처리되어야 합니다.

   a. 인증 워크플로와 동일한 패턴에 따라 AccessEnabler 도메인은 `navigateToUrl:` 또는 `navigateToUrl:useSVC:` 콜백을 통해 UI 응용 프로그램 계층에 요청하여 UIWebView/WKWebView 또는 SFSafariViewController 컨트롤러를 만들고 콜백의 `url` 매개 변수에 제공된 URL을 로드하도록 지시합니다. 백엔드 서버에 있는 로그아웃 끝점의 URL입니다.

   b. 응용 프로그램에서 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러의 활동을 모니터링하고 여러 리디렉션을 거치는 동안 특정 사용자 지정 URL이 로드되는 순간을 감지해야 합니다. 이 특정 사용자 지정 URL은 실제로 유효하지 않으며 제어자가 실제로 로드하기 위한 것이 아닙니다. 응용 프로그램에서 로그아웃 흐름이 완료되었으며 `UIWebView/WKWebView` 또는 `SFSafariViewController` 컨트롤러를 닫아도 안전하다는 신호로만 해석해야 합니다. 컨트롤러가 이 특정 사용자 지정 URL을 로드하면 응용 프로그램에서 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러를 닫고 AccessEnabler의 `handleExternalURL:url`API 메서드를 호출해야 합니다. `SFSafariViewController`컨트롤러를 사용해야 하는 경우 특정 사용자 지정 URL이 **`application's custom scheme`**(예: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`)에 의해 정의되며, 그렇지 않으면 이 특정 사용자 지정 URL이 **`ADOBEPASS_REDIRECT_URL`** 상수(즉, `adobepass://ios.app`)에 의해 정의됩니다.

   >[!NOTE]
   >
   >로그아웃 흐름은 사용자가 어떤 방식으로든 UIWebView/WKWebView 또는 SFSafariViewController와 상호 작용할 필요가 없다는 점에서 인증 흐름과 다릅니다. UI 애플리케이션 레이어는 UIWebView/WKWebView 또는 SFSafariViewController 를 사용하여 모든 리디렉션이 준수되는지 확인합니다. 따라서, 로그아웃 프로세스 중에 컨트롤러를 보이지 않게(즉, 숨겨지게) 하는 것이 가능하다(그리고 권장된다).


### I. Apple SSO를 사용하여 로그아웃 플로우 {#logout_flow_with_AppleSSO}

1. 사용자를 로그아웃하려면 [`logout()`](#$logout)을(를) 호출하십시오.
1. ID VSA203으로 [status()](#status_callback_implementation) 콜백이 호출됩니다.
1. 시스템 설정에서도 로그인하도록 사용자에게 지시해야 합니다. 이렇게 하지 않으면 애플리케이션이 다시 시작될 때 인증이 다시 수행됩니다.



<!--
### Related Information {#related}


- [iOS API Reference](#)

- [iOS Technical Overview](#)

- [Generating Digital Certificates](#)

- [Identifying Protected Resources](#)

- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)

- [iOS Authentication error - adobepass.ios.app cannot be found (Tech
  Note)](#)
-->
