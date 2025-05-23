---
title: iOS 인증 오류 - adobepass.ios.app을 찾을 수 없음
description: iOS 인증 오류 - adobepass.ios.app을 찾을 수 없음
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---

# (기존) iOS 인증 오류 - adobepass.ios.app을 찾을 수 없음 {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 문제 {#issue}

사용자가 인증 흐름을 통과하고 있으며 공급자로 자격 증명을 입력한 후 오류 페이지, 검색 페이지 또는 `adobepass.ios.app`을(를) 찾을 수 없음/해결할 수 없음을 알리는 다른 사용자 지정 페이지로 다시 리디렉션됩니다.

## 설명 {#explanation}

iOS에서 `adobepass.ios.app`은(는) AuthN 흐름이 완료되었음을 나타내는 최종 리디렉션 URL로 사용됩니다. 이 시점에서 앱은 AccessEnabler에 AuthN 토큰을 가져오고 AuthN 흐름을 완료하도록 요청해야 합니다.

문제는 `adobepass.ios.app`이(가) 실제로 존재하지 않으며 `webView`에 오류 메시지를 트리거한다는 것입니다. 이전 버전의 iOS DemoApp에서는 이 오류가 항상 AuthN 흐름이 끝날 때 트리거된다고 가정했으며 이에 따라 처리하도록 설정되었습니다(`indidFailLoadWithError`).

**참고:** 이 문제는 이후 버전의 DemoApp(iOS SDK 다운로드에 포함됨)에서 수정되었습니다.

불행히도, 이 가정은 옳지 않다. 발생한 오류를 단순히 전달하지 않고 대신 다음 중 하나를 수행하는 &quot;스마트&quot; DNS 또는 프록시 서버가 있습니다.

- 사용자 지정 오류 페이지 만들기
- 검색 페이지나 다른 유형의 고객 페이지 또는 포털로 전달합니다.

이러한 경우 iOS webView로 돌아오는 응답은 webView에 관한 한 매우 유효한 응답이며 이전 DemoApp이 의존했던 오류를 트리거하지 않습니다.

## 솔루션 {#solution}

DemoApp과 동일한 가정을 하지 마십시오. 대신 요청이 실행되기 전에(`shouldStartLoadWithRequest`에서) 가로채서 적절하게 처리합니다.

요청이 실행되기 전에 가로채는 방법의 예:

```obj-c
- (BOOL)webView:(UIWebView*)localWebView shouldStartLoadWithRequest:(NSURLRequest*)request navigationType:(UIWebViewNavigationType)navigationType {

NSString *absolutePath = [[request URL] absoluteString]; 
if ([absolutePath isEqualToString:ADOBEPASS_REDIRECT_URL] && ![APP_DELEGATE getAuthenticationWasCalled]) {

// user was logged ok => call getAuthenticationToken() 
[APP_DELEGATE setGetAuthenticationWasCalled:YES]; 
[[APP_DELEGATE accessEnabler] getAuthenticationToken];
return NO;

}

return YES;

}
```

몇 가지 주의해야 할 사항:

- 코드의 아무 곳에나 `adobepass.ios.app`을(를) 직접 사용하지 마십시오. 대신 상수 `ADOBEPASS_REDIRECT_URL`을(를) 사용하십시오.
- `return NO;` 문으로 인해 페이지가 로드되지 않습니다.
- `getAuthenticationToken` 호출이 코드에서 한 번만 호출되는지 확인하십시오. `getAuthenticationToken`을(를) 여러 번 호출하면 결과가 정의되지 않습니다.
