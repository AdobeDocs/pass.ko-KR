---
title: iOS/tvOS SDK 개요
description: iOS/tvOS SDK 개요
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3754'
ht-degree: 0%

---

# (기존) iOS/tvOS SDK 개요 {#iostvos-sdk-overview}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

</br>


## 소개 {#intro}

iOS AccessEnabler는 모바일 앱에서 TV Everywhere의 권한 부여 서비스를 위해 Adobe Pass 인증을 사용할 수 있도록 해주는 Objective C iOS/tvOS 라이브러리입니다. 구현은 권한 부여 API를 정의하는 *AccessEnabler* 인터페이스와 라이브러리에서 트리거하는 콜백을 설명하는 *EntitlementDelegate* 및 *[EntitlementStatus](#ios%20entitlement%20status)* 프로토콜로 구성됩니다. 프로토콜과 함께 인터페이스는 하나의 공통 이름인 AccessEnabler 라이브러리에서 참조됩니다.

## iOS 및 tvOS 요구 사항 {#reqs}

iOS 및 tvOS 플랫폼과 Adobe Pass 인증과 관련된 최신 기술 요구 사항은 [플랫폼/장치/도구 요구 사항](#ios)을 참조하고 SDK 다운로드에 포함된 릴리스 정보를 참조하십시오. 이 페이지의 나머지 부분에서 특정 SDK 버전 이상에 적용되는 변경 사항을 설명하는 섹션이 보입니다. 예를 들어, 다음은 1.7.5 SDK에 대한 적법한 참고 사항입니다.

## 기본 클라이언트 워크플로우 이해 {#flows}

기본 클라이언트 워크플로우는 일반적으로 브라우저 기반 Adobe Pass 인증 클라이언트의 워크플로와 동일하거나 매우 유사합니다. 그러나 아래에 설명된 대로 몇 가지 예외가 있습니다.

- [초기화 후 워크플로](#post-init)
- [일반 초기 인증 워크플로](#generic)
- [로그아웃 워크플로](#logout)


### 초기화 후 워크플로 {#post-init}

AccessEnabler에서 지원하는 모든 자격 부여 워크플로에서는 ID를 설정하기 위해 이전에 [`setRequestor()`](#setReq)을(를) 호출했다고 가정합니다. 이 호출을 수행하면 일반적으로 애플리케이션의 초기화/설정 단계 중에 요청자 ID를 한 번만 제공합니다.


iOS 네이티브 클라이언트를 사용하면 [`setRequestor()`](#setReq)에 대한 초기 호출 후에 진행 방법을 선택할 수 있습니다.

- 권한 부여 호출을 즉시 시작하고 필요한 경우 자동으로 큐에 대기시킬 수 있습니다.

- [`setRequestor()`](#setReq) 콜백을 구현하면 [`setRequestorComplete()`](#setReqComplete)의 성공/실패에 대한 확인을 받을 수 있습니다.

- 위의 두 가지 작업을 모두 수행할 수 있습니다.

앱이 [`setRequestor()`](#setReq)의 성공 알림을 기다리도록 하거나 AccessEnabler의 호출 큐 메커니즘을 사용하도록 선택할 수 있습니다. 모든 후속 권한 부여 및 인증 요청에는 요청자 ID와 관련 구성 정보가 필요하므로 [`setRequestor()`](#setReq) 메서드는 초기화가 완료될 때까지 모든 인증 및 권한 부여 API 호출을 효과적으로 차단합니다.



### 일반 초기 인증 워크플로 {#generic}

이 워크플로우의 목적은 MVPD으로 사용자를 로그인하는 것입니다. 로그인에 성공하면 백엔드 서버가 사용자에게 인증 토큰을 발행합니다. 인증은 일반적으로 권한 부여 프로세스의 일부로 수행되지만, 다음은 인증이 개별적으로 작동하는 방법에 대한 설명이며, 권한 부여 단계는 포함하지 않습니다.

이 워크플로우는 기본 클라이언트마다 일반적인 브라우저 기반 인증 워크플로와 다르지만, 1~5단계는 기본 클라이언트와 브라우저 기반 클라이언트 모두에 대해 동일합니다.

1. 응용 프로그램에서 올바른 캐시된 인증 토큰을 확인하는 AccessEnabler의 `getAuthentication() `API 메서드에 대한 호출로 인증 워크플로를 시작합니다.
1. 사용자가 현재 인증되면 AccessEnabler가 [`setAuthenticationStatus()`](#setAuthNStatus) 콜백 함수를 호출하여 성공을 나타내는 인증 상태를 전달하고 흐름을 종료합니다.
1. 사용자가 현재 인증되지 않은 경우 AccessEnabler는 지정된 MVPD에서 사용자의 마지막 인증 시도가 성공했는지 여부를 확인하여 인증 흐름을 계속합니다. MVPD ID가 캐시되고 `canAuthenticate` 플래그가 true이거나 [`setSelectedProvider()`](#setSelProv)을(를) 사용하여 MVPD을 선택한 경우 사용자에게 MVPD 선택 대화 상자가 표시되지 않습니다. 인증 흐름은 MVPD의 캐시된 값(즉, 마지막으로 성공한 인증 중에 사용된 동일한 MVPD)을 사용하여 계속됩니다. 백엔드 서버에 대한 네트워크 호출이 수행되고 사용자가 MVPD 로그인 페이지(아래 6단계)로 리디렉션됩니다.
1. 캐시된 MVPD ID가 없고 [`setSelectedProvider()`](#setSelProv)을(를) 사용하여 선택한 MVPD이 없거나 `canAuthenticate` 플래그가 false로 설정된 경우 [`displayProviderDialog()`](#dispProvDialog) 콜백이 호출됩니다. 이 콜백은 응용 프로그램에 지시하여 사용자에게 선택할 MVPD 목록을 제공하는 UI를 만듭니다. MVPD 선택기를 빌드하는 데 필요한 정보가 포함된 MVPD 개체 배열이 제공됩니다. 각 MVPD 개체는 MVPD 엔터티를 설명하며, MVPD의 ID(예: XFINITY, AT\&amp;T 등) 및 MVPD 로고를 찾을 수 있는 URL과 같은 정보를 포함합니다.
1. 특정 MVPD을 선택한 후에는 사용자가 선택한 내용을 애플리케이션이 AccessEnabler에 알려야 합니다. 사용자가 원하는 MVPD을 선택하면 [`setSelectedProvider()`](#setSelProv) 메서드 호출을 통해 AccessEnabler에 사용자 선택을 알립니다.
1. iOS AccessEnabler가 `navigateToUrl:` 콜백 또는 `navigateToUrl:useSVC:` 콜백을 호출하여 사용자를 MVPD 로그인 페이지로 리디렉션합니다. 둘 중 하나를 트리거하면 AccessEnabler가 응용 프로그램에 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러를 만들고 콜백의 `url` 매개 변수에 제공된 URL을 로드하도록 요청합니다. 백엔드 서버에 있는 인증 끝점의 URL입니다. tvOS AccessEnabler의 경우 [&#x200B; 매개 변수를 사용하여 &#x200B;](#status_callback_implementation)status()`statusDictionary` 콜백이 호출되고 두 번째 화면 인증에 대한 폴링이 즉시 시작됩니다. `statusDictionary`에 두 번째 화면 인증에 사용해야 하는 `registration code`이(가) 포함되어 있습니다.
1. iOS AccessEnabler의 경우 사용자가 MVPD의 로그인 페이지에 도달하여 애플리케이션 `UIWebView/WKWebView or SFSafariViewController `컨트롤러의 미디어를 통해 자격 증명을 입력합니다. 이 전송 중에는 여러 리디렉션 작업이 발생하며, 여러 리디렉션 작업 중에 컨트롤러가 로드하는 URL을 응용 프로그램에서 모니터링해야 합니다.
1. iOS AccessEnabler의 경우 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러가 특정 사용자 지정 URL을 로드하면 애플리케이션이 컨트롤러를 닫고 AccessEnabler의 `handleExternalURL:url `API 메서드를 호출해야 합니다. 이 특정 사용자 지정 URL은 실제로 유효하지 않으며 제어자가 실제로 로드하기 위한 것이 아닙니다. 응용 프로그램에서 인증 흐름이 완료되었으며 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러를 닫아도 안전하다는 신호로만 해석해야 합니다. 응용 프로그램에서 `SFSafariViewController `컨트롤러를 사용해야 하는 경우 특정 사용자 지정 URL은 `application's custom scheme`(예: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`)에 의해 정의되며, 그렇지 않으면 이 특정 사용자 지정 URL은 `ADOBEPASS_REDIRECT_URL` 상수(예: `adobepass://ios.app`)에 의해 정의됩니다.
1. 애플리케이션이 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러를 닫고 AccessEnabler의 `handleExternalURL:url `API 메서드를 호출하면 AccessEnabler가 백엔드 서버에서 인증 토큰을 검색하여 인증 흐름이 완료되었음을 애플리케이션에 알립니다. AccessEnabler가 상태 코드가 1인 [`setAuthenticationStatus()`](#setAuthNStatus) 콜백을 호출하여 성공을 나타냅니다. 이 단계를 실행하는 동안 오류가 발생하면 [`setAuthenticationStatus()`](#setAuthNStatus) 콜백이 인증 실패와 해당 오류 코드를 나타내는 상태 코드 0으로 트리거됩니다.


>[!WARNING]
>
> AccessEnabler가 앱에 대한 제어를 포기하는 단계(공급자 선택 대화 상자가 표시되거나 UIWebView/WKWebView 또는 SFSafariViewController가 표시되는 단계)에서는 사용자가 인증 흐름을 취소할 수 있습니다. 이러한 경우 앱은 AccessEnabler에 이 이벤트에 대해 알리고 [`setSelectedProvider()`](#setSelProv) API 메서드를 호출하여 null을 매개 변수로 전달합니다. 이렇게 하면 AccessEnabler가 내부 상태를 정리하고 인증 흐름을 재설정할 수 있습니다. tvOS에서 동일한 방법을 사용하여 인증 폴링을 취소할 수 있습니다.


### 로그아웃 워크플로 {#logout}

기본 클라이언트의 경우, 로그아웃은 위에서 설명한 인증 프로세스와 유사하게 처리됩니다.

1. 응용 프로그램에서 AccessEnabler의 `logout() `API 메서드에 대한 호출을 사용하여 로그아웃 워크플로우를 시작합니다. 로그아웃은 사용자가 Adobe Pass 인증 서버와 MVPD 서버에서 모두 로그아웃해야 하므로 일련의 HTTP 리디렉션 작업의 결과입니다. AccessEnabler 라이브러리에서 발급한 간단한 HTTP 요청으로는 이 흐름을 완료할 수 없으므로 HTTP 리디렉션 작업을 수행하려면 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러를 인스턴스화해야 합니다.

1. 인증 흐름과 유사한 패턴이 사용됩니다. iOS AccessEnabler가 `navigateToUrl:` 콜백 또는 `navigateToUrl:useSVC:`을(를) 트리거하여 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러를 만들고 콜백의 `url` 매개 변수에 제공된 URL을 로드합니다. 백엔드 서버에 있는 로그아웃 끝점의 URL입니다. tvOS AccessEnabler의 경우 `navigateToUrl:` 콜백이나 `navigateToUrl:useSVC:` 콜백이 호출되지 않습니다.

1. 여러 번의 리디렉션을 거치는 동안 응용 프로그램은 `UIWebView/WKWebView or SFSafariViewController ` 컨트롤러의 활동을 모니터링하고 특정 사용자 지정 URL이 로드되는 순간을 감지해야 합니다. 이 특정 사용자 지정 URL은 실제로 유효하지 않으며 제어자가 실제로 로드하기 위한 것이 아닙니다. 응용 프로그램에서 로그아웃 흐름이 완료되었으며 컨트롤러를 닫는 것이 안전하다는 신호로만 해석해야 합니다. 컨트롤러가 이 특정 사용자 지정 URL을 로드하면 응용 프로그램에서 컨트롤러를 닫고 AccessEnabler의 `handleExternalURL:url `API 메서드를 호출해야 합니다. 응용 프로그램에서 `SFSafariViewController `컨트롤러를 사용해야 하는 경우 특정 사용자 지정 URL은 `application's custom scheme`(예: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`)에 의해 정의되며, 그렇지 않으면 이 특정 사용자 지정 URL은 ` ADOBEPASS_REDIRECT_URL  `상수(예: `adobepass://ios.app`)에 의해 정의됩니다.

1. 결국 AccessEnabler가 상태 코드가 0인 [`setAuthenticationStatus()`](#setAuthNStatus) 콜백을 호출하여 로그아웃 흐름이 성공했음을 나타냅니다.

로그아웃 흐름은 사용자가 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러와 어떤 식으로든 상호 작용할 필요가 없다는 점에서 인증 흐름과 다릅니다. 따라서 Adobe에서는 로그아웃 프로세스 중에 컨트롤을 보이지 않게(즉, 숨김으로) 만드는 것이 좋습니다.

## 토큰 {#tokens}

- [정의 및 사용](#definitions)
- [캐싱 지침](#caching)
- [지속성](#persistence)
- [형식](#format)
- [장치 바인딩](#device_binding)


### 정의 및 사용 {#definitions}

Adobe Pass 인증 권한 부여 솔루션은 인증 및 권한 부여 워크플로가 성공적으로 완료되면 Adobe Pass 인증이 생성하는 특정 데이터(토큰)를 생성하는 것에 중점을 둡니다. 이러한 토큰은 클라이언트의 iOS 디바이스에 로컬로 저장됩니다.



토큰의 수명은 제한됩니다. 만료 시 인증 및/또는 권한 부여 워크플로의 재시작을 통해 토큰을 재발행해야 합니다.



자격 워크플로 중에 발급되는 세 가지 유형의 토큰이 있습니다.

- **인증 토큰:** 사용자 인증 워크플로의 최종 결과는 AccessEnabler가 사용자를 대신하여 인증 쿼리를 만드는 데 사용할 수 있는 인증 GUID입니다. 이 인증 GUID에는 사용자의 인증 세션 자체와 다를 수 있는 연결된 TTL(time-to-live) 값이 있습니다. 인증 토큰은 인증 요청을 시작하는 장치에 인증 GUID를 바인딩하여 생성됩니다.
- **인증 토큰:** 고유한 resourceID로 식별된 보호된 특정 리소스에 대한 액세스 권한을 부여합니다. 원래 resourceID와 함께 인증 당사자가 발급한 권한 부여로 구성됩니다. 이 정보는 요청을 시작하는 장치에 바인딩됩니다.
- **단기 미디어 토큰:** AccessEnabler는 단기 미디어 토큰을 반환하여 지정된 리소스에 대한 호스팅 응용 프로그램에 대한 액세스 권한을 부여합니다. 이 토큰은 특정 리소스에 대해 이전에 획득한 인증 토큰을 기반으로 생성됩니다. 또한 이 토큰은 장치에 바인딩되어 있지 않으며 관련 수명이 크게 짧습니다(기본값: 5분).

인증 및 인증에 성공하면 Adobe Pass 인증에서 인증, 권한 부여 및 수명이 짧은 미디어 토큰을 발행합니다. 이러한 토큰은 사용자의 장치에 캐시되어야 하며 관련 수명 기간에 사용되어야 합니다.



### 캐싱 지침 {#caching}

- 인증 토큰
- 인증 토큰
- 단기 미디어 토큰


#### 인증 토큰

- **AccessEnabler 1.7:** 이 SDK에서는 여러 프로그래머-MVPD 버킷과 여러 인증 토큰을 사용할 수 있는 새로운 토큰 저장 방법을 도입했습니다. 이제 &quot;요청자별 인증&quot; 시나리오와 일반 인증 흐름 모두에 동일한 저장소 레이아웃이 사용됩니다. 둘 간의 유일한 차이점은 인증이 수행되는 방식입니다. &quot;요청자별 인증&quot;에는 AccessEnabler가 다른 프로그래머용 스토리지의 인증 토큰을 기반으로 백채널 인증을 수행할 수 있도록 하는 새로운 개선 사항(수동 인증)이 포함되어 있습니다. 사용자는 한 번만 인증하면 되며, 이 세션은 추가 앱에서 인증 토큰을 가져오는 데 사용됩니다. 이 백채널 흐름은 [`setRequestor()`](#setReq) 호출 중에 발생하며 대부분 프로그래머에게 투명합니다. **그러나 여기서는 프로그래머가 기본 UI 스레드에서 setRequestor()를 호출해야 합니다.**
- **AccessEnabler 1.6 이상:** 인증 토큰이 장치에서 캐시되는 방식은 현재 MVPD과 연결된 &quot;**요청자별 인증&quot;** 플래그에 따라 다릅니다.

<!-- end list -->

1. &quot;요청자별 인증&quot; 기능이 비활성화되면 단일 인증 토큰이 전역 임시 보드에 로컬로 저장됩니다. 이 토큰은 현재 MVPD과 통합된 모든 애플리케이션 간에 공유됩니다.
1. &quot;요청자별 인증&quot; 기능을 활성화하면 토큰이 인증 흐름을 수행한 프로그래머와 명시적으로 연결됩니다(토큰은 전역 임시 보드에 저장되지 않고 프로그래머 애플리케이션에서만 볼 수 있는 개인 파일에 저장됩니다). 보다 구체적으로, 다른 애플리케이션 간의 SSO(Single Sign-On)가 비활성화됩니다. 사용자는 새 앱으로 전환할 때 인증 흐름을 명시적으로 수행해야 합니다(두 번째 앱의 프로그래머가 현재 MVPD과 통합되고 로컬 캐시에 해당 프로그래머에 대한 인증 토큰이 존재하지 않는다는 사실 제공).



#### 인증 토큰

AccessEnabler에서 RESOURCE당 하나의 인증 토큰만 캐시됩니다. 여러 인증 토큰이 캐시될 수 있지만 서로 다른 리소스와 연결되어 있습니다. 새 인증 토큰이 발행되고 *동일한 리소스*&#x200B;에 대한 이전 인증 토큰이 이미 있을 때마다 새 토큰이 캐시된 기존 값을 덮어씁니다.



#### 미디어 토큰

단기 미디어 토큰은 전혀 캐시되지 않아야 합니다. 미디어 토큰은 일회성 사용으로 제한되므로 인증 API가 호출될 때마다 서버에서 검색해야 합니다.



### 지속성 {#persistence}

토큰은 동일한 애플리케이션의 연속 실행 간에 지속적이어야 합니다. 즉, 인증 및 인증 토큰이 획득되고 사용자가 애플리케이션을 닫으면 사용자가 애플리케이션을 다시 열 때 애플리케이션에서 동일한 토큰을 사용할 수 있습니다. 더욱이, 이러한 토큰들은 다수의 애플리케이션들에 걸쳐 지속되는 것이 바람직하다. 즉, 사용자가 하나의 애플리케이션을 사용하여 특정 ID 공급자로 로그인한 후(인증 및 인증 토큰을 성공적으로 획득) 다른 애플리케이션을 통해 동일한 토큰을 사용할 수 있으며, 동일한 ID 공급자를 통해 로그인할 때 더 이상 자격 증명을 묻는 메시지가 표시되지 않습니다. 이러한 유형의 원활한 인증/권한 부여 워크플로우는 Adobe Pass 인증 솔루션을 진정한 TV-Everywhere로 만드는 요소입니다
구현.



## iOS

iOS AccessEnabler 라이브러리는 토큰 데이터를 *붙여넣기 보드*&#x200B;라는 &quot;클립보드와 유사한&quot; 데이터 구조에 저장하여 교차 애플리케이션 데이터 공유 문제를 해결합니다. 이 시스템 수준 공유 리소스는 원하는 영구 토큰 사용 사례를 구현할 수 있는 주요 구성 요소를 제공합니다.

- **구조화된 저장소에 대한 지원** - 붙여넣기 보드는 간단한 선형 버퍼와 같은 메모리 구조가 아닙니다. 사용자 지정 키 값을 기반으로 데이터 색인화를 허용하는 사전과 같은 저장 메커니즘을 제공합니다.
- **기본 파일 시스템을 사용한 데이터 지속성 지원** - 붙여넣기 보드 구조의 내용을 지속으로 표시할 수 있습니다. 이 경우 데이터는 장치의 내부 메모리에 저장됩니다.



토큰 캐시에 특정 토큰을 배치하면 AccessEnabler 라이브러리에서 해당 토큰의 유효성을 다른 시간에 확인합니다.  유효한 토큰은 다음과 같이 정의됩니다.

- 토큰의 TTL이 만료되지 않았습니다.
- 토큰 발급자는 허용된 ID 공급자 목록에 포함됩니다.



## tvOS

tvOS에서는 대지를 사용할 수 없으므로 tvOS AccessEnabler 라이브러리는 NsUserDefaults를 스토리지 옵션으로 사용합니다. 이는 인증 및 인증 토큰이 지속되는 문제를 해결하지만 저장된 정보를 상이한 애플리케이션 간에 공유할 수 없다.



**iOS 10 임시 보드 변경 사항 -** 이전 버전의 iOS에서 업그레이드하면 임시 보드가 지워집니다. 즉, 모든 애플리케이션을 다시 인증해야 합니다.



**iOS 7 임시 보드 변경 사항 -** 임시 보드가 iOS 7에서 작동하는 방식이 변경되어 iOS 7에서 실행 중인 응용 프로그램 간 교차 SSO가 제한됩니다. 동일한 `<Bundle Seed ID>`(`<Team ID>`)을(를) 가진 응용 프로그램은 토큰을 공유합니다. 즉, 동일한 프로그래머 X의 앱 A1 및 A2는 토큰을 공유하지만 앱 A1(프로그래머 X) 및 앱 A3(프로그래머 Y)은 토큰을 공유하지 않습니다.

- 번들 시드 ID/팀 ID는 동일한 프로비저닝 프로필에서 생성된 경우 두 앱 간에 동일합니다. 자세한 내용은 다음 링크를 참조하십시오.
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- 이 &quot;교차 SSO&quot; 제한은 사용된 Adobe Pass 인증 SDK에 관계없이 iOS 7에 제공됩니다.

iOS 7 이상 버전에서 SSO를 구성하는 방법에 대한 자세한 내용은 이 기술 노트를 참조하십시오(기술 노트는 Access Enabler v1.8 이상 버전에 적용됨). <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### 토큰 스토리지(AccessEnabler 1.7)

AccessEnabler 1.7부터 토큰 스토리지는 여러 인증 토큰을 보유할 수 있는 다중 레벨 중첩 맵 구조에 의존하여 여러 프로그래머-MVPD 조합을 지원할 수 있습니다. 이 새로운 스토리지는 AccessEnabler 공용 API에 영향을 주지 않으며 프로그래머 측에서는 변경할 필요가 없습니다. 다음 예는 다음과 같습니다
에서는 이 새로운 기능을 설명합니다.

1. App1 열기(Programmer1에서 개발).
1. MVPD1(Programmer1과 통합)을 사용하여 인증합니다.
1. 현재 응용 프로그램을 일시 중단 / 닫고 App2 (Programmer2에서 개발)를 엽니다.
1. Programmer2가 MVPD2와 통합되지 않았으므로 사용자가 App2에서 인증되지 않는다고 가정해 보겠습니다.
1. App2에서 MVPD2(Programmer2와 통합됨)로 인증합니다.
1. App1로 다시 전환하십시오. 사용자는 여전히 Programmer1로 인증됩니다.

이전 버전의 AccessEnabler에서는 토큰 저장소가 이전에 하나의 인증 토큰만 지원했으므로 6단계에서 사용자를 인증되지 않은 것으로 렌더링했습니다.



한 프로그래머/MVPD 세션에서 로그아웃하면 장치의 다른 프로그래머/MVPD 인증 토큰을 모두 포함하여 기본 저장소 전체가 지워집니다. 반면 인증 흐름([`setSelectedProvider(null)`](#setSelProv) 호출)을 취소하면 기본 저장소는 지워지지 않지만 현재 프로그래머/MVPD 인증 시도에만 영향을 미칩니다(현재 프로그래머의 MVPD을 지움으로써).



### 토큰 가져오기(AccessEnabler 1.7)

AccessEnabler 1.7에 포함된 또 다른 스토리지 관련 기능을 사용하면 이전 스토리지 영역에서 인증 토큰을 가져올 수 있습니다. 이 &quot;Token Importer&quot;를 사용하면 스토리지 버전이 업그레이드되어도 SSO 상태를 유지하면서 연속적인 AccessEnabler 릴리스 간의 호환성을 유지할 수 있습니다. 가져오기는 [`setRequestor()`](#setReq) 흐름 중에 실행되고 다음 두 가지 시나리오에서 실행됩니다(현재 프로그래머에 대한 유효한 인증 토큰이 현재 저장소에 없다고 가정).

- 특정 프로그래머가 개발한 1.7 앱의 첫 번째 설치
- 새로운 스토리지를 사용하는 향후 AccessEnabler에 대한 업그레이드 경로

가져오기 작업은 프로그래머에게 투명하며 클라이언트 애플리케이션에서 코드를 변경할 필요가 없습니다.



### 토큰 삭제기(AccessEnabler 1.7.5)

이 서비스는 AccessEnabler 1.7.5부터 [`setRequestor()`](#setReq)`. `추적을 위해 WiFi MAC 주소에서 IDFA로 iOS 7을 전환한 결과로 개발되었습니다. 삭제기는 현재 저장소에 유효한 인증 토큰(이전에 iOS7 이전에 MAC 주소를 사용하여 계산된 장치 ID 측면에서 유효함)만 포함되어 있는지 확인합니다. 토큰 삭제기는 모든 잘못된 토큰을 제거합니다.



토큰 삭제기 서비스는 iOS 6에서 AccessEnabler 1.7.5 애플리케이션을 사용한 다음 사용자가 iOS 7로 업데이트할 때 가장 유용합니다. 이렇게 되면 iOS 6에서 얻은 모든 인증 토큰이 무효화됩니다(MAC 주소를 사용하던 것에서 IDFA로 장치 ID 알고리즘 변경 사항 때문). 삭제기가 모든 잘못된 토큰을 지우고 사용자는 인증되지 않습니다.



토큰 삭제기가 잘못된 토큰을 제거하지 않으면 AccessEnabler는 잘못된 인증 토큰이 있기 때문에 인증을 받지 못합니다. 인증 오류 메시지는 명확하지 않고 문제의 원인을 파악하는 데 너무 유용하지 않을 수 있으므로 최종 사용자가 디버깅하는 것이 까다로울 수 있습니다.



### 형식 {#format}

- [인증 토큰](#authn_token)
- [AuthZ 토큰](#authz_token)
- [짧은 미디어 토큰](#short_token)
- [장치 바인딩](#device_binding)


AuthN 및 AuthZ 토큰의 형식은 백그라운드 정보용으로만 여기에 포함됩니다. 이러한 토큰의 구조는 언제든지 Adobe Pass 인증에 의해 변경될 수 있습니다. AuthN 및 AuthZ 토큰이 로컬 장치에 노출되지 않으므로 프로그래머는 앱을 구현하기 위해 AuthN 및 AuthZ 토큰의 정확한 구조를 알 필요가 없습니다. Short Media 토큰 *is*&#x200B;이(가) 프로그래머 애플리케이션에 노출되었습니다.



#### 인증 토큰 {#authn_token}

아래 목록은 인증 토큰의 형식을 제공합니다.

```
  <signatureInfo>base64(...)<signatureInfo>
  <simpleAuthenticationToken>
      <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
      <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
      <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
      <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
      <simpleTokenMsoID>Adobe</simpleTokenMsoID>
      <simpleTokenDeviceID>
          <simpleTokenFingerprint>
              HASH(true device identification info)
          </simpleTokenFingerprint>
      </simpleTokenDeviceID>   
  </simpleAuthenticationToken>
```


#### 인증 토큰 {#authz_token}

아래 목록은 인증 토큰의 형식을 제공합니다.

```
  <signatureInfo>base64(...)<signatureInfo>
  <simpleAuthorizationToken>
      <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
      <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
      <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
      <simpleTokenMsoID>Adobe</simpleTokenMsoID>
      <simpleTokenDeviceID>
          <simpleTokenFingerprint>
              HASH(true device identification info)
          </simpleTokenFingerprint>
      </simpleTokenDeviceID>
  </simpleAuthorizationToken>
```


#### 짧은 미디어 토큰 {#short_token}

아래 목록은 짧은 미디어 토큰의 형식을 제공합니다. 이 토큰은 프로그래머 애플리케이션에 노출됩니다. 이것은 성공적인 자격 프로세스가 끝나면 프로그래머 애플리케이션에 전달됩니다.

```
  <signatureInfo>signature<signatureInfo>
  <shortAuthorizationToken>
    <sessionGUID>session_guid</sessionGUID>
    <requestorID>requestor_id</requestorID>
    <resourceID>resource_id</resourceID>
    <ttl>ttl_in_ms</ttl>
    <issueTime>issue_time</issueTime>
    <mvpdId>mvpd_id</mvpdId>
    <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
  </shortAuthorizationToken>
```


### 장치 바인딩 {#device_binding}

위의 XML 목록에서 제목이 `simpleTokenFingerprint`인 태그를 참고하십시오. 이 태그의 목적은 기본 장치 ID 개별화 정보를 보유하는 것입니다. AccessEnabler 라이브러리는 이러한 개별화 정보를 가져와 권한 부여 호출 중에 Adobe Pass 인증 서비스에서 사용할 수 있도록 할 수 있습니다. 이 서비스는 이 정보를 사용하여 실제 토큰에 포함하므로 토큰을 특정 장치에 효과적으로 바인딩합니다. 이것의 최종 목표는 여러 디바이스에서 토큰을 전송할 수 없도록 하는 것입니다.



이는 명백히 보안 관련 기능이므로 이 정보는 보안 관점에서 본질적으로 &quot;민감합니다&quot;. 결과적으로 이러한 정보는 변조 및 도청으로부터 보호될 필요가 있다. 도청 문제는 HTTPS 프로토콜을 통해 인증/권한 부여 요청을 전송하여 해결됩니다. 변조 방지는 기기 식별 정보에 디지털 서명을 함으로써 처리된다. AccessEnabler 라이브러리는 디바이스에서 제공하는 정보에서 디바이스 ID를 계산한 다음, 디바이스 ID를 &quot;in the clear&quot;를 요청 매개 변수로 Adobe Pass 인증 서버에 보냅니다. Adobe Pass 인증 서버는 Adobe의 개인 키로 장치 ID를 디지털 서명하고 AccessEnabler로 반환되는 인증 토큰에 추가합니다. 따라서 장치 ID가 인증 토큰과 바인딩됩니다. 인증 흐름 중에 AccessEnabler는 인증 토큰과 함께 디바이스 ID를 지우고 다시 전송합니다. 유효성 검사 프로세스가 실패하면 자동으로 인증/권한 부여 워크플로가 실패합니다. Adobe Pass 인증 서버는 개인 키를 장치 ID에 적용하고 인증 토큰의 값과 비교합니다. 일치하지 않으면 해당 권한 흐름이 실패합니다.



**AccessEnabler 1.7.5의 장치 바인딩에 대한 참고 사항:** AccessEnabler 1.7.5부터 장치 ID를 계산하여 iOS 장치에 대한 개별화 정보를 제공하는 방법이 변경되었습니다. 이 변경 사항은 iOS 7의 변경 사항을 반영합니다. iOS 7부터 Apple은 IDFA(광고주용 식별자)를 위해 더 이상 WiFi MAC 주소를 추적 옵션으로 제공하지 않습니다. iOS 7에서 실행되는 앱의 개별화 정보는 IDFA를 기반으로 하며 해당 정보는 자격 흐름 토큰에 포함되어 있으므로 이러한 변경으로 인해 발생할 수 있는 사용자 경험에 여러 가지 영향을 줄 수 있습니다. 사용자가 업그레이드하고 있는 iOS 버전과 프로그래머가 업그레이드하고 있는 AccessEnabler 버전이 서로 다르기 때문입니다. 이 변경 사항에 대한 자세한 내용은 AccessEnabler SDK 1.7.5에 포함된 릴리스 노트를 참조하십시오.

<!--
## Related Information {#related}


- [iOS/tvOS Integration Cookbook](#)
- [iOS/tvOS API Reference](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [SSO on iOS when using the Adobe Pass Authentication Access
  Enabler](#)
-->
