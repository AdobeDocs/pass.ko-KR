---
title: iOS/tvOS v3.x 마이그레이션 안내서
description: iOS/tvOS v3.x 마이그레이션 안내서
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 0%

---

# iOS/tvOS v3.x 마이그레이션 안내서 {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!TIP]
> 
> **참고:**
>
> - iOS sdk 버전 3.1부터 구현자는 이제 WKWebView 또는 UIWebView를 서로 교환하여 사용할 수 있습니다. UIWebView는 더 이상 사용되지 않으므로 향후 iOS 버전과 관련된 문제를 방지하려면 앱을 WKWebView로 마이그레이션해야 합니다.
> - 마이그레이션은 단순히 UIWebView 클래스를 WKWebView로 전환한다는 것을 의미하므로 Adobe의 AccessEnabler에 대해서는 수행할 작업이 없습니다.

</br>

## 빌드 설정 업데이트 {#update}

이 릴리스에는 SWIFT 언어로 작성된 기능이 포함되어 있습니다. 앱이 전적으로 Objective-C인 경우 타겟의 빌드 설정에서 &quot;Always Embed Swift Standard Libraries&quot; 확인란을 &quot;예&quot;로 설정해야 합니다. 이 옵션이 설정되면 Xcode는 앱에서 번들 프레임워크를 스캔하며, 이 중 하나라도 Swift 코드를 포함하는 경우 해당 라이브러리를 앱의 번들로 복사합니다. 빌드 설정을 업데이트하지 않으면 앱이 AccessEnabler.framework 또는 다양한 기능을 로드할 수 없다는 오류와 함께 충돌할 수 있습니다 `ibswift*` 라이브러리.

</br>

## 소프트웨어 구문 추가 {#add}

> 소프트웨어 명령문을 얻는 방법에 대한 자세한 내용은 다음을 참조하십시오.
> 페이지:
> [응용 프로그램 등록](/help/authentication/iostvos-application-registration.md)

소프트웨어 문이 있는 경우 App Store에 새 버전의 애플리케이션을 배포하지 않고도 쉽게 취소하거나 변경할 수 있도록 원격 서버에서 호스팅하는 것이 좋습니다. 애플리케이션이 시작되면 원격 위치에서 소프트웨어 명령문을 가져와 AccessEnabler 생성자에 전달합니다.

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> API 정보 위치: [iOS / tvOS API 참조](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## 사용자 지정 URL 체계 추가 {#add-custom}

> 사용자 지정 URL 체계를 가져오는 방법에 대한 자세한 내용을 보려면 이 페이지로 이동하십시오. [고객 URL 체계 얻기](/help/authentication/iostvos-application-registration.md)

사용자 지정 URL 체계를 가져온 후에는 응용 프로그램의 info.plist 파일에 추가해야 합니다. 사용자 지정 구성표의 형식은 다음과 같습니다. `adbe.u-XFXJeTSDuJiIQs0HVRAg://`. 파일에 추가할 때 콜론과 슬래시를 생략해야 합니다. 위의 예는 다음과 같습니다 `adbe.u-XFXJeTSDuJiIQs0HVRAg`.

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>CUSTOM_URL_SCHEME_HERE</string>
            </array>
        </dict>
    </array>
```

</br>

## 사용자 지정 URL 체계에서 호출 가로채기 {#intercept}

이는 애플리케이션이 이전에 를 통해 수동 Safari View Controller(SVC) 처리를 활성화한 경우에만 적용됩니다. [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](/help/authentication/iostvos-sdk-api-reference.md) SVC(Safari View Controller)가 필요한 특정 MVPD에 대해 및 를 호출하므로 UIWebView/WKWebView 컨트롤러 대신 SFSafariViewController 컨트롤러를 사용하여 인증 및 로그아웃 끝점의 URL을 로드해야 합니다.

인증 및 로그아웃 흐름 중에 애플리케이션은 의 활동을 모니터링해야 합니다. `SFSafariViewController `몇 번의 리디렉션을 거치는 동안 컨트롤러에서 작동합니다. 응용 프로그램은 사용자가 정의한 특정 사용자 정의 URL을 로드하는 순간을 감지해야 합니다. `application's custom URL scheme` (예:`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`. 제어기가 이 특정 사용자 지정 URL을 로드하면 애플리케이션이 `SFSafariViewController` 및 AccessEnabler 호출 `handleExternalURL:url `API 메서드.

내 `AppDelegate` 다음 메서드를 추가합니다.

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> API 정보 위치: [외부 URL 처리](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## setRequestor 메서드 서명 업데이트 {#update-setreq}

새 SDK는 새 인증 메커니즘을 사용하므로 signedRequestId 매개 변수 또는 공개 키 및 암호(tvOS용)가 필요하지 않습니다. 다음 `setRequestor` 메서드는 단순화되고 requestorID만 필요합니다.

### iOS

이 코드:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId)
```

다음과 같이 됨:

```swift
    accessEnabler.setRequestor(requestorId)
```

</br>

### tvOS

이 코드:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId,
                    secret: "secret", publicKey: "public_key")
```

다음과 같이 됨:

```swift
    accessEnabler.setRequestor(requestorId)
```

> API 정보 위치: [요청자 설정](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## getAuthenticationToken 메서드를 handleExternalURL 메서드로 바꾸기 {#replace}

`getAuthentication` 이전에는 인증 흐름을 완료하는 데 메서드가 사용되었습니다. 이름이 잘못 알려져 이름이 로 변경되었습니다. `handleExternalURL` 및 는 url을 매개 변수로 사용합니다.

다음의 모든 발생 횟수 변경:

```swift
    accessEnabler.getAuthenticationToken()
```

이를 통해:

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> API 정보 위치: [외부 URL 처리](/help/authentication/iostvos-sdk-api-reference.md)
