---
title: iOS/tvOS API 참조
description: iOS/tvOS API 참조
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '6942'
ht-degree: 0%

---

# (기존) iOS/tvOS SDK API 참조 {#iostvos-sdk-api-reference}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 소개 {#intro}

이 페이지에서는 Adobe Pass 인증용 iOS/tvOS 기본 클라이언트가 노출하는 메소드 및 콜백 함수에 대해 설명합니다. 여기에 설명된 메서드와 콜백 함수는 `AccessEnabler.h` 및 `EntitlementDelegate.h` 헤더 파일에 정의되어 있습니다. 여기에서 찾을 수 있는 내용은 iOS AccessEnabler SDK입니다. `[SDK directory]/AccessEnabler/headers/api/`


관련 설명서:

* Adobe Pass 구현 방법에 대한 단계별 설명
이 API를 사용한 인증 권한 흐름은 [iOS 통합 Cookbook](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)을 참조하십시오.
* 최신 iOS AccessEnabler SDK에 대해서는 [iOS Native Access Enabler 라이브러리](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)를 참조하십시오.

>[!NOTE]
>
>Adobe은 Adobe Pass 인증 *공개* API만 사용할 것을 권장합니다.
>
>* 공개 API는 지원되는 모든 클라이언트 유형에서 사용할 수 있으며 완전히 테스트되었습니다. 모든 공개 기능의 경우 각 클라이언트 유형에 연결된 메서드의 해당 버전이 있는지 확인합니다.
>* 공개 API는 이전 버전과의 호환성을 지원하고 파트너 통합이 중단되지 않도록 최대한 안정적이어야 합니다. 그러나 비공개 API의 경우, 향후 언제든지 서명을 변경할 수 있는 권한이 있습니다. 현재 공개 Adobe Pass 인증 API 호출의 조합을 통해 지원되지 않는 특정 흐름이 발생하는 경우 가장 좋은 접근 방법은 알려 주는 것입니다. 귀사의 요구를 고려하여 공개 API를 수정하고 안정적인 솔루션을 제공할 수 있습니다.

</br>

## API 참조 {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - AccessEnabler 개체를 인스턴스화합니다.

* **[사용되지 않음]** [init](#init) - AccessEnabler 개체를 인스턴스화합니다.

* [`setOptions:options:`](#setOptions) - 프로필 또는 visitorID와 같은 글로벌 SDK 옵션을 구성합니다.

* [`setRequestor:`](#setReqV3) [`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - 프로그래머의 ID를 설정합니다.

* **[사용되지 않음]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - 프로그래머의 ID를 설정합니다.

* **[사용되지 않음]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos), [`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos)-프로그래머의 ID를 설정합니다.

* [`setRequestorComplete:`](#setReqComplete) - 구성 단계가 완료되었음을 응용 프로그램에 알립니다.

* [`checkAuthentication`](#checkAuthN) - 현재 사용자의 인증 상태를 확인합니다.

* [`getAuthentication`](#getAuthN), [`getAuthentication:withData:`](#getAuthN) - 전체 인증 워크플로를 시작합니다.

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN) [andFilter](#getAuthN_filter) - 전체 인증 워크플로를 시작합니다.

* [`displayProviderDialog:`](#dispProvDialog) - 사용자가 MVPD을 선택할 수 있는 적절한 UI 요소를 인스턴스화하도록 애플리케이션에 알립니다.

* [`setSelectedProvider:`](#setSelProv) - 사용자의 MVPD 선택을 AccessEnabler에 알립니다.

* [`navigateToUrl:`](#nav2url) - 사용자에게 MVPD 로그인 페이지를 표시해야 함을 애플리케이션에 알립니다.

* [`navigateToUrl:useSVC:`](#nav2urlSVC) - SFSafariViewController를 사용하여 사용자에게 MVPD 로그인 페이지를 표시해야 함을 애플리케이션에 알립니다.

* [`handleExternalURL:url`](#handleExternalURL) - 인증/로그아웃 흐름을 완료합니다.

* **[사용되지 않음]** [`getAuthenticationToken`](#getAuthNToken) - 백 엔드 서버에서 인증 토큰을 요청합니다.

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) - 응용 프로그램에 인증 흐름의 상태를 알립니다.

* [`checkPreauthorizedResources:`](#checkPreauth) - 사용자에게 특정 보호된 리소스를 볼 수 있는 권한이 이미 있는지 여부를 결정합니다.

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) - 사용자에게 특정 보호된 리소스를 볼 수 있는 권한이 이미 있는지 여부를 결정합니다.

* [`preauthorizedResources:`](#preauthResources) - 사용자가 이미 볼 수 있는 권한이 있는 리소스 목록을 제공합니다.

* [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ) - 현재 사용자의 인증 상태를 확인합니다.

* [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ) - 인증 흐름을 시작합니다.

* [`setToken:forResource:`](#setToken) - 권한 부여 흐름이 성공적으로 완료되었음을 응용 프로그램에 알립니다.

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) - 권한 부여 흐름이 실패했음을 응용 프로그램에 알립니다.

* [`logout`](#logout) - 로그아웃 흐름을 시작합니다.

* [`getSelectedProvider`](#getSelProv) - 현재 선택한 공급자를 결정합니다.

* [`selectedProvider:`](#selProv) - 현재 선택한 MVPD에 대한 정보를 애플리케이션에 전달합니다.

* [`getMetadata:`](#getMeta) - AccessEnabler 라이브러리에서 메타데이터로 노출된 정보를 검색합니다.

* [`presentTvProviderDialog:`](#presentTvDialog) - 응용 프로그램에 Apple SSO 대화 상자를 표시하도록 알립니다.

* [`dismissTvProviderDialog:`](#dismissTvDialog) - 응용 프로그램에 Apple SSO 대화 상자를 숨기도록 알립니다.

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - [`getMetadata:`](#getMeta) 호출에서 요청한 메타데이터를 전달합니다.

* [`sendTrackingData:forEventType:`](#sendTracking) - 추적 데이터 정보를 제공합니다.

* [`MVPD`](#mvpd) - MVPD 클래스입니다. [MVPD에 대한 정보를 포함합니다]

### init:softwareStatement {#initWithSoftwareStatement}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** AccessEnabler 개체를 인스턴스화합니다. 애플리케이션 인스턴스당 하나의 AccessEnabler 인스턴스가 있어야 합니다.

| **API 호출: iOS AccessEnabler 생성자** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**가용성:** v3.0+

**매개 변수:**

* **softwareStatement:** Adobe 시스템에서 응용 프로그램을 식별하는 문자열입니다. Software Statement를 얻는 방법을 확인하십시오.

[맨 위로...](#apis)



### init - [사용되지 않음]{#init}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** AccessEnabler 개체를 인스턴스화합니다. 애플리케이션 인스턴스당 하나의 AccessEnabler 인스턴스가 있어야 합니다.

| API 호출: iOS AccessEnabler 생성자 |
| --- |
| ```- (id) init;``` |

**가용성:** v1.0+ **종료 시간:** v3.0

**매개 변수:** 없음

[맨 위로...](#apis)

</br>

### setOptions:options {#setOptions}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:**&#x200B;에서 전역 SDK 옵션을 구성합니다. NSDictionary를 인수로 수락합니다. 사전의 값은 SDK에서 수행하는 모든 네트워크 호출과 함께 서버로 전달됩니다.

**참고:** 값이 현재 흐름(인증/권한 부여)에 관계없이 서버에 전달됩니다. 값을 변경하려면 언제든지 이 메서드를 호출할 수 있습니다.

| API 호출: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**가용성:** v2.3.0+

**매개 변수:**

* *options*: 전역 SDK 옵션이 포함된 NSDictionary입니다. 현재 다음 옵션을 사용할 수 있습니다.
   * **applicationProfile** - 이 값을 기반으로 서버 구성을 만드는 데 사용할 수 있습니다.
   * **visitorID** - Experience Cloud ID 서비스입니다. 이 값은 나중에 고급 분석 보고서에 사용할 수 있습니다.
   * **handleSVC** - 프로그래머가 SFSafariViewControllers를 처리할지 여부를 나타내는 부울. 자세한 내용은 iOS SDK 3.2 이상에서 [SFSafaariViewController 지원](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)을 참조하십시오.
      * **false,**(으)로 설정하면 SDK에서 최종 사용자에게 SFSafariViewController가 자동으로 표시됩니다. SDK은 MVPD 로그인 페이지 URL로 이동합니다.
      * **true,**(으)로 설정하면 SDK에서 최종 사용자에게 SFSafariViewController를 자동으로 **NOT**&#x200B;합니다. SDK이 **navigate(toUrl:{url}, useSVC:YES)**&#x200B;을(를) 추가로 트리거합니다.
* **device\_info** - [클라이언트 정보 전달](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)에 설명된 클라이언트 정보입니다.

[맨 위로...](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 프로그래머의 ID를 설정합니다. 각 프로그래머는 Adobe Pass 인증 시스템에 대한 Adobe 등록 시 고유 ID가 지정됩니다. SSO 및 원격 토큰을 처리할 때, 애플리케이션이 백그라운드에 있을 때 인증 상태가 변경될 수 있고, 시스템 상태와 동기화하기 위해 애플리케이션이 전경으로 전환될 때 setRequestor를 다시 호출할 수 있습니다(SSO가 활성화된 경우 원격 토큰을 가져오고, 그 사이에 로그아웃이 발생한 경우 로컬 토큰을 삭제).

서버 응답에는 프로그래머의 ID에 첨부된 일부 구성 정보와 함께 MVPD 목록이 포함됩니다. 서버 응답은 AccessEnabler 코드에서 내부적으로 사용됩니다. 작업의 상태(즉, SUCCESS/FAIL)만 `setRequestorComplete:` 콜백을 통해 응용 프로그램에 표시됩니다.

`urls` 매개 변수를 사용하지 않으면 결과 네트워크 호출은 기본 서비스 공급자 URL(Adobe RELEASE/프로덕션 환경)을 대상으로 합니다.


`urls` 매개 변수에 대한 값이 제공되면 결과 네트워크 호출은 `urls` 매개 변수에 제공된 모든 URL을 대상으로 합니다. 모든 구성 요청은 별도의 스레드에서 동시에 트리거됩니다. MVPD 목록을 컴파일할 때 첫 번째 응답자가 우선합니다. 목록에 있는 각 MVPD에 대해 AccessEnabler는 연관된 서비스 공급업체의 URL을 기억합니다. 모든 후속 자격 요청은 구성 단계 동안 대상 MVPD과 쌍을 이룬 서비스 공급자와 연결된 URL로 전달됩니다.

| API 호출: 요청자 구성 |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**가용성:** v3.0+

| API 호출: 요청자 구성 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**가용성:** v3.0+

**매개 변수:**

* *requestorID*: 프로그래머와 연결된 고유 ID입니다. Adobe Pass 인증 서비스에 처음 등록할 때 Adobe이 할당한 고유 ID를 사이트에 전달합니다.
* *url*: 선택적 매개 변수. 기본적으로 Adobe 서비스 공급자가 사용됩니다(http://sp.auth.adobe.com/). 이 배열을 사용하면 Adobe에서 제공하는 인증 및 권한 부여 서비스에 대한 끝점을 지정할 수 있습니다(디버깅 목적으로 다른 인스턴스를 사용할 수 있음). 이 옵션을 사용하여 여러 Adobe Pass 인증 서비스 공급자 인스턴스를 지정할 수 있습니다. 이렇게 하면 MVPD 목록이 모든 서비스 공급자의 끝점으로 구성됩니다. 각 MVPD은 가장 빠른 서비스 공급자, 즉 먼저 응답하고 해당 MVPD을 지원하는 공급자와 연결됩니다.

>[!NOTE]
>
>`serviceProviders` 매개 변수 없이 호출하면 라이브러리는 기본 서비스 공급자로부터 구성을 검색합니다(즉, 프로덕션 프로필의 경우 `https://sp.auth.adobe.com`, 스테이징 프로필의 경우 `https://sp.auth-staging.adobe.com`). `serviceProviders` 매개 변수가 제공된 경우 URL 배열이어야 합니다. 구성 정보는 지정된 모든 끝점에서 검색되고 병합됩니다. 서로 다른 서비스 공급자 응답에 중복 정보가 있는 경우 가장 빠르게 응답하는 서버(즉, 응답 시간이 가장 짧은 서버가 우선함)를 위해 충돌이 해결됩니다.

**트리거된 콜백:** [`setRequestorComplete:`](#setReqComplete)

[맨 위로...](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [사용되지 않음] {#setReq}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 프로그래머의 ID를 설정합니다. 각 프로그래머는 Adobe Pass 인증 시스템에 대한 Adobe 등록 시 고유 ID가 지정됩니다. SSO 및 원격 토큰을 처리할 때 애플리케이션이 백그라운드에 있을 때 인증 상태가 변경될 수 있으며, 시스템 상태와 동기화하기 위해 애플리케이션이 전경으로 전환될 때 setRequestor를 다시 호출할 수 있습니다(SSO가 활성화된 경우 원격 토큰을 가져오고 그 사이에 로그아웃이 발생한 경우 로컬 토큰을 삭제).

서버 응답에는 프로그래머의 ID에 첨부된 일부 구성 정보와 함께 MVPD 목록이 포함됩니다. 서버 응답은 AccessEnabler 코드에서 내부적으로 사용됩니다. 작업의 상태(즉, SUCCESS/FAIL)만 `setRequestorComplete:` 콜백을 통해 응용 프로그램에 표시됩니다.

`urls` 매개 변수를 사용하지 않으면 결과 네트워크 호출은 기본 서비스 공급자 URL(Adobe RELEASE/프로덕션 환경)을 대상으로 합니다.

`urls` 매개 변수에 대한 값이 제공되면 결과 네트워크 호출은 `urls` 매개 변수에 제공된 모든 URL을 대상으로 합니다. 모든 구성 요청은 별도의 스레드에서 동시에 트리거됩니다. MVPD 목록을 컴파일할 때 첫 번째 응답자가 우선합니다. 목록에 있는 각 MVPD에 대해 AccessEnabler는 연관된 서비스 공급업체의 URL을 기억합니다. 모든 후속 자격 요청은 구성 단계 동안 대상 MVPD과 쌍을 이룬 서비스 공급자와 연결된 URL로 전달됩니다.

| API 호출: 요청자 구성 |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**가용성:** v1.0+ **종료 시간:** v3.0

| API 호출: 요청자 구성 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**가용성:** v1.0+ **종료 시간:** v3.0

**매개 변수:**

* *requestorID*: 프로그래머와 연결된 고유 ID입니다. Adobe Pass 인증 서비스에 처음 등록할 때 Adobe이 할당한 고유 ID를 사이트에 전달합니다.
* *signedRequestorID*: **이 매개 변수는 iOS AccessEnabler 버전 1.2 이상에 있습니다.** 개인 키로 디지털 서명된 요청자 ID의 복사본입니다. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*: 선택적 매개 변수. 기본적으로 Adobe 서비스 공급자가 사용됩니다(http://sp.auth.adobe.com/). 이 배열을 사용하면 Adobe에서 제공하는 인증 및 권한 부여 서비스에 대한 끝점을 지정할 수 있습니다(디버깅 목적으로 다른 인스턴스를 사용할 수 있음). 이 옵션을 사용하여 여러 Adobe Pass 인증 서비스 공급자 인스턴스를 지정할 수 있습니다. 이렇게 하면 MVPD 목록이 모든 서비스 공급자의 끝점으로 구성됩니다. 각 MVPD은 가장 빠른 서비스 공급자, 즉 먼저 응답하고 해당 MVPD을 지원하는 공급자와 연결됩니다.

**참고:** `serviceProviders` 매개 변수 없이 호출되면 라이브러리는 기본 서비스 공급자로부터 구성을 검색합니다(프로덕션 프로필의 경우 `https://sp.auth.adobe.com`, 스테이징 프로필의 경우 `https://sp.auth-staging.adobe.com`). `serviceProviders` 매개 변수가 제공된 경우 URL 배열이어야 합니다. 구성 정보는 지정된 모든 끝점에서 검색되고 병합됩니다. 서로 다른 서비스 공급자 응답에 중복 정보가 있는 경우, 가장 빠르게 응답하는 서버(즉, 응답 시간이 가장 짧은 서버가 우선함)를 위해 충돌이 해결됩니다.

**트리거된 콜백:** [`setRequestorComplete:`](#setReqComplete)


[맨 위로...](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [사용되지 않음] {#setReq_tvos}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 프로그래머의 ID를 설정합니다. 각 프로그래머는 Adobe Pass 인증 시스템에 대한 Adobe 등록 시 고유 ID가 지정됩니다. 이 설정은 응용 프로그램 수명 주기 동안 한 번만 수행해야 합니다.

서버 응답에는 프로그래머의 ID에 첨부된 일부 구성 정보와 함께 MVPD 목록이 포함됩니다. 서버 응답은 AccessEnabler 코드에서 내부적으로 사용됩니다. 작업의 상태(즉, SUCCESS/FAIL)만 `setRequestorComplete:` 콜백을 통해 응용 프로그램에 표시됩니다.

`urls` 매개 변수를 사용하지 않으면 결과 네트워크 호출은 기본 서비스 공급자 URL(Adobe RELEASE/프로덕션 환경)을 대상으로 합니다.

`urls` 매개 변수에 대한 값이 제공되면 결과 네트워크 호출은 `urls` 매개 변수에 제공된 모든 URL을 대상으로 합니다. 모든 구성 요청은 별도의 스레드에서 동시에 트리거됩니다. MVPD 목록을 컴파일할 때 첫 번째 응답자가 우선합니다. 목록에 있는 각 MVPD에 대해 AccessEnabler는 연관된 서비스 공급업체의 URL을 기억합니다. 모든 후속 자격 요청은 구성 단계 동안 대상 MVPD과 쌍을 이룬 서비스 공급자와 연결된 URL로 전달됩니다.



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 요청자 구성</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID
               secret:(NSString *)secret
            publicKey:(NSString *)publicKey;</code></pre></td>
</tr>
</tbody>
</table>


**가용성:** v2.0+ **종료 시간:** v3.0

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 요청자 구성</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID 
     serviceProviders:(NSArray *)urls</code></pre>
<p><code class="sourceCode objectivec">               secret:(NSString *)secret</code></p>
<p><code class="sourceCode objectivec">            publicKey:(NSString *)publicKey;</code></p></td>
</tr>
</tbody>
</table>

**가용성:** v2.0+ **종료 시간:** v3.0

**매개 변수:**

* *requestorID*: 프로그래머와 연결된 고유 ID입니다. 처음 시작할 때 Adobe에서 할당한 고유 ID를 사이트에 전달합니다   Adobe Pass 인증 서비스에 등록되었습니다.
* *signedRequestorID*: **이 매개 변수는 iOS AccessEnabler에 있습니다.   버전 1.2 이상** 개인 키로 디지털 서명된 요청자 ID의 복사본입니다. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*: 선택적 매개 변수, 기본적으로 Adobe 서비스 공급자   가 사용됩니다(http://sp.auth.adobe.com/). 이 배열을 사용하면 Adobe에서 제공하는 인증 및 권한 부여 서비스에 대한 끝점을 지정할 수 있습니다(디버깅 목적으로 다른 인스턴스를 사용할 수 있음). 이 옵션을 사용하여 여러 Adobe Pass 인증 서비스 공급자 인스턴스를 지정할 수 있습니다. 이렇게 하면 MVPD 목록이 모든 서비스 공급자의 끝점으로 구성됩니다. 각 MVPD은 가장 빠른 서비스 공급자, 즉 먼저 응답하고 해당 MVPD을 지원하는 공급자와 연결됩니다.
* secret and publicKey: 두 번째 화면 호출에 서명하는 데 사용되는 비밀 및 공개 키입니다. 자세한 내용은 [클라이언트 없는 설명서](#create_dev)를 참조하세요.

`serviceProviders` 매개 변수 없이 호출하면 라이브러리는 기본 서비스 공급자(예: 프로덕션 프로필의 경우 `https://sp.auth.adobe.com`, 스테이징 프로필의 경우 https://sp.auth-staging.adobe.com)에서 구성을 검색합니다. `serviceProviders` 매개 변수가 제공된 경우 URL 배열이어야 합니다. 구성 정보는 지정된 모든 끝점에서 검색되고 병합됩니다. 서로 다른 서비스 공급자 응답에 중복 정보가 있는 경우, 가장 빠르게 응답하는 서버(즉, 응답 시간이 가장 짧은 서버가 우선함)를 위해 충돌이 해결됩니다.

**트리거된 콜백:** [`setRequestorComplete:`](#setReqComplete)

[맨 위로...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

응용 프로그램에 구성 단계가 완료되었음을 알리는 AccessEnabler에 의해 트리거된 **설명** 콜백입니다. 앱에서 권한 부여 요청 발급을 시작할 수 있다는 신호입니다. 구성 단계가 완료될 때까지 애플리케이션에서 권한 부여 요청을 실행할 수 없습니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>콜백: 요청자 구성 완료</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**가용성:** v1.0+

**매개 변수**:

* *상태*: 다음 값 중 하나를 사용할 수 있습니다.
   * `ACCESS_ENABLER_STATUS_SUCCESS` - 구성 단계가 완료되었습니다.
   * `ACCESS_ENABLER_STATUS_ERROR` - 구성 단계 실패

**트리거 기준:**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[맨 위로...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 현재 사용자의 인증 상태를 확인합니다.
이렇게 하려면 로컬에서 유효한 인증 토큰을 검색하십시오
토큰 저장 공간. 이 메서드는 네트워크 호출을 수행하지 않으며 기본 스레드에서 호출하는 것이 좋습니다.
애플리케이션에서 사용자의 인증 상태를 쿼리하고
그에 따라 UI를 업데이트합니다(즉, 로그인/로그아웃 UI 업데이트). 다음
인증 상태는 를 통해 애플리케이션에 전달됩니다.
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus) 콜백입니다.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 상태 확인</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

**매개 변수:** 없음

**트리거된 콜백:**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[맨 위로...](#apis)

</br>

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 전체 인증 워크플로를 시작합니다. 인증 상태를 확인하는 것으로 시작됩니다. 아직 인증되지 않은 경우 인증 흐름 state-machine이 시작됩니다.

* 마지막 인증 시도가 성공하면 MVPD   선택 단계를 건너뛰고   [`navigateToUrl:`](#nav2url) 콜백이 트리거됩니다. 다음   응용 프로그램에서는 이 콜백을 사용하여 사용자에게 MVPD의 로그인 페이지를 제공하는 WebView 컨트롤을 인스턴스화합니다. **[참고: Access Enabler 1.5부터 SDK의 제한으로 인해 이 기능을 사용할 수 없습니다].**
* 마지막 인증 시도에 실패했거나 사용자가 명시적으로 로그아웃한 경우 [`displayProviderDialog:`](#dispProvDialog) 콜백은   트리거됨. 애플리케이션이 이 콜백을 사용하여 MVPD 선택 UI를 표시합니다. 또한 앱에서 [`setSelectedProvider:`](#setSelProv) 메서드를 통해 사용자의 MVPD 선택에 대해 AccessEnabler 라이브러리에 알려 인증 흐름을 다시 시작해야 합니다.

MVPD 로그인 페이지에서 사용자의 자격 증명이 확인됨에 따라 애플리케이션이 MVPD 로그인 페이지에서 사용자가 인증하는 동안 발생하는 여러 리디렉션 작업을 모니터링해야 합니다. 올바른 자격 증명을 입력하면 WebView 컨트롤이 `ADOBEPASS_REDIRECT_URL` 상수로 정의된 사용자 지정 URL로 리디렉션됩니다. 이 URL은 WebView에서 로드하기 위한 것이 아닙니다. 애플리케이션은 이 URL을 가로채고 이 이벤트를 로그인 단계가 완료되었음을 나타내는 신호로 해석해야 합니다. 그런 다음 AccessEnabler에 제어 권한을 넘겨 인증 흐름을 완료합니다([handleExternalURL](#handleExternalURL) 메서드 호출).

마지막으로 인증 상태는 [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) 콜백을 통해 응용 프로그램에 전달됩니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 흐름을 시작합니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 흐름을 시작합니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**가용성:** v1.9+

**매개 변수:**

* *forceAuthn*: 사용자가 이미 인증되었는지 여부에 관계없이 인증 흐름을 시작해야 하는지 여부를 지정하는 플래그입니다.
* *data*: Pay-TV Pass 서비스로 보낼 키-값 쌍으로 구성된 사전입니다. Adobe은 이 데이터를 사용하여 SDK을 변경하지 않고 향후 기능을 활성화할 수 있습니다.

**트리거된 콜백:** `setAuthenticationStatus:errorCode:`, [`displayProviderDialog:`](#dispProvDialog), `sendTrackingData:forEventType:`


[맨 위로...](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 전체 인증 워크플로를 시작합니다. 인증 상태를 확인하는 것으로 시작됩니다. 아직 인증되지 않은 경우 인증 흐름 state-machine이 시작됩니다.

* 현재 요청자가 SSO를 지원하는 MVPD이 하나 이상 있는 경우 [presentTvProviderDialog()](#presentTvDialog)이(가) 호출됩니다. SSO를 지원하는 MVPD이 없으면 클래식 인증 흐름이 시작되고 필터 매개 변수가 무시됩니다.
* 사용자가 완료되면 Apple SSO 흐름 [`dismissTvProviderDialog()`](#dismissTvDialog)이(가) 트리거되고 인증 프로세스가 완료됩니다.

마지막으로 인증 상태는 [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) 콜백을 통해 응용 프로그램에 전달됩니다.

**가용성:** v2.4+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 흐름을 시작합니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSDictionary *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 흐름을 시작합니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSDictionary *)filter;</code></pre>
<div>
 </div></td>
</tr>
</tbody>
</table>



**매개 변수:**

* *forceAuthn*: 사용자가 이미 인증되었는지 여부에 관계없이 인증 흐름을 시작해야 하는지 여부를 지정하는 플래그입니다.
* *data*: Pay-TV Pass 서비스로 보낼 키-값 쌍으로 구성된 사전입니다. Adobe은 이 데이터를 사용하여 SDK을 변경하지 않고 향후 기능을 활성화할 수 있습니다.
* 필터: MVPD SSO 대화 상자에 표시되어야 하는 두 개의 Apple ID 목록이 있는 사전입니다. SSO를 지원하지 않는 모든 MVPD은 무시되지만 순서는 준수됩니다. 사전에는 두 개의 키가 있어야 합니다.
   * TV\_PROVIDERS: 선택기에 표시되어야 하는 모든 MVPD가 있는 목록
   * FEATURED\_TV\_PROVIDERS: 선택기에 기능으로 표시되어야 하는 모든 MVPD가 포함된 목록입니다. TV\_PROVIDERS 목록에도 이 목록의 MVPD를 지정해야 합니다.

**가용성:** v2.0 - v2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 흐름을 시작합니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSArray *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 흐름을 시작합니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSArray *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**매개 변수:**

* *forceAuthn*: 사용자가 이미 인증되었는지 여부에 관계없이 인증 흐름을 시작해야 하는지 여부를 지정하는 플래그입니다.
* *data*: Pay-TV Pass 서비스로 보낼 키-값 쌍으로 구성된 사전입니다. Adobe은 이 데이터를 사용하여 SDK을 변경하지 않고 향후 기능을 활성화할 수 있습니다.
* 필터: Apple SSO 대화 상자에 표시되는 MVPD ID 목록입니다. SSO를 지원하지 않는 모든 MVPD은 무시되지만 순서는 준수됩니다.

**트리거된 콜백:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[맨 위로...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 사용자가 원하는 MVPD을 선택할 수 있도록 적절한 UI 요소를 인스턴스화해야 함을 응용 프로그램에 알리기 위해 AccessEnabler에 의해 트리거된 콜백입니다. 콜백은 MVPD 개체 목록에 선택 UI 패널을 올바르게 빌드하는 데 도움이 되는 추가 정보(예: MVPD 로고를 가리키는 URL, 친숙한 표시 이름 등)를 제공합니다.

사용자가 원하는 MVPD을 선택하면 `setSelectedProvider:`을(를) 호출하여 사용자 선택에 해당하는 MVPD의 ID를 전달함으로써 인증 흐름을 다시 시작하는 데 상위 계층 응용 프로그램이 필요합니다.

**인증 흐름을 중단하는 중** - 사용자가 &quot;뒤로&quot; 단추를 누를 수 있는 지점이며, 이는 인증 흐름을 중단하는 것과 같습니다. 이 시나리오에서는 응용 프로그램이 [setSelectedProvider:](#setSelProv) 메서드를 호출하고 null을 매개 변수로 전달하여 AccessEnabler에 인증 상태 컴퓨터를 다시 설정할 수 있는 기회를 제공해야 합니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>콜백: MVPD 선택 UI 표시</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**가용성:** v1.0+

**매개 변수**:

* *mvpds*: 응용 프로그램에서 MVPD 선택 UI 요소를 만드는 데 사용할 수 있는 MVPD 관련 정보가 들어 있는 MVPD 개체 목록입니다.

**트리거 대상:** `getAuthentication`, [`getAuthentication:withData:`](#getAuthN),`getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)


[맨 위로...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 이 메서드는 사용자의 MVPD 선택을 Access Enabler에 알리기 위해 응용 프로그램에서 호출됩니다. 애플리케이션은 이 방법을 사용하여 인증에 사용되는 서비스 제공자를 선택 또는 변경할 수 있다.

선택한 MVPD이 TempPass MVPD인 경우 나중에 getAuthentication()을 호출할 필요 없이 해당 MVPD을 사용하여 자동으로 인증됩니다.

getAuthentication() 메서드에 추가 매개 변수가 제공되는 프로모션 임시 패스에는 이 작업이 가능하지 않습니다.

*null*&#x200B;을(를) 매개 변수로 전달할 때 Access Enabler는 사용자가 인증 흐름을 취소했다고 가정하고(즉, &quot;뒤로&quot; 단추를 누름) 인증 상태 컴퓨터를 다시 설정하고 `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` 오류 코드로 [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) 콜백을 호출하여 응답합니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 현재 선택한 공급자를 설정합니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

**매개 변수:** 없음

**트리거된 콜백:** `setAuthenticationStatus:errorCode:`,`sendTrackingData:forEventType:`, [`navigateToUrl:`](#nav2url)

[맨 위로...](#apis)

</br>

#### navigateToUrl: {#nav2url}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명:** 응용 프로그램에 UIWebView/WKWebView 컨트롤러를 인스턴스화하고 콜백의 **`url`** 매개 변수에 제공된 URL을 로드하도록 요청하기 위해 AccessEnabler에 의해 트리거된 콜백입니다. 콜백은 인증 끝점의 URL 또는 로그아웃 끝점의 URL을 나타내는 **`url`** 매개 변수를 전달합니다.

UIWebView/WKWebView` `컨트롤러가 여러 리디렉션을 거치면서 응용 프로그램에서 컨트롤러의 활동을 모니터링하고 `ADOBEPASS_REDIRECT_URL `상수로 정의된 특정 사용자 지정 URL을 로드하는 순간을 감지해야 합니다(예: `adobepass://ios.app`). 이 특정 사용자 지정 URL은 실제로 유효하지 않으며 제어자가 실제로 로드하기 위한 것이 아닙니다. 인증 또는 로그아웃 흐름이 완료되었으며 컨트롤러를 닫는 것이 안전하다는 신호로 응용 프로그램에서 해석해야 합니다. 컨트롤러가 이 특정 사용자 지정 URL을 로드하면 응용 프로그램에서 UIWebView/WKWebView를 닫고 AccessEnabler의 `handleExternalURL:url `API 메서드를 호출해야 합니다.

**참고:** 인증 흐름의 경우 사용자가 &quot;뒤로&quot; 단추를 누를 수 있는 지점이며, 이는 인증 흐름을 중단하는 것과 같습니다. 이러한 경우 응용 프로그램에서 매개 변수로 **`nil`**&#x200B;을(를) 전달하고 AccessEnabler에 인증 상태 컴퓨터를 다시 설정할 수 있는 기회를 제공하는 [setSelectedProvider:](#setSelProv) 메서드를 호출해야 합니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>콜백: MVPD 로그인 페이지 표시</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) navigateToUrl:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

**매개 변수**:

* *url*: MVPD의 로그인 페이지를 가리키는 URL

**트리거:** [setSelectedProvider:](#setSelProv)



[맨 위로...](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명:** 이전에 응용 프로그램에서 [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions) 호출을 통해 수동 Safari View Controller(SVC) 처리를 사용하도록 설정한 경우 및 Safari View Controller(SVC)가 필요한 MVPD의 경우에만 `navigateToUrl:` 콜백 대신 AccessEnabler에서 트리거된 콜백입니다. 다른 모든 MVPD에 대해 `navigateToUrl:` 콜백이 호출됩니다. SVC(Safari View Controller)를 관리하는 방법에 대한 자세한 내용은 [iOS SDK 3.2 이상에서 SFSafariViewController 지원](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)을 참조하십시오.

`navigateToUrl:` 콜백과 마찬가지로 AccessEnabler가 `navigateToUrl:useSVC:`을(를) 트리거하여 응용 프로그램에 `SFSafariViewController` 컨트롤러를 인스턴스화하고 콜백의 **`url`** 매개 변수에 제공된 URL을 로드하도록 요청합니다. 콜백은 인증 끝점의 URL 또는 로그아웃 끝점의 URL을 나타내는 **`url`** 매개 변수와 응용 프로그램에서 `SFSafariViewController`을(를) 사용해야 함을 지정하는 **`useSVC`** 매개 변수를 전달합니다.

`SFSafariViewController` 컨트롤러가 여러 리디렉션을 거치는 동안 응용 프로그램은 컨트롤러의 활동을 모니터링하고 `application's custom scheme`에 의해 정의된 특정 사용자 지정 URL을 로드하는 순간을 감지해야 합니다(예: **&#x200B; `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`**. 이 특정 사용자 지정 URL은 실제로 유효하지 않으며 제어자가 실제로 로드하기 위한 것이 아닙니다. 인증 또는 로그아웃 흐름이 완료되었으며 컨트롤러를 닫는 것이 안전하다는 신호로 응용 프로그램에서 해석해야 합니다. 컨트롤러가 이 특정 사용자 지정 URL을 로드하면 응용 프로그램에서 `SFSafariViewController`을(를) 닫고 AccessEnabler의 `handleExternalURL:url `API 메서드를 호출해야 합니다.

**참고:** 인증 흐름의 경우 사용자가 &quot;뒤로&quot; 단추를 누를 수 있는 지점이며, 이는 인증 흐름을 중단하는 것과 같습니다. 이러한 경우 응용 프로그램에서 매개 변수로 **`nil`**&#x200B;을(를) 전달하고 AccessEnabler에 인증 상태 컴퓨터를 다시 설정할 수 있는 기회를 제공하는 [setSelectedProvider:](#setSelProv) 메서드를 호출해야 합니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>콜백: SFSafariViewController에서 MVPD 로그인 페이지 표시</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
&#x200B;- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:**&#x200B;v 3.2+

**매개 변수**:

* *url:* MVPD의 로그인 페이지를 가리키는 URL
* *useSVC:* SFSafariViewController에 URL을 로드할지 여부를 지정합니다.

[setSelectedProvider:](#setSelProv) 전에 **트리거:**&#x200B;[ setOptions:](#setOptions)

[맨 위로...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 응용 프로그램에서 이 메서드를 호출하여 인증 또는 로그아웃 흐름을 완료합니다. 이 메서드는 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러가 특정 사용자 지정 URL로 리디렉션되는 순간을 응용 프로그램에서 감지한 직후에 호출해야 합니다. 응용 프로그램에서 `SFSafariViewController `컨트롤러를 사용해야 하는 경우 특정 사용자 지정 URL은 `application's custom scheme`(예: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`)에 의해 정의되며, 그렇지 않으면 이 특정 사용자 지정 URL은 `ADOBEPASS_REDIRECT_URL `상수(예: `adobepass://ios.app`)에 의해 정의됩니다.

인증 흐름의 경우 AccessEnabler는 백엔드 서버에서 인증 토큰을 검색하여 토큰 저장소에 로컬로 저장하여 흐름을 완료합니다. AccessEnabler는 성공을 나타내는 상태 코드가 1인 `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> 콜백을 호출하여 인증 흐름이 완료되었음을 응용 프로그램에 알립니다. 이 단계를 실행하는 동안 오류가 발생하면 `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> 콜백이 인증 실패와 해당 오류 코드를 나타내는 상태 코드 0으로 트리거됩니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 또는 로그아웃 흐름 완료</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v3.0+

**매개 변수:**

* *url*: ` UIWebView/WKWebView or SFSafariViewController ` 컨트롤에서 가로챈 URL을 문자열로 표시합니다.


**트리거된 콜백:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[맨 위로...](#apis)

</br>

#### getAuthenticationToken - [사용되지 않음] {#getAuthNToken}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 백 엔드 서버에서 인증 토큰을 요청하여 인증 흐름을 완료합니다. 이 메서드는 MVPD 로그인 페이지를 호스팅하는 WebView 컨트롤이 `ADOBEPASS_REDIRECT_URL` 상수로 정의된 사용자 지정 URL로 리디렉션되는 이벤트에 대한 응답으로만 응용 프로그램에서 호출해야 합니다.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 토큰 검색</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+ **종료 시간:** v3.0

**매개 변수:** 없음

**트리거된 콜백:** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[맨 위로...](#apis)

&lt;/br

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

응용 프로그램에 인증 흐름의 상태를 알리는 AccessEnabler에 의해 트리거된 **설명** 콜백입니다. 사용자의 상호 작용으로 인해 또는 예기치 않은 다른 시나리오(예: 네트워크 연결 문제 등)로 인해 이 플로우가 실패할 수 있는 위치가 많습니다. 이 콜백은 인증 흐름의 성공/실패 상태를 응용 프로그램에 알리는 동시에 필요한 경우 실패 이유에 대한 추가 정보를 제공합니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>콜백: 인증 흐름 상태 보고</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setAuthenticationStatus:(int)status 
                       errorCode:(NSString *)code; </code></pre></td>
</tr>
</tbody>
</table>


**가용성:** v1.0+

**매개 변수**:

* *상태*: 다음 값 중 하나를 사용할 수 있습니다.
   * `ACCESS_ENABLER_STATUS_SUCCESS` - 인증 흐름이 완료되었습니다.
   * `ACCESS_ENABLER_STATUS_ERROR` - 인증 흐름 실패
* *code*: 실패 이유입니다. *status*&#x200B;이(가) `ACCESS_ENABLER_STATUS_SUCCESS`이면 *code*&#x200B;은(는) 빈 문자열입니다(즉, `USER_AUTHENTICATED` 상수로 정의됨). 실패할 경우 이 매개 변수는 다음 값 중 하나를 사용할 수 있습니다.
   * `USER_NOT_AUTHENTICATED_ERROR` - 사용자가 인증되지 않았습니다. 로컬 토큰 캐시에 올바른 인증 토큰이 없는 경우 [checkAuthentication:](#checkAuthN) 메서드 호출에 대한 응답입니다.
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler가       인증 상태-상위 계층 응용 프로그램 후 컴퓨터       인증 흐름을 중단하기 위해 *null*&#x200B;을(를) [`setSelectedProvider:`](#setSelProv)(으)로 전달했습니다.  사용자가 인증 흐름을 취소한 것 같습니다(즉, &quot;뒤로&quot; 단추 누름).
   * `GENERIC_AUTHENTICATION_ERROR` - 네트워크를 사용할 수 없거나 사용자가 인증 흐름을 명시적으로 취소하는 등의 이유로 인증 흐름이 실패했습니다.

**트리거 대상:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ)

[맨 위로...](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 응용 프로그램에서 이 메서드를 사용하여 사용자가 특정 보호된 리소스를 볼 수 있는 권한이 있는지 확인합니다. 이 메서드의 기본 목적은 UI **을(를) 장식하는 데 사용할 정보를 검색하는 것입니다(예: 잠금 및 잠금 해제 아이콘으로 액세스 상태를 표시).**

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 현재 선택한 공급자를 설정합니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.3+

**매개 변수:**

* 인증을 확인해야 하는 리소스의 *리소스:* 배열. 목록의 각 요소는 리소스 ID를 나타내는 문자열이어야 합니다. 다음     리소스 ID에는 호출의 리소스 ID와 동일한 제한이 적용됩니다. 즉, 프로그래머와 MVPD 또는 미디어 RSS 조각 간에 설정된 합의된 값이어야 합니다.

**트리거된 콜백:** [`preauthorizedResources:`](#preauthResources)

[맨 위로...](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 응용 프로그램에서 이 메서드를 사용하여 사용자가 특정 보호된 리소스를 볼 수 있는 권한이 있는지 확인합니다. 이 메서드의 주요 목적은 UI를 장식하는 데 사용할 정보를 검색하는 것입니다(예: 잠금 및 잠금 해제 아이콘으로 액세스 상태를 표시). **cache** 매개 변수는 내부 캐시가 리소스 확인에 사용되는지 여부를 제어합니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 현재 선택한 공급자를 설정합니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources cache:(BOOL)cache; </code></pre></td>
</tr>
</tbody>
</table>



**가용성:** v3.1+



**매개 변수:**

* 인증을 확인해야 하는 리소스의 *리소스:* 배열. 목록의 각 요소는 리소스 ID를 나타내는 문자열이어야 합니다. 리소스 ID는 `getAuthorization:` 호출에서 리소스 ID와 동일한 제한을 받습니다. 즉, 프로그래머와 MVPD 또는 미디어 RSS 조각 간에 설정된 합의된 값이어야 합니다.
* *cache:* 리소스 확인에 내부 캐시를 사용할지 여부를 지정하는 부울. false인 경우 캐시가 무시되어 이 API가 호출될 때마다 서버가 호출됩니다.

**트리거된 콜백:** [`preauthorizedResources:`](#preauthResources)

[맨 위로...](#apis)

</br>

### preauthorizedSources: {#preauthResources}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명:** 콜백이 `checkPreauthorizedResources:`에 의해 트리거되었습니다. 사용자가 이미 볼 수 있는 권한이 있는 리소스 목록을 제공합니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 현재 선택한 공급자를 설정합니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.3+

**매개 변수:**

* `resources`: 사용자에게 이미 보기 권한이 부여된 리소스 배열입니다.

**트리거 대상:** [`checkPreauthorizedResources:`](#checkPreauth)



[맨 위로...](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 응용 프로그램에서 인증 상태를 확인하는 데 이 메서드를 사용합니다. 먼저 인증 상태를 확인하는 것으로 시작됩니다. 인증되지 않으면 [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) 콜백이 트리거되고 메서드가 종료됩니다. 사용자가 인증되면 권한 부여 플로우도 트리거됩니다. [`getAuthorization:`](#getAuthZ) 메서드에 대한 자세한 내용을 참조하십시오.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 상태 확인</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 상태 확인</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource:
                   withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.9+

**매개 변수:**

* *resource*: 사용자가 권한 부여를 요청하는 리소스의 ID입니다.
* *data*: Pay-TV Pass 서비스로 보낼 키-값 쌍으로 구성된 사전입니다. Adobe은 이 데이터를 사용하여 SDK을 변경하지 않고 향후 기능을 활성화할 수 있습니다.

**트리거된 콜백:**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed),`setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[맨 위로...](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 응용 프로그램에서 인증 흐름을 시작하는 데 이 메서드를 사용합니다. 사용자가 아직 인증되지 않은 경우 인증 플로우도 시작합니다. 사용자가 인증되면 AccessEnabler는 인증 토큰(로컬 토큰 캐시에 유효한 인증 토큰이 없는 경우) 및 단기 미디어 토큰에 대한 요청을 계속 발행합니다. 짧은 미디어 토큰을 얻으면 인증 흐름이 완료된 것으로 간주됩니다. [`setToken:forResource:`](#setToken) 콜백이 트리거되고 짧은 미디어 토큰이 응용 프로그램에 매개 변수로 전달됩니다. 어떤 이유로든 인증에 실패하면 [`tokenRequestFailed:forEventType:`](#tokenReqFailed) 콜백이 트리거되고 오류 코드/세부 정보가 제공됩니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 흐름 시작</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 인증 흐름 시작</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource:
                 withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>



**가용성:** v1.9+

**매개 변수:**

* *resource*: 사용자가 권한 부여를 요청하는 리소스의 ID입니다.
* *data*: Pay-TV Pass 서비스로 보낼 키-값 쌍으로 구성된 사전입니다. Adobe은 이 데이터를 사용하여 SDK을 변경하지 않고 향후 기능을 활성화할 수 있습니다.

**트리거된 콜백:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**추가 콜백이 트리거됨:**\
이 메서드는 다음 콜백을 트리거할 수도 있습니다(인증 흐름도 시작된 경우). `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

>[!NOTE]
>
>가능하면 `getAuthorization:` / `getAuthorization:withData:` 대신 `checkAuthorization:` / `checkAuthorization:withData:`을(를) 사용하십시오. `getAuthorization:` / `getAuthorization:withData:` 메서드가 전체 인증 흐름을 시작하며(사용자가 인증되지 않은 경우) 이로 인해 프로그래머측의 복잡한 구현이 발생할 수 있습니다.

[맨 위로...](#apis)

</br>

### `setToken:forResource:` {#setToken}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

인증 흐름이 성공적으로 완료되었음을 응용 프로그램에 알리는 AccessEnabler에 의해 트리거된 **설명** 콜백입니다. 수명이 짧은 미디어 토큰도 매개 변수로 제공됩니다.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>콜백: 인증 흐름이 완료되었습니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setToken:(NSString *)token 
      forResource:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

**매개 변수**:

* *token*: 수명이 짧은 미디어 토큰
* *resource*: 인증을 받은 리소스입니다.

**트리거 대상:** [`checkAuthorization:`](#checkAuthZ) , [`checkAuthorization:withData:`](#checkAuthZ), [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ)

[맨 위로...](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 인증 흐름이 실패했음을 상위 계층 응용 프로그램에 알리는 AccessEnabler에 의해 트리거된 콜백입니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: 인증 흐름 실패</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) tokenRequestFailed:(NSString *)resource 
                  errorCode:(NSString *)code 
           errorDescription:(NSString *)description; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

**매개 변수**:

* *resource*: 인증을 받은 리소스입니다.
* *code*: 오류 시나리오와 연결된 오류 코드입니다. 가능한 값:
   * `USER_NOT_AUTHORIZED_ERROR` - 사용자가 권한을 부여할 수 없습니다.
(특정 리소스에 대해)
* *설명*: 오류 시나리오에 대한 추가 세부 정보입니다. 어떤 이유로든 이 설명 문자열을 사용할 수 없는 경우 Adobe Pass 인증에서 빈 문자열 **(&quot;)**&#x200B;을(를) 보냅니다.\
  이 문자열은 MVPD에서 사용자 지정 오류 메시지 또는 판매 관련 메시지를 전달하는 데 사용할 수 있습니다. 예를 들어 구독자가 리소스에 대한 인증을 거부하면 MVPD에서 다음과 같은 메시지를 보낼 수 있습니다. &quot;현재 패키지에서 이 채널에 대한 액세스 권한이 없습니다. 패키지를 업그레이드하려면 **여기**&#x200B;를 클릭하세요.&quot; 이 콜백을 통해 Adobe Pass Authentication에서 메시지를 표시하거나 무시할 수 있는 옵션이 있는 프로그래머에게 전달합니다. Adobe Pass 인증은 이 매개 변수를 사용하여 오류가 발생했을 수 있는 조건에 대한 알림을 제공할 수도 있습니다. 예를 들어 &quot;공급자의 인증 서비스와 통신하는 동안 네트워크 오류가 발생했습니다.&quot;와 같습니다.

**트리거 대상:** `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)

[맨 위로...](#apis)

</br>

### 로그아웃 {#logout}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 로그아웃 흐름을 시작하기 위해 응용 프로그램에서 이 메서드를 호출합니다. 로그아웃은 사용자가 Adobe Pass 인증 서버와 MVPD 서버에서 모두 로그아웃해야 하므로 일련의 HTTP 리디렉션 작업의 결과입니다. AccessEnabler 라이브러리에서 발급한 간단한 HTTP 요청으로는 이 흐름을 완료할 수 없으므로 HTTP 리디렉션 작업을 수행하려면 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러를 인스턴스화해야 합니다.

로그아웃 흐름은 사용자가 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러와 어떤 식으로든 상호 작용할 필요가 없다는 점에서 인증 흐름과 다릅니다. 따라서 Adobe은 로그아웃 프로세스 중에 컨트롤을 보이지 않게(즉, 숨겨지도록) 만들 것을 권장합니다.

인증 흐름과 유사한 패턴이 사용됩니다. iOS AccessEnabler가 `navigateToUrl:` 콜백 또는 `navigateToUrl:useSVC:`을(를) 트리거하여 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러를 만들고 콜백의 `url` 매개 변수에 제공된 URL을 로드합니다. 백엔드 서버에 있는 로그아웃 끝점의 URL입니다. tvOS AccessEnabler의 경우 `navigateToUrl:` 콜백이나 `navigateToUrl:useSVC:` 콜백이 호출되지 않습니다.

여러 번의 리디렉션을 거치는 동안 응용 프로그램은 `UIWebView/WKWebView or SFSafariViewController ` 컨트롤러의 활동을 모니터링하고 특정 사용자 지정 URL이 로드되는 순간을 감지해야 합니다. 이 특정 사용자 지정 URL은 실제로 유효하지 않으며 제어자가 실제로 로드하기 위한 것이 아닙니다. 응용 프로그램에서 로그아웃 흐름이 완료되었으며 컨트롤러를 닫는 것이 안전하다는 신호로만 해석해야 합니다. 컨트롤러가 이 특정 사용자 지정 URL을 로드하면 응용 프로그램에서 컨트롤러를 닫고 AccessEnabler의 `handleExternalURL:url `API 메서드를 호출해야 합니다. 응용 프로그램에서 `SFSafariViewController `컨트롤러를 사용해야 하는 경우 특정 사용자 지정 URL은 `application's custom scheme`(예: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`)에 의해 정의되며, 그렇지 않으면 이 특정 사용자 지정 URL은 `ADOBEPASS_REDIRECT_URL `상수(예: `adobepass://ios.app`)에 의해 정의됩니다.

결국 AccessEnabler가 상태 코드가 0인 [`setAuthenticationStatus()`](#setAuthNStatus) 콜백을 호출하여 로그아웃 흐름이 성공했음을 나타냅니다.

**참고:** 사용자가 Apple SSO를 사용하여 로그인하면 VSA203 상태가 트리거됩니다. 이 경우 시스템 설정에서도 로그아웃하라는 메시지가 표시됩니다. 이렇게 하지 않으면 애플리케이션이 다시 시작될 때 인증이 다시 수행됩니다.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 로그아웃 흐름 시작</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

**매개 변수:** 없음

**콜백 트리거됨:** `navigateToUrl:`, [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[맨 위로...](#apis)

</br>

### getSelectedProvider {#getSelProv}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 이 메서드를 사용하여 현재 선택한 공급자를 확인합니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 현재 선택한 MVPD 확인</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

**매개 변수:** 없음

**트리거된 콜백:** [`selectedProvider:`](#selProv)

[맨 위로...](#apis)

</br>

### selectedProvider {#selProv}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 현재 선택한 MVPD에 대한 정보를 응용 프로그램에 전달하는 AccessEnabler에 의해 트리거되는 콜백입니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: 현재 선택한 MVPD에 대한 정보</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) selectedProvider:(MVPD *)mvpd;</code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

**매개 변수**:

* *mvpd*: 현재 선택한 MVPD에 대한 정보가 포함된 개체

**트리거 대상:** [`getSelectedProvider`](#getSelProv)

[맨 위로...](#apis)

</br>

### getMetadata: {#getMeta}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 이 메서드를 사용하여 AccessEnabler 라이브러리에서 메타데이터로 노출된 정보를 검색합니다. 응용 프로그램에서 사전 기반 *키* 입력 매개 변수를 제공하여 이 데이터에 액세스할 수 있습니다.

프로그래머가 사용할 수 있는 메타데이터에는 두 가지 유형이 있습니다.

* 정적 메타데이터(인증 토큰 TTL, 인증 토큰 TTL 및 장치 ID)
* 사용자 메타데이터(사용자 ID, 우편번호와 같은 사용자별 정보. 인증 및 권한 부여 흐름 동안 MVPD에서 사용자의 장치로 전달될 수 있음)

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: AccessEnabler for metadata 쿼리</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

**매개 변수:**

* *keyDictionary*: 사전 데이터 구조이며, 다음과 같습니다.
형식:
   * 키가 `METADATA_OPCODE_KEY`이고 값이 `METADATA_AUTHENTICATION`인 경우 인증 토큰 만료 시간을 얻기 위해 쿼리가 수행됩니다.
   * 키가 `METADATA_OPCODE_KEY`이고 값이 `METADATA_AUTHORIZATION` **및**&#x200B;인 경우\
     키가 `METADATA_RESOURCE_ID_KEY`이고 값이 특정 리소스 ID인 경우 지정된 리소스와 연결된 인증 토큰의 만료 시간을 얻기 위해 쿼리가 수행됩니다.
   * 키가 `METADATA_OPCODE_KEY`이고 값이 `METADATA_DEVICE_ID`인 경우 현재 장치 ID를 얻기 위해 쿼리가 수행됩니다. 이 기능은 기본적으로 비활성화되어 있으며 프로그래머는 사용 권한 및 요금에 대한 정보를 Adobe에 문의해야 합니다.
   * 키가 `METADATA_OPCODE_KEY`이고 값이 `METADATA_USER_META` **이고** 키가 `METADATA_USER_META_KEY`이고 값이 메타데이터 이름인 경우 사용자 메타데이터에 대한 쿼리가 수행됩니다. 사용 가능한 사용자 메타데이터 유형 목록:
      * `zip` - 우편 번호 목록
      * `householdID` - 세대 식별자. MVPD에서 하위 계정을 지원하지 않는 경우 `userID`과(와) 동일합니다.
      * `maxRating` - 사용자의 최대 자녀 보호 등급 컬렉션
      * `userID` - 사용자 식별자. MVPD에서 하위 계정을 지원하고 사용자가 주 계정이 아닌 경우 `userID`은(는) `householdID.`과(와) 달라집니다
      * `channelID` - 사용자가 볼 수 있는 채널 목록입니다.

  >[!NOTE]
  >
  >프로그래머가 사용할 수 있는 실제 사용자 메타데이터는 MVPD이 사용할 수 있도록 하는 항목에 따라 다릅니다. 이 목록은 새 메타데이터를 사용할 수 있고 Adobe Pass 인증 시스템에 추가됨에 따라 확장됩니다.

**트리거된 콜백:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**추가 정보:** [사용자 메타데이터](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)

[맨 위로...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 현재 요청자가 SSO를 지원하는 MVPD을 하나 이상 지원하는 경우 [getAuthentication()](#getAuthN)을 호출한 후 AccessEnabler에 의해 트리거된 콜백입니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>콜백: SSO 흐름의 결과</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) presentTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v2.0+

**매개 변수**:

* viewController: Apple SSO 대화 상자를 나타냅니다. 이 viewController를 화면에 표시해야 합니다.

**트리거 대상:** [`getAuthentication`](#getAuthN)

**추가 정보:** [iOS/tvOS Single Sign On](#presentTvDialog)

[맨 위로...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

사용자가 Apple SSO 대화 상자를 닫은 후 AccessEnabler에서 트리거된 **설명** 콜백입니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>콜백: SSO 흐름의 결과</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) dismissTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v2.0+

**매개 변수**:

* viewController: Apple SSO 대화 상자를 나타냅니다. 이 viewController를 화면에서 제거해야 합니다.

**트리거한 사람:** 사용자 작업

**추가 정보:** [iOS/tvOS Single Sign On](#presentTvDialog)

[맨 위로...](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

[`getMetadata:`](#getMeta) 호출을 통해 요청된 메타데이터를 전달하는 AccessEnabler에 의해 트리거된 **설명** 콜백입니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: 메타데이터 검색 요청의 결과</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setMetadataStatus:(id)metadata
                 encrypted:(bool)encrypted 
                    forKey:(int)key 
              andArguments:(NSDictionary *)arguments; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:** v1.0+

**매개 변수**:

* *메타데이터*: 요청한 메타데이터입니다. 이 값은 정적 메타데이터(인증 TTL, 권한 부여 TTL, 장치 ID)의 경우 `NSString`입니다.  사용자별 메타데이터를 요청할 때 복잡한 개체입니다. 일반적으로 이 복합 개체는 JSON 페이로드의 Objective-C 표현입니다(예: &#39;&lbrace;&quot;street&quot;: &quot;Main Avenue&quot;, &quot;buildings&quot;: [&quot;150&quot;, &quot;320&quot;]&#39;)는 Objective-C에서 NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;buildings&quot; -> NSArray(&quot;150&quot;, &quot;320&quot;))로 변환됨).   샘플 메타데이터 JSON 개체:

```JSON
        {
            updated: 1334243471,
            encrypted: ["encryptedProp"],
            data: {
                zip: ["12345", "34567"],
                maxRating: { 
                    "MPAA": "PG-13",
                    "VCHIP": "TV-Y", 
                    "URL": "http://exam.pl/e/manage/ratings"
                },
                householdID: "3456",
                userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
                channelID: ["channel-1", "channel-2"]
            }
```

* *encrypted*: 검색된 메타데이터의 암호화 여부를 지정하는 부울 값입니다. 이 매개 변수는 사용자 메타데이터 요청에만 중요하며 항상 암호화되지 않은 상태로 수신되는 정적 메타데이터(예: 인증 TTL)에는 의미가 없습니다. 이 매개 변수가 true로 설정되면 화이트리스트에 추가한 개인 키([`setRequestor:setSignedRequestorId:`](#setReq) 및 `setRequestor:setSignedRequestorId:serviceProviders: ` 호출에서 요청자 ID 서명에 사용되는 것과 동일한 개인 키)를 사용하여 RSA 암호 해독을 수행하여 암호화되지 않은 사용자 메타데이터 값을 얻는 것은 프로그래머가 결정합니다.

* *key*: 메타데이터 검색 요청을 만드는 데 사용되는 키입니다.

* *인수*: [`getMetadata:`](#getMeta) 호출에 전달된 동일한 사전입니다. 애플리케이션이 요청을 응답과 일치시킬 수 있도록 하기 위해 제공됩니다.

**트리거 대상:** [`getMetadata:`](#getMeta)

**추가 정보:** [사용자 메타데이터](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)


[맨 위로...](#apis)

</br>

### MVPD {#mvpd}

**파일:** AccessEnabler/headers/model/MVPD.h

**설명** MVPD 개체를 설명합니다. MVPD 속성에 대한 정보를 가져오는 데 사용할 수 있습니다.

**가용성:** v1.0+ [boardingStatus 속성은 v2.2]에서 사용할 수 있습니다.

**속성**:

* (NSString) ID - MVPD Id입니다.
* (NSString) displayName - MVPD 이름입니다. [선택기에 표시하는 데 사용해야 합니다]
* (NSString) logoURL - MVPD 로고 주소.
* (BOOL) enablePlatformServices - true인 경우 MVPD에서 [Apple SSO](#presentTvDialog)와 같은 SSO 서비스를 지원합니다.
* (NSString) boardingStatus - 3개의 값을 가질 수 있습니다.
   * nil - MVPD은 Apple SSO를 지원하지 않습니다.
   * 선택기 - MVPD은 Apple 선택기에 나타날 수 있지만 인증 흐름은 Adobe에 의해 수행됩니다.
   * 지원됨 - MVPD은 Apple에서 완전히 지원되며 Apple의 SSO 토큰을 사용합니다.

[맨 위로...](#apis)

</br>

## 이벤트 추적 {#tracking}

AccessEnabler는 자격 흐름과 관련이 없는 추가 콜백을 트리거합니다. [`sendTrackingData()`](#sendTracking) 콜백 함수를 구현하는 것은 선택 사항이지만 응용 프로그램에서 특정 이벤트를 추적하고 인증/권한 부여 시도 성공/실패 횟수와 같은 통계를 컴파일할 수 있도록 합니다.

### sendTrackingData:forEventType: {#sendTracking}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 인증/권한 부여 흐름의 완료/실패와 같은 다양한 이벤트가 발생할 때 응용 프로그램에 대한 AccessEnabler 신호로 트리거되는 콜백입니다. Adobe Pass 인증 1.6을 사용하면 장치 유형, AccessEnabler 클라이언트 유형 및 운영 체제가 [`sendTrackingData()`](#sendTracking)에 의해 보고됩니다. [`sendTrackingData()`](#sendTracking) 콜백은 이전 버전과 호환되는 상태로 유지됩니다.

**콜백: 이벤트 추적**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**가용성:** v1.0+

**참고:** 장치 형식 및 운영 체제는 공용 Java 라이브러리(<http://java.net/projects/user-agent-utils>) 및 사용자 에이전트 문자열을 사용하여 파생됩니다. 이 정보는 운영 지표를 장치 범주로 분류하는 거친 방법으로만 제공되지만, 잘못된 결과에 대해서는 Adobe이 책임을 지지 않을 수 있습니다. 그에 따라 새로운 기능을 사용하십시오.

* 장치 유형에 가능한 값:
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* AccessEnabler 클라이언트 유형에 사용할 수 있는 값은 다음과 같습니다.
   * `flash`
   * `html5`
   * `ios`
   * `android`


**매개 변수**:

* *event*: 추적 중인 이벤트의 코드입니다. 가능한 추적 이벤트 유형에는 세 가지가 있습니다.
   * 인증 토큰 요청이 반환될 때마다 **authorizationDetection:**(이벤트는 `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** 인증 확인이 발생할 때마다(이벤트는 `TRACKING_AUTHENTICATION`)
   * 사용자가 MVPD 선택 양식에서 MVPD을 선택할 때 **mvpdSelection:**(이벤트는 `TRACKING_GET_SELECTED_PROVIDER`)
* *data*: 보고된 이벤트와 연결된 추가 데이터입니다. 이 데이터는 값 목록 형태로 표시됩니다.

**트리거 기준:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ), `setSelectedProvider:`

*data* 배열의 값을 해석하기 위한 지침:

* TrackingEventType `TRACKING_AUTHENTICATION:`의 경우
   * **0** - 토큰 요청의 성공 여부(true/false) 및 성공 여부:
   * **1** - MVPD ID 문자열
   * **2** - GUID(md5 해시됨)
   * **3** - 토큰이 캐시에 이미 있습니다(true/false).
   * **4** - 장치 유형
   * **5** - AccessEnabler 클라이언트 유형
   * **6** - 운영 체제 유형

* TrackingEventType `TRACKING_AUTHORIZATION:`의 경우
   * **0** - 토큰 요청의 성공 여부(true/false) 및 성공 여부:
   * **1** - MVPD ID
   * **2** - GUID(md5 해시됨)
   * **3** - 토큰이 캐시에 이미 있습니다(true/false).
   * **4** - 오류
   * **5** - 세부 정보
   * **6** - 장치 유형
   * **7** - AccessEnabler 클라이언트 유형
   * **8** - 운영 체제 유형
* TrackingEventType `TRACKING_GET_SELECTED_PROVIDER:`의 경우
   * **0** - 현재 선택한 MVPD의 ID
   * **1** - 장치 유형
   * **2** - AccessEnabler 클라이언트 유형
   * **3** - 운영 체제 유형

</br>
