---
title: Amazon FireOS 통합 Cookbook
description: Amazon FireOS 통합 Cookbook
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 0%

---

# Amazon FireOS 통합 Cookbook {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>


## 소개 {#intro}

이 문서에서는 프로그래머의 상위 수준 애플리케이션이 Amazon FireOS에 의해 노출된 API를 통해 구현할 수 있는 자격 워크플로에 대해 설명합니다 `AccessEnabler` 라이브러리입니다.

Amazon FireOS용 Adobe Pass 인증 권한 부여 솔루션은 궁극적으로 두 개의 도메인으로 나뉩니다.

- UI 도메인 - UI를 구현하고 가 제공하는 서비스를 사용하는 상위 수준 애플리케이션 계층입니다. `AccessEnabler` 제한된 콘텐츠에 대한 액세스 권한을 제공하는 라이브러리입니다.
- 다음 `AccessEnabler` 도메인 - 여기에서 권한 부여 워크플로가 다음과 같은 형태로 구현됩니다.
   - Adobe의 백엔드 서버에 대한 네트워크 호출
   - 인증 및 권한 부여 워크플로와 관련된 비즈니스 논리 규칙
   - 다양한 리소스 관리 및 워크플로 상태 처리(예: 토큰 캐시)

의 목표 `AccessEnabler` 도메인은 권한 부여 워크플로의 모든 복잡성을 숨기고, 상위 레이어 애플리케이션에 을 제공합니다 `AccessEnabler` library) 단순 권한 프리미티브 세트입니다. 이 프로세스를 통해 권한 부여 워크플로를 구현할 수 있습니다.

1. 요청자 ID를 설정합니다.
1. 특정 ID 공급자에 대한 인증을 확인하고 받으십시오.
1. 특정 리소스에 대한 인증을 확인하고 받으십시오.
1. 로그아웃.

다음 `AccessEnabler`의 네트워크 활동은 다른 스레드에서 수행되므로 UI 스레드가 차단되지 않습니다. 따라서 두 애플리케이션 도메인 간의 양방향 통신 채널은 완전히 비동기적인 패턴을 따라야 합니다.

- UI 애플리케이션 계층은에 메시지를 보냅니다. `AccessEnabler` 에 의해 노출된 API 호출을 통한 도메인 `AccessEnabler` 라이브러리입니다.
- 다음 `AccessEnabler` 에 포함된 콜백 메서드를 통해 UI 레이어에 응답합니다. `AccessEnabler` UI 계층이 `AccessEnabler` 라이브러리입니다.

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

      - 트리거 기준 `setRequestor()`는 성공 또는 실패를 반환합니다.     성공 은 권한 부여 호출을 계속할 수 있음을 나타냅니다.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - 트리거 기준 `getAuthentication()` 사용자가 공급자(MVPD)를 선택하지 않고 아직 인증되지 않은 경우에만 해당합니다. 다음 `mvpds` 매개 변수는 사용자가 사용할 수 있는 공급자의 배열입니다.

   - [`setAuthenticationStatus(status, reason)`](#$setAuthNStatus)

      - 트리거 기준 `checkAuthentication()` 항상. 트리거 기준 `getAuthentication()` 사용자가 이미 인증되고 공급자를 선택한 경우에만 해당합니다.

      - 반환된 상태가 인증되었거나 인증되지 않았습니다. 이유는 인증 실패 또는 로그아웃 작업을 설명합니다.

   - [navigateToUrl(url)](#$navigateToUrl)

      - AmazonFireOS SDK에서 무시되는 메서드는 가 트리거되는 Android 플랫폼에서 사용됩니다. `getAuthentication()` 사용자가 MVPD를 선택하면  다음 `url` 매개 변수는 MVPD의 로그인 페이지 위치를 제공합니다.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

      - 트리거 기준 `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
다음 `event` 매개 변수는 발생한 권한 부여 이벤트를 나타냅니다. `data` 매개 변수는 이벤트와 관련된 값의 목록입니다.

   - [`setToken(token, resource)`](#$setToken)

      - 트리거 기준 `checkAuthorization()` 및 `getAuthorization()` 리소스를 볼 수 있는 인증 성공 후.
      - 다음 `token` 매개 변수는 단기 미디어 토큰입니다. `resource` 매개 변수는 사용자에게 보기 권한이 있는 콘텐츠입니다.

   - [`tokenRequestFailed(resource, code, description)`](#$tokenRequestFailed)

      - 트리거 기준 `checkAuthorization()` 및 `getAuthorization()` 인증에 실패한 후.
      - 다음 `resource` 매개 변수는 사용자가 보려고 한 콘텐츠입니다. `code` parameter는 발생한 실패 유형을 나타내는 오류 코드입니다. `description` 매개 변수는 오류 코드와 관련된 오류를 설명합니다.

   - [`selectedProvider(mvpd)`](#$selectedProvider)

      - 트리거 기준 `getSelectedProvider()`.
      - 다음 `mvpd` 매개 변수는 사용자가 선택한 공급자에 대한 정보를 제공합니다.

   - [&#39;setMetadataStatus(메타데이터, 키, 인수)&#39;](#$setMetadataStatus)

      - 트리거 기준 `getMetadata().`
      - 다음 `metadata` 매개 변수는 요청한 특정 데이터를 제공합니다. `key` 매개 변수는 `getMetadata()` 요청 및 `arguments` 매개 변수가에 전달된 것과 동일한 사전입니다. `getMetadata()`.

   - [&#39;preauthorizedResources(resources)&#39;](#$preauthResources)

      - 트리거 기준 `checkPreauthorizedResources()`.
      - 다음 `authorizedResources` 매개 변수는 사용자에게 보기 권한이 있는 리소스를 제공합니다.


![](assets/android-entitlement-flows.png)


### B. 시작 흐름 {#startup_flow}

1. 상위 수준 응용 프로그램을 시작합니다.
1. Adobe Pass 인증을 시작합니다.

   1. 호출 [`getInstance`](#$getInstance) Adobe Pass 인증의 단일 인스턴스를 만들려면 `AccessEnabler`.

      - **종속성:** Adobe Pass 인증 기본 Amazon FireOS 라이브러리(`AccessEnabler`)

   1. 호출` setRequestor()` 프로그래머의 신원을 확인하다. `requestorID` 및 (선택 사항) Adobe Pass 인증 엔드포인트 배열.

      - **종속성:** 유효한 Adobe Pass 인증 요청자 ID(Adobe Pass 인증 계정 관리자와 협력하여 정리하십시오.)

      - **트리거:** setRequestorComplete() callback

   요청자 ID가 완전히 설정될 때까지 권한 부여 요청을 완료할 수 없습니다. 이는 setRequestor()가 계속 실행 중인 동안 모든 후속 권한 요청(예:`checkAuthentication()`)은 차단됩니다.

   두 가지 구현 옵션이 있습니다. 요청자 식별 정보가 백엔드 서버로 전송되면 UI 애플리케이션 레이어가 다음 두 가지 접근 방식 중 하나를 선택할 수 있습니다.</p>

   1. 의 트리거를 기다립니다. `setRequestorComplete()` callback (일부) `AccessEnabler` 위임).  이 옵션은 다음과 같은 가장 확실성을 제공합니다. `setRequestor()` 완료되었으므로 대부분의 구현에 권장됩니다.
   1. 이 트리거될 때까지 기다리지 않고 계속합니다. `setRequestorComplete()` 콜백을 실행하고 권한 부여 요청 실행을 시작합니다. 이러한 호출(checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout)은 `AccessEnabler` 라이브러리: `setRequestor()`. 네트워크 연결이 불안정한 경우 이 옵션이 중단되는 경우가 있습니다.

1. 호출 [checkAuthentication()](#$checkAuthN) 전체 인증 흐름을 시작하지 않고 기존 인증을 확인합니다.  이 호출이 성공하면 인증 플로우로 직접 진행할 수 있습니다.  그렇지 않으면 인증 플로우로 이동합니다.

- **종속성:** 에 대한 성공적인 호출 `setRequestor()` (이 종속성은 모든 후속 호출에도 적용됩니다.)

- **트리거:** setAuthenticationStatus() callback

### C. 인증 흐름 {#authn_flow}

1. 호출 [`getAuthentication()`](#$getAuthN) 인증 흐름을 시작하거나 사용자가 이미 인증되었다는 확인을 받습니다.

   **트리거:**

   - 사용자가 이미 인증된 경우 setAuthenticationStatus() 콜백입니다.  이 경우 로 직접 이동합니다. [인증 흐름](#authz_flow).
   - 사용자가 아직 인증되지 않은 경우 displayProviderDialog() 콜백입니다.

1. (으)로 전송된 공급자 목록이 있는 사용자 표시 `displayProviderDialog()`.
1. 사용자가 공급자를 선택하면 사용자가 로그인할 수 있도록 WebView에서 공급자 페이지가 열립니다

   >[!NOTE]
   >
   >이 시점에서 사용자는 인증 흐름을 취소할 수 있습니다. 이 경우 `AccessEnabler` 은(는) 내부 상태를 정리하고 인증 흐름을 재설정합니다.

1. 사용자가 성공적으로 로그인하면 WebView 가 닫힙니다.
1. 호출 `getAuthenticationToken(),` 다음을 지시합니다. `AccessEnabler` 백엔드 서버에서 인증 토큰을 검색합니다.
1. [선택 사항] 호출 [`checkPreauthorizedResources(resources)`](#$checkPreauth) 을 눌러 사용자가 볼 수 있는 권한이 있는 리소스를 확인합니다. 다음 `resources` 매개 변수는 사용자의 인증 토큰과 연결된 보호된 리소스의 배열입니다.

   **트리거:** `preAuthorizedResources()` callback\
   **실행 지점:** 완료된 인증 흐름 이후

1. 인증이 성공하면 인증 플로우로 진행합니다.


### D. 인증 흐름 {#authz_flow}

1. 호출 [`getAuthorization()`](#$getAuthZ) 인증 흐름을 시작합니다.

   종속성: 유효한 ResourceID가 MVPD와(과) 합의되었습니다.

   **참고:** ResourceID는 다른 장치 또는 플랫폼에서 사용되는 ID와 동일해야 하며 MVPD에서 동일합니다.

1. 인증 및 권한 부여의 유효성을 검사합니다.

   - 다음과 같은 경우 `getAuthorization()` 호출 성공: 사용자에게 유효한 AuthN 및 AuthZ 토큰이 있습니다(사용자는 요청된 미디어를 시청하도록 인증되고 승인됨).
   - If `getAuthorization()` 실패: throw된 예외를 검사하여 해당 유형(AuthN, AuthZ 또는 그 외 다른 것)을 확인합니다.
      - 인증(AuthN) 오류인 경우 인증 흐름을 다시 시작합니다.
      - 인증(AuthZ) 오류인 경우 사용자에게 요청된 미디어를 볼 수 있는 권한이 없으며 사용자에게 일종의 오류 메시지가 표시되어야 합니다.
      - 다른 유형의 오류(연결 오류, 네트워크 오류 등)가 있는 경우 그런 다음 사용자에게 적절한 오류 메시지를 표시합니다.

1. 짧은 미디어 토큰의 유효성을 검사합니다.

   Adobe Pass 인증 미디어 토큰 검증기 라이브러리를 사용하여 다음에서 반환된 단기 미디어 토큰을 확인합니다. `getAuthorization()` 위의 호출:

   - 유효성 검사가 성공하는 경우: 사용자에 대해 요청된 미디어를 재생합니다.
   - 유효성 검사에 실패하는 경우: AuthZ 토큰이 잘못되었으며 미디어 요청이 거부되고 오류 메시지가 사용자에게 표시되어야 합니다.

1. 일반 애플리케이션 플로우로 돌아갑니다.

### E. 미디어 흐름 보기 {#media_flow}

1. 사용자가 보려는 미디어를 선택합니다.
1. 미디어는 보호됩니까?  응용 프로그램에서 선택한 미디어가 보호되어 있는지 확인합니다.
   - 선택한 미디어가 보호되면 애플리케이션이 [인증 흐름](#authz_flow) 위.
   - 선택한 미디어가 보호되지 않으면 사용자를 위해 미디어를 재생합니다.

### F. 로그아웃 흐름 {#logout_flow}

1. 호출 [`logout()`](#$logout) 로그아웃할 수 있습니다. 다음 `AccessEnabler` 단일 사인온을 통해 로그인을 공유하는 모든 요청자에서 현재 MVPD에 대해 사용자가 얻은 모든 캐시된 값과 토큰을 지웁니다. 캐시를 지운 후 `AccessEnabler` 서버 호출을 수행하여 서버측 세션을 정리합니다.  서버 호출로 인해 IdP로 SAML 리디렉션이 발생할 수 있으므로(IdP측에서 세션 정리가 허용됨) 이 호출은 모든 리디렉션을 따라야 합니다. 따라서 이 호출은 WebView 컨트롤 내에서 처리되며 사용자가 볼 수 없습니다.

   **참고:** 로그아웃 흐름은 사용자가 어떤 방식으로든 WebView와 상호 작용할 필요가 없다는 점에서 인증 흐름과 다릅니다. 따라서 로그아웃 프로세스 중에 WebView 컨트롤을 보이지 않게(즉, 숨김으로) 만들 수 있습니다(권장).
