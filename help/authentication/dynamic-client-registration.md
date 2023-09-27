---
title: 동적 클라이언트 등록
description: 동적 클라이언트 등록
exl-id: 9bc2597d-b634-4542-849b-8e91a76cb8da
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# 동적 클라이언트 등록 {#dynamic-client-registration}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 컨텍스트 {#context}

최신 보안 방식과 향상된 UX 및 플랫폼 소유자 요구 사항에 맞게 Adobe Pass 인증 Android SDK 및 iOS SDK는 을 채택하는 방향으로 나아가고 있습니다 [Android Chrome 사용자 지정 탭](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} and [Apple Safari view controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}.

현재 AdobePass 구현에서는 MVPD 로그인 페이지를 표시하는 웹 환경을 제공하기 위해 플랫폼별 웹 보기를 사용합니다. 이러한 웹 보기는 플랫폼 브라우저와 자격 증명 관리를 공유하지 않으므로 Adobe Pass 인증 애플리케이션을 사용할 때 브라우저가 저장한 암호를 사용할 수 없습니다. 또한 보안상의 이유로 일부 플랫폼에서는 인증 작업을 위해 WebView 컨트롤러를 사용하지 않는 방향으로 전환하고 있습니다. Google과 Apple 모두 &quot;Chrome 사용자 지정 탭&quot; 및 &quot;Safari 보기 컨트롤러&quot;와 같은 대체 옵션을 제공합니다. 각 브라우저의 단일 사용 탭입니다. Adobe Pass 인증은 2018년에 이러한 새로운 구성 요소를 채택할 예정입니다.

## 세부 사항 {#details}

현재 Adobe Pass 인증에서 애플리케이션을 식별하고 등록하는 두 가지 방법이 있습니다.

* 브라우저 기반 클라이언트는 허용된 도메인 목록을 통해 등록됩니다
* iOS 및 Android 애플리케이션과 같은 기본 애플리케이션 클라이언트는 서명된 요청자 메커니즘을 통해 등록됩니다

Chrome 사용자 지정 탭 및 Safari 보기 컨트롤러에 대한 새로운 흐름을 해결하기 위해 Adobe Pass은 새 애플리케이션을 등록하기 위한 새로운 클라이언트 등록 메커니즘을 제안합니다. 이 메커니즘을 사용하면 애플리케이션을 보다 안전하고 세밀하게 제어할 수 있으며 모든 플랫폼에서 애플리케이션을 등록하는 데 이 메커니즘을 사용할 수 있습니다.

<!--
## Related Information

- [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
- [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)
-->
