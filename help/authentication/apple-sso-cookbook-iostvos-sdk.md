---
title: Apple SSO Cookbook(iOS/tvOS SDK)
description: Apple SSO Cookbook(iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '1867'
ht-degree: 0%

---

# Apple SSO Cookbook(iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 소개 {#Introduction}

Adobe Primetime 인증 AccessEnabler iOS/tvOS SDK는 Apple SSO(Single Sign-On) 워크플로라고 하는 기능을 통해 iOS, iPadOS 또는 tvOS에서 실행되는 클라이언트 애플리케이션의 최종 사용자를 위한 플랫폼 SSO 인증을 지원할 수 있습니다.

이 문서는 기존 AccessEnabler iOS/tvOS SDK 설명서에 대한 확장 역할을 하며, 이들 설명서는 [여기](/help/authentication/iostvos-sdk-api-reference.md).

</br>

## Cookbook {#Cookbook}

Apple SSO 사용자 환경을 이용하려면 한 애플리케이션에서 AccessEnabler iOS/tvOS SDK를 통합한 후 아래에 제시된 팁 순서를 따라야 합니다.

</br>

### 전제 조건 {#Prerequisites}

</br>

#### 권한

>[!TIP]
>
> **<u>Pro 팁:</u>** 사용자의 구독 정보에 액세스하려면 장치의 카메라 또는 마이크에 대한 액세스를 제공하는 것과 유사하게, 사용자는 진행할 수 있도록 애플리케이션에 권한을 부여해야 합니다. 이 권한은 애플리케이션별로 요청해야 하며 디바이스는 사용자 선택을 저장합니다. 사용자가 애플리케이션 설정(TV 공급자 권한 액세스) 또는 의 섹션으로 이동하여 결정을 변경할 수 있음을 염두에 두십시오 *`Settings -> TV Provider`* iOS/iPadOS에서 또는 *`Settings -> Accounts -> TV Provider`* tvOS에서

>[!TIP]
>
> **<u>Pro 팁:</u>** 애플리케이션이 전경 상태로 전환되면 사용자의 권한을 요청하는 것이 좋지만, 애플리케이션이 다음을 확인할 수 있으므로 제안일 뿐입니다 [액세스 권한](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) 사용자 인증이 필요하기 전에 언제든지 사용자의 구독 정보입니다. 또한 AccessEnabler iOS/tvOS SDK API는 필요할 때 자동으로 사용자의 권한을 요청합니다.

>[!TIP]
>
> **<u>Pro 팁:</u>** 사용자가 자신의 구독 정보에 대한 액세스 권한을 부여하지 않거나 비디오 구독자 계정 프레임워크와의 통신에 실패하는 경우 AccessEnabler iOS/tvOS SDK는 일반 인증 플로우로 돌아갑니다.

>[!TIP]
>
> **<u>Pro 팁:</u>** SSO(Single Sign-On) 사용자 경험의 이점을 설명하여 구독 정보에 대한 액세스 권한을 거부하는 사용자에게 인센티브를 제공하는 것이 좋습니다. 사용자가 애플리케이션 설정(TV 공급자 권한 액세스) 또는 의 섹션으로 이동하여 결정을 변경할 수 있음을 염두에 두십시오 *`Settings -> TV Provider`* iOS/iPadOS에서 또는 *`Settings -> Accounts -> TV Provider`* tvOS에서


```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                   // Do nothing.
                
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                   // Incentivize users who refuse to give permission to access subscription information by explaining the benefits of the Single Sign-On (SSO) user experience. Please bear in mind that the user can change its decision by going to the application settings (TV Provider permission access) or to the section from Settings -> TV Provider on iOS/iPadOS or Settings -> Accounts -> TV Provider on tvOS.
                   ...
                }
    }
    ... 
```

</br>

#### 콜백

>[!TIP]
>
> **<u>Pro 팁:</u>** 다음 목록 구현 [콜백](/help/authentication/iostvos-sdk-api-reference.md) Apple SSO 워크플로에만 해당됩니다.

- [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Apple MVPD 선택기를 열 때 트리거된 콜백입니다.
- [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Apple MVPD 선택기를 닫을 때 트리거된 콜백입니다.

</br>

#### 오류 보고

>[!TIP]
>
> **<u>Pro 팁:</u>** 다음 목록 구현 [고급 오류 코드](/help/authentication/error-reporting.md) Apple SSO 워크플로에만 해당됩니다.

- ***년*** - 사용자가 Apple MVPD 선택기에서 &quot;기타 TV 공급자&quot; 옵션을 선택했습니다.
- ***-*** - 사용자가 Apple MVPD 선택기에서 현재 요청자가 지원하지 않는(통합 또는 Single Sign-On이 비활성화됨) TV 공급자를 선택했습니다.
- ***N005*** - 사용자가 일반 MVPD 선택기 또는 Apple MVPD 선택기를 취소하기로 했습니다.
- ***VSA403*** - 애플리케이션에 대한 사용자의 TV 공급자 권한이 거부되었습니다.
- ***VSA404*** - 사용자의 TV 공급자 권한이 애플리케이션에 대해 결정되지 않았습니다.
- ***VSA503*** - 비디오 구독자 계정 메타데이터 요청이 실패했습니다. 더 많은 컨텍스트가 *메시지* 필드.
- ***AAPL / APPL_ERROR*** - 비디오 구독자 계정 메타데이터 요청이 실패했습니다. 더 많은 컨텍스트가 *세부 사항* 필드.

</br>

### 인증 {#Authentication}

>[!TIP]
>
> **<u>팁:</u>** iOS/iPadOS/tvOS 구현에 대해 아래 단계를 따르십시오.

1. 응용 프로그램은 다음을 수행해야 합니다. [초기화](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) AccessEnabler iOS/tvOS SDK.
1. 응용 프로그램은 다음을 수행해야 합니다. [현재 요청자 식별자 설정](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **중요 사항:** 이 두 번째 단계는 [고급 오류 코드](/help/authentication/error-reporting.md) Apple SSO 워크플로(해당하는 경우)에만 해당됩니다 **다음 중 하나가 참입니다.**:

   - ***VSA403*** - 애플리케이션에 대한 사용자의 TV 공급자 권한이 거부되었습니다.
   - ***VSA404*** - 사용자의 TV 공급자 권한이 애플리케이션에 대해 결정되지 않았습니다.
   - ***APPL*** - AccessEnabler iOS/tvOS SDK와 비디오 구독자 계정 프레임워크 간의 통신에 오류가 발생했습니다.

   이 두 번째 단계에서는 경우에 따라 Apple SSO 프로필을 Adobe 인증 토큰으로 자동으로 교환하려고 합니다 **위의 모든 내용이 false입니다.** 및 **다음은 모두 true입니다.**:

   - 애플리케이션에 대한 사용자의 TV 공급자 권한이 부여됩니다.
   - 사용자가 장치 시스템 수준에서 TV 공급자 계정에 로그인합니다.
   - AccessEnabler iOS/tvOS SDK가 비디오 구독자 계정 프레임워크에서 사용자의 TV 공급자 식별자를 수신했습니다.
   - 사용자의 TV Provider와 애플리케이션의 통합은 Adobe Primetime TVE Dashboard를 통해 활성화됩니다.
   - 사용자의 TV Provider Single Sign-On with Application은 Adobe Primetime TVE Dashboard를 통해 활성화됩니다.
   - Adobe Primetime TVE Dashboard를 통해 사용자의 TV Provider가 성능이 저하되지 않습니다.
   - AccessEnabler iOS/tvOS SDK가 비디오 구독자 계정 프레임워크에서 사용자의 TV 공급자 SAML 응답을 수신했습니다.

   **<u>Pro 팁:</u>** 이 두 번째 단계는 다른 콜백을 트리거하지 않으며 [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) 애플리케이션이 인증을 명시적으로 시작하지 않았으므로 콜백입니다.

1. 응용 프로그램은 다음을 수행해야 합니다. [인증 상태 확인](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **중요 사항:** 이 세 번째 단계는 [고급 오류 코드](/help/authentication/error-reporting.md) Apple SSO 워크플로(해당하는 경우)에만 해당됩니다 **다음 중 하나가 참입니다.**:

   - ***VSA403** - 사용자가 장치 시스템 수준에서 TV 공급자 계정에 로그인했지만 사용자의 TV 공급자 권한이 애플리케이션에 대해 거부됩니다.
   - ***VSA404** - 사용자가 장치 시스템 수준에서 TV 공급자 계정에 로그인하지만 사용자의 TV 공급자 권한은 응용 프로그램에 대해 확인되지 않습니다.
   - ***APPL\_ERROR** - 사용자가 장치 시스템 수준에서 TV 공급자 계정에 로그인되어 있지만 AccessEnabler iOS/tvOS SDK와 비디오 구독자 계정 프레임워크 간의 통신에 오류가 발생했습니다.

   **중요 사항:** 이 세 번째 단계는 [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) 콜백 *상태* case의 경우 0과 같음 **다음 중 하나가 참입니다.**:

   - 사용자는 장치 시스템 수준에서 또는 일반 인증 흐름을 통해 TV 공급자 계정에 로그인되지 않습니다.
   - 사용자는 장치 시스템 수준에서 또는 일반 인증 플로우를 통해 TV 공급자 계정에 로그인하지만 사용자의 TV 공급자 인증 토큰 TTL이 통과했습니다.
   - 사용자는 장치 시스템 수준에서 또는 일반 인증 플로우를 통해 TV 공급자 계정에 로그인하지만 Adobe Primetime TVE Dashboard를 통해 사용자의 TV 공급자 통합 애플리케이션을 사용할 수 없습니다.
   - 사용자는 장치 시스템 수준에서 TV 공급자 계정에 로그인하지만 Adobe Primetime TVE Dashboard를 통해 사용자의 TV 공급자 Single Sign-On이 비활성화됩니다.
   - 사용자는 장치 시스템 수준에서 TV 공급자 계정에 로그인하지만 사용자의 TV 공급자 권한은 응용 프로그램에 대해 거부됩니다.
   - 사용자는 장치 시스템 수준에서 TV 공급자 계정에 로그인하지만 사용자의 TV 공급자 권한은 응용 프로그램에 대해 확인되지 않습니다.
   - 사용자가 장치 시스템 수준에서 TV 공급자 계정에 로그인되어 있지만 AccessEnabler iOS/tvOS SDK와 비디오 구독자 계정 프레임워크 간의 통신에 오류가 발생했습니다.

   **중요 사항:** 이 세 번째 단계는 [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) 콜백 *상태* case의 경우 1 **위의 모든 내용은 false입니다.**


1. 응용 프로그램은 다음을 수행해야 합니다. [인증 초기화](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) 이전 인증 상태 검사에서 [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) 콜백 *상태* 이(가) 0과 같습니다.

   **<u>Pro 팁:</u>** 다음 AccessEnabler iOS/tvOS SDK API 중 하나를 구현합니다 [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) 또는 [getAuthentication:filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **중요 사항:** 이 네 번째 단계는 [고급 오류 코드](/help/authentication/error-reporting.md) Apple SSO 워크플로(해당하는 경우)에만 해당됩니다 **다음 중 하나가 참입니다.**:

   - ***VSA403*** - 애플리케이션에 대한 사용자의 TV 공급자 권한이 거부되었습니다.
   - ***VSA404*** - 사용자의 TV 공급자 권한이 애플리케이션에 대해 결정되지 않았습니다.
   - ***VSA503*** - AccessEnabler iOS/tvOS SDK와 비디오 구독자 계정 프레임워크 간의 통신에 오류가 발생했습니다.
   - ***년*** - 사용자가 Apple MVPD 선택기에서 &quot;기타 TV 공급자&quot; 옵션을 선택했습니다.
   - ***-*** - 사용자가 Apple MVPD 선택기에서 현재 요청자가 지원하지 않는(통합 또는 Single Sign-On이 비활성화됨) TV 공급자를 선택했습니다.
   - ***N005*** - 사용자가 일반 MVPD 선택기 또는 Apple MVPD 선택기를 취소하기로 했습니다.

   **중요 사항:** 이 네 번째 단계는 를 트리거하여 일반 인증 플로우로 돌아갑니다. [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) 콜백 및 **1** 위의 [고급 오류 코드](/help/authentication/error-reporting.md), 사례 **위 중 하나가 참입니다.**.

   **중요 사항:** 이 네 번째 단계는 를 트리거하여 일반 인증 플로우로 돌아갑니다. [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) 또는 [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) 콜백 및 **없음** 위의 [고급 오류 코드](/help/authentication/error-reporting.md): 사용자가 Apple SSO를 지원하지 않지만 Apple MVPD 선택기에 있는 TV 공급자를 선택한 경우.

   **<u>Pro 팁:</u>** AccessEnabler iOS/tvOS SDK는 [setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) 사용자가 Apple SSO를 지원하지 않지만 Apple MVPD 선택기에 있는 TV 공급자를 선택한 경우 API입니다.

   **중요 사항:** 이 네 번째 단계에서는 경우에 따라 Apple SSO 프로필을 Adobe 인증 토큰으로 자동 교환하려고 합니다 **위의 모든 내용이 false입니다.** 및 **다음은 모두 true입니다.**:

   - 애플리케이션에 대한 사용자의 TV 공급자 권한이 부여됩니다.
   - 사용자는 장치 시스템 수준에서 TV 공급자 계정에 로그인되어 있습니다.
   - AccessEnabler iOS/tvOS SDK가 비디오 구독자 계정 프레임워크에서 사용자의 TV 공급자 식별자를 수신했습니다.
   - 사용자의 TV Provider와 애플리케이션의 통합은 Adobe Primetime TVE Dashboard를 통해 활성화됩니다.
   - 사용자의 TV Provider Single Sign-On with Application은 Adobe Primetime TVE Dashboard를 통해 활성화됩니다.
   - Adobe Primetime TVE Dashboard를 통해 사용자의 TV Provider가 성능이 저하되지 않습니다.
   - AccessEnabler iOS/tvOS SDK가 비디오 구독자 계정 프레임워크에서 사용자의 TV 공급자 SAML 응답을 수신했습니다.



>**<u>Pro 팁:</u>** 이 네 번째 단계는 [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus) 콜백(무관) *상태* 따라서 응용 프로그램에서 인증을 명시적으로 시작했기 때문입니다.


</br>

### 메타데이터 {#Metadata}

응용 프로그램에는 &quot; &quot;을(를) 사용하여 플랫폼 SSO를 통해 로그인한 결과로서 인증이 발생했는지 여부를 확인하는 옵션이 있습니다.*tokenSource&quot;* [사용자 메타데이터](/help/authentication/iostvos-sdk-api-reference.md#getMeta) AccessEnabler iOS/tvOS SDK의 API입니다.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

</br>

### 로그아웃 {#Logout}

다음 [비디오 구독자 계정](https://developer.apple.com/documentation/videosubscriberaccount) 프레임워크는 장치 시스템 수준에서 TV 공급자 계정에 로그인한 사용자를 프로그래밍 방식으로 로그아웃시키는 API를 제공하지 않습니다. 따라서 로그아웃이 완전히 적용되려면 최종 사용자가에서 명시적으로 로그아웃해야 합니다 *`Settings -> TV Provider`* iOS/iPadOS에서 또는 *`Settings -> Accounts -> TV Provider`* tvOS에서 사용자가 가질 수 있는 다른 옵션은 특정 응용 프로그램 설정 섹션(TV 공급자 권한 액세스)에서 사용자의 구독 정보에 액세스할 수 있는 권한을 철회하는 것입니다.

>[!TIP]
>
> **<u>팁:</u>** AccessEnabler iOS/tvOS SDK를 통해 구현 [로그아웃](/help/authentication/iostvos-sdk-api-reference.md#logout) API.


>[!TIP]
>
> **<u>Pro 팁:</u>** tvOS 구현에 대해 아래 단계를 따르십시오.

- 응용 프로그램은 다음을 수행해야 합니다. [로그아웃 시작](/help/authentication/iostvos-sdk-api-reference.md#logout) AccessEnabler iOS/tvOS SDK에서 이렇게 하면 MVPD 측의 세션 정리가 용이하지 않습니다.
- 애플리케이션에서 사용자가에서 명시적으로 로그아웃하도록 지시/프롬프트를 표시해야 합니다. *`Settings -> Accounts -> TV Provider`* tvOS에서만 [*VSA203* 상태 코드가 트리거됨](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>Pro 팁:</u>** iOS/iPadOS 구현에 대한 아래 절차를 따르십시오.

- 응용 프로그램은 다음을 수행해야 합니다. [로그아웃 시작](/help/authentication/iostvos-sdk-api-reference.md#logout) AccessEnabler iOS/tvOS SDK에서 이렇게 하면 MVPD 측의 세션 정리가 용이해집니다.
- 애플리케이션에서 사용자가에서 명시적으로 로그아웃하도록 지시/프롬프트를 표시해야 합니다. *`Settings -> TV Provider`* iOS/iPadOS에서만 [*VSA203* 상태 코드가 트리거됨](/help/authentication/error-reporting.md).


<!--
## Resources {#Resources}

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [AccessEnabler iOS/tvOS SDK Overview](/help/authentication/iostvos-sdk-overview.md)
- [AccessEnabler iOS/tvOS SDK Cookbook](/help/authentication/iostvos-sdk-cookbook.md)
- [AccessEnabler iOS/tvOS SDK API Reference](/help/authentication/iostvos-sdk-api-reference.md)
- [Error Reporting](/help/authentication/error-reporting.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
