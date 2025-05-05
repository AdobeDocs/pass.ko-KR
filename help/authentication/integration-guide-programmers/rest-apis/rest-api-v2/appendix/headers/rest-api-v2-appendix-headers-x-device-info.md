---
title: 헤더 - X-Device-Info
description: REST API V2 - 헤더 - X-Device-Info
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
source-git-commit: 42df16e34783807e1b5eb1a12ca9db92f4e4c161
workflow-type: tm+mt
source-wordcount: '1133'
ht-degree: 2%

---

# 헤더 - X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

<b>X-Device-Info</b> 요청 헤더에는 실제 스트리밍 장치와 관련된 클라이언트 정보(장치, 연결 및 응용 프로그램)가 포함되어 있으며 MVPD가 적용할 수 있는 플랫폼별 규칙을 결정하는 데 사용됩니다.

## 구문 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>: &lt;device_information&gt;</td>
   </tr>
   <tr>
      <td>헤더 유형</td>
      <td>요청 헤더</td>
   </tr>
   <tr>
      <td>표준</td>
      <td>아니요</td>
   </tr>
</table>

## 지시문 {#directives}

<b>&lt;device_information></b>

다음 표에 필요한 것으로 표시된 특성을 적어도 포함하는 JSON 요소의 `Base64-encoded` 값입니다.

<table style="table-layout:auto">
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">현재 상태</th>
        <th style="background-color: #EFF2F7; width: 15%;">키</th>
        <th style="background-color: #EFF2F7;">설명</th>    
        <th style="background-color: #EFF2F7; width: 15%;">제한됨</th>
        <th style="background-color: #EFF2F7;">가능한 값</th>
    </tr>
    <tr>
        <td></td>
        <td>기본 하드웨어 유형</td>
        <td>장치의 기본 하드웨어 유형입니다.</td>
        <td>&check;</td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>카메라</li>
                <li>DataCollectionTerminal</li>
                <li>데스크탑</li>
                <li>포함된 네트워크 모듈</li>
                <li>eReader</li>
                <li>게임콘솔</li>
                <li>GeolocationTracker</li>
                <li>안경</li>
                <li>MediaPlayer</li>
                <li>휴대폰</li>
                <li>결제 단말기</li>
                <li>플러그인 모뎀</li>
                <li>셋톱 박스</li>
                <li>TV</li>
                <li>태블릿</li>
                <li>WirelessHotspot</li>
                <li>손목시계</li>
                <li>알 수 없음</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>필수</i></td>
        <td>모델</td>
        <td>장치의 모델 이름.</td>
        <td></td>
        <td>예: iPhone, SM-G930V, AppleTV 등</td>
    </tr>
    <tr>
        <td><i>필수</i></td>
        <td>버전</td>
        <td>디바이스 버전.</td>
        <td></td>
        <td>예: 2.0.1 등</td>
    </tr>
    <tr>
        <td></td>
        <td>제조업체</td>
        <td>장치의 제조 회사/조직.</td>
        <td></td>
        <td>예: 삼성, LG, ZTE, 화웨이, 모토로라, Apple 등</td>
    </tr>
    <tr>
        <td></td>
        <td>공급업체</td>
        <td>장치의 판매 회사/조직.</td>
        <td></td>
        <td>예: Apple, Samsung, LG, Google 등</td>
    </tr>
    <tr>
        <td><i>필수</i></td>
        <td>osName</td>
        <td>장치의 운영 체제(OS) 이름.</td>
        <td>&check;</td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>Android</li>
                <li>Chrome</li>
                <li>리눅스</li>
                <li>Mac</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku 운영 체제</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osFamily</td>
        <td>장치의 운영 체제(OS) 그룹 이름입니다.</td>
        <td>&check;</td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>리눅스</li>
                <li>플레이스테이션</li>
                <li>Roku 운영 체제</li>
                <li>Symbian</li>
                <li>티젠</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osVendor</td>
        <td>장치의 운영 체제(OS) 공급업체.</td>
        <td>&check;</td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>모질라</li>
                <li>닌텐도</li>
                <li>노키아</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>소니</li>
                <li>Tizen 프로젝트</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>필수</i></td>
        <td>osVersion</td>
        <td>장치의 운영 체제(OS) 버전.</td>
        <td></td>
        <td>예: 10.2, 9.0.1 등</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>브라우저의 이름입니다.</td>
        <td>&check;</td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>Android 브라우저</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>시몽키</li>
                <li>Symbian 브라우저</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVendor</td>
        <td>브라우저의 빌드 회사/조직입니다.</td>
        <td>&check;</td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>모토로라</li>
                <li>모질라</li>
                <li>넷스케이프</li>
                <li>닌텐도</li>
                <li>노키아</li>
                <li>Samsung</li>
                <li>소니 에릭슨</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVersion</td>
        <td>장치의 브라우저 버전.</td>
        <td></td>
        <td>예: 60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>장치의 사용자 에이전트.</td>
        <td></td>
        <td>예: Mozilla/5.0 (Macintosh, Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, 예: Gecko) 버전/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td></td>
        <td>displayWidth</td>
        <td>장치의 실제 화면 너비입니다.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayHeight</td>
        <td>장치의 실제 화면 높이입니다.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayPi</td>
        <td>장치의 실제 화면 픽셀 밀도입니다.</td>
        <td></td>
        <td>예: 294</td>
    </tr>
    <tr>
        <td></td>
        <td>대각 화면 크기</td>
        <td>디바이스의 물리적 화면 대각선 치수(인치)입니다.</td>
        <td></td>
        <td>예: 5.5, 10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIp</td>
        <td>HTTP 요청을 전송하는 데 사용되는 장치의 IP입니다.</td>
        <td></td>
        <td>예: 8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>HTTP 요청을 전송하는 데 사용되는 장치의 포트입니다.</td>
        <td></td>
        <td>예: 53124</td>
    </tr>
    <tr>
        <td><i>필수</i></td>
        <td>connectionType</td>
        <td>네트워크 연결 유형입니다.</td>
        <td></td>
        <td>예: WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionSecure</td>
        <td>네트워크 연결 보안 상태입니다.</td>
        <td>&check;</td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>true - 보안 네트워크의 경우</li>
                <li>false - 공용 핫스팟의 경우</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>applicationId</td>
        <td>애플리케이션의 고유 식별자.</td>
        <td></td>
        <td>예: REF30</td>
    </tr>
</table>


## 예시 {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```

## 요리책 {#cookbooks}

>[!IMPORTANT]
> 
> 코드 조각 및 설명서 리소스는 참조 목적으로 제공됩니다.
> 
> 코드 조각은 완벽하지 않으며 프로젝트에서 작업하려면 추가 수정이 필요할 수 있습니다.
>
> 실제 구현과 관계없이 `X-Device-Info` 헤더에는 [지시문](#directives) 섹션에 설명된 대로 형식이 지정된 값이 있어야 합니다.

### 브라우저 {#browsers}

브라우저에서 실행 중인 클라이언트 응용 프로그램의 경우 브라우저에서 `User-Agent` 헤더에 필요한 최소 정보 집합을 자동으로 보내므로 `X-Device-Info` 헤더를 생략할 수 있습니다.

클라이언트 응용 프로그램이 장치 식별 메커니즘을 제공하는 라이브러리 또는 서비스를 통합하는 경우 `X-Device-Info` 헤더를 사용하여 장치, 연결 및 응용 프로그램에 대한 추가 정보를 제공할 수 있습니다.

### 모바일 장치 {#mobile-devices}

#### iOS 및 iPadOS {#ios-ipados}

[iOS 또는 iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes)를 실행하는 장치의 `X-Device-Info` 헤더를 빌드하려면 다음 문서와 코드 조각 아래를 참조하십시오.

* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice)용 Apple 개발자 설명서입니다.
* [연결 가능성](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html)에 대한 Apple 개발자 설명서입니다.
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html)에 대한 Linux 설명서.

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
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

장치 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|---------------|------------------------|-----------------|
| 모델 | uname.machine | iPhone |
| 공급업체 | 하드코드 | Apple |
| 제조업체 | 하드코드 | Apple |
| 버전 | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 320 |
| displayHeight | UIScreen.mainScreen | 568 |
| osName | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10.2 |

연결 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Reachability currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |


애플리케이션 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|---------------|-----------|-----------------|
| applicationId | 하드코드 | REF30 |

#### Android {#android}

[Android](https://developer.android.com/about/versions)을(를) 실행하는 장치의 `X-Device-Info` 헤더를 빌드하려면 다음 문서와 코드 조각 아래를 참조할 수 있습니다.

* [Build](https://developer.android.com/reference/android/os/Build.html) 클래스에 대한 Android 개발자 설명서입니다.

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
          clientInformation.put("model", Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer", Build.MANUFACTURER);
          clientInformation.put("version", Build.DEVICE);
          clientInformation.put("osName", "Android");
          clientInformation.put("osVersion", Build.VERSION.RELEASE);
          clientInformation.put("connectionType", connectionType);
          clientInformation.put("applicationId", "REF30");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

장치 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|---------------|-----------------------------|-----------------|
| 모델 | Build.MODEL | GT-I9505 |
| 공급업체 | Build.BRAND | 삼성 |
| 제조업체 | Build.MANUFACTURER | 삼성 |
| 버전 | Build.DEVICE | jflte |
| displayWidth | DisplayMetrics.widthPixels | 600 |
| displayHeight | DisplayMetrics.heightPixels | 800 |
| osName | 하드코드 | Android |
| osVersion | Build.VERSION.RELEASE | 5.0.1 |

연결 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

애플리케이션 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|---------------|-----------|-----------------|
| applicationId | 하드코드 | REF30 |

### TV 연결 장치 {#tv-connected-devices}

#### tvOS {#tvos}

[tvOS](https://developer.apple.com/documentation/tvos-release-notes)을(를) 실행하는 장치의 `X-Device-Info` 헤더를 빌드하려면 다음 문서와 코드 조각 아래를 참조하십시오.

* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice)용 Apple 개발자 설명서입니다.
* [연결 가능성](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html)에 대한 Apple 개발자 설명서입니다.
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html)에 대한 Linux 설명서.

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
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

장치 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|---------------|------------------------|-----------------|
| 모델 | uname.machine | 애플TV |
| 공급업체 | 하드코드 | Apple |
| 제조업체 | 하드코드 | Apple |
| 버전 | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 1920 |
| displayHeight | UIScreen.mainScreen | 1080 |
| osName | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10.2 |

연결 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Reachability currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |

애플리케이션 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|---------------|-----------|-----------------|
| applicationId | 하드코드 | REF30 |

#### Fire OS {#fireos}

[Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html)를 실행하는 장치의 `X-Device-Info` 헤더를 빌드하려면 다음 문서를 참조할 수 있습니다.

* [Build](https://developer.android.com/reference/android/os/Build.html) 클래스에 대한 Android 개발자 설명서입니다.
* [Fire TV 장치 식별](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html)을 위한 Amazon 개발자 설명서입니다.

장치 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|---------------|-----------------------------|-----------------|
| 모델 | Build.MODEL | AFTM |
| 공급업체 | Build.BRAND | Amazon |
| 제조업체 | Build.MANUFACTURER | Amazon |
| 버전 | Build.DEVICE | 몬토야 |
| displayWidth | DisplayMetrics.widthPixels |                 |
| displayHeight | DisplayMetrics.heightPixels |                 |
| osName | 하드코드 | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1 |

연결 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

애플리케이션 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|---------------|-----------|-----------------|
| applicationId | 하드코드 | REF30 |

#### Roku 운영 체제 {#rokuos}

[Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md)를 실행하는 장치의 `X-Device-Info` 헤더를 만들려면 다음 문서를 참조할 수 있습니다.

* [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)에 대한 Roku 개발자 설명서.

장치 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|---------------|--------------------------------------------|-----------------|
| 모델 | 하드코드 | &quot;Roku&quot; |
| 공급업체 | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| 제조업체 | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| 버전 | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
| displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
| displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| osName | 하드코드 | &quot;Roku&quot; |
| osVersion | ifDeviceInfo.getVersion() |                 |

연결 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
| connectionSecure | 하드코드 | 연결이 유선인 경우 true |

애플리케이션 정보는 다음과 같은 방법으로 구성할 수 있습니다.

| 키 | Source | 값(예) |
|---------------|-----------|-----------------|
| applicationId | 하드코드 | REF30 |

### 기타 {#others}

설명서에서 다루지 않는 장치 플랫폼의 경우 클라이언트 정보(장치, 연결 및 애플리케이션)는 일반적으로 장치의 하드웨어 및 OS 설명서에 지정된 사용 가능한 하드웨어 및 운영 체제(OS) 속성에 연결되어야 합니다.
