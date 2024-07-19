---
title: iOS SDK 3.1+에서 WKWebView 지원
description: iOS SDK 3.1+에서 WKWebView 지원
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# iOS SDK 3.1 이상에서 WKWebView 지원 {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>

**Apple이 iOS에서 UIWebView를 사용하지 않기 때문에 WKWebView를 지원하도록 iOS SDK 3.1을 업데이트했습니다.**

## 호환성 {#compatibility}

iOS SDK 버전 3.1부터 구현자는 이제 WKWebView 또는 UIWebView를 서로 교환하여 사용할 수 있습니다. UIWebView는 Apple에서 더 이상 사용되지 않으므로 향후 iOS 버전과 관련된 문제를 방지하려면 앱을 WKWebView로 마이그레이션해야 합니다.

마이그레이션은 단순히 UIWebView 클래스를 WKWebView로 전환한다는 것을 의미하므로 Adobe의 AccessEnabler에 대해서는 수행할 작업이 없습니다.

## 알려진 문제 {#known-issues}

Adobe의 AccessEnabler가 숨겨진 내부 UIWebView 인스턴스를 사용하여 특정 MVPD에 대해 &quot;[수동 인증](/help/authentication/sso-passive-authn.md)&quot;을 수행했습니다. &quot;수동&quot; 플로우는 각 요청자 ID에 대한 인증이 필요한 MVPD에 유용했으며, 이 플로우는 SSO 경험(Adobe SSO)을 시뮬레이션하기 위해 여러 iOS 애플리케이션에서 동일한 팀 ID를 사용한 프로그래머에게 도움이 되었습니다. 이 기능은 현재 제한된 수의 MVPD에서 사용되고 있습니다.

이 기능에서는 Adobe이 &quot;수동&quot; 흐름 동안 인증 쿠키를 캡처하고 재생할 수 있도록 하는 UIWebView의 동작을 사용했습니다. WKWebView는 Adobe이 로그인 시 설정된 쿠키를 캡처하고 WKWebView의 숨겨진 인스턴스를 사용하여 해당 쿠키를 재생하지 못하도록 하는 강력한 보안을 도입했습니다. 이러한 보안 개선으로 인해 &quot;수동&quot; 흐름이 매우 구체적인 구현 시나리오(동일한 팀 ID를 사용하는 여러 애플리케이션)에서 매우 제한된 MVPD 세트에만 도움이 되는 것을 고려하여 Adobe은 웹 보기를 사용하여 MVPD에 대한 &quot;수동 인증&quot; 기능을 제거했습니다.

이 기능은 SFSafariViewController를 사용하도록 구성된 MVPD에 대해 여전히 존재하지만, 이 경우 SFSafariViewController를 &quot;숨겨진&quot; 방식으로 사용할 수 없으므로 &quot;수동&quot; 인증이 사용자에게 표시됩니다.
