---
title: JavaScript SDK Cookbook
description: JavaScript SDK Cookbook
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '957'
ht-degree: 0%

---

# (기존) JavaScript SDK Cookbook {#javascript-sdk-cookbook}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 소개 {#intro}

이 문서에서는 프로그래머의 상위 수준 애플리케이션이 Adobe Pass 인증 서비스와의 JavaScript 통합을 위해 구현하는 권한 부여 워크플로에 대해 설명합니다. JavaScript API 참조에 대한 링크는 전체에 포함됩니다.

또한 [관련 정보](#related) 섹션에는
JavaScript 코드 샘플 세트에 대한 링크입니다.

## 권한 흐름 {#entitlement}

1. [사전 요구 사항](#prereq)
2. [시작 흐름](#startup)
3. [인증 흐름](#authn)
4. [인증 흐름](#authz)
5. [미디어 흐름 보기](#logout)

</br>

![](../../../../assets/javascript-flows.png)


## 사전 요구 사항 {#prereq}

**종속성:**

- Adobe Pass 인증 라이브러리(AccessEnabler)를 Adobe Pass 인증 계정 관리자와 함께 사용하여 이 라이브러리를 정리합니다.
- 유효한 Adobe Pass 인증 요청자 ID입니다. Adobe Pass 인증 계정 관리자와 협력하여 이 작업을 조정하십시오.

콜백 함수를 만듭니다.

- `entitlementLoaded`
</br>

**트리거:** AccessEnabler를 로드하고 초기화를 마쳤습니다.

- `displayProviderDialog(mvpds)`

  **트리거:** `getAuthentication(),` 사용자가 공급자(MVPD)를 선택하지 않았고 아직 인증되지 않은 경우에만
mvpds 매개 변수는 사용자가 사용할 수 있는 공급자의 배열입니다.

- `setAuthenticationStatus(status, errorcode)`

  **트리거:**
   - 매번 `checkAuthentication()`입니다.
   - `getAuthentication()`은(는) 사용자가 이미 인증되고 공급자를 선택한 경우에만 가능합니다.

  반환된 상태는 성공 또는 실패입니다. errorcode는 실패 유형을 설명합니다.

- `createIFrame(width, height)`

  **트리거:** `setSelectedProvider(providerID)`, 선택한 공급자가 IFrame에 표시하도록 구성된 경우에만.

  >[!NOTE]
  >
  >공급자는 인증 화면을 리디렉션 또는 iFrame으로 렌더링하도록 구성되었으며 프로그래머는 두 가지 모두를 고려해야 합니다.

- `sendTrackingData(event, data)`

  **트리거:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  `event` 매개 변수는 발생한 권한 부여 이벤트를 나타냅니다. `data` 매개 변수는 이벤트와 관련된 값 목록입니다.
- `setToken(token, resource)`
  리소스를 볼 수 있는 권한을 부여한 후 **트리거:** `checkAuthorization()` 및 `getAuthorization()`.   `token` 매개 변수는 수명이 짧은 미디어 토큰이고 `resource` 매개 변수는 사용자가 볼 수 있는 콘텐츠입니다.

- `tokenRequestFailed(resource, code, description)`
  인증에 실패한 후 **트리거:**`checkAuthorization()`&#x200B;및`getAuthorization()`.\
  `resource` 매개 변수는 사용자가 보려고 한 콘텐츠입니다. `code` 매개 변수는 오류가 발생한 유형을 나타내는 오류 코드입니다. `description` 매개 변수는 오류 코드와 관련된 오류를 설명합니다.

- `selectedProvider(mvpd)`

  **트리거:** [`getSelectedProvider()`]&#x200B;(#$getSelProv `mvpd` 매개 변수는 다음에서 선택한 공급자에 대한 정보를 제공합니다.
사용자.

- `setMetadataStatus(metadata, key, arguments)`

  **트리거:** `getMetadata().`\
  `metadata` 매개 변수는 요청한 특정 데이터를 제공합니다. 키 매개 변수는 `getMetadata()`요청에 사용된 키이고 `arguments` 매개 변수는 `getMetadata()`에 전달된 것과 동일한 사전입니다.


## &#x200B;2. 시작 흐름

**I. AccessEnabler JavaScript 로드:**

**스테이징 프로필용**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

또는...

**프로덕션 프로필용**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**트리거:** 초기화가 완료되면 Adobe Pass
인증이 `entitlementLoaded()` 콜백 함수를 호출합니다. AccessEnabler와 애플리케이션 통신을 위한 진입점입니다.


**II.** `setRequestor()`(으)로 전화를 걸어
프로그래머 ID; 프로그래머 `requestorID` 및
(선택 사항) Adobe Pass 인증 엔드포인트 배열.

**트리거:**&#x200B;이(가) 없지만 필요한 경우 `displayProviderDialog()`을(를) 호출할 수 있습니다.


**III.** 전체 `checkAuthentication()`인증 흐름[을 시작하지 않고 기존 인증을 확인하려면 ]을(를) 호출하십시오.  이 호출이 성공하면 `authorization flow`(으)로 바로 진행할 수 있습니다.  그렇지 않으면 `authentication flow`(으)로 진행합니다.

**종속성:** `setRequestor()`에 대한 호출이 성공했습니다(이 종속성은 모든 후속 호출에도 적용됨).

**트리거:** `setAuthenticationStatus()` 콜백

</br>

## &#x200B;3. 인증 흐름</span>


**종속성:** `setRequestor()`에 대한 호출이 성공했습니다(이 종속성은 모든 후속 호출에도 적용됨).


인증 상태를 가져오거나 공급자 인증 흐름을 트리거하려면 `getAuthentication()`을(를) 호출하십시오.

**트리거:**

- `displayProviderDialog()`사용자가 아직 인증되지 않은 경우
- 인증이 이미 발생한 경우 `setAuthenticationStatus()`

AccessEnabler가 `setAuthenticationStatus()`을(를) 사용하여 `isAuthenticated == 1`을(를) 호출하면 인증 흐름이 완료됩니다.

## &#x200B;4. 인증 흐름 {#authz}

**종속성:**

- `setRequestor()`에 대한 호출이 성공했습니다(이 종속성은 이후의 모든 호출에도 적용됨).
- MVPD과 합의된 유효한 ResourceID입니다. ResourceID는 다른 장치 또는 플랫폼에서 사용되는 ID와 동일해야 하며 MVPD에서 동일합니다.

`getAuthorization()`을(를) 호출하고 요청된 미디어에 대한 ResourceID를 전달합니다. 호출이 성공하면 짧은 미디어 토큰이 반환되고, 이 토큰을 사용하면 사용자에게 요청된 미디어를 볼 수 있는 권한이 부여됩니다.

- 호출이 전달된 경우: 사용자에게 유효한 AuthN 토큰이 있으며 사용자는 요청된 미디어를 볼 수 있는 권한이 있습니다.
- 호출이 실패한 경우: throw된 예외를 검사하여 해당 유형(AuthN, AuthZ 또는 그 외 다른 것)을 확인합니다.
- 호출이 AuthN 오류인 경우 AuthN 흐름을 다시 시작합니다.
- 호출이 AuthZ 오류인 경우 사용자에게 요청된 미디어를 볼 수 있는 권한이 없으며 사용자에게 일종의 오류 메시지가 표시되어야 합니다.
- 다른 오류(연결 오류, 네트워크 오류 등)가 있는 경우 사용자에게 적절한 오류 메시지를 표시합니다.

미디어 토큰 검증기를 사용하여 `getAuthorization()` 호출에서 반환된 shortMediaToken의 유효성을 검사하십시오.


**종속성:** 짧은 미디어 토큰 확인자(여기에 포함)
AccessEnabler 라이브러리)

- 유효성 검사가 성공하는 경우: 사용자에 대해 요청된 미디어를 표시/재생합니다.
- 실패하면: AuthZ 토큰이 올바르지 않고 미디어 요청이 거부되어야 하며 사용자에게 오류 메시지가 표시되어야 합니다.

## &#x200B;5. 미디어 흐름 보기 {#logout}

- 사용자가 보려는 미디어를 선택합니다.
   - 미디어가 보호됩니까?
      - 앱이 미디어가 보호되는지 확인합니다.
         - 미디어가 보호되면 앱에서 위의 인증(AuthZ) 흐름이 시작됩니다.
         - 미디어가 보호되지 않은 경우 미디어 보기 플로우를 계속 진행합니다.
         - 미디어 재생

## 방문자 ID 구성 {#visitorID}

분석 관점에서 [Experience Cloud visitorID](https://experienceleague.adobe.com/docs/id-service/using/home.html) 값을 구성하는 것은 매우 중요합니다. EC visitorID 값이 설정되면 SDK은 모든 네트워크 호출과 함께 이 정보를 전송하며 Adobe Pass 인증 서비스는 이 정보를 수집합니다. 이렇게 하면 Adobe Pass 인증 서비스의 분석 데이터를 다른 애플리케이션이나 웹 사이트에서 얻은 다른 분석 보고서와 상호 연관시킬 수 있습니다. EC visitorID 설정 방법에 대한 정보는 [여기](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en)에서 찾을 수 있습니다.


>[!NOTE]
>
>이 기능은 JS SDK 버전 3.1.0부터 사용할 수 있습니다.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
