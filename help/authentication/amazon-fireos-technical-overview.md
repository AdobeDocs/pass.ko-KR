---
title: Amazon FireOS 기술 개요
description: Amazon FireOS 기술 개요
exl-id: 939683ee-0dd9-42ab-9fde-8686d2dc0cd0
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2166'
ht-degree: 0%

---

# Amazon FireOS 기술 개요 {#amazon-fireos-technical-overview}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>

## 소개 {#intro}

Amazon FireOS AccessEnabler는 애플리케이션에서 사용하는 AccessEnabler 스텁 라이브러리와 모바일 앱이 TV Everywhere의 권한 부여 서비스를 위해 Adobe Pass 인증을 사용할 수 있도록 하는 시스템 수준의 Java Android 라이브러리인 두 가지 구성 요소로 표시됩니다. Amazon FireOS용 Android 구현은 권한 부여 API를 정의하는 AccessEnabler 인터페이스와 라이브러리가 트리거하는 콜백을 설명하는 EntitlementDelegate 프로토콜로 구성됩니다. 시스템 수준 AccessEnabler Android 라이브러리를 사용하면 Amazon 서비스에 액세스하여 플랫폼 수준에서 Single Sing On을 사용할 수 있습니다.

## 기본 클라이언트 워크플로우 이해 {#native_client_workflows}

기본 클라이언트 워크플로우는 일반적으로 브라우저 기반 Adobe Pass 인증 클라이언트의 워크플로와 동일하거나 매우 유사합니다. 그러나 아래에 설명된 대로 몇 가지 예외가 있습니다.


### 초기화 후 워크플로 {#post-init}

AccessEnabler에서 지원하는 모든 자격 부여 워크플로에서는 ID를 설정하기 위해 이전에 [`setRequestor()`](#setRequestor)을(를) 호출했다고 가정합니다. 이 호출을 수행하면 일반적으로 애플리케이션의 초기화/설정 단계 중에 요청자 ID를 한 번만 제공합니다.

기본 클라이언트(예: AmazonFireOS)의 경우, [`setRequestor()`](#setRequestor)에 대한 초기 호출 후에 다음 방법을 선택할 수 있습니다.

- 권한 부여 호출을 즉시 시작하고 필요한 경우 자동으로 큐에 대기시킬 수 있습니다.
- 또는 setRequestorComplete() 콜백을 구현하여 [`setRequestor()`](#setRequestor)의 성공/실패를 확인할 수 있습니다.
- 또는 둘 다 수행합니다.

[`setRequestor()`](#setRequestor)의 성공 알림을 기다릴 것인지 또는 AccessEnabler의 호출 큐 메커니즘을 사용할 것인지 여부는 사용자가 결정합니다. 모든 후속 권한 부여 및 인증 요청에는 요청자 ID와 관련 구성 정보가 필요하므로 [`setRequestor()`](#setRequestor) 메서드는 초기화가 완료될 때까지 모든 인증 및 권한 부여 API 호출을 효과적으로 차단합니다.

### 일반 초기 인증 워크플로 {#generic}

이 워크플로우의 목적은 MVPD로 사용자를 로그인하는 것입니다.  로그인에 성공하면 백엔드 서버가 사용자에게 인증 토큰을 발행합니다. 인증은 일반적으로 권한 부여 프로세스의 일부로 수행되지만, 다음은 인증이 개별적으로 작동하는 방법에 대한 설명이며, 권한 부여 단계는 포함하지 않습니다.

다음 기본 클라이언트 워크플로우는 일반적인 브라우저 기반 인증 워크플로와 다르지만 기본 클라이언트와 브라우저 기반 클라이언트 모두에 대해 1~5단계는 동일합니다.

1. 페이지 또는 플레이어가 캐시된 올바른 인증 토큰을 확인하는 [getAuthentication()](#getAuthN)을 호출하여 인증 워크플로를 시작합니다. 이 메서드에는 선택적 `redirectURL` 매개 변수가 있습니다. `redirectURL`에 대한 값을 제공하지 않으면 인증이 성공하면 사용자는 인증이 초기화된 URL로 반환됩니다.
1. AccessEnabler가 현재 인증 상태를 확인합니다. 사용자가 현재 인증되면 AccessEnabler가 `setAuthenticationStatus()` 콜백 함수를 호출하여 성공을 나타내는 인증 상태를 전달합니다(아래 7단계).
1. 사용자가 인증되지 않은 경우 AccessEnabler는 지정된 MVPD로 사용자의 마지막 인증 시도가 성공했는지 여부를 확인하여 인증 흐름을 계속합니다. MVPD ID가 캐시되고 `canAuthenticate` 플래그가 true이거나 [`setSelectedProvider()`](#setSelectedProvider)을(를) 사용하여 MVPD를 선택한 경우 사용자에게 MVPD 선택 대화 상자가 표시되지 않습니다. 인증 흐름은 MVPD의 캐시된 값(즉, 마지막으로 성공한 인증 중에 사용된 동일한 MVPD)을 사용하여 계속됩니다. 백엔드 서버에 대한 네트워크 호출이 수행되며 사용자가 MVPD 로그인 페이지로 리디렉션됩니다(아래 6단계).
1. 캐시된 MVPD ID가 없고 [`setSelectedProvider()`](#setSelectedProvider)을(를) 사용하여 선택한 MVPD가 없거나 `canAuthenticate` 플래그가 false로 설정되어 있으면 [`displayProviderDialog()`](#displayProviderDialog) 콜백이 호출됩니다. 이 콜백은 페이지나 플레이어가 선택할 MVPD 목록을 사용자에게 제공하는 UI를 만들도록 지시합니다. MVPD 선택기를 만드는 데 필요한 정보가 들어 있는 MVPD 개체 배열이 제공됩니다. 각 MVPD 개체는 MVPD 엔터티를 설명하고 MVPD의 ID(예: XFINITY, AT\&amp;T 등)와 같은 정보를 포함합니다. MVPD 로고를 찾을 수 있는 URL입니다.
1. 특정 MVPD를 선택한 후에는 페이지나 플레이어에서 사용자가 선택한 내용을 AccessEnabler에 알려야 합니다. Flash이 아닌 클라이언트의 경우 사용자가 원하는 MVPD를 선택하면 [`setSelectedProvider()`](#setSelectedProvider) 메서드를 호출하여 AccessEnabler에 사용자 선택을 알립니다. Flash 클라이언트가 대신 &quot;`mvpdSelection`&quot; 유형의 공유 `MVPDEvent`을(를) 디스패치하여 선택한 공급자를 전달합니다.
1. Amazon 응용 프로그램의 경우 [`navigateToUrl()`](#navigagteToUrl) 콜백이 무시됩니다. Access Enabler 라이브러리는 사용자를 인증하기 위해 일반적인 WebView 컨트롤에 액세스할 수 있도록 합니다.
1. `WebView`을(를) 통해 사용자는 MVPD의 로그인 페이지에 도달하고 자격 증명을 입력합니다. 이 전송 중에 여러 리디렉션 작업이 발생합니다.
1. WebView에서 인증을 완료하면 이 인증을 닫고 AccessEnabler에 사용자가 성공적으로 로그인했음을 알려주면 AccessEnabler가 백엔드 서버에서 실제 인증 토큰을 검색합니다. AccessEnabler가 상태 코드가 1인 [`setAuthenticationStatus()`](#setAuthNStatus) 콜백을 호출하여 성공을 나타냅니다. 이러한 단계를 실행하는 동안 오류가 발생하면 사용자가 인증되지 않았음을 나타내는 해당 오류 코드와 함께 상태 코드 0으로 [`setAuthenticationStatus()`](#setAuthNStatus) 콜백이 트리거됩니다.

### 로그아웃 워크플로 {#logout}

기본 클라이언트의 경우, 로그아웃은 위에서 설명한 인증 프로세스와 유사하게 처리됩니다. 이 패턴을 따라 AccessEnabler는 `WebView` 컨트롤을 만들고 백엔드 서버에 있는 로그아웃 끝점의 URL을 사용하여 컨트롤을 로드합니다. 로그아웃 프로세스가 완료되면 토큰이 지워집니다.

로그아웃 흐름은 사용자가 `WebView`과(와) 어떤 식으로든 상호 작용할 필요가 없다는 점에서 인증 흐름과 다릅니다. 로그아웃이 완료되면 AccessEnabler가 상태 코드가 0인 `setAuthenticationStatus()` 콜백을 호출하여 사용자가 인증되지 않았음을 나타냅니다.

## 토큰 {#tokens}

### 정의 및 사용 {#definitions}

Adobe Pass 인증 권한 부여 솔루션은 인증 및 권한 부여 워크플로가 성공적으로 완료되면 Adobe Pass 인증이 생성하는 특정 데이터(토큰)를 생성하는 것에 중점을 둡니다. 이러한 토큰은 클라이언트의 Amazon FireOS 디바이스에 로컬로 저장됩니다.

토큰의 수명은 제한됩니다. 만료 시 인증 및/또는 권한 부여 워크플로의 재시작을 통해 토큰을 재발행해야 합니다.

자격 워크플로 중에 발급되는 세 가지 유형의 토큰이 있습니다.

- **인증 토큰** - 사용자 인증 워크플로의 최종 결과는 AccessEnabler가 사용자를 대신하여 인증 쿼리를 만드는 데 사용할 수 있는 인증 GUID입니다. 이 인증 GUID에는 사용자의 인증 세션 자체와 다를 수 있는 연결된 TTL(time-to-live) 값이 있습니다. Adobe Pass 인증은 인증 요청을 시작하는 장치에 인증 GUID를 바인딩하여 인증 토큰을 생성합니다.
- **인증 토큰** - 고유한 `resourceID`(으)로 식별된 특정 보호된 리소스에 대한 액세스 권한을 부여합니다. 원본 `resourceID`과(와) 함께 권한 부여 당사에서 발급한 권한 부여로 구성됩니다. 이 정보는 요청을 시작하는 장치에 바인딩됩니다.
- **단기 미디어 토큰** - AccessEnabler는 단기 미디어 토큰을 반환하여 지정된 리소스에 대한 호스팅 응용 프로그램에 대한 액세스 권한을 부여합니다. 이 토큰은 특정 리소스에 대해 이전에 획득한 인증 토큰을 기반으로 생성됩니다. 또한 이 토큰은 장치에 바인딩되어 있지 않으며 관련 수명이 크게 짧습니다(기본값: 5분).

인증 및 인증에 성공하면 Adobe Pass 인증에서 인증, 권한 부여 및 수명이 짧은 미디어 토큰을 발행합니다. 이러한 토큰은 사용자의 장치에 캐시되어야 하며 관련 수명 기간에 사용되어야 합니다.

### 캐싱 지침 {#caching}


#### 인증 토큰

- **FireOS용 AccessEnabler 1.10.1**&#x200B;은(는) Android 1.9.1용 AccessEnabler를 기반으로 합니다. 이 SDK는 여러 프로그래머-MVPD 버킷과 여러 인증 토큰을 활성화하는 새로운 토큰 저장 방법을 도입했습니다.

#### 인증 토큰

AccessEnabler는 특정 시점에 리소스당 하나의 인증 토큰만 캐시합니다. 여러 인증 토큰이 캐시될 수 있지만 서로 다른 리소스와 연결되어 있습니다. 새 인증 토큰이 발행되고 동일한 리소스에 대한 이전 인증 토큰이 이미 있을 때마다 새 토큰이 기존 캐시된 값을 덮어씁니다.

#### 미디어 토큰

단기 미디어 토큰은 전혀 캐시되지 않아야 합니다. 미디어 토큰은 일회성 사용으로 제한되므로 인증 API가 호출될 때마다 서버에서 검색해야 합니다.

### 지속성 {#persistence}

토큰은 동일한 애플리케이션의 연속 실행 간에 지속적이어야 합니다. 즉, 인증 및 인증 토큰이 획득되고 사용자가 애플리케이션을 닫으면 사용자가 애플리케이션을 다시 열 때 애플리케이션에서 동일한 토큰을 사용할 수 있습니다. 더욱이, 이러한 토큰들은 다수의 애플리케이션들에 걸쳐 지속되는 것이 바람직하다. 즉, 사용자가 하나의 애플리케이션을 사용하여 특정 ID 공급자로 로그인한 후(인증 및 인증 토큰을 성공적으로 획득) 다른 애플리케이션을 통해 동일한 토큰을 사용할 수 있으며, 동일한 ID 공급자를 통해 로그인할 때 더 이상 자격 증명을 묻는 메시지가 표시되지 않습니다.

이러한 유형의 원활한 인증/권한 부여 워크플로우는 Adobe Pass 인증 솔루션을 진정한 TV Everywhere 구현으로 만드는 요소입니다. 엔지니어링 관점에서 볼 때, Android AccessEnabler 라이브러리는 토큰 데이터를 외부 스토리지에 있는 데이터베이스 파일에 저장하여 교차 애플리케이션 데이터 공유 문제를 해결합니다. 이 시스템 수준 공유 리소스는 원하는 영구 토큰 사용 사례를 구현할 수 있는 주요 구성 요소를 제공합니다.

- 구조화된 저장소 지원 - Adobe Pass 인증 토큰 저장소는 단순한 선형 버퍼와 같은 메모리 구조가 아닙니다. 사용자 지정 키 값을 기반으로 데이터 색인화를 허용하는 사전과 같은 저장 메커니즘을 제공합니다.
- 기본 파일 시스템을 사용한 데이터 지속성 지원 - 데이터베이스 파일의 내용은 기본적으로 유지되고 데이터는 장치의 외부 메모리에 저장됩니다.

토큰 캐시에 특정 토큰을 배치하면 AccessEnabler 라이브러리에서 해당 토큰의 유효성을 다른 시간에 확인합니다.  유효한 토큰은 다음과 같이 정의됩니다.

- 토큰의 TTL이 만료되지 않았습니다.
- 토큰 발급자는 허용된 ID 공급자 목록에 포함됩니다

토큰 저장소는 여러 인증 토큰을 보유할 수 있는 다중 레벨 중첩 맵 구조에 의존하여 여러 프로그래머-MVPD 조합을 지원할 수 있다. 이 새로운 스토리지는 AccessEnabler 공용 API에 영향을 주지 않으며 프로그래머 측에서는 변경할 필요가 없습니다. 다음은 이러한 최신 기능을 보여주는 예입니다.

1. App1 열기(Programmer1에서 개발).
1. MVPD1(Programmer1과 통합됨)으로 인증합니다.
1. 현재 응용 프로그램을 일시 중단 / 닫고 App2 (Programmer2에서 개발)를 엽니다.
1. Programmer2가 MVPD2와 통합되지 않았으므로 사용자가 App2에서 인증되지 않는다고 가정해 보겠습니다.
1. App2에서 MVPD2(Programmer2와 통합됨)로 인증합니다.
1. App1로 다시 전환하십시오. 사용자는 여전히 Programmer1로 인증됩니다.

### 형식 {#format}

#### 인증 토큰 {#authn_token}

아래 목록은 인증 토큰의 형식을 제공합니다.

```JSON
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

```JSON
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


#### 짧은 미디어 토큰 {#short_media_token}

아래 목록은 짧은 미디어 토큰의 형식을 제공합니다.  이 토큰은 프로그래머 애플리케이션에 노출됩니다.  이것은 성공적인 자격 프로세스가 끝나면 프로그래머 애플리케이션에 전달됩니다.

```JSON
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


#### 장치 바인딩 {#device_binding}

위의 XML 목록에서 제목이 `simpleTokenFingerprint`인 태그를 참고하십시오. 이 태그의 목적은 기본 장치 ID 개별화 정보를 보유하는 것입니다. AccessEnabler 라이브러리는 이러한 개별화 정보를 가져와 권한 부여 호출 중에 Adobe Pass 인증 서비스에서 사용할 수 있도록 할 수 있습니다. 이 서비스는 이 정보를 사용하여 실제 토큰에 포함하므로 토큰을 특정 장치에 효과적으로 바인딩합니다. 이것의 최종 목표는 여러 디바이스에서 토큰을 전송할 수 없도록 하는 것입니다.

위의 XML 목록에서 simpleTokenFingerprint라는 태그를 참고하십시오. 이 태그의 목적은 기본 장치 ID 개별화 정보를 보유하는 것입니다. AccessEnabler 라이브러리는 이러한 개별화 정보를 가져와 권한 부여 호출 중에 Adobe Pass 인증 서비스에서 사용할 수 있도록 할 수 있습니다. 이 서비스는 이 정보를 사용하여 실제 토큰에 포함하므로 토큰을 특정 장치에 효과적으로 바인딩합니다. 이것의 최종 목표는 여러 디바이스에서 토큰을 전송할 수 없도록 하는 것입니다.

이는 명백히 보안 관련 기능이므로 이 정보는 보안 관점에서 본질적으로 &quot;민감합니다&quot;. 결과적으로 이러한 정보는 변조 및 도청으로부터 보호될 필요가 있다. 도청 문제는 HTTPS 프로토콜을 통해 인증/권한 부여 요청을 전송하여 해결됩니다. 변조 방지는 기기 식별 정보에 디지털 서명을 함으로써 처리된다. AccessEnabler 라이브러리는 디바이스에서 제공하는 정보에서 디바이스 ID를 계산한 다음, 디바이스 ID를 &quot;in the clear&quot;를 요청 매개 변수로 Adobe Pass 인증 서버에 보냅니다.  Adobe Pass 인증 서버는 Adobe의 개인 키로 장치 ID를 디지털 서명하고 AccessEnabler에 반환되는 인증 토큰에 추가합니다. 따라서 장치 ID가 인증 토큰과 바인딩됩니다.  인증 흐름 중에 AccessEnabler는 인증 토큰과 함께 디바이스 ID를 지우고 다시 전송합니다.  유효성 검사 프로세스가 실패하면 자동으로 인증/권한 부여 워크플로가 실패합니다.  Adobe Pass 인증 서버는 개인 키를 장치 ID에 적용하고 인증 토큰의 값과 비교합니다.  일치하지 않으면 해당 권한 흐름이 실패합니다.
