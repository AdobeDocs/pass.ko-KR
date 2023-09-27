---
title: Apple SSO 개요
description: Apple SSO 개요
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1452'
ht-degree: 0%

---

# Apple SSO 개요 {#apple-sso-overview}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 소개 {#Introduction}

Apple은 사용자가 장치 시스템 수준에서 TV 공급자 계정에 로그인할 수 있는 API를 제공하여 앱별로 인증할 필요가 없습니다.

따라서 Apple과 Adobe Pass 인증은 iPhone, iPad 및 Apple TV 소유자를 위한 TV Everywhere 생태계에서 플랫폼 SSO(Single Sign-On) 사용자 경험을 구축하기 위해 파트너 관계를 맺었습니다.

Apple 장치에서 SSO(Single Sign-On) 사용자 경험을 활용하려면 완료해야 하는 필수 구성 요소 목록이 있습니다.

</br>

## 전제 조건 {#Prerequisites}

프로그래머, MVPD, Adobe Pass 인증 또는 Apple 등 TVE 비즈니스와 관련된 하나 또는 여러 엔터티에 전제 조건이 적용될 수 있습니다.

</br>

### 프로그래머 {#Programmer}

SSO(Single Sign-On) 사용자 경험을 활용하려면 한 프로그래머가 다음을 수행해야 합니다.

1. Xcode 버전 8 이상 및 iOS/tvOS 버전 10 이상을 사용하십시오.

1. 다음 권한 보유: [비디오 구독자 단일 사인온 권한](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) Apple 개발자 계정에 구성되었습니다. 활성화하려면 Apple에 문의하십시오. [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount) Apple 팀 ID용

1. 를 통해 원하는 각 통합(Channel x MVPD) 및 원하는 플랫폼(iOS / tvOS)에 대해 SSO(Single Sign-On) 활성화 [Adobe Primetime TVE 대시보드](https://console.auth.adobe.com/).

1. Adobe Pass 인증 팀에서 제공하는 다음 두 가지 솔루션 중 하나를 사용하여 Apple SSO 워크플로를 통합합니다.

   - Adobe Pass 인증 REST API는 iOS, iPadOS 또는 tvOS에서 실행되는 클라이언트 애플리케이션의 최종 사용자를 위한 플랫폼 SSO(Single Sign-On) 인증을 지원할 수 있습니다. 도 참조하십시오. [Apple SSO Cookbook (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md).

   - Adobe Pass Authentication AccessEnabler iOS/tvOS SDK는 iOS, iPadOS 또는 tvOS에서 실행되는 클라이언트 애플리케이션의 최종 사용자에 대한 플랫폼 SSO(Single Sign-On) 인증을 지원할 수 있습니다. 도 참조하십시오. [Apple SSO Cookbook(iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md).

   - **<u>Pro 팁:</u>** 사용자의 구독 정보에 액세스하려면 장치의 카메라 또는 마이크에 대한 액세스를 제공하는 것과 유사하게, 사용자는 진행할 수 있도록 애플리케이션에 권한을 부여해야 합니다. 이 권한은 애플리케이션별로 요청해야 하며 디바이스는 사용자 선택을 저장합니다. 사용자가 애플리케이션 설정(TV 공급자 권한 액세스) 또는 의 섹션으로 이동하여 결정을 변경할 수 있음을 염두에 두십시오 *`Settings -> TV Provider`* iOS/iPadOS에서 또는 *`Settings -> Accounts -> TV Provider`* tvOS에서

   - **<u>Pro 팁:</u>** 애플리케이션이 전경 상태로 전환되면 사용자의 권한을 요청하는 것이 좋지만, 애플리케이션이 다음을 확인할 수 있으므로 제안일 뿐입니다 [액세스 권한](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) 사용자 인증이 필요하기 전에 언제든지 사용자의 구독 정보입니다. 또한 AccessEnabler iOS/tvOS SDK API는 필요할 때 자동으로 사용자의 권한을 요청합니다.

   - **<u>Pro 팁:</u>** SSO(Single Sign-On) 사용자 경험의 이점을 설명하여 구독 정보에 대한 액세스 권한을 거부하는 사용자에게 인센티브를 제공하는 것이 좋습니다. 사용자가 애플리케이션 설정(TV 공급자 권한 액세스) 또는 의 섹션으로 이동하여 결정을 변경할 수 있음을 염두에 두십시오 *`Settings -> TV Provider`* iOS/iPadOS에서 또는 *`Settings -> Accounts -> TV Provider`* tvOS에서

결과는 다음 사용자 플로우에 따라 경험을 만들어야 합니다. 이 워크플로우는 애플리케이션 개발을 시작하기 전에 알아 두는 것이 좋습니다.

- [IPHONE / IPAD](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf) 사용자 흐름
- [Apple](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) 사용자 흐름


>[!IMPORTANT]
>
> 단일 사인온 기능이 **활성화됨** iOS/tvOS용 **및** Apple의 경우 **온보딩된(지원됨) 또는 선택기** MVPD Apple SSO 워크플로에서 인증/로그아웃 흐름에는 Apple 및 Adobe Pass 인증 솔루션이 모두 포함되며, 다른 모든 흐름(권한 부여, 사전 권한 부여, 메타데이터 등)은 포함됩니다. 는 Adobe Pass 인증만으로 서비스됩니다.


>[!IMPORTANT]
>
> 단일 사인온 기능이 **비활성화됨** iOS/tvOS용 **또는** Apple의 경우 **온보딩되지 않음(지원되지 않음)** MVPD의 인증/로그아웃 흐름은 Apple SSO 워크플로에서 Adobe Pass 인증만 제공하는 일반 워크플로로 대체됩니다.


>[!IMPORTANT]
>
> Apple SSO 워크플로의 한 가지 주요 이점은 SSO(Single Sign-On) 기능이 있을 때 Apple TV에서 제공할 수 있는 단일 화면 인증 사용자 흐름으로 표시됩니다 **활성화됨** tvOS용 **및** Apple의 경우 **온보딩(지원됨)** MVPD.


### MVPD {#MVPD}

SSO(Single Sign-On) 사용자 경험을 활용하려면 한 MVPD가 다음을 수행해야 합니다.



1. Apple 측의 Apple SSO 워크플로에 온보딩됩니다. 온보딩 프로세스를 용이하게 하려면 Apple에 문의하십시오.
1. 사용자 로그인 양식을 처리할 수 있는 JavaScript TVML 애플리케이션을 제공합니다. 적절한 문서를 받으려면 Apple에 문의하십시오.
1. 온보딩 프로세스 중에 Apple에서 할당한 공급자 식별자를 나타내는 문자열 값을 제공합니다. 구성을 변경하려면 Adobe Pass 인증에 문의하십시오.

</br>

## FAQ {#FAQ}

1. Apple SSO 워크플로에서 문제가 발생하는 경우 AccessEnabler iOS/tvOS SDK를 사용하는 애플리케이션이 일반 인증 플로우로 전환할 수 있습니까?
   - 이 작업은 가능하지만 [Adobe Primetime TVE 대시보드](https://console.auth.adobe.com/). 다음 *단일 사인온 활성화* 은(는) 다음에 설정되어야 합니다. *아니요* 원하는 통합(Channel x MVPD)과 원하는 플랫폼(iOS/tvOS)에 사용할 수 있습니다.
   - 애플리케이션이 호출 후에만 구성 변경을 승인합니다. [setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) AccessEnabler iOS/tvOS SDK를 사용하는 경우 API입니다.
1. 다른 장치 또는 다른 애플리케이션에서 플랫폼 SSO를 통해 로그인한 결과 인증이 언제 발생했는지 애플리케이션이 알 수 있습니까?
   - 이 정보는 사용할 수 없습니다.
1. 동일한 장치에서 플랫폼 SSO를 통해 로그인하여 인증이 언제 발생했는지 애플리케이션이 알 수 있습니까?
   - 이 정보는 사용자 메타데이터 키의 일부로 사용할 수 있습니다. *tokenSource*&#x200B;문자열 값: 이 경우 &quot;Apple&quot;를 반환합니다.
1. 사용자가 로 이동하여 로그인하면 어떻게 됩니까? *`Settings -> TV Provider`* iOS/iPadOS에서 또는 *`Settings -> Accounts -> TV Provider`* 애플리케이션과 통합되지 않은 MVPD를 사용하는 tvOS 섹션
   - 사용자가 애플리케이션을 실행하면 Apple SSO 워크플로를 통해 사용자가 인증되지 않습니다. 따라서 응용 프로그램은 일반 인증 흐름으로 폴백하여 자체 MVPD 선택기를 표시해야 합니다.
1. 사용자가 로 이동하여 로그인하면 어떻게 됩니까? *`Settings -> TV Provider`* iOS/iPadOS에서 또는 *`Settings -> Accounts -> TV Provider`* MVPD를 사용하는 tvOS 섹션에서 *단일 사인온 활성화* 설정 *아니요* 다음에 있음 [Adobe Primetime TVE 대시보드](https://console.auth.adobe.com/) iOS/tvOS 플랫폼용
   - 사용자가 애플리케이션을 실행하면 Apple SSO 워크플로를 통해 사용자가 인증되지 않습니다. 따라서 응용 프로그램은 일반 인증 흐름으로 폴백하여 자체 MVPD 선택기를 표시해야 합니다.
1. 사용자에게 Apple에서 온보딩하지 않았지만(지원되지 않음) Apple 선택기에 있는 MVPD가 있는 경우 어떻게 됩니까?
   - 사용자가 애플리케이션을 실행하면 사용자는 인증 플로우를 완료하지 않고 Apple SSO 워크플로를 통해서만 MVPD를 선택합니다. 따라서 애플리케이션은 일반 인증 플로우로 폴백해야 하지만 이미 선택한 MVPD를 사용할 수 있습니다.
1. 사용자에게 Apple에서 온보딩하지 않은(지원되지 않는) MVPD가 있는 경우 어떻게 됩니까?
   - 사용자가 애플리케이션을 시작하면 사용자는 Apple SSO 워크플로를 통해 &quot;기타 TV 공급자&quot; 선택 옵션을 선택합니다. 따라서 응용 프로그램은 일반 인증 흐름으로 폴백하여 자체 MVPD 선택기를 표시해야 합니다.
1. 사용자가 의 매체를 통해 성능이 저하되는 MVPD를 보유한 경우 어떻게 됩니까? [Adobe Primetime TVE 대시보드](https://console.auth.adobe.com/)?
   - 사용자가 애플리케이션을 실행하면 사용자는 Apple SSO 워크플로가 아닌 성능 저하 메커니즘을 통해 인증됩니다.
   - 사용자에게는 경험이 원활하게 제공되어야 하며 애플리케이션에는 를 통해 정보가 전달됩니다. *N* AccessEnabler iOS/tvOS SDK를 사용하는 경우 경고 코드.
1. Apple SSO와 비 Apple SSO 인증 흐름 간에 MVPD 사용자 ID가 변경됩니까?
   - 사용자 ID는 변경되지 않지만 선택한 각 공급자에 대해 확인해야 합니다.
1. 인증 TTL에 변경 사항이 있습니까?
   - Adobe Pass 인증은 프로그래머가 각 MVPD와 통합하는 데 필요한 TTL을 계속 준수합니다.
   - Apple SSO를 통해 한 프로그래머 애플리케이션에서 다른 프로그래머 애플리케이션으로 이동할 때 두 번째 애플리케이션은 해당 프로그래머 x MVPD 통합의 TTL을 갖게 됩니다(인증되는 첫 번째 애플리케이션의 TTL을 공유하지 않음)

|                                      | Adobe Pass 인증 TTL 만료됨 | Adobe Pass 인증 TTL 유효 |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Apple의 장치 토큰 TTL 만료됨** | 사용자가 인증되지 않았습니다(MVPD 선택기가 표시되어야 함). | 사용자는 인증되고 TTL은 Adobe Pass 인증 토큰의 남은 시간입니다 |
| **Apple의 장치 토큰 TTL 유효** | 사용자가 자동으로 인증되어 TVE 대시보드에 지정된 TTL로 다른 Adobe Pass 인증 토큰을 얻습니다 | 사용자는 인증되고 TTL은 Adobe Pass 인증 토큰의 남은 시간입니다 |

<!--

## Resources {#Resources}

- [Apple SSO Cookbook (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md)
- [Apple SSO Cookbook (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md)
- [Sign in with your TV provider on your iPhone, iPad, or iPod touch](https://support.apple.com/en-us/HT207035)
- [Use your pay TV or cable provider with Apple TV](https://support.apple.com/en-us/HT207035)
- [TV providers that let you sign in on your iPhone, iPad, or Apple TV](https://support.apple.com/en-us/HT208084)
- [TV Provider Authentication](https://developer.apple.com/design/human-interface-guidelines/tvos/system-capabilities/tv-provider-authentication/)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
