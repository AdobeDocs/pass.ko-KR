---
title: 클라이언트 정보 전달(장치, 연결 및 애플리케이션)
description: 클라이언트 정보 전달(장치, 연결 및 애플리케이션)
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1666'
ht-degree: 1%

---

# (기존) 클라이언트 정보(장치, 연결 및 애플리케이션) 전달 {#pass-client-info}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 범위 {#pass-client-info-scope}

이 문서에서는 프로그래머 애플리케이션에서 Adobe Pass 인증 REST API 또는 SDK로 클라이언트 정보(장치, 연결 및 애플리케이션)를 전달하기 위한 세부 정보 및 쿠키 설명서를 집계합니다.

클라이언트 정보 제공의 이점은 다음과 같습니다.

* HBA를 지원할 수 있는 일부 디바이스 유형 및 MVPD의 경우 HBA(Home Base Authentication)를 올바르게 설정할 수 있습니다.
* 일부 디바이스 유형의 경우 TTL을 올바르게 적용하는 기능(예: TV에 연결된 디바이스의 인증 세션에 대해 더 긴 TTL을 구성).
* ESM(자격 서비스 모니터링)을 사용하여 장치 유형 간에 분류된 보고서에서 비즈니스 지표를 올바르게 집계하는 기능입니다.
* 다양한 비즈니스 규칙을 적절하게 적용하는 기능을 차단 해제합니다(예: 성능 저하).

## 개요 {#pass-client-info-overview}

클라이언트 정보는 다음과 같이 구성됩니다.

* **장치** 사용자가 프로그래머 콘텐츠를 사용하려는 장치의 하드웨어 및 소프트웨어 특성에 대한 정보입니다.
* **연결** 사용자가 Adobe Pass 인증 서비스 및/또는 프로그래머 서비스(예: 서버 간 구현)에 연결하는 장치의 연결 특성에 대한 정보입니다.
* **응용 프로그램** 사용자가 프로그래머 콘텐츠를 사용하려는 등록된 응용 프로그램에 대한 정보입니다.

클라이언트 정보는 다음 표에 표시된 키로 구축된 JSON 개체입니다.

>[!NOTE]
>
>다음 **키**&#x200B;은(는) 클라이언트 정보 JSON 개체에서 보낼 **필수**&#x200B;입니다. **모델**, **osName**.
>
>`primaryHardwareType`, `osName`, `osFamily`, `browserName`, `browserVendor`, `connectionSecure` 키에 **restricted** 값이 있습니다.

|   | 키 | 제한됨 | 설명 | 가능한 값 |
|---|---|---|---|---|
|            | 기본 하드웨어 유형 | # 예 | 디바이스의 기본 하드웨어 유형입니다. | # 값이 제한됨:                                                                     카메라                                                      DataCollectionTerminal                                                      데스크탑                                                      포함된 네트워크 모듈                                                      eReader                                                      게임콘솔                                                      GeolocationTracker                                                      안경                                                      MediaPlayer                                                      휴대폰                                                      결제 단말기                                                      플러그인 모뎀                                                      셋톱 박스                                                      TV                                                      태블릿                                                      WirelessHotspot                                                      손목시계                                                      알 수 없음 |
| #mandatory | 모델 | 아니요 | 장치의 모델 이름입니다. | 예: iPhone, SM-G930V, AppleTV 등 |
|            | 버전 | 아니요 | 디바이스 버전. | 예: 2.0.1 등 |
|            | 제조업체 | 아니요 | 장치의 제조 회사/조직입니다. | 예: 삼성, LG, ZTE, 화웨이, 모토로라, Apple 등 |
|            | 공급업체 | 아니요 | 장치의 판매 회사/조직입니다. | 예: Apple, Samsung, LG, Google 등 |
| #mandatory | osName | # 예 | 장치의 운영 체제(OS) 이름입니다. | # 값이 제한됨:                                                   Android                   Chrome                   리눅스                   Mac                   OS X                   OpenBSD                   Roku 운영 체제                   Windows                   iOS                   tvOS                   webOS |
|            | osFamily | 예 | 장치의 운영 체제(OS) 그룹 이름입니다. | # 값이 제한됨:                                                   Android                   BSD                   리눅스                   플레이스테이션                   Roku 운영 체제                   Symbian                   티젠                   Windows                   iOS                   macOS                   tvOS                   webOS |
|            | osVendor | 아니요 | 장치의 운영 체제(OS) 공급업체. | Amazon                   Apple                   Google                   LG                   Microsoft                   모질라                   닌텐도                   노키아                   Roku                   Samsung                   소니                   Tizen 프로젝트 |
|            | osVersion | 아니요 | 장치의 운영 체제(OS) 버전입니다. | 예: 10.2, 9.0.1 등 |
|            | browserName | # 예 | 브라우저의 이름입니다. | # 값이 제한됨:                                                   Android 브라우저                   Chrome                   Edge                   Firefox                   Internet Explorer                   Opera                   Safari                   시몽키                   Symbian 브라우저 |
|            | browserVendor | # 예 | 브라우저의 빌드 회사/조직입니다. | # 값이 제한됨:                                                   Amazon                   Apple                   Google                   Microsoft                   모토로라                   모질라                   넷스케이프                   닌텐도                   노키아                   Samsung                   소니 에릭슨 |
|            | browserVersion | 아니요 | 장치의 브라우저 버전입니다. | 예: 60.0.3112 |
|            | userAgent | 아니요 | 장치의 사용자 에이전트입니다. | 예: Mozilla/5.0 (Macintosh, Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, 예: Gecko) 버전/10.0.3 Safari/602.4.8 |
|            | displayWidth | 아니요 | 장치의 실제 화면 너비입니다. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | 아니요 | 장치의 실제 화면 높이입니다. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPi | 아니요 | 장치의 실제 화면 픽셀 밀도입니다. | 예: 294 |
|            | 대각 화면 크기 | 아니요 | 디바이스의 물리적 화면 대각선 치수(인치)입니다. | 예: 5.5, 10.1 |
|            | connectionIp | 아니요 | HTTP 요청을 보내는 데 사용되는 장치의 IP입니다. | 예: 8.8.4.4 |
|            | connectionPort | 아니요 | HTTP 요청을 전송하는 데 사용되는 장치의 포트입니다. | 예: 53124 |
|            | connectionType | 아니요 | 네트워크 연결 유형입니다. | 예: WiFi, LAN, 3G, 4G, 5G |
|            | connectionSecure | # 예 | 네트워크 연결 보안 상태입니다. | # 값이 제한됨:                                                   true - 보안 네트워크의 경우                   false - 공용 핫스팟의 경우 |
|            | applicationId | 아니요 | 애플리케이션 고유 식별자. | 예: CNN |

## API 참조 {#api-ref}

이 섹션에서는 Adobe Pass 인증 REST API 또는 SDK를 사용할 때 클라이언트 정보를 처리하는 API에 대해 설명합니다.

### 나머지 API {#rest-api}

Adobe Pass 인증 서비스는 다음과 같은 방법으로 클라이언트 정보 수신을 지원합니다.

* **헤더로서: &quot;X-Device-Info&quot;**
* **쿼리 매개 변수: &quot;device_info&quot;**
* **post 매개 변수로: &quot;device_info&quot;**

>[!IMPORTANT]
>
>세 시나리오 모두에서 헤더 또는 매개 변수의 페이로드는 **Base64로 인코딩되고 URL로 인코딩되어야 합니다**.

**SDK**

#### JavaScript SDK {#js-sdk}

AccessEnabler JavaScript SDK은 기본적으로 클라이언트 정보 JSON 개체를 빌드하며, 이 개체는 재정의되지 않는 한 Adobe Pass 인증 서비스로 전달됩니다.

AccessEnabler JavaScript SDK은 [setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options))의 *applicationId* 옵션 매개 변수를 통해 클라이언트 정보 JSON 개체에서 &quot;applicationId&quot; 키를 **재정의만**&#x200B;할 수 있습니다.

>[!CAUTION]
>
>`applicationId` 매개 변수 값은 일반 텍스트 문자열 값이어야 합니다.
>프로그래머 애플리케이션에서 applicationId를 전달하기로 결정한 경우 나머지 클라이언트 정보 키는 AccessEnabler JavaScript SDK에서 계속 계산됩니다.

#### iOS/tvOS SDK {#ios-tvos-sdk}

AccessEnabler iOS/tvOS SDK은 기본적으로 클라이언트 정보 JSON 개체를 빌드하며, 재정의되지 않는 한 Adobe Pass 인증 서비스로 전달됩니다.

AccessEnabler iOS/tvOS SDK은 [setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setoptions)의 device_info 매개 변수를 통해 **전체** 클라이언트 정보 JSON 개체를 재정의합니다.

>[!CAUTION]
>
>*device_info* 매개 변수 값은 **Base64 인코딩** *NSString* 값이어야 합니다.
>
>프로그래머 응용 프로그램에서 *device_info*&#x200B;을(를) 전달하기로 결정하는 경우 AccessEnabler iOS/tvOS SDK에서 계산한 모든 클라이언트 정보 키가 재정의됩니다. 따라서 가능한 한 많은 키에 대한 값을 계산하고 전달하는 것이 매우 중요합니다. 구현에 대한 자세한 내용은 [개요](#pass-client-info-overview) 테이블 및 [iOS/tvOS Cookbook](#ios-tvos)을 참조하십시오.

#### Android/FireOS SDK {#and-fire-os-sdk}

`AccessEnabler` Android/FireOS SDK은 기본적으로 클라이언트 정보 JSON 개체를 빌드하며, 재정의되지 않는 한 Adobe Pass 인증 서비스로 전달됩니다.

`AccessEnabler` Android/FireOS SDK은 [setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setOptions)의/[setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#fire_setOption)의 `device_info` 매개 변수를 통해 **전체** 클라이언트 정보 JSON 개체를 재정의합니다.

>[!NOTE]
>
>`device_info` 매개 변수 값은 **Base64 인코딩** 문자열 값이어야 합니다.

>[!IMPORTANT]
>
>프로그래머 응용 프로그램에서 `device_info`을(를) 전달하기로 결정하는 경우 `AccessEnabler` Android/FireOS SDK에서 계산한 모든 클라이언트 정보 키가 재정의됩니다. 따라서 가능한 한 많은 키에 대한 값을 계산하고 전달하는 것이 매우 중요합니다. 구현에 대한 자세한 내용은 [개요](#pass-client-info-overview) 테이블 및 [Android](#android) 및 [FireOS](#fire-tv) Cookbook을 참조하십시오.

## 요리책 {#cookbooks}

이 섹션에서는 다른 장치 유형의 경우 클라이언트 정보 JSON 개체를 작성하기 위한 설명서에 대해 설명합니다.

>[!IMPORTANT]
>
>**(으)로 표시된 키!**&#x200B;은(는) 필수입니다.

### Android {#android}

장치 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 값(예) |
|---|---------------|-----------------------------|---------------|
| ! | 모델 | Build.MODEL | GT-I9505 |
|   | 공급업체 | Build.BRAND | 삼성 |
|   | 제조업체 | Build.MANUFACTURER | 삼성 |
| ! | 버전 | Build.DEVICE | jflte |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | 하드코드 | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.0.1 |

연결 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 값(예) |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

애플리케이션 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 값(예) |
|---|---------------|-----------|--------------|
|   | applicationId | 하드코드 | CNN |

>[!IMPORTANT]
>
>장치, 연결 및 애플리케이션 정보를 동일한 JSON 개체에 추가해야 합니다. 그런 다음 결과 개체는 **Base64로 인코딩**&#x200B;해야 합니다. 또한 Adobe Pass 인증 REST API의 경우 값은 **URL로 인코딩됨**&#x200B;이어야 합니다.

**샘플 코드**

```JAVA
private JSONObject computeClientInformation() {
     String LOGGING_TAG = "DefineClass.class";
  
     JSONObject clientInformation = new JSONObject();

     String connectionType;

     try {
          ConnectivityManager cm = (ConnectivityManager) getContext().getSystemService(CONNECTIVITY_SERVICE);
          NetworkInfo activeNetwork = cm.getActiveNetworkInfo();

          if (activeNetwork != null && activeNetwork.isConnectedOrConnecting()) {
              switch (activeNetwork.getType()) {
                    case ConnectivityManager.TYPE_WIFI: {
                        connectionType = "WIFI";
                        break;
                    }
                    case ConnectivityManager.TYPE_BLUETOOTH: {
                        connectionType = "BLUETOOTH";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE: {
                        connectionType = "MOBILE";
                        break;
                    }
                    case ConnectivityManager.TYPE_ETHERNET: {
                        connectionType = "ETHERNET";
                        break;
                    }
                    case ConnectivityManager.TYPE_VPN: {
                        connectionType = "VPN";
                        break;
                    }
                    case ConnectivityManager.TYPE_DUMMY: {
                        connectionType = "DUMMY";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE_DUN: {
                        connectionType = "MOBILE_DUN";
                        break;
                    }
                    case ConnectivityManager.TYPE_WIMAX: {
                        connectionType = "WIMAX";
                        break;
                    }
                    default:
                       connectionType = ConnectivityManager.EXTRA_OTHER_NETWORK_INFO;
              }
          } else {
                connectionType = ConnectivityManager.EXTRA_NO_CONNECTIVITY;
          }
     } catch (Exception e) {
          connectionType = "notAccessible";
     }

     try {
          clientInformation.put("model",Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer",Build.MANUFACTURER);
          clientInformation.put("version",Build.DEVICE);
          clientInformation.put("osName","Android");
          clientInformation.put("osVersion",Build.VERSION.RELEASE);
          clientInformation.put("connectionType",connectionType);
          clientInformation.put("applicationId","CNN");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

>[!NOTE]
>
>**리소스:**
>* Java 개발자 설명서에서 공용 클래스 [빌드](https://developer.android.com/reference/android/os/Build.html){target=_blank}.

### 파이어TV {#fire-tv}

장치 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 값(예:) |
|---|---------------|-----------------------------|--------------|
| ! | 모델 | Build.MODEL | AFTM |
|   | 공급업체 | Build.BRAND | Amazon |
|   | 제조업체 | Build.MANUFACTURER | Amazon |
| ! | 버전 | Build.DEVICE | 몬토야 |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | 하드코드 | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1 |

연결 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 값(예) |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

애플리케이션 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 값(예) |
|---|---------------|-----------|--------------|
|   | applicationId | 하드코드 | CNN |

>[!IMPORTANT]
>
>장치, 연결 및 애플리케이션 정보를 동일한 JSON 개체에 추가해야 합니다. 그런 다음 결과 개체는 **Base64로 인코딩**&#x200B;해야 합니다. 또한 Adobe Pass 인증 REST API의 경우 값은 **URL로 인코딩됨**&#x200B;이어야 합니다.

>[!NOTE]
>
>**리소스:**
>* Android 개발자 설명서에서 공용 클래스 [빌드](https://developer.android.com/reference/android/os/Build.html){target=_blank}.
>* [FireTV 장치 식별](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

장치 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 값(예) |
|---|---------------|------------------------|--------------|
| ! | 모델 | uname.machine | iPhone |
|   | 공급업체 | 하드코드 | Apple |
|   | 제조업체 | 하드코드 | Apple |
| ! | 버전 | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10.2 |

연결 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 값(예) |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [Reachability currentReachabilityStatus] |              |
|   | connectionSecure |                                           |              |


애플리케이션 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 값(예) |
|---|---------------|-----------|--------------|
|   | applicationId | 하드코드 | CNN |

>[!IMPORTANT]
>
>장치, 연결 및 애플리케이션 정보를 동일한 JSON 개체에 추가해야 합니다. 그런 다음 결과 개체가 Base64로 인코딩되어야 합니다. 또한 Adobe Pass 인증 REST API의 경우 값은 URL로 인코딩되어야 합니다.

**샘플 코드**

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"CNN" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

>[!NOTE]
>
>**리소스:**
>* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
>* [uname](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
>* [연결 정보](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

장치 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ! | 모델 | 하드코드 | &quot;Roku&quot; |
|     | 공급업체 | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
|     | 제조업체 | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| ! | 버전 | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
|     | displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
|     | displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| ! | osName | 하드코드 | &quot;Roku&quot; |
| ! | osVersion | ifDeviceInfo.getVersion() |                 |

연결 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 값(예) |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
|   | connectionSecure | 하드코드 | 연결이 유선인 경우 true |

애플리케이션 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 값(예) |
|---|---------------|-----------|--------------|
|   | applicationId | 하드코드 | CNN |

>[!IMPORTANT]
>
>장치, 연결 및 애플리케이션 정보를 동일한 JSON 개체에 추가해야 합니다. 그런 다음 결과 개체는 **Base64로 인코딩**&#x200B;해야 합니다. 또한 Adobe Pass 인증 REST API의 경우 값은 URL로 인코딩되어야 합니다.

>[!NOTE]
>
>자세한 내용은 [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)을(를) 참조하십시오

### 엑스박스 {#xbox}

장치 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 값(예) |
|---|---|---|---|
| ! | 모델 | EasClientDeviceInformation.SystemProductName |                 |
|   | 공급업체 | 하드코드 | Microsoft |
|   | 제조업체 | 하드코드 | Microsoft |
| ! | 버전 | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | displayWidth | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayHeight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ! | osName | EasClientDeviceInformation.OperatingSystem |                 |
| ! | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

연결 정보는 다음과 같은 방법으로 구성할 수 있습니다.

|   | 키 | Source | 예 |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | NetworkAuthenticationType | &quot;없음&quot;, &quot;Wpa&quot; 등 |

애플리케이션 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|---|---|---|
| applicationId | 하드코드 | CNN |

>[!IMPORTANT]
>
>장치, 연결 및 애플리케이션 정보를 동일한 JSON 개체에 추가해야 합니다. 그런 다음 결과 개체는 **Base64로 인코딩**&#x200B;해야 합니다. 또한 Adobe Pass 인증 REST API의 경우 값은 **URL로 인코딩됨**&#x200B;이어야 합니다.

**리소스**

* [EasClientDeviceInformation 클래스](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [DisplayInformation 클래스](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
