---
title: Amazon FireOS 통합 Cookbook
description: Amazon FireOS 통합 Cookbook
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1447'
ht-degree: 0%

---

# (기존) Amazon FireOS 통합 Cookbook {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

</br>


## 소개 {#intro}

이 문서에서는 프로그래머의 상위 수준 응용 프로그램이 Amazon FireOS `AccessEnabler` 라이브러리에서 노출된 API를 통해 구현할 수 있는 권한 부여 워크플로에 대해 설명합니다.

Amazon FireOS용 Adobe Pass 인증 권한 부여 솔루션은 궁극적으로 두 개의 도메인으로 나뉩니다.

- UI 도메인 - UI를 구현하고 `AccessEnabler` 라이브러리에서 제공하는 서비스를 사용하여 제한된 콘텐츠에 대한 액세스를 제공하는 상위 수준 응용 프로그램 계층입니다.
- `AccessEnabler` 도메인 - 권한 부여 워크플로가 다음과 같은 형식으로 구현됩니다.
   - Adobe의 백엔드 서버에 대한 네트워크 호출
   - 인증 및 권한 부여 워크플로와 관련된 비즈니스 논리 규칙
   - 다양한 리소스 관리 및 워크플로 상태 처리(예: 토큰 캐시)

`AccessEnabler` 도메인의 목표는 권한 부여 워크플로의 모든 복잡성을 숨기고 `AccessEnabler` 라이브러리를 통해 상위 계층 응용 프로그램에 간단한 권한 부여 기본 요소 집합을 제공하는 것입니다. 이 프로세스를 통해 권한 부여 워크플로를 구현할 수 있습니다.

1. 요청자 ID를 설정합니다.
1. 특정 ID 공급자에 대한 인증을 확인하고 받으십시오.
1. 특정 리소스에 대한 인증을 확인하고 받으십시오.
1. 로그아웃.

`AccessEnabler`의 네트워크 활동이 다른 스레드에서 수행되므로 UI 스레드가 차단되지 않습니다. 따라서 두 애플리케이션 도메인 간의 양방향 통신 채널은 완전히 비동기적인 패턴을 따라야 합니다.

- UI 응용 프로그램 레이어가 `AccessEnabler` 라이브러리에서 노출된 API 호출을 통해 `AccessEnabler` 도메인에 메시지를 보냅니다.
- `AccessEnabler`은(는) UI 레이어가 `AccessEnabler` 라이브러리에 등록하는 `AccessEnabler` 프로토콜에 포함된 콜백 메서드를 통해 UI 레이어에 응답합니다.

## 권한 흐름 {#entitlement}

1. [전제 조건](#prereqs)
1. [시작 흐름](#startup_flow)
1. [인증 흐름](#authn_flow)
1. [인증 흐름](#authz_flow)
1. [미디어 흐름 보기](#media_flow)
1. [로그아웃 흐름](#logout_flow)

### A. 사전 요구 사항 {#prereqs}

1. 콜백 함수를 만듭니다.
   - [`setRequestorComplete()`](#$setRequestorComplete)

      - `setRequestor()`에 의해 트리거된 은(는) 성공 또는 실패를 반환합니다.     성공 은 권한 부여 호출을 계속할 수 있음을 나타냅니다.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - 사용자가 공급자(MVPD)를 선택하지 않았고 아직 인증되지 않은 경우에만 `getAuthentication()`에 의해 트리거됩니다. `mvpds` 매개 변수는 사용자가 사용할 수 있는 공급자의 배열입니다.

   - [`setAuthenticationStatus(status, reason)`](#$setAuthNStatus)

      - `checkAuthentication()`에 의해 매번 트리거됩니다. 사용자가 이미 인증되고 공급자를 선택한 경우에만 `getAuthentication()`에 의해 트리거됩니다.

      - 반환된 상태가 인증되었거나 인증되지 않았습니다. 이유는 인증 실패 또는 로그아웃 작업을 설명합니다.

   - [navigateToUrl(url)](#$navigateToUrl)

      - AmazonFireOS SDK에서 무시된 메서드는 사용자가 MVPD을 선택한 후 `getAuthentication()`에 의해 트리거되는 Android 플랫폼에서 사용됩니다.  `url` 매개 변수는 MVPD 로그인 페이지의 위치를 제공합니다.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

      - `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`에 의해 트리거됩니다.
`event` 매개 변수는 발생한 권한 부여 이벤트를 나타냅니다. `data` 매개 변수는 이벤트와 관련된 값 목록입니다.

   - [`setToken(token, resource)`](#$setToken)

      - 리소스를 볼 수 있는 권한을 부여한 후 `checkAuthorization()` 및 `getAuthorization()`에 의해 트리거됩니다.
      - `token` 매개 변수는 수명이 짧은 미디어 토큰이고 `resource` 매개 변수는 사용자가 볼 수 있는 콘텐츠입니다.

   - [`tokenRequestFailed(resource, code, description)`](#$tokenRequestFailed)

      - 인증에 실패한 후 `checkAuthorization()` 및 `getAuthorization()`에 의해 트리거됩니다.
      - `resource` 매개 변수는 사용자가 보려고 한 콘텐츠입니다. `code` 매개 변수는 오류가 발생한 유형을 나타내는 오류 코드입니다. `description` 매개 변수는 오류 코드와 관련된 오류를 설명합니다.

   - [`selectedProvider(mvpd)`](#$selectedProvider)

      - `getSelectedProvider()`에 의해 트리거됩니다.
      - `mvpd` 매개 변수는 사용자가 선택한 공급자에 대한 정보를 제공합니다.

   - [&#39;setMetadataStatus(메타데이터, 키, 인수)&#39;](#$setMetadataStatus)

      - `getMetadata().`에 의해 트리거됨
      - `metadata` 매개 변수는 요청한 특정 데이터를 제공합니다. `key` 매개 변수는 `getMetadata()` 요청에 사용된 키이고 `arguments` 매개 변수는 `getMetadata()`에 전달된 것과 동일한 사전입니다.

   - [&#39;preauthorizedResources(resources)&#39;](#$preauthResources)

      - `checkPreauthorizedResources()`에 의해 트리거됩니다.
      - `authorizedResources` 매개 변수는 사용자가 볼 수 있는 권한을 가진 리소스를 제공합니다.


![](../../../../assets/android-entitlement-flows.png)


### B. 시작 흐름 {#startup_flow}

1. 상위 수준 응용 프로그램을 시작합니다.
1. Adobe Pass 인증을 시작합니다.

   1. [`getInstance`](#$getInstance)을(를) 호출하여 Adobe Pass 인증 `AccessEnabler`의 단일 인스턴스를 만듭니다.

      - **종속성:** Adobe Pass 인증 기본 Amazon FireOS 라이브러리(`AccessEnabler`)

   1. ` setRequestor()`을(를) 호출하여 프로그래머의 ID를 설정합니다. 프로그래머의 `requestorID` 및 (선택적으로) Adobe Pass 인증 끝점 배열을 전달합니다.

      - **종속성:** 올바른 Adobe Pass 인증 요청자 ID(Adobe Pass 인증 계정 관리자와 함께 이 작업을 처리하십시오.)

      - **트리거:** setRequestorComplete() 콜백

   요청자 ID가 완전히 설정될 때까지 권한 부여 요청을 완료할 수 없습니다. 이는 setRequestor()가 계속 실행 중인 동안 후속 권한 요청(예: `checkAuthentication()`)이 모두 차단되었음을 의미합니다.

   두 가지 구현 옵션이 있습니다. 요청자 식별 정보가 백엔드 서버로 전송되면 UI 애플리케이션 레이어가 다음 두 가지 접근 방식 중 하나를 선택할 수 있습니다.</p>

   1. `setRequestorComplete()` 콜백(`AccessEnabler` 대리자의 일부)이 트리거될 때까지 기다립니다.  이 옵션을 사용하면 `setRequestor()`이(가) 가장 확실하게 완료되므로 대부분의 구현에 권장됩니다.
   1. `setRequestorComplete()` 콜백이 트리거될 때까지 기다리지 않고 계속하고 권한 부여 요청 실행을 시작합니다. 이러한 호출(checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout)은 `setRequestor()` 이후에 실제 네트워크 호출을 수행하는 `AccessEnabler` 라이브러리에 의해 큐에 있습니다. 네트워크 연결이 불안정한 경우 이 옵션이 중단되는 경우가 있습니다.

1. 전체 인증 흐름을 시작하지 않고 기존 인증을 확인하려면 [checkAuthentication()](#$checkAuthN)을 호출하십시오.  이 호출이 성공하면 인증 플로우로 직접 진행할 수 있습니다.  그렇지 않으면 인증 플로우로 이동합니다.

- **종속성:** `setRequestor()`에 대한 호출이 성공했습니다(이 종속성은 모든 후속 호출에도 적용됨).

- **트리거:** setAuthenticationStatus() 콜백

### C. 인증 흐름 {#authn_flow}

1. 인증 흐름을 시작하거나 사용자가 이미 인증되었음을 확인하려면 [`getAuthentication()`](#$getAuthN)을(를) 호출하십시오.

   **트리거:**

   - 사용자가 이미 인증된 경우 setAuthenticationStatus() 콜백입니다.  이 경우 [인증 흐름](#authz_flow)으로 바로 진행하십시오.
   - 사용자가 아직 인증되지 않은 경우 displayProviderDialog() 콜백입니다.

1. `displayProviderDialog()`(으)로 보낸 공급자 목록을 사용자에게 표시합니다.
1. 사용자가 공급자를 선택하면 사용자가 로그인할 수 있도록 WebView에서 공급자 페이지가 열립니다

   >[!NOTE]
   >
   >이 시점에서 사용자는 인증 흐름을 취소할 수 있습니다. 이 경우 `AccessEnabler`이(가) 내부 상태를 정리하고 인증 흐름을 다시 설정합니다.

1. 사용자가 성공적으로 로그인하면 WebView 가 닫힙니다.
1. `AccessEnabler`이(가) 백 엔드 서버에서 인증 토큰을 검색하도록 지시하는 `getAuthenticationToken(),`을 호출합니다.
1. [선택 사항] [`checkPreauthorizedResources(resources)`](#$checkPreauth)을(를) 호출하여 사용자가 볼 수 있는 권한이 있는 리소스를 확인합니다. `resources` 매개 변수는 사용자의 인증 토큰과 연결된 보호된 리소스 배열입니다.

   **트리거:** `preAuthorizedResources()` 콜백\
   완료된 인증 흐름 이후의 **실행 지점:**

1. 인증이 성공하면 인증 플로우로 진행합니다.


### D. 인증 흐름 {#authz_flow}

1. 인증 흐름을 시작하려면 [`getAuthorization()`](#$getAuthZ)을(를) 호출하십시오.

   종속성: MVPD과 동의한 유효한 리소스 ID입니다.

   **참고:** ResourceID는 다른 장치나 플랫폼에서 사용되는 ID와 동일해야 하며 MVPD에서 동일합니다.

1. 인증 및 권한 부여의 유효성을 검사합니다.

   - `getAuthorization()` 호출이 성공하면 사용자에게 유효한 AuthN 및 AuthZ 토큰이 있습니다(사용자는 요청된 미디어를 볼 수 있도록 인증되고 권한이 부여됨).
   - `getAuthorization()`이(가) 실패할 경우: throw된 예외를 검사하여 해당 유형(AuthN, AuthZ 또는 기타)을 확인합니다.
      - 인증(AuthN) 오류인 경우 인증 흐름을 다시 시작합니다.
      - 인증(AuthZ) 오류인 경우 사용자에게 요청된 미디어를 볼 수 있는 권한이 없으며 사용자에게 일종의 오류 메시지가 표시되어야 합니다.
      - 다른 유형의 오류(연결 오류, 네트워크 오류 등)가 있는 경우 사용자에게 적절한 오류 메시지를 표시합니다.

1. 짧은 미디어 토큰의 유효성을 검사합니다.

   Adobe Pass 인증 미디어 토큰 검증기 라이브러리를 사용하여 위의 `getAuthorization()` 호출에서 반환된 단기 미디어 토큰을 확인하십시오.

   - 유효성 검사가 성공하는 경우: 사용자에 대해 요청된 미디어를 재생합니다.
   - 유효성 검사에 실패하는 경우: AuthZ 토큰이 잘못되었으며 미디어 요청이 거부되고 오류 메시지가 사용자에게 표시되어야 합니다.

1. 일반 애플리케이션 플로우로 돌아갑니다.

### E. 미디어 흐름 보기 {#media_flow}

1. 사용자가 보려는 미디어를 선택합니다.
1. 미디어는 보호됩니까?  응용 프로그램에서 선택한 미디어가 보호되어 있는지 확인합니다.
   - 선택한 미디어가 보호되어 있으면 응용 프로그램에서 위의 [인증 흐름](#authz_flow)을 시작합니다.
   - 선택한 미디어가 보호되지 않으면 사용자를 위해 미디어를 재생합니다.

### F. 로그아웃 흐름 {#logout_flow}

1. 사용자를 로그아웃하려면 [`logout()`](#$logout)을(를) 호출하십시오. `AccessEnabler`은(는) SSO(Single Sign-On)를 통해 로그인을 공유하는 모든 요청자에서 현재 MVPD에 대해 사용자가 얻은 캐시된 값 및 토큰을 모두 지웁니다. 캐시를 지운 후 `AccessEnabler`에서 서버 호출을 수행하여 서버측 세션을 정리합니다.  서버 호출로 인해 IdP로 SAML 리디렉션이 발생할 수 있으므로(IdP측에서 세션 정리가 허용됨) 이 호출은 모든 리디렉션을 따라야 합니다. 따라서 이 호출은 WebView 컨트롤 내에서 처리되며 사용자가 볼 수 없습니다.

   **참고:** 로그아웃 흐름은 사용자가 어떤 식으로든 WebView와 상호 작용할 필요가 없다는 점에서 인증 흐름과 다릅니다. 따라서 로그아웃 프로세스 중에 WebView 컨트롤을 보이지 않게(즉, 숨김으로) 만들 수 있습니다(권장).
