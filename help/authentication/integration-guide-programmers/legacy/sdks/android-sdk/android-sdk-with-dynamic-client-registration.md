---
title: Android SDK(동적 클라이언트 등록 포함)
description: Android SDK(동적 클라이언트 등록 포함)
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1301'
ht-degree: 0%

---

# (기존) Android SDK(동적 클라이언트 등록 기능 포함) {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 소개 {#Intro}

세션 쿠키를 사용하지 않고 인증을 사용하도록 Android AccessEnabler SDK for Android이 수정되었습니다. 점점 더 많은 브라우저가 쿠키에 대한 액세스를 제한하고 있으므로 인증을 허용하는 다른 방법을 사용해야 합니다.

Android의 경우 Chrome 사용자 정의 탭을 사용하면 다른 애플리케이션의 쿠키에 대한 액세스가 제한됩니다.

>**Android SDK 3.0.0** 소개:

- 동적 클라이언트 등록은 서명된 요청자 ID 및 세션 쿠키 인증을 기반으로 현재 앱 등록 메커니즘을 대체합니다
- 인증 흐름에 대한 Chrome 사용자 정의 탭

>[!NOTE]
>
>Chrome 사용자 정의 탭이 없는 이전 Android 버전의 경우 이전 AccessEnabler SDK 버전과 유사한 WebView 인증을 사용합니다.


## 동적 클라이언트 등록 {#DCR}

Android SDK v3.0+는 [동적 클라이언트 등록 개요](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)에 정의된 대로 동적 클라이언트 등록 절차를 사용합니다.


## 기능 데모 {#Demo}

기능의 자세한 컨텍스트를 제공하고 TVE 대시보드를 사용하여 소프트웨어 구문을 관리하는 방법 및 Android SDK의 일부로 Adobe에서 제공하는 데모 응용 프로그램을 사용하여 생성된 구문을 테스트하는 방법에 대한 데모가 포함된 [이 웨비나](https://my.adobeconnect.com/pzkp8ujrigg1/)를 시청하십시오.

## API 변경 사항 {#API}


### Factory.getInstance

**설명:** Access Enabler 개체를 인스턴스화합니다. 애플리케이션 인스턴스당 하나의 Access Enabler 인스턴스가 있어야 합니다.

| API 호출: 생성자 |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        AccessEnablerException 발생 |


**가용성:** v3.0+

**매개 변수:**

- *appContext*: Android 응용 프로그램 컨텍스트
- softwareStatement: &quot;software\_statement&quot;가 strings.xml에 설정된 경우 TVE 대시보드 또는 *null*&#x200B;에서 얻은 값
- redirectUrl : 고유 url, TVE 대시보드에 명시적으로 추가된 도메인 중 하나 또는 &quot;redirect\_uri&quot;가 strings.xml에 설정된 경우 *null*

참고 : 잘못된 softwareStatement 또는 redirectUrl로 인해 응용 프로그램에서 AccessEnabler를 초기화하거나 Adobe Pass 인증 및 권한 부여에 대한 응용 프로그램을 등록하지 못합니다
</br>
참고 : strings.xml의 redirectUrl 매개 변수 또는 redirect\_uri는 애플리케이션의 TVE Dashboard에 추가된 도메인 값이어야 합니다(예: TVE Dashboard에 추가된 도메인 &#39;adobe.com&#39;의 경우 redirectUrl은 &#39;com.adobe&#39;이어야 함).


### setRequestor

**설명:** 채널의 ID를 설정합니다. Adobe Pass 인증 시스템에 대해 Adobe에 등록하면 각 채널에는 고유 ID가 지정됩니다. SSO 및 원격 토큰을 처리할 때 애플리케이션이 백그라운드에 있을 때 인증 상태가 변경될 수 있으며, 시스템 상태와 동기화하기 위해 애플리케이션이 전경으로 전환될 때 setRequestor를 다시 호출할 수 있습니다(SSO가 활성화된 경우 원격 토큰을 가져오고 그 사이에 로그아웃이 발생한 경우 로컬 토큰을 삭제).

서버 응답에는 채널 ID에 첨부된 일부 구성 정보와 함께 MVPD 목록이 포함됩니다. 서버 응답은 Access Enabler에서 내부적으로 사용됩니다. 작업의 상태(즉, SUCCESS/FAIL)만 setRequestorComplete() 콜백을 통해 응용 프로그램에 표시됩니다.

*url* 매개 변수를 사용하지 않으면 결과 네트워크 호출은 기본 서비스 공급자 URL(Adobe 릴리스/프로덕션 환경)을 대상으로 합니다.

*url* 매개 변수에 대한 값이 제공되면 결과 네트워크 호출은 *url* 매개 변수에 제공된 모든 URL을 대상으로 합니다. 모든 구성 요청은 별도의 스레드에서 동시에 트리거됩니다. MVPD 목록을 컴파일할 때 첫 번째 응답자가 우선합니다. 목록에 있는 각 MVPD에 대해 Access Enabler는 연관된 서비스 공급자의 URL을 기억합니다. 모든 후속 자격 요청은 구성 단계 동안 대상 MVPD과 쌍을 이룬 서비스 공급자와 연결된 URL로 전달됩니다.

| API 호출: 요청자 구성 |
| --- |
| ```public void setRequestor(String requestorId)``` |

**가용성:** v3.0+

| API 호출: 요청자 구성 |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**가용성:** v3.0+

**매개 변수:**

- *requestorID*: 채널과 연결된 고유 ID입니다. Adobe Pass 인증 서비스에 처음 등록할 때 Adobe에서 할당한 고유 ID를 사이트에 전달합니다.
- *url*: 선택적 매개 변수. 기본적으로 Adobe 서비스 공급자는 [http://sp.auth.adobe.com/](http://sp.auth.adobe.com/)에 사용됩니다. 이 배열을 사용하면 Adobe에서 제공하는 인증 및 권한 부여 서비스에 대한 끝점을 지정할 수 있습니다(디버깅 목적으로 다른 인스턴스를 사용할 수 있음). 이 옵션을 사용하여 여러 Adobe Pass 인증 서비스 공급자 인스턴스를 지정할 수 있습니다. 이렇게 하면 MVPD 목록이 모든 서비스 공급자의 끝점으로 구성됩니다. 각 MVPD은 가장 빠른 서비스 공급자, 즉 먼저 응답하고 해당 MVPD을 지원하는 공급자와 연결됩니다.

사용되지 않음:

- *signedRequestorID*: 개인 키로 디지털 서명된 요청자 ID의 사본입니다. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**트리거된 콜백:** `setRequestorComplete()`

### 로그아웃

**설명:** 로그아웃 흐름을 시작하려면 이 메서드를 사용하십시오. 로그아웃은 사용자가 Adobe Pass 인증 서버와 MVPD 서버에서 모두 로그아웃해야 하므로 일련의 HTTP 리디렉션 작업의 결과입니다. 따라서 이 플로우는 로그아웃을 실행할 ChromeCustomTab 창을 엽니다.

| API 호출: 로그아웃 흐름 시작 |
| --- |
| 공개 무효 로그아웃() |

**가용성:** v3.0+

**매개 변수:** 없음

**트리거된 콜백:** `setAuthenticationStatus()`
</br></br>

## 프로그래머 구현 흐름 {#Progr}

### **1. 응용 프로그램 등록**

a. Adobe Pass( TVE Dashboard)에서 software\_statement 및 redirect\_uri 가져오기

b. 이러한 값을 Adobe Pass SDK에 전달하는 방법에는 두 가지가 있습니다.

strings.xml에 를 추가합니다.

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

AccessEnabler.getInstance(appContext,softwareStatement, 호출
redirectUrl)


### &#x200B;2. 애플리케이션 구성

a. setRequestor(requestor\_id)

SDK은 다음 작업을 수행합니다.

- 응용 프로그램 등록: **software\_statement**&#x200B;을(를) 사용하면 SDK에서 **client\_id, client\_secret, client\_id\_issued\_at, redirect\_uris, grant\_types**&#x200B;을(를) 가져옵니다. 이 정보는 애플리케이션의 내부 저장소에 저장됩니다.

- client\_id, client\_secret 및 grant\_type=&quot;client\_credentials&quot; 를 사용하여 **access\_token**&#x200B;을 가져옵니다. 이 액세스\_token은 SDK에서 Adobe Pass 서버로 호출하는 각 호출에 사용됩니다.

**토큰 오류 응답:**

| 오류 응답 | | |
| --- | --- | --- |
| HTTP 400(잘못된 요청) | {&quot;error&quot;: &quot;invalid\_request&quot;} | 요청에 필수 매개 변수가 없거나, 지원되지 않는 매개 변수 값(권한 부여 유형 제외)이 포함되어 있거나, 매개 변수를 반복하거나, 여러 자격 증명을 포함하거나, 클라이언트 인증을 위해 둘 이상의 메커니즘을 활용하거나, 형식이 잘못되었습니다. |
| HTTP 400(잘못된 요청) | {&quot;error&quot;: &quot;invalid\_client&quot;} | 클라이언트를 알 수 없으므로 클라이언트 인증에 실패했습니다. SDK은 인증 서버에 다시 등록해야 합니다. |
| HTTP 400(잘못된 요청) | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | 인증된 클라이언트는 이 권한 부여 유형을 사용할 수 있는 권한이 없습니다. |

- MVPD에 수동 인증이 필요한 경우 해당 MVPD으로 수동 실행을 위한 Chrome 사용자 정의 탭이 열리고 완료되면 닫힙니다

b. checkAuthentication()

- true : 인증으로 이동
- false : MVPD 선택으로 이동합니다.

c. getAuthentication : SDK은 호출 매개 변수에 **access_token**&#x200B;을 포함합니다.

- mvpd 기억됨 : setSelectedProvider(mvpd_id)로 이동
- mvpd 선택 안 됨 : displayProviderDialog
- mvpd 선택됨 : setSelectedProvider(mvpd_id)로 이동

d. setSelectedProvider

- mvpd\_id 인증 url이 ChromeCustomTab에 로드되었습니다.
- 로그인 성공 : delegate.setAuthenticationStatus ( SUCCESS )
- 로그인 취소됨 : MVPD 선택 재설정
- 인증이 완료되면 캡처하기 위해 URL 체계가 &quot;adobepass://redirect_uri&quot;로 설정됩니다.

e. get/checkAuthorization : SDK은 Authorization: Bearer **access_token**(으)로 헤더에 **access_token**&#x200B;을 포함합니다.

- 승인이 성공하면 를 얻기 위해 호출됩니다.
미디어 토큰

f. 로그아웃 :

- SDK은 현재 요청자에 대한 유효한 토큰을 삭제합니다(SSO를 통하지 않고 다른 애플리케이션에서 획득한 인증은 유효한 상태로 유지됨).
- SDK은 Chrome 사용자 지정 탭을 열어 mvpd_id 로그아웃 끝점에 도달합니다. 완료되면 Chrome 사용자 정의 탭이 닫힙니다.
- 로그아웃이 완료되는 순간을 캡처하기 위해 URL 체계가 &quot;adobepass://logout&quot;로 설정됩니다.
- 로그아웃하면 sendTrackingData(new Event(EVENT_LOGOUT,USER_NOT_AUTHENTICATED_ERROR) 및 콜백: setAuthenticationStatus(0,&quot;Logout&quot;)가 트리거됩니다.

**참고:** 호출마다 **access_token,**&#x200B;이 필요하므로 가능한 아래 오류 코드는 SDK에서 처리됩니다.


| 오류 응답 | | |
| --- | ---|--- |
| invalid_request | 400 | 요청 형식이 잘못되었습니다. SDK에서 서버 호출 수행을 중지해야 합니다. |
| invalid_client | 403 | 클라이언트 ID는 더 이상 요청을 수행할 수 없습니다. SDK는 클라이언트 등록을 다시 수행해야 합니다. |
| access_denied | 401 | 액세스\_토큰이 잘못되었습니다. SDK는 새 access_token을 요청해야 합니다. |
