---
title: Apple SSO 개요
description: Apple SSO 개요
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1260'
ht-degree: 0%

---

# Apple SSO 개요 {#apple-sso-overview}

>[!IMPORTANT]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

Apple은 사용자가 장치 시스템 수준에서 TV 공급자 계정에 로그인할 수 있는 기능을 제공하므로 앱별로 인증할 필요가 없습니다.

Adobe Pass Authentication은 Apple과 협력하여 iPhone, iPad 및 Apple TV 소유자를 위한 TV Everywhere 생태계에서 Partner SSO(Single Sign-On) 사용자 경험을 만들었습니다.

Apple 장치에서 SSO(Single Sign-On) 사용자 경험을 활용하려면 아래에 문서화된 필수 구성 요소 목록을 완료해야 합니다.

최종 결과에서는 다음 사용자 흐름에 따라 경험을 만들어야 합니다. 애플리케이션 개발을 시작하기 전에 알아 두는 것이 좋습니다.

* SSO(Single Sign-On) [iPhone 및 iPad의 사용자 흐름](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf)입니다.
* SSO(Single Sign-On) [Apple TV에 대한 사용자 흐름](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf)입니다.

## 사전 요구 사항 {#apple-sso-prerequisites}

온보딩 전제 조건은 프로그래머, MVPD, Adobe Pass 인증 또는 Apple과 같은 TVE 비즈니스와 관련된 하나 또는 여러 엔티티에 적용될 수 있습니다.

### 프로그래머 {#apple-sso-prerequisites-programmer}

SSO(Single Sign-On) 사용자 경험을 활용하려면 한 프로그래머가 다음을 수행해야 합니다.

* Apple에 문의하여 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount)를 Apple 팀 ID의 일부로 사용하도록 설정하고 [비디오 구독자 Single Sign-On 권한](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on)을 Apple 개발자 계정의 일부로 구성하십시오.

   * Xcode 버전 8 이상 및 iOS/tvOS 버전 10 이상을 사용합니다.

* [&#x200B; 속성을 &#x200B;](https://experience.adobe.com/#/pass/authentication)&#x200B;(으)로 설정하여 `Enable Single Sign On`iOS TVE 대시보드`Yes`를 통해 원하는 각 통합 및 플랫폼(Adobe Pass/tvOS)에 대해 SSO(Single Sign-On)를 사용하도록 설정하십시오.

| Adobe 단일 사인온 활성화 | Apple **온보딩된(지원됨)**&#x200B;개의 MVPD | Apple **선택기** MVPD | Apple **온보딩되지 않음(지원되지 않음)** MVPD |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| 예(활성화됨) | 인증 및 로그아웃 플로우에는 Apple 및 Adobe Pass 인증 솔루션이 모두 포함되며 다른 모든 플로우(인증, 사전 권한 부여, 메타데이터 등)는 Adobe Pass 인증에서만 서비스됩니다. | 인증 및 로그아웃 흐름은 Adobe Pass 인증에서만 서비스하는 일반 흐름으로 돌아갑니다. | 인증 및 로그아웃 흐름은 Adobe Pass 인증에서만 서비스하는 일반 흐름으로 돌아갑니다. |
| 아니요(비활성화됨) | 인증 및 로그아웃 흐름은 Adobe Pass 인증에서만 서비스하는 일반 흐름으로 돌아갑니다. | 인증 및 로그아웃 흐름은 Adobe Pass 인증에서만 서비스하는 일반 흐름으로 돌아갑니다. | 인증 및 로그아웃 흐름은 Adobe Pass 인증에서만 서비스하는 일반 흐름으로 돌아갑니다. |

* iOS, iPadOS 또는 tvOS에서 실행되는 클라이언트 애플리케이션의 최종 사용자를 위해 Adobe Pass 인증에서 제공하는 다음 솔루션 중 하나를 사용하여 SSO(Single Sign-On) 사용자 흐름을 통합합니다.

   * Adobe Pass 인증 REST API V2는 Partner SSO(Single Sign-On)를 지원합니다.

     [Apple SSO Cookbook(REST API V2)](apple-sso-cookbook-rest-api-v2.md) 설명서를 참조하십시오.

   * 기존 Adobe Pass 인증 REST API V1은 Partner SSO(Single Sign-On)를 지원합니다.

     [(기존) Apple SSO Cookbook(REST API V1)](../../../../legacy/sso-access/apple-sso-cookbook-rest-api-v1.md) 설명서를 참조하십시오.

   * 기존 Adobe Pass 인증 AccessEnabler iOS/tvOS SDK은 Partner SSO(Single Sign-On)를 지원합니다.

     [(기존) Apple SSO Cookbook(iOS/tvOS SDK)](../../../../legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md) 설명서를 참조하십시오.

### MVPD {#apple-sso-prerequisites-mvpd}

SSO(Single Sign-On) 사용자 경험을 활용하려면 하나의 MVPD에서 다음 작업을 수행해야 합니다.

* Apple 측에서 온보딩 프로세스를 시작하려면 Apple에 문의하십시오.

   * 사용자 로그인 양식을 처리할 수 있는 JavaScript TVML 애플리케이션을 통합하고 개발하는 방법에 대한 기술 설명서를 요청합니다.

* Adobe 측에서 온보딩 프로세스를 시작하려면 Adobe Pass 인증에 문의하십시오.

   * 온보딩 프로세스 중에 Apple에서 할당한 TV 공급자 식별자를 나타내는 문자열 값을 제공합니다.

## FAQ {#FAQ}

* Apple SSO 워크플로가 잘못된 경우 Adobe Pass Authentication AccessEnabler iOS/tvOS SDK을 사용하는 애플리케이션이 일반 인증 플로우로 돌아갈 수 있습니까?

  가능하지만, 원하는 통합 및 플랫폼(iOS/tvOS)에 대해 [아니요](https://experience.adobe.com/#/pass/authentication)에서 **SSO(Single Sign-On) 활성화**&#x200B;를 설정하기 위해 **Adobe Pass TVE 대시보드**&#x200B;를 통해 구성을 변경해야 합니다. 클라이언트 응용 프로그램이 [setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3) API를 호출한 후에만 구성 변경을 승인합니다.


* 애플리케이션이 Apple SSO를 통해 로그인한 결과 인증이 언제 발생했는지 알 수 있습니까?

  이 정보는 사용자 메타데이터 키 *tokenSource*&#x200B;의 일부로 사용할 수 있으며, 이 경우 문자열 값 &quot;Apple&quot;를 반환해야 합니다.


* 다른 애플리케이션에서 Apple SSO를 통해 로그인한 결과 인증이 언제 발생했는지 애플리케이션이 알 수 있습니까?

  이 정보는 사용할 수 없습니다.


* 사용자가 iOS/iPadOS의 *`Settings -> TV Provider`* 또는 tvOS 섹션의 *`Settings -> Accounts -> TV Provider`*(으)로 이동하여 응용 프로그램과 통합되지 않은 MVPD을 사용하여 로그인하면 어떻게 됩니까?

  사용자가 애플리케이션을 실행하면 Apple SSO 워크플로를 통해 사용자가 인증되지 않습니다. 따라서 애플리케이션은 일반 인증 플로우로 돌아가 자체 MVPD 선택기를 제공해야 합니다.


* 사용자가 iOS/tvOS 플랫폼용 *`Settings -> TV Provider`* Adobe Pass TVE 대시보드&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;를 통해 **아니요**&#x200B;에 **단일 사인온 사용**&#x200B;이 설정된 MVPD을 사용하여 iOS/iPadOS의 [&#x200B; 또는 tvOS의 &#x200B;](https://experience.adobe.com/#/pass/authentication) 섹션으로 이동하여 로그인하면 어떻게 됩니까?

  사용자가 애플리케이션을 실행하면 Apple SSO 워크플로를 통해 사용자가 인증되지 않습니다. 따라서 애플리케이션은 일반 인증 플로우로 돌아가 자체 MVPD 선택기를 제공해야 합니다.


* 사용자에게 Apple에서 온보딩하지 않은(지원되지 않는) MVPD이 있지만 Apple 선택기에 있는 경우 어떻게 됩니까?

  사용자가 애플리케이션을 실행하면 인증 플로우를 완료하지 않고 Apple SSO 워크플로를 통해서만 MVPD을 선택할 수 있습니다. 따라서 애플리케이션은 일반 인증 플로우로 돌아가야 하지만 이미 선택한 MVPD을 사용할 수 있습니다.


* 사용자에게 Apple에서 온보딩하지 않은(지원되지 않는) MVPD이 있는 경우 어떻게 됩니까?

  사용자가 애플리케이션을 시작하면 사용자는 Apple SSO 워크플로를 통해 &quot;기타 TV 공급자&quot; 선택 옵션을 선택합니다. 따라서 애플리케이션은 일반 인증 플로우로 돌아가 자체 MVPD 선택기를 제공해야 합니다.


* 사용자가 [Adobe Pass TVE 대시보드](https://experience.adobe.com/#/pass/authentication)를 통해 성능이 저하된 MVPD을 사용하는 경우 어떻게 됩니까?

  사용자가 애플리케이션을 실행하면 사용자는 Apple SSO 워크플로가 아닌 성능 저하 메커니즘을 통해 인증됩니다. Adobe Pass Authentication AccessEnabler iOS/tvOS SDK을 사용하는 경우 애플리케이션이 *N010* 경고 코드를 통해 알림을 받는 동안 사용자에게 원활한 경험이 제공되어야 합니다.


* Apple SSO와 비 Apple SSO 인증 흐름 간에 MVPD 사용자 ID가 변경됩니까?

  사용자 ID는 변경되지 않지만 선택한 각 공급자에 대해 확인해야 합니다.


* 인증 TTL에 변경 사항이 있습니까?

  Adobe Pass 인증은 프로그래머가 각 MVPD과의 통합에 필요한 TTL을 계속 준수합니다. Apple SSO를 통해 한 프로그래머 애플리케이션에서 다른 프로그래머 애플리케이션으로 이동할 때 두 번째 애플리케이션에는 해당 프로그래머 x MVPD 통합의 TTL이 있습니다(인증되는 첫 번째 애플리케이션의 TTL은 공유되지 않음)

|                                      | Adobe Pass 인증 TTL 만료됨 | Adobe Pass 인증 TTL 유효 |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **Apple의 장치 토큰 TTL 만료** | 사용자가 인증되지 않았습니다(MVPD 선택기가 표시되어야 함). | 사용자는 인증되고 TTL은 Adobe Pass 인증 토큰/프로필의 남은 시간입니다 |
| **Apple의 장치 토큰 TTL 유효** | 사용자가 자동으로 인증되어 TVE 대시보드에 지정된 TTL로 다른 Adobe Pass 인증 토큰/프로필을 가져옵니다 | 사용자는 인증되고 TTL은 Adobe Pass 인증 토큰/프로필의 남은 시간입니다 |
