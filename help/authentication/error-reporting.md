---
title: 오류 보고
description: 오류 보고
exl-id: a52bd2cf-c712-40a2-a25e-7d9560b46ba6
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2961'
ht-degree: 1%

---

# 오류 보고 {#error-reporting}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.


## 개요 {#overview}

Adobe Pass 인증의 오류 보고는 현재 두 가지 방법으로 구현됩니다.

* **고급 오류 보고** 구현자는 의 경우 오류 콜백을 등록합니다. [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk) 또는 라는 인터페이스 메서드를 구현합니다.`status`의 경우 &quot; [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk) 및 [AccessEnabler Android SDK](#accessenabler-android-sdk)고급 오류 보고를 받습니다. 오류는 다음과 같이 분류됩니다. **정보**, **경고**, 및 **오류** 유형. 이 보고 시스템은 **비동기**, 해당 **여러 오류가 트리거되는 순서는 보장되지 않습니다**.  고급 오류 보고 시스템에 대한 자세한 내용은 [고급 오류 보고](#advanced-error-reporting) 섹션.

* **원래 오류 보고 -** 특정 요청이 실패할 때 특정 콜백 함수에 오류 메시지가 전달되는 정적 보고 시스템입니다. 오류는 일반, 인증 및 권한 부여 유형으로 그룹화됩니다. 원래 시스템에서 보고된 오류 목록은 [원래 오류 보고](#original-error-reporting) 섹션.


## 고급 오류 보고 {#advanced-error-reporting}

* [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk)
* [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk)
* [AccessEnabler Android SDK](#accessenabler-android-sdk)
* [AccessEnabler Fireos SDK](#accessenabler-fireos-sdk)

>[!IMPORTANT]
>
>옛 것 [원래 오류 보고](#original-error-reporting) API는 이전과 마찬가지로 계속 작동하며, 고급 오류 보고로 기능이 중단되지는 않지만 원래 오류 보고는 더 이상 업데이트를 받지 않습니다. 모든 새로운 오류 및 업데이트가 고급 오류 보고 시스템에 발생합니다.

### AccessEnabler JavaScript SDK {#accessenabler-javascript-sdk}

새 오류 보고 시스템은 선택 사항으로 남아 있으므로 구현자는 오류 처리기 콜백을 명시적으로 등록하여 고급 오류 보고서를 수신할 수 있습니다. 이 시스템에는 여러 오류 콜백을 동적으로 등록 및 등록 취소할 수 있는 기능이 포함되어 있습니다. 또한 AccessEnabler JavaScript SDK가 로드되면 다른 초기화를 수행하지 않고도 새 오류 콜백을 등록할 수 있습니다(호출 전) `setRequestor()`)를 입력하여 초기화 및 구성 오류에 대한 고급 보고서를 받을 수 있습니다.


#### 구현 {#access-enab-js-imp}

yourErrorHandler(errorData:Object)


오류 처리기 콜백 함수는 다음 구조의 단일 개체(맵)를 수신합니다.

```JavaScript
    {
      errorId: "CFG410",
      level: "error",
      message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

### 1. 바인딩 {#bind}

**`.bind(eventType:String, handlerName:String):void`**

이벤트에 대한 핸들러를 연결합니다.

**`eventType`** - &quot;만`errorEvent`&quot;값을 지정하면 AccessEnabler JavaScript SDK에서 고급 오류 보고서 콜백을 트리거합니다.

**`handlerName`** - 오류 처리기 함수의 이름을 지정하는 문자열.


두 바인드 매개 변수는 모두 다음 집합의 문자만 사용해야 합니다. `[0-9a-zA-Z][-._a-zA-Z0-9]`즉, 매개 변수는 숫자나 문자로 시작해야 하며 하이픈, 마침표, 밑줄 및 영숫자만 포함할 수 있습니다.  또한 매개 변수는 1,024자를 초과할 수 없습니다.

**예** 바인딩 오류 처리기의 경우:

```JavaScript
accessEnabler.bind('errorEvent', 'myCustomErrorHandler');
accessEnabler.bind('errorEvent', 'errorLogger');
```

기술적 한계로 인해 닫힘이나 익명 함수를 바인딩할 수 없습니다. 두 번째 매개 변수에서 메서드의 이름을 지정해야 합니다.


### 2. 바인딩 해제 {#unbind}

**`.unbind(eventType:String, handlerName:String=null):void`**

이전에 첨부된 이벤트 처리기를 제거합니다.

**`eventType`** - &#39;`errorEvent`&#39; 값을 지정하면 AccessEnabler JavaScript SDK에서 고급 오류 보고서 콜백을 트리거합니다.

**`handlerName`** - 이 null이거나 지정된 의 연결된 모든 처리기가 누락된 경우 오류 처리기 함수의 이름을 지정하는 문자열 `eventType` 제거됩니다.

두 바인드 매개 변수는 모두 다음 집합의 문자만 사용해야 합니다. `[0-9a-zA-Z][-._a-zA-Z0-9]`즉, 매개 변수는 숫자나 문자로 시작해야 하며 하이픈, 마침표, 밑줄 및 영숫자만 포함할 수 있습니다.  또한 매개 변수는 1,024자를 초과할 수 없습니다.

**예** 단일 오류 핸들러를 제거하는 경우:

`accessEnabler.unbind('errorEvent', 'errorLogger');`

**예** 모든 오류 처리기 제거:

`accessEnabler.unbind('errorEvent');`


### AccessEnabler iOS/tvOS SDK {#accessenabler-ios-tvos-sdk}

새 오류 보고 시스템은 필수 항목이므로 구현자는 새 목표 C &quot;EntitlementStatus&quot; 프로토콜을 명시적으로 준수해야 합니다. 이 새로운 접근 방식을 통해 프로그래머는 고급 오류 보고를 받을 수 있습니다.

#### 구현 {#accessenab-ios-tvossdk-imp}

구현자는 다음을 준수해야 합니다 **EntitlementStatus** 프로토콜:

**EntitlementStatus.h**

```OBJ-C
    #import <Foundation/Foundation.h>
     
    @protocol EntitlementStatus <NSObject>
    - (void)status:(NSDictionary *)statusDictionary;
    @end
```

사용자 **상태** 함수는 단일 개체를 수신합니다( `NSDictionary`)를 사용할 수 있습니다.

```OBJ-C
    {
          errorId: "CFG410",
          level: "error",
          message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

**1. 선언**

```OBJ-C
    @interface MyEntitlementStatusDelegate : NSObject <EntitlementStatus>
```

**2. 구현**

```OBJ-C
    @implementation DemoAppAppDelegate     
    // very simple implementation
    - (void)status:(NSDictionary *)statusDict {
        NSLog(@"%@:\n%@", statusDict[@"level"], statusDict);
    }
```


### AccessEnabler Android SDK {#accessenabler-android-sdk}

구현자가 을 명시적으로 준수해야 하므로 새 오류 보고 시스템은 필수입니다. `IAccessEnablerDelegate` 인터페이스 정의 프로토콜입니다. 이 새로운 접근 방식을 통해 프로그래머는 고급 오류 보고를 받을 수 있습니다.

#### 구현 {#access-enablr-androidsdk-imp}

구현자가 새 를 처리해야 합니다. `status` 인터페이스의 메서드`IAccessEnablerDelegate`. 다음 **`status`** 함수가 단일 **`AdvancedStatus`** 다음 모델이 있는 개체:

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**샘플**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

### AccessEnabler Fireos SDK {#accessenabler-fireos-sdk}


구현자가 을 명시적으로 준수해야 하므로 새 오류 보고 시스템은 필수입니다. `IAccessEnablerDelegate` 인터페이스 정의 프로토콜입니다. 이 새로운 접근 방식을 통해 프로그래머는 고급 오류 보고를 받을 수 있습니다.

#### 구현 {#access-enab-fireos-sdk-}

구현자가 새 를 처리해야 합니다. `status`인터페이스의 메서드`IAccessEnablerDelegate`. 다음 **`status`** 함수가 단일 **`AdvancedStatus`** 다음 모델이 있는 개체:

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**샘플**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

## 고급 오류 코드 참조 {#advanced-error-codes-reference}

다음 표에는 최신 오류 API에 의해 노출된 오류 코드와 이를 수정하기 위해 취해야 할 제안 작업이 나열되고 설명되어 있습니다.

| ID | 레벨 | 설명 | 개발자 작업 | 사용자 작업 | JavaScript | iOS/tvOS | Android |
|---|-------------|------------|----------------|---|---|---|---|
| AAPL&amp; AAPL_ERROR | 오류 | 일반 Apple SSO 오류 | 오류에는 원래 VSA 오류가 있는 세부 정보 필드가 포함되어 있습니다. | 해당 사항 없음 | 해당 사항 없음 | 예 | 해당 사항 없음 |
| VSA203 | 정보 | 사용자는 플랫폼 SSO를 통해 로그인하여 인증이 진행되는 동안 애플리케이션에서 로그아웃을 결정했습니다. | tvOS의 설정 -> 계정 -> TV 공급자에서 명시적으로 로그아웃하도록 사용자에게 지시하거나 확인합니다. <br><br> iOS/iPadOS의 설정 -> TV 공급자에서 명시적으로 로그아웃하도록 사용자에게 지시하거나 확인합니다. | tvOS의 설정 -> 계정 -> TV 공급자에서 명시적으로 로그아웃합니다. <br> <br> iOS/iPadOS의 설정 -> TV 공급자에서 명시적으로 로그아웃 | 해당 사항 없음 | 예 | 해당 사항 없음 |
| VSA404 | 정보 | 응용 프로그램 비디오 구독자 계정 권한이 확인되지 않았습니다. | SSO(Single Sign-On) 사용자 경험의 이점을 설명하여 구독 정보에 대한 액세스 권한을 거부하는 사용자에게 인센티브를 제공합니다. | 사용자는 애플리케이션 설정(TV 공급자 액세스) 또는 설정 -> iOS/iPadOS의 TV 공급자 또는 설정 -> 계정 -> tvOS의 TV 공급자 섹션으로 이동하여 결정을 변경할 수 있습니다. | 해당 사항 없음 | 예 | 해당 사항 없음 |
| VSA503 | 정보 | 응용 프로그램 비디오 구독자 계정 메타데이터 요청이 실패했습니다. | MVPD 끝점이 응답하지 않습니다. 응용 프로그램이 일반 인증 흐름으로 전환될 수 있습니다. | 해당 사항 없음 | 해당 사항 없음 | 예 | 해당 사항 없음 |
| 500 | 오류 | 내부 오류 | AccessEnablerDebug를 사용하고 디버그 로그(console.log 출력)를 검사하여 무엇이 잘못되었는지 확인합니다. | 해당 사항 없음 | 예 | 예 | 해당 사항 없음 |
| SEC403 | 오류 | 도메인 보안 오류입니다. 요청자가 잘못된 도메인을 사용하고 있습니다. 특정 요청자 ID에 사용되는 모든 도메인은 Adobe으로 허용 목록에 추가해야 합니다. | - 허용된 도메인 목록에서만 AccessEnabler 로드 <br> <br> - 사용된 요청자 ID에 대한 도메인 허용 목록을 관리하려면 Adobe에게 문의하십시오. <br> <br> - iOS: 올바른 인증서를 사용하고 있으며 서명이 제대로 생성되었는지 확인합니다. | 해당 사항 없음 | 해당 사항 없음 | 예 | 해당 사항 없음 |
| SEC412 | 경고 | [릴리스 2.5에서 사용 가능] 장치 ID가 일치하지 않습니다. 이 문제는 기본 플랫폼이 장치 ID를 변경할 때마다 발생할 수 있습니다. 이 경우 기존 토큰이 지워지고 사용자가 더 이상 인증되지 않습니다. 이 문제는 사용자가 JS SDK를 사용하고 로밍 중일 때 합법적으로 발생합니다(JS에서 클라이언트 IP는 장치 ID의 일부임). 그렇지 않으면 사기 시도를 나타낼 수 있습니다. 즉, 다른 디바이스에서 토큰을 복사하려는 시도입니다. | - 경고 수를 모니터링합니다. 뚜렷한 이유 없이 급증하는 경우(최근 브라우저 업데이트가 없음, 새 운영 체제) 사기 시도를 나타낼 수 있습니다.  <br> <br>- 원할 경우 다시 로그인해야 함을 사용자에게 알립니다. | 다시 로그인합니다. | 예 | 예 | 예 부터 3.2 |
| SEC420 | 오류 | Adobe Pass 인증 서버와 통신할 때 HTTP 보안 오류가 발생합니다. 이 오류는 일반적으로 스푸핑/프록시가 있을 때 발생합니다. | - 로드 `[https://]{SP_FQDN\}` 예를 들어 브라우저에서 SSL 인증서를 수동으로 승인합니다. **https://api.auth.adobe.com** 또는 **https://api.auth-staging.adobe.com** <br> <br>- 프록시 인증서를 신뢰할 수 있는 것으로 표시 | 일반 사용자의 경우 이 문제가 발생할 경우 중간자 공격 가능성이 있음을 나타냅니다. | 예 | 예 | 예 부터 3.2 |
| CFG100 | 경고 | 클라이언트 컴퓨터 날짜/시간/시간대가 올바르게 설정되지 않았습니다. 이로 인해 인증/권한 부여 오류가 발생할 수 있습니다. | - 사용자에게 올바른 시간을 설정하도록 알립니다. <br> <br> 권리 유형 흐름이 실패할 수 있으므로 이를 방지하기 위해 조치를 취하십시오. | 올바른 날짜/시간을 설정합니다. | 예 | 예 | 예 부터 3.2 |
| CFG400 | 오류 | 잘못된 요청자 ID가 입력되었습니다. | 개발자는 유효한 요청자 ID를 지정해야 합니다. | 해당 사항 없음 | 예 | 예 | 예 부터 3.2 |
| CFG404 | 오류 | Adobe Pass 인증 서버를 찾을 수 없습니다. 이 문제는 다음 3가지 경우에 발생할 수 있습니다. <br><br> - 개발자에게 잘못된 스푸핑이 있습니다. <br><br> -사용자에게 네트워킹 문제가 있어 Adobe Pass 인증 도메인에 연결할 수 없습니다. <br><br> -Adobe Pass 인증 서버가 잘못 구성되었습니다. <br><br>  **참고:** Firefox에서는 CFG404 대신 CFG400이 표시됩니다(브라우저 제한 사항) | - 스푸핑을 확인합니다. <br><br> -네트워크/DNS 설정을 확인합니다. <br><br> - Adobe에 알립니다. | 네트워크/DNS 설정을 확인합니다. | 예 | 예 | 예 부터 3.2 |
| CFG410 | 오류 | AccessEnabler가 너무 오래되었습니다. | 사용자에게 캐시를 지우도록 알립니다. | 브라우저 캐시를 지웁니다. | 예 | 해당 사항 없음 | 예 부터 3.2 |
| CFG5xx | 오류 | Adobe Pass 인증 서버에 내부 오류가 발생했습니다. xx는 임의의 숫자일 수 있습니다. | - Adobe Pass 인증을 사용할 수 없음을 사용자에게 알립니다. <br><br> - Adobe Pass 인증을 무시합니다. <br> <br> - Adobe에게 알립니다. | 나중에 시도하십시오. | 예 | 예 | 예 부터 3.2 |
| N000 | 정보 | 사용자가 인증되지 않았습니다. | 해당 사항 없음 | 로그인. | 예 | 예 | 예 부터 3.2 |
| N001 | 정보 | 백그라운드에서 수동 인증 시도가 시작되었습니다. 이 문제는 &quot;요청자별 인증&quot;으로 구성된 MVPD에 대해 발생합니다. 사용자가 자동으로 인증되기를 기대하지만 이로 인해 초기화 시 성능이 저하됩니다. | 선택적으로 사용자에게 알리거나 사용자에게 &quot;작업이 진행 중&quot;이라고 알리는 UI를 표시합니다. | 잠깐. | 예 | 예 | 예 부터 3.2 |
| N003 | 정보 | 사용자가 Apple MVPD 선택기에서 &quot;기타 TV 공급자&quot; 옵션을 선택합니다. | 다음 *displayProviderDialog* 콜백이 호출되고 응용 프로그램이 일반 인증 흐름으로 전환될 수 있습니다. | 일반 MVPD를 선택하고 로그인 화면을 계속합니다. | 해당 사항 없음 | 예 | 해당 사항 없음 |
| N004 | 정보 | 사용자는 현재 요청자가 지원하지 않는 TV 공급자를 선택합니다. | 다음 *displayProviderDialog* 콜백이 호출되고 응용 프로그램이 일반 인증 흐름으로 전환될 수 있습니다. | 일반 MVPD를 선택하고 로그인 화면을 계속합니다. | 해당 사항 없음 | 예 | 해당 사항 없음 |
| N005 | 정보 | MVPD 선택기가 취소되었습니다. | 해당 사항 없음 | 해당 사항 없음 | 예 | 예 | 예 부터 3.2 |
| N010 | 경고 | 선택한 MVPD에 대해 authenticate-all degradation 규칙이 적용되는 동안 사용자가 인증되었습니다. | 선택적으로 사용자에게 MVPD 문제로 인해 &quot;무료&quot; 액세스를 받고 있음을 알립니다. | 해당 사항 없음 | 예 | 예 | 예 부터 3.2 |
| N011 | 정보 | 사용자가 TempPass를 사용하여 인증되었습니다. | - 사용자에게 알립니다. <br> <br> - 선택적으로 일반 MVPD 목록을 표시합니다. | (선택 사항) 일반 MVPD로 로그인합니다. | 예 | 예 | 예 부터 3.2 |
| N111 | 경고 | 만료된 TempPass | - 사용자에게 알립니다. <br> <br> - 일반 MVPD 목록을 표시합니다. <br> <br> - TempPass 옵션을 숨깁니다. | 일반 MVPD로 로그인합니다. | 예 | 예 | 예 부터 3.2 |
| N130 | 오류 | **세션에서 인증 토큰을 찾을 수 없습니다.**  이는 다음 중 하나로 인해 발생할 수 있습니다. <br> <br> 1. 브라우저에 (타사) 쿠키가 비활성화되었습니다. (AccessEnabler JavaScript SDK 버전 4.x에는 적용되지 않음) <br> <br> 2. 브라우저에 사이트 간 추적 방지 가 활성화되어 있습니다(Safari 11+). <br> <br> 3. 세션이 만료되었습니다. <br> <br> 4. 프로그래머가 잘못된 연속 인증 API를 호출합니다. <br> <br> 참고: 이 오류 코드는 전체 페이지 리디렉션 인증 흐름에 사용할 수 없습니다. | 1. (타사) 쿠키를 활성화하라는 메시지 표시 <br> <br> 2. 사이트 간 추적을 비활성화하도록 사용자에게 홍보 <br> <br> 3. 사용자에게 다시 인증하라는 메시지 표시 <br> <br> 4. 올바른 순서로 API 호출 | 1. (타사) 쿠키 사용 <br> <br> 2. 사이트 간 추적 비활성화 <br> <br> 3. 재인증 <br> <br> 4. 해당 사항 없음 | 예 | 예 | 예 부터 3.2 |
| N500 | 오류 | 내부 오류입니다. <br> <br> 참고: 원래 오류 시스템의 &quot;일반 인증 오류&quot; 및 &quot;내부 인증 오류&quot;입니다. 이 오류는 결국 단계적으로 제거됩니다. | AccessEnablerDebug를 사용하고 디버그 로그(console.log 출력)를 검사하여 무엇이 잘못되었는지 확인합니다. | 해당 사항 없음 | 예 | 예 | 해당 사항 없음 |
| R401 | 오류 | 액세스 토큰을 가져오는 도중 오류가 발생했습니다. <br> <br> 참고: 이 오류는 복구할 수 없습니다. 응용 프로그램을 사용할 수 없음을 사용자에게 알립니다. | - iOS: 애플리케이션에서 소프트웨어 명령문과 사용자 정의 체계를 확인합니다. <br> <br> - JavaScript: 웹 사이트 애플리케이션에서 소프트웨어 구문을 확인합니다. <br> <br> Zendesk를 사용하여 티켓을 열고 시스템을 일시적으로 사용할 수 없음을 사용자에게 알립니다. | 해당 사항 없음 | 예, v4.0부터 | v3.0부터 예 | 예 부터 3.2 |
| R400 | 오류 | 응용 프로그램이 등록되지 않았습니다. 소프트웨어 문이 잘못되었거나 취소되었습니다. <br> <br> 참고: 이 오류는 복구할 수 없습니다. 응용 프로그램을 사용할 수 없음을 사용자에게 알립니다. | - iOS: 애플리케이션에서 소프트웨어 명령문과 사용자 정의 체계를 확인합니다. <br> <br> - JavaScript: 웹 사이트 애플리케이션에서 소프트웨어 구문을 확인합니다. <br> <br> Zendesk를 사용하여 티켓을 열고 시스템을 일시적으로 사용할 수 없음을 사용자에게 알립니다. | 해당 사항 없음 | 예, v4.0부터 | v3.0부터 예 | 예 부터 3.2 |
| REG500 | 오류 | 서버에서 등록 코드를 가져올 수 없습니다. <br> <br> 참고: 이 오류는 복구할 수 없습니다. 응용 프로그램을 사용할 수 없음을 사용자에게 알립니다. | Zendesk를 사용하여 티켓을 열고 시스템을 일시적으로 사용할 수 없음을 사용자에게 알립니다. | 해당 사항 없음 | 예, v4.0부터 | v3.0부터 예 | 예 부터 3.2 |
| REGCODE | 성공 | tvOS 플랫폼에서 setSelectedProvider API라고 하는 애플리케이션입니다. | 제공된 등록 코드를 사용하여 로그인할 두 번째 장치(화면)를 사용하도록 사용자에게 지시/표시합니다. | 두 번째 장치(화면)에서 regcode를 사용하여 인증을 시작합니다. | 해당 사항 없음 | tvOS에만 해당 | 해당 사항 없음 |
| Z010 | 경고 | 선택한 MVPD에 대해 authenticate-all 또는 authorize-all 열화 규칙이 적용되는 동안 사용자가 승인되었습니다. | 선택적으로 사용자에게 MVPD 문제로 인해 &quot;무료&quot; 액세스를 받고 있음을 알립니다. | 해당 사항 없음 | 예 | 예 | 예 부터 3.2 |
| Z011 | 정보 | 사용자가 TempPass를 사용하여 승인됨 | 선택적으로 사용자에게 알림 | 해당 사항 없음 | 예 | 예 | 예 부터 3.2 |
| Z100 | 오류 | 사용자에게 요청된 리소스에 대한 구독이 없거나 MVPD에서 시작된 다른 이유(예: 비디오가 사용자 계정에 대한 자녀 보호 설정과 일치하지 않음)로 인해 인증에 실패했습니다 | - 재생을 허용하지 않습니다. <br> <br> - 사용자에게 알립니다. <br> <br> - 오류 메시지의 &#39;message&#39; 키에는 MVPD에서 제공하는 보다 자세한 메시지가 포함될 수 있습니다. | 해당 사항 없음 | 예 | 예 | 예 부터 3.2 |
| Z110 | 오류 | 반복된 MVPD 거부 때문에 인증이 거부되었습니다. 사기 시도나 DOS 가능성이 있습니다. | - 재생을 허용하지 않습니다. <br> <br> - 사용자에게 알립니다. | 해당 사항 없음 | 예 | 예 | 예 부터 3.2 |
| Z120 | 오류 | MVPD와 통신하는 중 기술적인 이유로 승인이 거부되었습니다. 가능한 네트워크 오류입니다. | - 재생을 허용하지 않습니다. <br> <br> - MVPD에 문제가 발생하여 나중에 다시 시도해야 함을 사용자에게 알립니다. | 나중에 시도하십시오. | 예 | 예 | 예 부터 3.2 |
| Z130 | 오류 | 잘못된/잘못된 형식의 리소스가 사용되었기 때문에 권한 부여가 거부되었습니다. | 리소스 문자열을 확인하고 수정하십시오. 일반적으로 이 오류는 잘못된 MRSS나 MRSS 대신 일반 문자열을 사용하기 때문에 발생합니다. | 해당 사항 없음 | 예 | 예 | 예 부터 3.2 |
| Z169 | 오류 | 지정된 리소스에 authzNone 성능 저하 규칙이 적용되었기 때문에 인증이 거부되었습니다. | 사용자에게 알림 | 해당 사항 없음 | 예 | 예 | 예 부터 3.2 |
| Z500 | 오류 | 내부 오류입니다. <br> <br>  참고: 이는 레거시 &quot;일반 인증 오류&quot; 및 &quot;내부 인증 오류&quot;입니다. 이 오류는 결국 단계적으로 제거됩니다. | AccessEnablerDebug를 사용하고 디버그 로그(console.log 출력)를 검사하여 무엇이 잘못되었는지 확인합니다. | 해당 사항 없음 | 예 | 예 | 예 부터 3.2 |
| P100 | 오류 | 사전 인증에 실패했습니다. 너무 많은 리소스에 대한 권한 부여를 요청하기 때문일 수 있습니다. | - 허용된 최대 리소스 수보다 많이 사용하지 마십시오. <br> <br> - Adobe Pass 인증 지원 센터에 문의하여 허용된 최대 리소스 수를 찾거나 설정합니다. | 해당 사항 없음 | v3.0부터 예 | 예 | 예 부터 3.2 |
| IS2XX | 오류 | 이러한 오류 코드는 개별화 서버 끝점 응답 데이터의 형식이 잘못되었거나 필요한 개별화 정보가 누락된 경우 반환됩니다. | Zendesk를 사용하여 티켓을 열고 시스템을 일시적으로 사용할 수 없음을 사용자에게 알립니다. | 해당 사항 없음 | v3.0부터 예 | 해당 사항 없음 | 해당 사항 없음 |
| IS4XX | 오류 | 이러한 오류 코드는 개별화 서버 엔드포인트 오류 4XX의 경우 반환됩니다. - 은 응답의 HTTP 상태 코드입니다. | Zendesk를 사용하여 티켓을 열고 시스템을 일시적으로 사용할 수 없음을 사용자에게 알립니다. | 해당 사항 없음 | v3.0부터 예 | 해당 사항 없음 | 해당 사항 없음 |
| IS5XX | 오류 | 이러한 오류 코드는 개별화 서버 엔드포인트 오류 5XX의 경우 반환됩니다. - 은 응답의 HTTP 상태 코드입니다. | Zendesk를 사용하여 티켓을 열고 시스템을 일시적으로 사용할 수 없음을 사용자에게 알립니다. | 해당 사항 없음 | v3.0부터 예 | 해당 사항 없음 | 해당 사항 없음 |
| IS0 | 오류 | 이 코드는 개별화 서버 끝점이 전혀 응답하지 않아 연결 시간이 초과된 경우에 반환됩니다 | Zendesk를 사용하여 티켓을 열고 시스템을 일시적으로 사용할 수 없음을 사용자에게 알립니다. | 해당 사항 없음 | v3.0부터 예 | 해당 사항 없음 | 해당 사항 없음 |
| LS011 | 경고 | LSO/LocalStorage 문제 및 WebStorage 문제(또는 비가용성)로 인해 AccessEnabler가 휘발성 스토리지를 사용하고 있습니다. <br> <br> 인증/권한 부여가 현재 페이지 이상으로 지속되지 않습니다. 페이지를 로드할 때마다 사용자를 인증해야 합니다. 구성된 TTL은 페이지 다시 로드 시 적용되지 않습니다. | - 제한 사항을 사용자에게 알립니다. <br> <br> - 사용 가능한 저장 공간을 늘리는 방법을 사용자에게 알립니다. <br> <br> - 또는 스토리지를 지우려면 로그아웃합니다. | - 스토리지 용량 증가 <br> <br> - 스토리지를 지우려면 로그아웃합니다. | 예 | 해당 사항 없음 | 해당 사항 없음 |

<br>

## 원래 오류 보고 {#original-error-reporting}

이 섹션에서는 원래 오류 코드와 함께 원래 오류 보고 시스템에 대해 설명합니다. 원래 오류 보고 시스템에서는 AccessEnabler가 다음 두 콜백 함수에 오류를 전달합니다. `setAuthenticationStatus()` 에 대한 호출 후 `checkAuthentication()`; `tokenRequestFailed()`, 호출 실패 후 `checkAuthorization()` 또는 `getAuthorization()`.

원래 오류 보고 및 상태 API는 이전과 동일하게 계속 작동합니다. 그러나 앞으로는 원래 오류 보고 API가 업데이트되지 않습니다. 이전 오류에 대한 모든 새 오류 보고 및 업데이트가 새 오류에만 반영됩니다. [고급 오류 보고 시스템](#advanced-error-reporting).


원래 오류 보고 시스템을 사용하는 예는 [JavaScript API 참조](/help/authentication/javascript-sdk-api-reference.md):[setAuthenticationStatus()](/help/authentication/javascript-sdk-api-reference.md#set-authn-status-isauthn-error) 및 [tokenRequestFailed()](/help/authentication/javascript-sdk-api-reference.md#token-request-failed-error-msg) 함수, [iOS/tvOS API 참조](/help/authentication/iostvos-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/javascript-sdk-api-reference.md#setAuthNStatus)및 [tokentRequestFailed()](/help/authentication/javascript-sdk-api-reference.md#tokenReqFailed), [Android API 참조](/help/authentication/android-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/android-sdk-api-reference.md#setAuthNStatus) 및 [tokenRequestFailed()](/help/authentication/android-sdk-api-reference.md#setAuthNStatus#tokenRequestFailed).

### 원래 콜백 오류 코드 {#original-callback-error-codes}

| **일반 오류** | |
|---|---|
| 내부 오류 | 요청을 처리하는 동안 시스템 오류가 발생했습니다. |
| 공급자가 선택되지 않음 오류 | 고객이 공급자 선택 대화 상자에서 취소할 때 발생합니다. |
| 공급자를 사용할 수 없음 오류 | 사용할 수 있는 공급자가 없을 때 발생합니다. |
|  |  |
| **인증 오류** | |
| 일반 인증 오류 | 이유를 알 수 없거나 게시할 수 없을 때 반환됩니다. |
| 내부 인증 오류 | 인증을 시도하는 동안 시스템 오류가 발생했습니다. |
| 사용자가 인증되지 않음 오류 | 사용자가 인증되지 않았습니다. |
|  |  |
| **인증 오류** |  |
| 일반 인증 오류 | 이유를 알 수 없거나 게시할 수 없을 때 반환됩니다. |
| 내부 인증 오류 | 권한을 부여하는 동안 시스템 오류가 발생했습니다. |
| 사용자가 승인되지 않음 오류 | 고객은 요청된 콘텐츠를 볼 수 있는 권한이 없습니다. |

<!--
## Related Information {#related-information}

* [JavaScript API Reference](/help/authentication/javascript-sdk-api-reference.md)
* [iOS/tvOS API Reference](/help/authentication/iostvos-sdk-api-reference.md)
* **Android API Reference**
-->
