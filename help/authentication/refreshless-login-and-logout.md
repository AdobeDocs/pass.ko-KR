---
title: 새로 고침 없이 로그인 및 로그아웃
description: 새로 고침 없이 로그인 및 로그아웃
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1772'
ht-degree: 0%

---

# 새로 고침 없이 로그인 및 로그아웃 {#tefresh-less-login-and-logout}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

웹 애플리케이션의 경우 사용자 인증 및 로그아웃을 위해 가능한 몇 가지 시나리오를 고려해야 합니다.  MVPD를 인증하려면 사용자가 MVPD의 웹 페이지에 로그인해야 하며, 다음과 같은 추가 요소가 작용합니다.

- 일부 MVPD는 사이트에서 로그인 페이지로 전체 리디렉션해야 합니다
- 일부 MVPD에서는 사이트에서 iFrame을 열어 MVPD의 로그인 페이지를 표시해야 합니다
- 일부 브라우저에서는 iFrame 시나리오를 잘 처리하지 않으므로 이러한 브라우저에서는 iFrame 대신 팝업 창을 사용하는 것이 더 좋습니다

Adobe Pass 인증 2.7 이전에는 이러한 모든 사용자 인증 시나리오에 프로그래머 페이지의 전체 페이지 새로 고침이 포함되었습니다.2.7 및 이후 릴리스의 경우 Adobe Pass 인증 팀은 이러한 흐름을 개선하여 사용자가 로그인 및 로그아웃 중에 앱에서 페이지 새로 고침을 경험할 필요가 없도록 했습니다.


## 자세한 설명 {#detailed_description}

원래 인증 및 로그아웃 흐름에 대한 요약으로 시작한 다음 개선된 인증 및 로그아웃 흐름에 따라 설명하겠습니다. 처음 네 개의 섹션은 일반적인 MVPD(비 TempPass)를 처리하는 반면, 마지막 섹션은 TempPass에 적용해야 하는 특수 구현을 설명합니다.

- [원본 인증 흐름](#orig_authn)
- [원래 로그아웃 흐름](#orig_logout)
- [향상된 인증 흐름](#improved_authn)
- [로그아웃 플로우 개선](#improved_logout)
- [TempPass 흐름](#improved_temppass)

</br>

## 원래 인증/로그아웃 흐름 {#orig_authn}

**인증**

Adobe Pass 인증 웹 클라이언트에는 MVPD의 요구 사항에 따라 두 가지 인증 방법이 있습니다.

1. **전체 페이지 리디렉션 -** 사용자가 프로그래머 웹 사이트의 MVPD 선택기에서 공급자(전체 페이지 리디렉션으로 구성)를 선택한 후 `setSelectedProvider(<mvpd>)` 이 호출되면 사용자는 MVPD의 로그인 페이지로 리디렉션됩니다. 사용자가 유효한 자격 증명을 제공하면 프로그래머 웹 사이트로 다시 리디렉션됩니다. AccessEnabler가 초기화되고 다음 기간 동안 Adobe Pass 인증에서 인증 토큰이 검색됩니다 `setRequestor`.
1. **iFrame/팝업 창 -** 사용자가 공급자(iFrame으로 구성)를 선택한 후 `setSelectedProvider(<mvpd>)` AccessEnabler에서 를 호출합니다. 이 작업은 `createIFrame(width, height)` 콜백, 프로그래머에게 이름이 있는 iFrame(또는 브라우저/환경 설정에 따라 팝업)을 만들도록 알리는 것 `"mvpdframe"` 및 제공된 차원. iFrame/popup이 생성되면 AccessEnabler는 MVPD의 로그인 페이지를 iFrame/popup에 로드합니다. 사용자가 유효한 자격 증명을 제공하고 iFrame/popup이 Adobe Pass 인증으로 리디렉션되며, iFrame/popup을 닫고 상위 페이지(프로그래머 웹 사이트)를 다시 로드하는 JS 코드 조각을 반환합니다. 흐름 1과 유사하게, 인증 토큰은 `setRequestor`.

다음 `displayProviderDialog` callback (트리거됨) `getAuthentication`/`getAuthorization`)는 MVPD 목록과 적절한 설정을 반환합니다. 다음 `iFrameRequired` MVPD의 속성을 사용하면 프로그래머가 흐름 1을 활성화해야 하는지 흐름 2를 활성화해야 하는지 알 수 있습니다. 프로그래머는 흐름 2에 대해서만 추가 작업(iFrame/팝업 생성)을 수행해야 합니다.

**인증 취소**

사용자가 로그인 페이지를 닫아 인증 흐름을 명시적으로 취소하는 상황도 있다. 다음은 프로그래머에게 제안된 시나리오와 솔루션입니다.

1. **전체 페이지 리디렉션 -** 로그인 페이지를 닫으면 사용자는 프로그래머 웹 사이트로 다시 이동하여 처음부터 전체 흐름을 시작해야 합니다. 이 시나리오에서는 프로그래머 측에서 명시적인 작업이 필요하지 않습니다.
1. **iFrame -** 프로그래머는 iFrame을 `div` (또는 유사한 UI 구성 요소)에 닫기 버튼이 연결되어 있습니다. 사용자가 닫기 단추를 누르면 프로그래머가 관련 UI와 함께 iFrame을 제거하고 다음을 수행합니다 `setSelectedProvider(null)`. 이 호출을 통해 AccessEnabler는 내부 상태를 지우고 사용자가 후속 인증 흐름을 시작할 수 있습니다. `setAuthenticationStatus` 및 `sendTrackingData(AUTHENTICATION_DETECTION...)` 이(가) 실패한 인증 흐름을 알리기 위해 트리거됩니다(둘 다 `getAuthentication` 및 `getAuthorization`).
1. **팝업 -** 일부 브라우저에서는 창 닫기 이벤트를 정확하게 감지할 수 없으므로 다른 접근 방식을 사용해야 합니다(위의 iFrame 흐름과 대조됨). Adobe은 프로그래머가 로그인 팝업이 있는지 주기적으로 확인하는 타이머를 초기화할 것을 권장합니다. 창이 없으면 프로그래머는 사용자가 로그인 흐름을 수동으로 취소했는지 확인하고 호출을 계속할 수 있습니다 `setSelectedProvider(null)`. 트리거된 콜백은 위의 흐름 2와 동일합니다.

</br>

## 원래 로그아웃 흐름 {#orig_logout}

AccessEnabler의 로그아웃 API는 라이브러리의 로컬 상태를 지우고 현재 탭/창에 MVPD의 로그아웃 URL을 로드합니다. 브라우저는 MVPD의 로그아웃 끝점으로 이동하고 프로세스가 완료되면 사용자는 프로그래머 웹 사이트로 다시 리디렉션됩니다. 사용자를 대신하여 필요한 유일한 작업은 로그아웃 단추/링크를 누르고 흐름을 시작하는 것입니다. MVPD의 로그아웃 끝점에는 사용자 상호 작용이 필요하지 않습니다.

**페이지 새로 고침을 통한 원래 인증/로그아웃 흐름**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## 향상된(새로 고침 불필요) 인증 {#improved_authn}

>[!NOTE]
>
>향상된 새로 고침 없는 로그인 및 로그아웃 흐름을 사용하려면 브라우저가 웹 메시지를 포함한 최신 HTML5 기술을 지원해야 합니다.

위에서 설명한 인증(로그인) 및 로그아웃 흐름 모두 각 흐름이 완료된 후 기본 페이지를 다시 로드하여 유사한 사용자 경험을 제공합니다.  현재 기능은 새로 고침 없는(백그라운드) 로그인 및 로그아웃을 제공하여 사용자 경험을 개선하는 것을 목표로 합니다. 프로그래머는 두 개의 부울 플래그( )를 전달하여 백그라운드 로그인 및 로그아웃을 활성화/비활성화할 수 있습니다.`backgroundLogin` 및 `backgroundLogout`) 로 이동합니다. `configInfo` 매개 변수 `setRequestor` API. 기본적으로 백그라운드 로그인/로그아웃은 비활성화됩니다(이전 구현과의 호환성을 제공).

**예:**

```JSON
    var configInfo = {
        callSetConfig: true,
        backgroundLogin: true,
        backgroundLogout: true
    };
    accessEnabler.setRequestor(REQUESTOR_ID, null, configInfo);
```

**인증**

다음은 원래 인증 흐름과 향상된 흐름 간의 전환을 설명하는 지점입니다.

1. 전체 페이지 리디렉션은 MVPD 로그인이 수행되는 새 브라우저 탭으로 대체됩니다. 프로그래머는 (를 통해) 새 탭을 만들어야 합니다. `window.open`) `mvpdwindow` 사용자가 MVPD를 선택할 때(와 함께) `iFrameRequired = false`). 그러면 프로그래머가 `setSelectedProvider(<mvpd>)`를 사용하여 AccessEnabler가 새 탭에서 MVPD 로그인 URL을 로드할 수 있습니다. 사용자가 유효한 자격 증명을 제공한 후 Adobe Pass Authentication에서 탭을 닫고 AccessEnabler에 인증 흐름이 완료되었음을 알리는 window.postMessage를 프로그래머 웹 사이트로 보냅니다. 다음 콜백이 트리거됩니다.

   - 플로우가에 의해 시작된 경우 `getAuthentication`: `setAuthenticationStatus` 및 `sendTrackingData(AUTHENTICATION_DETECTION...)` 인증 성공/실패 신호를 보내도록 트리거됩니다.

   - 플로우가에 의해 시작된 경우 `getAuthorization`: `setToken/tokenRequestFailed` 및 `sendTrackingData(AUTHORIZATION_DETECTION...)` 인증 성공/실패 신호를 보내도록 트리거됩니다.

1. iFrame/팝업 창 흐름은 대부분 변경되지 않고 유지됩니다. 차이점은 사용자가 유효한 자격 증명을 제공한 후 상위 페이지가 다시 로드되지 않는다는 것입니다. 로그인 후 iFrame/팝업이 자동으로 닫히고 `window.postMessage` 이 상위 페이지로 전송되어 흐름이 완료되었음을 AccessEnabler에 알립니다. 이전 흐름에서와 동일한 콜백이 트리거됩니다. **여기에 다음 새 콜백을 추가합니다.`destroyIFrame`**. 다음 `destroyIFrame` 콜백을 사용하면 프로그래머가 iFrame 관련/보조 구성 요소(예: UI 장식)를 제거할 수 있습니다. 로그인이 완료된 후 Adobe Pass 인증이 프로그래머 페이지를 다시 로드하여 콜백에 있는 모든 UI 구성 요소를 삭제하므로 이전 인증 흐름에는 이 콜백의 존재가 필요하지 않습니다.

</br>

>[!IMPORTANT]
> 
>MVPD 로그인 iFrame 또는 팝업 창을 AccessEnabler 인스턴스가 포함된 페이지의 직접 하위로 로드해야 합니다. MVPD 로그인 iFrame 또는 팝업 창이 AccessEnabler 인스턴스가 포함된 페이지 아래에 두 개 이상 중첩된 경우 흐름이 중단될 수 있습니다. 예를 들어, 기본 페이지와 MVPD iFrame(Page =\> iFrame =\> MVPD iFrame) 사이에 iFrame이 있는 경우, 로그인 흐름이 실패할 수 있습니다.

</br>

**인증 취소**

다음은 인증을 취소하기 위한 흐름입니다.

1. **브라우저 탭 -** 탭은 기본적으로 새 창이므로 닫기 이벤트를 캡처하는 것은 이전 인증 흐름에서 시나리오 3에 설명된 것과 동일한 제한 사항을 갖습니다. 또한, 사용자가 수동으로 닫은 탭과 로그인 흐름 종료 시 자동으로 닫은 탭을 구별할 방법이 없기 때문에 여기서는 타이머 접근 방식이 가능하지 않다. 여기서 해결 방법은 사용자가 흐름을 취소할 때 AccessEnabler가 &#39;자동&#39;으로 유지되기 위한 것입니다(콜백이 트리거되지 않음). 또한 프로그래머는 특정 작업을 수행할 필요가 없습니다. 사용자는 &quot;다중 인증 요청 오류&quot; 오류를 수신하지 않고 다른 인증 흐름을 시작할 수 있습니다(이 오류는 백그라운드 로그인을 위해 AccessEnabler에서 비활성화됨).

1. **iFrame -** 프로그래머는 시나리오 2에서 설명한 접근 방식을 이전 인증 흐름(iFrame 및 연결된 닫기 버튼에서 래퍼 UI 만들기)에서 트리거할 수 있습니다 `setSelectedProvider(null)`. 이 접근 방식은 더 이상 강력한 요구 사항이 아니지만(위의 시나리오 1에서 설명한 대로 백그라운드 로그인에 대해 여러 인증 흐름이 허용됨) Adobe은 여전히 이 방식을 권장합니다.

1. **팝업 -** 이는 위의 브라우저 탭 흐름과 동일합니다.

</br>

## 로그아웃 플로우 개선 {#improved_logout}

새 로그아웃 흐름은 숨겨진 iFrame에서 수행되므로 전체 페이지 리디렉션을 제거합니다.  이는 사용자가 MVPD의 로그아웃 페이지에서 특정 작업을 수행할 필요가 없기 때문에 가능한 것입니다.

로그아웃 흐름이 완료되면 iFrame을 사용자 지정 Adobe Pass 인증 끝점으로 리디렉션합니다. 다음을 수행하는 JS 코드 조각을 제공합니다. `window.postMessage` 을 추가하여 AccessEnabler에 로그아웃이 완료되었음을 알립니다. 다음 콜백이 트리거됩니다. `setAuthenticationStatus()` 및 `sendTrackingData(AUTHENTICATION_DETECTION ...)`를 호출하여 사용자가 더 이상 인증되지 않음을 나타냅니다.

아래 그림은 사용자가 애플리케이션의 기본 페이지를 새로 고치지 않고 MVPD에 로그인할 수 있도록 하는 새로 고침 없는 흐름을 보여 줍니다.

**향상된(새로 고침 불필요) 인증/로그아웃 흐름**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## TempPass 흐름 {#improved_temppas}

새로 고침 없는 로그인은 TempPass 유형의 MVPD에 대해 다른 접근 방식을 따릅니다.

TempPass 흐름에서는 명시적인 사용자 상호 작용 없이 창을 자동으로 만들고 닫아야 하므로 일부 브라우저(팝업 차단기)에 문제가 발생할 수 있습니다. 따라서 AccessEnabler는 프로그래머가 생성한 웹 컨테이너를 필요로 하지 않고 백그라운드에서 로그인 단계를 구현합니다.

새로 고침 없는 로그인 및 로그아웃을 위해 TempPass를 구현할 때 프로그래머가 알아야 하는 측면은 다음과 같습니다.

- 인증을 시작하기 전에 비 TempPass MVPD에 대해서만 iFrame 또는 팝업 창을 만들어야 합니다. 프로그래머는 다음을 읽음으로써 MVPD가 TempPass인지 여부를 감지할 수 있습니다. `tempPass` MVPD 개체의 속성(반환: `setConfig()` / `displayProviderDialog()`).

- 다음 `createIFrame()` 콜백은 TempPass에 대한 검사를 포함해야 하며 MVPD가 TempPass가 아닌 경우에만 해당 논리를 실행해야 합니다.

- 다음 `destroyIFrame()` 콜백은 TempPass에 대한 검사를 포함해야 하며 MVPD가 TempPass가 아닌 경우에만 해당 논리를 실행해야 합니다.

- 다음 `setAuthenticationStatus()` 및 `sendTrackingData()` 콜백은 인증이 완료된 후 호출됩니다(일반 MVPD의 경우 새로 고침이 필요 없는 흐름과 정확히 동일).

>[!NOTE]
>
>이 플로우는 새로 고침 없는 임시 패스에만 사용할 수 있습니다. 새로 고침 플로우의 경우 TempPass를 명시적으로 처리해야 합니다(TempPass에 iFrame/popup이 필요한 경우)

</br>

다음 코드 샘플은 프로그래머 웹 사이트에서 MVPD 창을 처리하는 방법을 보여 줍니다(일반 MVPD와 TempPass의 경우 모두).

```javascript
    var aeHostname = "https://entitlement.auth.adobe.com";
    var mvpdWindow = null;
    var mvpd = <mvpd_object_from_displayProviderDialog>;
    var useIframeLogin = <boolean_depending_on_browser_or_Programmer_preferences>;
    var backgroundLogin = <boolean_depending_on_Programmer_preferences>;
     
    // Do not create any windows for refreshless and temp pass
    if (!(backgroundLogin && mvpd.tempPass)) {
        if (backgroundLogin && !mvpd.popup) {
            mvpdWindow = window.open(aeHostname, "mvpdwindow");
        } else if (mvpd.popup && !useIframeLogin) {
            var width = mvpd.width;
            var height = mvpd.height;
            // Center on screen
            var top = (document.all) ? window.screenTop : window.screenY + 100;
            var left = (document.all) ? window.screenLeft : window.screenX + window.innerWidth / 2 - width / 2;
        
            mvpdWindow = window.open(aeHostname, "mvpdframe",
                           "width=" + width + ",height=" + height + ",top=" + top + ",left=" + left);
            // Monitor the mvpd popup for close
            if (!backgroundLogin) {
                clearInterval(cancelTimer);
                cancelTimer = setInterval(function () {
                    if (mvpdWindow && mvpdWindow.closed) {
                        clearInterval(cancelTimer);
                        $('#mvpddiv').hide();
                        accessEnablerAPI.setSelectedProvider(null);
                    }
                }, 200);
            }
        }
    }
```
