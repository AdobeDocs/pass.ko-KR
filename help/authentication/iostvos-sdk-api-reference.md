---
title: iOS/tvOS API 참조
description: iOS/tvOS API 참조
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '7010'
ht-degree: 0%

---

# iOS/tvOS SDK API 참조 {#iostvos-sdk-api-reference}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 소개 {#intro}

이 페이지에서는 Adobe Pass 인증용 iOS/tvOS 기본 클라이언트가 노출하는 메소드 및 콜백 함수에 대해 설명합니다. 여기에 설명된 메서드와 콜백 함수는 `AccessEnabler.h` 및 `EntitlementDelegate.h` 헤더 파일: iOS AccessEnabler SDK의 다음 위치에서 찾을 수 있습니다. `[SDK directory]/AccessEnabler/headers/api/`


관련 설명서:

* 기본 Adobe Pass 인증 권한 흐름에 대한 설명은 을 참조하십시오. [권한 흐름](/help/authentication/entitlement-flow.md).
* 이 API를 사용하여 Adobe Pass 인증 권한 흐름을 구현하는 방법에 대한 단계별 안내는 다음을 참조하십시오. [iOS 통합 Cookbook](/help/authentication/iostvos-sdk-cookbook.md).
* 최신 iOS AccessEnabler SDK에 대해서는 다음을 참조하십시오. [iOS Native Access Enabler 라이브러리](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>Adobe은 Adobe Pass 인증만 사용할 것을 권장합니다 *공용* API:
>
>* 공개 API는 지원되는 모든 클라이언트 유형에서 사용할 수 있으며 완전히 테스트되었습니다. 모든 공개 기능의 경우 각 클라이언트 유형에 연결된 메서드의 해당 버전이 있는지 확인합니다.
>* 공개 API는 이전 버전과의 호환성을 지원하고 파트너 통합이 중단되지 않도록 최대한 안정적이어야 합니다. 그러나 비공개 API의 경우, 향후 언제든지 서명을 변경할 수 있는 권한이 있습니다. 현재 공개 Adobe Pass 인증 API 호출의 조합을 통해 지원되지 않는 특정 흐름이 발생하는 경우 가장 좋은 접근 방법은 알려 주는 것입니다. 귀사의 요구를 고려하여 공개 API를 수정하고 안정적인 솔루션을 제공할 수 있습니다.

</br>

## API 참조 {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - AccessEnabler 개체를 인스턴스화합니다.

* **[더 이상 사용되지 않음]** [init](#init) - AccessEnabler 개체를 인스턴스화합니다.

* [setOptions:options:](#setOptions) - 프로필 또는 visitorID와 같은 글로벌 SDK 옵션을 구성합니다.

* [setRequestor:](#setReqV3)[요청자 ID](#setReqV3),[setRequestor:requestorID:서비스 공급자:](#setReqV3) - 프로그래머의 ID를 설정합니다.

* **[더 이상 사용되지 않음]** [setRequestor:signedRequestorId:](#setReq),[setRequestor:signedRequestorId:서비스 공급자:](#setReq) - 프로그래머의 ID를 설정합니다.

* **[더 이상 사용되지 않음]** [setRequestor:signedRequestorId:secret:publicKey](#setReq_tvos), [setRequestor:signedRequestorId:서비스 공급자:secret:공개 키](#setReq_tvos)- 프로그래머의 정체성을 확립합니다.

* [setRequestorComplete:](#setReqComplete) - 구성 단계가 완료되었음을 애플리케이션에 알립니다.

* [checkAuthentication](#checkAuthN) - 현재 사용자의 인증 상태를 확인합니다.

* [getAuthentication](#getAuthN), [getAuthentication:withData:](#getAuthN) - 전체 인증 워크플로를 시작합니다.

* [getAuthentication:filter](#getAuthN_filter),[getAuthentication:withData:](#getAuthN)[andFilter](#getAuthN_filter) - 전체 인증 워크플로를 시작합니다.

* [displayProviderDialog:](#dispProvDialog) - 사용자가 MVPD를 선택할 수 있는 적절한 UI 요소를 인스턴스화하도록 애플리케이션에 알립니다.

* [setSelectedProvider:](#setSelProv) - AccessEnabler에 사용자의 MVPD 선택을 알립니다.

* [navigateToUrl:](#nav2url) - 애플리케이션에 MVPD 로그인 페이지를 표시해야 한다고 알립니다.

* [navigateToUrl:useSVC:](#nav2urlSVC) - SFSafariViewController를 사용하여 애플리케이션에 MVPD 로그인 페이지를 표시해야 한다고 알립니다.

* [handleExternalURL:url](#handleExternalURL) - 인증/로그아웃 흐름을 완료합니다.

* **[더 이상 사용되지 않음]** [getAuthenticationToken](#getAuthNToken) - 백엔드 서버에서 인증 토큰을 요청합니다.

* [setAuthenticationStatus:errorCode:](#setAuthNStatus) - 응용 프로그램에 인증 흐름의 상태를 알립니다.

* [checkPreauthorizedResources:](#checkPreauth) - 사용자에게 특정 보호된 리소스를 볼 수 있는 권한이 이미 있는지 여부를 결정합니다.

* [checkPreauthorizedResources:cache:](#checkPreauthCache) - 사용자에게 특정 보호된 리소스를 볼 수 있는 권한이 이미 있는지 여부를 결정합니다.

* [preauthorizedSources:](#preauthResources) - 사용자에게 이미 보기 권한이 부여된 리소스 목록을 제공합니다.

* [checkAuthorization:](#checkAuthZ), [checkAuthorization:withData:](#checkAuthZ) - 현재 사용자의 인증 상태를 확인합니다.

* [getAuthorization:](#getAuthZ), [getAuthorization:withData:](#getAuthZ) - 인증 흐름을 시작합니다.

* [setToken:forResource:](#setToken) - 권한 부여 플로우가 성공적으로 완료되었음을 애플리케이션에 알립니다.

* [token요청 실패:errorCode:errorDescription:](#tokenReqFailed) - 권한 부여 플로우가 실패했음을 애플리케이션에 알립니다.

* [로그아웃](#logout) - 로그아웃 흐름을 시작합니다.

* [getSelectedProvider](#getSelProv) - 현재 선택한 공급자를 결정합니다.

* [selectedProvider:](#selProv) - 현재 선택한 MVPD에 대한 정보를 애플리케이션에 전달합니다.

* [getMetadata:](#getMeta) - AccessEnabler 라이브러리에서 메타데이터로 노출된 정보를 검색합니다.

* [presentTvProviderDialog:](#presentTvDialog) - 애플리케이션에 Apple SSO 대화 상자를 표시하도록 알립니다.

* [dismissTvProviderDialog:](#dismissTvDialog) - Apple SSO 대화 상자를 숨기도록 애플리케이션에 알립니다.

* [setMetaStatus:encrypted:forKey:andArguments:](#setMetaStatus) - 가 요청한 메타데이터를 전달합니다. [getMetadata:](#getMeta) 호출합니다.

* [sendTrackingData:forEventType:](#sendTracking) - 추적 데이터 정보를 제공합니다.

* [MVPD](#mvpd) - MVPD 클래스. [MVPD에 대한 정보를 포함합니다.]

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



### init - [더 이상 사용되지 않음]{#init}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** AccessEnabler 개체를 인스턴스화합니다. 애플리케이션 인스턴스당 하나의 AccessEnabler 인스턴스가 있어야 합니다.

| API 호출: iOS AccessEnabler 생성자 |
| --- |
| ```- (id) init;``` |

**가용성:** v1.0+ **종료:** v3.0

**매개 변수:** 없음

[맨 위로...](#apis)

</br>

### setOptions:options {#setOptions}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 글로벌 SDK 옵션을 구성합니다. NSDictionary를 인수로 수락합니다. 사전의 값은 SDK에서 수행하는 모든 네트워크 호출과 함께 서버로 전달됩니다.

**참고:** 값은 현재 흐름(인증/권한 부여)과 관계없이 서버에 전달됩니다. 값을 변경하려면 언제든지 이 메서드를 호출할 수 있습니다.

| API 호출: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**가용성:** v2.3.0+

**매개 변수:**

* *옵션*: 글로벌 SDK 옵션이 포함된 NSDictionary입니다. 현재 다음 옵션을 사용할 수 있습니다.
   * **applicationProfile** - 이 값을 기반으로 서버 구성을 만드는 데 사용할 수 있습니다.
   * **visitorID** - Experience Cloud ID 서비스. 이 값은 나중에 고급 분석 보고서에 사용할 수 있습니다.
   * **handleSvc** - 프로그래머가 SFSafariViewControllers를 처리할지 여부를 나타내는 부울. 다음을 참조하십시오. [iOS SDK 3.2+에서 SFSafariViewController 지원](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) 을 참조하십시오.
      * 로 설정된 경우 **false,** sdk는 최종 사용자에게 SFSafariViewController를 자동으로 제공합니다. SDK는 MVPD 로그인 페이지 URL로 이동합니다.
      * 로 설정된 경우 **true,** sdk는 다음을 수행합니다 **아님** sfsAfariViewController를 사용하여 최종 사용자를 자동으로 표시합니다. SDK가 추가로 트리거합니다. **navigate(toUrl:{url}, useSVC:YES)**.
* **device\_info** - 다음에 설명된 클라이언트 정보 [클라이언트 정보 전달](/help/authentication/passing-client-information-device-connection-and-application.md).

[맨 위로...](#apis)


### setRequestor:requestorID, setRequestor:requestorID:서비스 공급자: {#setReqV3}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 프로그래머의 ID를 설정합니다. 각 프로그래머는 Adobe Pass 인증 시스템에 대한 Adobe 등록 시 고유 ID가 지정됩니다. SSO 및 원격 토큰을 처리할 때, 애플리케이션이 백그라운드에 있을 때 인증 상태가 변경될 수 있고, 시스템 상태와 동기화하기 위해 애플리케이션이 전경으로 전환될 때 setRequestor를 다시 호출할 수 있습니다(SSO가 활성화된 경우 원격 토큰을 가져오고, 그 사이에 로그아웃이 발생한 경우 로컬 토큰을 삭제).

서버 응답에는 프로그래머의 ID에 첨부된 일부 구성 정보와 함께 MVPD 목록이 포함됩니다. 서버 응답은 AccessEnabler 코드에서 내부적으로 사용됩니다. 작업의 상태(즉, SUCCESS/FAIL)만 을 통해 애플리케이션에 표시됩니다. `setRequestorComplete:` callback.

다음과 같은 경우 `urls` 매개 변수가 사용되지 않으면 결과 네트워크 호출은 기본 서비스 공급자 URL(Adobe RELEASE/프로덕션 환경)을 타깃팅합니다.


에 대한 값이 제공되는 경우 `urls` 매개 변수, 결과 네트워크 호출은에 제공된 모든 URL을 타겟팅합니다. `urls` 매개 변수. 모든 구성 요청은 별도의 스레드에서 동시에 트리거됩니다. MVPD 목록을 컴파일할 때 첫 번째 응답자가 우선합니다. 목록에 있는 각 MVPD에 대해 AccessEnabler는 관련 서비스 공급자의 URL을 기억합니다. 모든 후속 자격 요청은 구성 단계 동안 대상 MVPD와 쌍을 이룬 서비스 공급자와 연결된 URL로 전달됩니다.

| API 호출: 요청자 구성 |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**가용성:** v3.0+

| API 호출: 요청자 구성 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**가용성:** v3.0+

**매개 변수:**

* *요청자 ID*: 프로그래머와 연결된 고유 ID입니다. Adobe Pass 인증 서비스에 처음 등록할 때 Adobe이 할당한 고유 ID를 사이트에 전달합니다.
* *url*: 선택적 매개 변수. 기본적으로 Adobe 서비스 공급자가 사용됩니다(http://sp.auth.adobe.com/). 이 배열을 사용하면 Adobe에서 제공하는 인증 및 권한 부여 서비스에 대한 끝점을 지정할 수 있습니다(디버깅 목적으로 다른 인스턴스를 사용할 수 있음). 이 옵션을 사용하여 여러 Adobe Pass 인증 서비스 공급자 인스턴스를 지정할 수 있습니다. 이렇게 하면 MVPD 목록은 모든 서비스 공급자의 끝점으로 구성됩니다. 각 MVPD는 가장 빠른 서비스 공급자, 즉 먼저 응답하고 해당 MVPD를 지원하는 공급자와 연결됩니다.

>[!NOTE]
>
>를 사용하지 않고 호출되는 경우 `serviceProviders` 매개 변수를 지정하면 라이브러리가 기본 서비스 공급자로부터 구성을 검색합니다(즉, `https://sp.auth.adobe.com` 프로덕션 프로필 또는 `https://sp.auth-staging.adobe.com` 스테이징 프로필용). 다음과 같은 경우 `serviceProviders` 매개 변수가 제공됩니다. 이 매개 변수는 URL의 배열이어야 합니다. 구성 정보는 지정된 모든 끝점에서 검색되고 병합됩니다. 서로 다른 서비스 공급자 응답에 중복 정보가 있는 경우 가장 빠르게 응답하는 서버(즉, 응답 시간이 가장 짧은 서버가 우선함)를 위해 충돌이 해결됩니다.

**트리거된 콜백:** [`setRequestorComplete:`](#setReqComplete)

[맨 위로...](#apis)

</br>

### setRequestor:setSignedRequestorId:, setRequestor:setSignedRequestorId:serviceProviders: - [더 이상 사용되지 않음] {#setReq}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 프로그래머의 ID를 설정합니다. 각 프로그래머는 Adobe Pass 인증 시스템에 대한 Adobe 등록 시 고유 ID가 지정됩니다. SSO 및 원격 토큰을 처리할 때 애플리케이션이 백그라운드에 있을 때 인증 상태가 변경될 수 있으며, 시스템 상태와 동기화하기 위해 애플리케이션이 전경으로 전환될 때 setRequestor를 다시 호출할 수 있습니다(SSO가 활성화된 경우 원격 토큰을 가져오고 그 사이에 로그아웃이 발생한 경우 로컬 토큰을 삭제).

서버 응답에는 프로그래머의 ID에 첨부된 일부 구성 정보와 함께 MVPD 목록이 포함됩니다. 서버 응답은 AccessEnabler 코드에서 내부적으로 사용됩니다. 작업의 상태(즉, SUCCESS/FAIL)만 을 통해 애플리케이션에 표시됩니다. `setRequestorComplete:` callback.

다음과 같은 경우 `urls` 매개 변수가 사용되지 않으면 결과 네트워크 호출은 기본 서비스 공급자 URL(Adobe RELEASE/프로덕션 환경)을 타깃팅합니다.

에 대한 값이 제공되는 경우 `urls` 매개 변수, 결과 네트워크 호출은에 제공된 모든 URL을 타겟팅합니다. `urls` 매개 변수. 모든 구성 요청은 별도의 스레드에서 동시에 트리거됩니다. MVPD 목록을 컴파일할 때 첫 번째 응답자가 우선합니다. 목록에 있는 각 MVPD에 대해 AccessEnabler는 관련 서비스 공급자의 URL을 기억합니다. 모든 후속 자격 요청은 구성 단계 동안 대상 MVPD와 쌍을 이룬 서비스 공급자와 연결된 URL로 전달됩니다.

| API 호출: 요청자 구성 |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**가용성:** v1.0+ **종료:** v3.0

| API 호출: 요청자 구성 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**가용성:** v1.0+ **종료:** v3.0

**매개 변수:**

* *요청자 ID*: 프로그래머와 연결된 고유 ID입니다. Adobe Pass 인증 서비스에 처음 등록할 때 Adobe이 할당한 고유 ID를 사이트에 전달합니다.
* *signedRequestorID*: **이 매개 변수는 iOS AccessEnabler 버전 1.2 이상에 있습니다.** 개인 키로 디지털 서명된 요청자 ID의 사본입니다. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*: 선택적 매개 변수. 기본적으로 Adobe 서비스 공급자가 사용됩니다(http://sp.auth.adobe.com/). 이 배열을 사용하면 Adobe에서 제공하는 인증 및 권한 부여 서비스에 대한 끝점을 지정할 수 있습니다(디버깅 목적으로 다른 인스턴스를 사용할 수 있음). 이 옵션을 사용하여 여러 Adobe Pass 인증 서비스 공급자 인스턴스를 지정할 수 있습니다. 이렇게 하면 MVPD 목록은 모든 서비스 공급자의 끝점으로 구성됩니다. 각 MVPD는 가장 빠른 서비스 공급자, 즉 먼저 응답하고 해당 MVPD를 지원하는 공급자와 연결됩니다.

**참고:** 를 사용하지 않고 호출되는 경우 `serviceProviders` 매개 변수를 지정하면 라이브러리가 기본 서비스 공급자로부터 구성을 검색합니다(즉,`https://sp.auth.adobe.com` 프로덕션 프로필 또는 `https://sp.auth-staging.adobe.com` 스테이징 프로필용). 다음과 같은 경우 `serviceProviders` 매개 변수가 제공됩니다. 이 매개 변수는 URL의 배열이어야 합니다. 구성 정보는 지정된 모든 끝점에서 검색되고 병합됩니다. 서로 다른 서비스 공급자 응답에 중복 정보가 있는 경우, 가장 빠르게 응답하는 서버(즉, 응답 시간이 가장 짧은 서버가 우선함)를 위해 충돌이 해결됩니다.

**트리거된 콜백:** [`setRequestorComplete:`](#setReqComplete)


[맨 위로...](#apis)

### setRequestor:setSignedRequestorId:secret:publicKey, setRequestor:setSignedRequestorId:서비스 공급자:secret:공개 키 - [더 이상 사용되지 않음] {#setReq_tvos}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 프로그래머의 ID를 설정합니다. 각 프로그래머는 Adobe Pass 인증 시스템에 대한 Adobe 등록 시 고유 ID가 지정됩니다. 이 설정은 응용 프로그램 수명 주기 동안 한 번만 수행해야 합니다.

서버 응답에는 프로그래머의 ID에 첨부된 일부 구성 정보와 함께 MVPD 목록이 포함됩니다. 서버 응답은 AccessEnabler 코드에서 내부적으로 사용됩니다. 작업의 상태(즉, SUCCESS/FAIL)만 을 통해 애플리케이션에 표시됩니다. `setRequestorComplete:` callback.

다음과 같은 경우 `urls` 매개 변수가 사용되지 않으면 결과 네트워크 호출은 기본 서비스 공급자 URL(Adobe RELEASE/프로덕션 환경)을 타깃팅합니다.

에 대한 값이 제공되는 경우 `urls` 매개 변수, 결과 네트워크 호출은에 제공된 모든 URL을 타겟팅합니다. `urls` 매개 변수. 모든 구성 요청은 별도의 스레드에서 동시에 트리거됩니다. MVPD 목록을 컴파일할 때 첫 번째 응답자가 우선합니다. 목록에 있는 각 MVPD에 대해 AccessEnabler는 관련 서비스 공급자의 URL을 기억합니다. 모든 후속 자격 요청은 구성 단계 동안 대상 MVPD와 쌍을 이룬 서비스 공급자와 연결된 URL로 전달됩니다.



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


**가용성:** v2.0+ **종료:** v3.0

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

**가용성:** v2.0+ **종료:** v3.0

**매개 변수:**

* *요청자 ID*: 프로그래머와 연결된 고유 ID입니다. Adobe Pass 인증 서비스에 처음 등록할 때 Adobe이 할당한 고유 ID를 사이트에 전달합니다.
* *signedRequestorID*: **이 매개 변수는 iOS AccessEnabler 버전 1.2 이상에 있습니다.** 개인 키로 디지털 서명된 요청자 ID의 사본입니다. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*: 선택적 매개 변수. 기본적으로 Adobe 서비스 공급자가 사용됩니다(http://sp.auth.adobe.com/). 이 배열을 사용하면 Adobe에서 제공하는 인증 및 권한 부여 서비스에 대한 끝점을 지정할 수 있습니다(디버깅 목적으로 다른 인스턴스를 사용할 수 있음). 이 옵션을 사용하여 여러 Adobe Pass 인증 서비스 공급자 인스턴스를 지정할 수 있습니다. 이렇게 하면 MVPD 목록은 모든 서비스 공급자의 끝점으로 구성됩니다. 각 MVPD는 가장 빠른 서비스 공급자, 즉 먼저 응답하고 해당 MVPD를 지원하는 공급자와 연결됩니다.
* secret and publicKey: 두 번째 화면 호출에 서명하는 데 사용되는 비밀 및 공개 키입니다. 자세한 내용은 [클라이언트 없는 설명서](#create_dev).

를 사용하지 않고 호출되는 경우 `serviceProviders` 매개 변수를 사용하면 라이브러리가 기본 서비스 공급자로부터 구성을 검색합니다(예: `https://sp.auth.adobe.com` 프로덕션 프로필의 경우 또는 스테이징 프로필의 경우 https://sp.auth-staging.adobe.com). 다음과 같은 경우 `serviceProviders` 매개 변수가 제공됩니다. 이 매개 변수는 URL의 배열이어야 합니다. 구성 정보는 지정된 모든 끝점에서 검색되고 병합됩니다. 서로 다른 서비스 공급자 응답에 중복 정보가 있는 경우, 가장 빠르게 응답하는 서버(즉, 응답 시간이 가장 짧은 서버가 우선함)를 위해 충돌이 해결됩니다.

**트리거된 콜백:** [`setRequestorComplete:`](#setReqComplete)

[맨 위로...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 구성 단계가 완료되었음을 애플리케이션에 알리는 AccessEnabler에 의해 트리거되는 콜백입니다. 앱에서 권한 부여 요청 발급을 시작할 수 있다는 신호입니다. 구성 단계가 완료될 때까지 애플리케이션에서 권한 부여 요청을 실행할 수 없습니다.

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
로컬 토큰 저장 공간에서 유효한 인증 토큰을 검색하여 이를 수행합니다. 이 메서드를 호출하면 네트워크 호출이 수행되지 않습니다. 애플리케이션에서 사용자의 인증 상태를 쿼리하고 그에 따라 UI를 업데이트(즉, 로그인/로그아웃 UI 업데이트)하는 데 사용됩니다. 인증 상태는 를 통해 애플리케이션에 전달됩니다. [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) callback.


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

### getAuthentication, getAuthentication:withData: {#getAuthN}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 전체 인증 워크플로를 시작합니다. 인증 상태를 확인하는 것으로 시작됩니다. 아직 인증되지 않은 경우 인증 흐름 state-machine이 시작됩니다.

* 마지막 인증 시도가 성공하면 MVPD 선택 단계가 생략되고 [`navigateToUrl:`](#nav2url) 콜백이 트리거됩니다. 응용 프로그램은 이 콜백을 사용하여 사용자에게 MVPD의 로그인 페이지를 제공하는 WebView 컨트롤을 인스턴스화합니다. **[참고: Access Enabler 1.5부터 SDK의 제한으로 인해 이 기능을 사용할 수 없습니다].**
* 마지막 인증 시도가 실패했거나 사용자가 명시적으로 로그아웃한 경우 [`displayProviderDialog:`](#dispProvDialog) 콜백이 트리거됩니다. 응용 프로그램은 이 콜백을 사용하여 MVPD 선택 UI를 표시합니다. 또한 앱은 를 통해 사용자의 MVPD 선택에 대해 AccessEnabler 라이브러리에 알려 인증 흐름을 재개해야 합니다. [`setSelectedProvider:`](#setSelProv) 메서드를 사용합니다.

사용자의 자격 증명이 MVPD 로그인 페이지에서 확인되므로 응용 프로그램은 사용자가 MVPD의 로그인 페이지에서 인증을 받는 동안 발생하는 여러 리디렉션 작업을 모니터링해야 합니다. 올바른 자격 증명을 입력하면 WebView 컨트롤은 `ADOBEPASS_REDIRECT_URL` 일정합니다. 이 URL은 WebView에서 로드하기 위한 것이 아닙니다. 애플리케이션은 이 URL을 가로채고 이 이벤트를 로그인 단계가 완료되었음을 나타내는 신호로 해석해야 합니다. 그런 다음 AccessEnabler에 제어 권한을 넘겨 인증 흐름을 완료합니다(호출). [핸들 외부 URL](#handleExternalURL) 메서드).

마지막으로, 인증 상태는 를 통해 애플리케이션에 전달됩니다. [setAuthenticationStatus:errorCode:](#setAuthNStatus) callback.

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

* *forceAuthn*: 사용자가 이미 인증되었는지 여부에 상관없이 인증 흐름을 시작할지 여부를 지정하는 플래그입니다.
* *데이터*: Pay-TV 패스 서비스로 보낼 키-값 쌍으로 구성된 사전입니다. Adobe은 이 데이터를 사용하여 SDK를 변경하지 않고 향후 기능을 활성화할 수 있습니다.

**트리거된 콜백:** ` setAuthenticationStatus:errorCode:, `[`displayProviderDialog:`](#dispProvDialog)`,`` sendTrackingData:forEventType:`


[맨 위로...](#apis)

</br>

### getAuthentication:filter, getAuthentication:withData:andFilter {#getAuthN_filter}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 전체 인증 워크플로를 시작합니다. 인증 상태를 확인하는 것으로 시작됩니다. 아직 인증되지 않은 경우 인증 흐름 state-machine이 시작됩니다.

* [presentTvProviderDialog()](#presentTvDialog) 현재 요청자가 SSO를 지원하는 MVPD를 하나 이상 가지고 있으면 이 호출됩니다. SSO를 지원하는 MVPD가 없으면 클래식 인증 흐름이 시작되고 필터 매개 변수가 무시됩니다.
* 사용자가 Apple SSO 플로우를 완료한 후 [dismissTvProviderDialog()](#dismissTvDialog) 이 트리거되고 인증 프로세스가 완료됩니다.

마지막으로, 인증 상태는 를 통해 애플리케이션에 전달됩니다. [setAuthenticationStatus:errorCode:](#setAuthNStatus) callback.

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

* *forceAuthn*: 사용자가 이미 인증되었는지 여부에 상관없이 인증 흐름을 시작할지 여부를 지정하는 플래그입니다.
* *데이터*: Pay-TV 패스 서비스로 보낼 키-값 쌍으로 구성된 사전입니다. Adobe은 이 데이터를 사용하여 SDK를 변경하지 않고 향후 기능을 활성화할 수 있습니다.
* 필터: Apple SSO 대화 상자에 표시되어야 하는 MVPD ID의 두 목록이 있는 사전입니다. SSO를 지원하지 않는 모든 MVPD는 무시되지만 순서는 준수됩니다. 사전에는 두 개의 키가 있어야 합니다.
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

* *forceAuthn*: 사용자가 이미 인증되었는지 여부에 상관없이 인증 흐름을 시작할지 여부를 지정하는 플래그입니다.
* *데이터*: Pay-TV 패스 서비스로 보낼 키-값 쌍으로 구성된 사전입니다. Adobe은 이 데이터를 사용하여 SDK를 변경하지 않고 향후 기능을 활성화할 수 있습니다.
* 필터: Apple SSO 대화 상자에 표시되는 MVPD ID 목록입니다. SSO를 지원하지 않는 모든 MVPD는 무시되지만 순서는 준수됩니다.

**트리거된 콜백:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[맨 위로...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 사용자가 원하는 MVPD를 선택할 수 있도록 적절한 UI 요소를 인스턴스화해야 함을 응용 프로그램에 알리기 위해 AccessEnabler에 의해 트리거된 콜백입니다. 콜백은 MVPD 개체 목록에 선택 UI 패널을 올바르게 구성하는 데 도움이 되는 추가 정보(예: MVPD의 로고, 친숙한 표시 이름 등을 가리키는 URL)를 제공합니다.

사용자가 원하는 MVPD를 선택하면, 상위 계층 애플리케이션은 를 호출하여 인증 흐름을 재개하는 것이 요구된다 `setSelectedProvider:` 사용자 선택에 해당하는 MVPD의 ID를 전달합니다.

**인증 흐름 중단** - 사용자가 &quot;뒤로&quot; 단추를 누를 수 있는 지점이며, 이는 인증 흐름을 중단하는 것과 같습니다. 이 시나리오에서는 응용 프로그램에서 [setSelectedProvider:](#setSelProv) 메서드, null을 매개 변수로 전달하여 AccessEnabler에 인증 상태 시스템을 재설정할 수 있는 기회를 제공합니다.

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

* *mvpds*: 애플리케이션에서 MVPD 선택 UI 요소를 빌드하는 데 사용할 수 있는 MVPD 관련 정보를 포함하는 MVPD 개체 목록입니다.

**트리거 기준:** ` getAuthentication, `[getAuthentication:withData:](#getAuthN),` getAuthorization:, `[getAuthorization:withData:](#getAuthZ)


[맨 위로...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 사용자의 MVPD 선택을 Access Enabler에 알리기 위해 애플리케이션에서 이 메서드를 호출합니다. 애플리케이션은 이 방법을 사용하여 인증에 사용되는 서비스 제공자를 선택 또는 변경할 수 있다.

선택한 MVPD가 TempPass MVPD인 경우 나중에 getAuthentication()을 호출할 필요 없이 해당 MVPD에 대해 자동으로 인증됩니다.

getAuthentication() 메서드에 추가 매개 변수가 제공되는 프로모션 임시 패스에는 이 작업이 가능하지 않습니다.

통과 시 *null* 매개 변수로 Access Enabler는 사용자가 인증 흐름을 취소했다고 가정하고(즉, &quot;뒤로&quot; 단추를 누른 경우) 인증 상태 시스템을 재설정하고 를 호출하여 응답합니다. [setAuthenticationStatus:errorCode:](#setAuthNStatus) 다음을 포함한 콜백 `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` 오류 코드.

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

**트리거된 콜백:** ` setAuthenticationStatus:errorCode:,sendTrackingData:forEventType:,  `[`navigateToUrl:`](#nav2url)

[맨 위로...](#apis)

</br>

#### navigateToUrl: {#nav2url}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명:** UIWebView/WKWebView 컨트롤러를 인스턴스화하고 콜백에 제공된 URL을 로드하도록 응용 프로그램에 요청하기 위해 AccessEnabler에 의해 트리거되는 콜백 **`url`** 매개 변수. 콜백은 **`url`** 인증 끝점의 URL 또는 로그아웃 끝점의 URL을 나타내는 매개 변수입니다.

UIWebView/WKWebView로` `컨트롤러가 여러 번의 리디렉션을 거치면 애플리케이션이 컨트롤러의 활동을 모니터링하고 사용자가 정의한 특정 사용자 지정 URL을 로드하는 순간을 감지해야 합니다. `ADOBEPASS_REDIRECT_URL `상수(즉, `adobepass://ios.app`). 이 특정 사용자 지정 URL은 실제로 유효하지 않으며 제어자가 실제로 로드하기 위한 것이 아닙니다. 인증 또는 로그아웃 흐름이 완료되었으며 컨트롤러를 닫는 것이 안전하다는 신호로 응용 프로그램에서 해석해야 합니다. 컨트롤러가 이 특정 사용자 지정 URL을 로드하면 애플리케이션이 UIWebView/WKWebView를 닫고 AccessEnabler를 호출해야 합니다. `handleExternalURL:url `API 메서드.

**참고:** 인증 흐름의 경우 사용자가 &quot;뒤로&quot; 단추를 누를 수 있는 지점이며, 이는 인증 흐름의 중단에 해당합니다. 이러한 시나리오에서는 응용 프로그램에서 [setSelectedProvider:](#setSelProv) 방법 전달 **`nil`** 를 매개 변수로 사용하여 AccessEnabler에서 인증 상태 시스템을 재설정할 수 있습니다.

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

**트리거 기준:** [setSelectedProvider:](#setSelProv)



[맨 위로...](#apis)

</br>

#### navigateToUrl:useSVC: {#nav2urlSVC}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명:** AccessEnabler 대신 AccessEnabler에 의해 트리거된 콜백 `navigateToUrl:` 애플리케이션이 이전에 를 통해 수동 SVC(Safari View Controller) 처리를 활성화한 경우의 콜백 [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions) SVC(Safari View Controller)가 필요한 MVPD인 경우에만 를 호출합니다. 다른 모든 MVPD의 경우 `navigateToUrl:` 콜백이 호출됩니다. 다음을 참조하십시오.[iOS SDK 3.2+에서 SFSafariViewController 지원](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) safari SVC(View Controller)를 관리하는 방법에 대한 자세한 내용

와 유사 `navigateToUrl:` 콜백 `navigateToUrl:useSVC:` 는 AccessEnabler에 의해 트리거되어 응용 프로그램에 인스턴스화를 요청합니다. `SFSafariViewController` 콜백에 제공된 URL을 컨트롤러 및 **`url`** 매개 변수. 콜백은 **`url`** 인증 끝점의 URL 또는 로그아웃 끝점의 URL을 나타내는 매개 변수 및 **`useSVC`** 응용 프로그램에서 `SFSafariViewController`.

다음으로: `SFSafariViewController` 컨트롤러는 여러 번의 리디렉션을 거치며, 애플리케이션이 컨트롤러의 활동을 모니터링하고 사용자가 정의한 특정 사용자 지정 URL을 로드하는 순간을 감지해야 합니다. `application's custom scheme` (예: ** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). 이 특정 사용자 지정 URL은 실제로 유효하지 않으며 제어자가 실제로 로드하기 위한 것이 아닙니다. 인증 또는 로그아웃 흐름이 완료되었으며 컨트롤러를 닫는 것이 안전하다는 신호로 응용 프로그램에서 해석해야 합니다. 제어기가 이 특정 사용자 지정 URL을 로드하면 애플리케이션이 `SFSafariViewController` 및 AccessEnabler 호출 `handleExternalURL:url `API 메서드.

**참고:** 인증 흐름의 경우 사용자가 &quot;뒤로&quot; 단추를 누를 수 있는 지점이며, 이는 인증 흐름의 중단에 해당합니다. 이러한 시나리오에서는 응용 프로그램에서 [setSelectedProvider:](#setSelProv) 방법 전달 **`nil`** 를 매개 변수로 사용하여 AccessEnabler에서 인증 상태 시스템을 재설정할 수 있습니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>콜백: SFSafariViewController에서 MVPD 로그인 페이지를 표시합니다.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**가용성:**v 3.2+

**매개 변수**:

* *url:* mvpd의 로그인 페이지를 가리키는 URL
* *useSVC:* url을 SFSafariViewController에 로드할지 여부입니다.

**트리거 기준:**[ setOptions:](#setOptions) 다음 이전 [setSelectedProvider:](#setSelProv)

[맨 위로...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 이 메서드는 응용 프로그램에서 호출하여 인증 또는 로그아웃 흐름을 완료합니다. 이 메서드는 응용 프로그램에서 `UIWebView/WKWebView or SFSafariViewController` 컨트롤러가 특정 사용자 지정 URL로 리디렉션됩니다. 응용 프로그램에서 `SFSafariViewController `컨트롤러 특정 사용자 지정 URL은 `application's custom scheme` (예:`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), 그렇지 않은 경우 이 특정 사용자 지정 URL은 `ADOBEPASS_REDIRECT_URL `상수(즉, `adobepass://ios.app`).

인증 흐름의 경우 AccessEnabler는 백엔드 서버에서 인증 토큰을 검색하여 토큰 저장소에 로컬로 저장하여 흐름을 완료합니다. AccessEnabler는 인증 흐름을 호출하여 애플리케이션에 알립니다. `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> 성공을 나타내는 상태 코드가 1인 콜백입니다. 이 단계를 실행하는 동안 오류가 발생하면 `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> 콜백은 인증 실패와 해당 오류 코드를 나타내는 상태 코드 0으로 트리거됩니다.

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

* *url*: 의 가로챈 URL ` UIWebView/WKWebView or SFSafariViewController ` 컨트롤을 문자열로 표시합니다.


**트리거된 콜백:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[맨 위로...](#apis)

</br>

#### getAuthenticationToken - [더 이상 사용되지 않음] {#getAuthNToken}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 백엔드 서버에서 인증 토큰을 요청하여 인증 흐름을 완료합니다. 이 메서드는 MVPD 로그인 페이지를 호스팅하는 WebView 컨트롤이 로 정의된 사용자 지정 URL로 리디렉션되는 이벤트에 대한 응답으로만 응용 프로그램에서 호출해야 합니다. `ADOBEPASS_REDIRECT_URL` 일정합니다.


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

**가용성:** v1.0+ **종료:** v3.0

**매개 변수:** 없음

**트리거된 콜백:** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[맨 위로...](#apis)

&lt;/br

#### setAuthenticationStatus:errorCode: {#setAuthNStatus}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 응용 프로그램에 인증 흐름의 상태를 알리는 AccessEnabler에 의해 트리거되는 콜백입니다. 사용자의 상호 작용으로 인해 또는 예기치 않은 다른 시나리오(예: 네트워크 연결 문제 등)로 인해 이 플로우가 실패할 수 있는 위치가 많습니다. 이 콜백은 인증 흐름의 성공/실패 상태를 응용 프로그램에 알리는 동시에 필요한 경우 실패 이유에 대한 추가 정보를 제공합니다.

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
* *코드*: 실패 이유. If *상태* 은(는) `ACCESS_ENABLER_STATUS_SUCCESS`, 그런 다음 *코드* 는 빈 문자열(즉, `USER_AUTHENTICATED` 상수). 실패할 경우 이 매개 변수는 다음 값 중 하나를 사용할 수 있습니다.
   * `USER_NOT_AUTHENTICATED_ERROR` - 사용자가 인증되지 않았습니다. 에 대한 응답으로 [checkAuthentication:](#checkAuthN) 로컬 토큰 캐시에 유효한 인증 토큰이 없는 경우 메서드 호출.
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler가 상위 레이어 애플리케이션을 통과한 후 인증 상태 시스템을 재설정했습니다. *null* 끝 [`setSelectedProvider:`](#setSelProv) 을 클릭하여 인증 흐름을 중단합니다.  사용자가 인증 흐름을 취소한 것 같습니다(즉, &quot;뒤로&quot; 단추 누름).
   * `GENERIC_AUTHENTICATION_ERROR` - 네트워크를 사용할 수 없거나 사용자가 인증 흐름을 명시적으로 취소하는 등의 이유로 인증 흐름이 실패했습니다.

**트리거 기준:** ` checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN),` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ)

[맨 위로...](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 이 방법은 애플리케이션에서 사용자가 특정 보호된 리소스를 볼 수 있는 권한이 이미 있는지 확인하는 데 사용됩니다. 이 메서드의 기본 목적은 UI를 장식하는 데 사용할 정보를 검색하는 것입니다 **(예: 잠금 및 잠금 해제 아이콘으로 액세스 상태를 표시).**

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

* *리소스:* 권한 부여를 확인해야 하는 리소스 배열. 목록의 각 요소는 리소스 ID를 나타내는 문자열이어야 합니다. 리소스 ID는 호출에서 리소스 ID와 동일한 제한을 받습니다. 즉, 프로그래머와 MVPD 또는 미디어 RSS 조각 간에 설정된 합의된 값이어야 합니다.

**트리거된 콜백:** [`preauthorizedResources:`](#preauthResources)

[맨 위로...](#apis)

</br>

### checkPreauthorizedResources:cache: {#checkPreauthCache}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 이 방법은 애플리케이션에서 사용자가 특정 보호된 리소스를 볼 수 있는 권한이 이미 있는지 확인하는 데 사용됩니다. 이 메서드의 주요 목적은 UI를 장식하는 데 사용할 정보를 검색하는 것입니다(예: 잠금 및 잠금 해제 아이콘으로 액세스 상태를 표시). 다음 **캐시** 매개 변수는 리소스 확인에 내부 캐시를 사용할지 여부를 제어합니다.

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

* *리소스:* 권한 부여를 확인해야 하는 리소스 배열. 목록의 각 요소는 리소스 ID를 나타내는 문자열이어야 합니다. 리소스 ID에는 의 리소스 ID와 동일한 제한이 적용됩니다. `getAuthorization:` 즉, 프로그래머와 MVPD 간에 설정된 합의된 값 또는 미디어 RSS 조각이어야 합니다.
* *캐시:* 리소스 확인에 내부 캐시를 사용할지 여부를 지정하는 부울. false인 경우 캐시가 무시되어 이 API가 호출될 때마다 서버가 호출됩니다.

**트리거된 콜백:** [`preauthorizedResources:`](#preauthResources)

[맨 위로...](#apis)

</br>

### preauthorizedSources: {#preauthResources}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명:** 콜백 트리거 기준 `checkPreauthorizedResources:`. 사용자가 이미 볼 수 있는 권한이 있는 리소스 목록을 제공합니다.

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

**트리거 기준:** [`checkPreauthorizedResources:`](#checkPreauth)



[맨 위로...](#apis)

</br>

### checkAuthorization:, checkAuthorization:withData: {#checkAuthZ}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 이 메서드는 애플리케이션에서 인증 상태를 확인하는 데 사용됩니다. 먼저 인증 상태를 확인하는 것으로 시작됩니다. 인증되지 않은 경우 [token요청 실패:errorCode:errorDescription:](#tokenReqFailed) callback이 트리거되고 메서드가 종료됩니다. 사용자가 인증되면 권한 부여 플로우도 트리거됩니다. 다음에서 세부 사항 보기 [`getAuthorization:`](#getAuthZ) 메서드를 사용합니다.


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

* *리소스*: 사용자가 인증을 요청하는 리소스의 ID입니다.
* *데이터*: Pay-TV 패스 서비스로 보낼 키-값 쌍으로 구성된 사전입니다. Adobe은 이 데이터를 사용하여 SDK를 변경하지 않고 향후 기능을 활성화할 수 있습니다.

**트리거된 콜백:**
[token요청 실패:errorCode:errorDescription:](#tokenReqFailed)`,setToken:forResource:, sendTrackingData:forEventType:, setAuthenticationStatus:errorCode:`

[맨 위로...](#apis)

</br>

### getAuthorization:, getAuthorization:withData: {#getAuthZ}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 이 메서드는 애플리케이션에서 인증 흐름을 시작하는 데 사용됩니다. 사용자가 아직 인증되지 않은 경우 인증 플로우도 시작합니다. 사용자가 인증되면 AccessEnabler는 인증 토큰(로컬 토큰 캐시에 유효한 인증 토큰이 없는 경우) 및 단기 미디어 토큰에 대한 요청을 계속 발행합니다. 짧은 미디어 토큰을 얻으면 인증 흐름이 완료된 것으로 간주됩니다. 다음 [setToken:forResource:](#setToken) 콜백이 트리거되고 짧은 미디어 토큰이 애플리케이션에 매개 변수로 전달됩니다. 어떤 이유로든 승인이 실패하면 [token요청 실패:forEventType:](#tokenReqFailed) 콜백이 트리거되고 오류 코드/세부 정보가 제공됩니다.

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

* *리소스*: 사용자가 인증을 요청하는 리소스의 ID입니다.
* *데이터*: Pay-TV 패스 서비스로 보낼 키-값 쌍으로 구성된 사전입니다. Adobe은 이 데이터를 사용하여 SDK를 변경하지 않고 향후 기능을 활성화할 수 있습니다.

**트리거된 콜백:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**추가 콜백이 트리거됨:**\
이 메서드는 다음 콜백을 트리거할 수도 있습니다(인증 플로우도 시작된 경우). `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

**참고: checkAuthorization: / checkAuthorization을 사용하십시오.:withData: getAuthorization: / getAuthorization 대신:withData: 가능한 한. getAuthorization: / getAuthorization:withData: 메서드는 전체 인증 흐름을 시작하고(사용자가 인증되지 않은 경우) 이로 인해 프로그래머측의 구현이 복잡해질 수 있습니다.**

[맨 위로...](#apis)

</br>

### setToken:forResource: {#setToken}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 인증 흐름이 성공적으로 완료되었음을 응용 프로그램에 알리는 AccessEnabler에 의해 트리거된 콜백입니다. 수명이 짧은 미디어 토큰도 매개 변수로 제공됩니다.


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

* *토큰*: 수명이 짧은 미디어 토큰
* *리소스*: 인증을 받은 리소스

**트리거 기준:** [checkAuthorization:](#checkAuthZ)` , `[checkAuthorization:withData:](#checkAuthZ),` `[getAuthorization:](#getAuthZ), [getAuthorization:withData:](#getAuthZ)

[맨 위로...](#apis)

</br>

### token요청 실패:errorCode:errorDescription: {#tokenReqFailed}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 인증 플로우가 실패했음을 상위 계층 응용 프로그램에 알리는 AccessEnabler에 의해 트리거되는 콜백입니다.

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

* *리소스*: 인증을 받은 리소스입니다.
* *코드*: 실패 시나리오와 연결된 오류 코드. 가능한 값:
   * `USER_NOT_AUTHORIZED_ERROR` - 사용자가 지정된 리소스에 대해 권한을 부여할 수 없음
* *설명*: 실패 시나리오에 대한 추가 세부 정보. 어떤 이유로든 이 설명 문자열을 사용할 수 없는 경우 Adobe Pass 인증이 빈 문자열을 보냅니다 **(&quot;&quot;)**.\
  이 문자열은 MVPD에서 사용자 지정 오류 메시지 또는 판매 관련 메시지를 전달하는 데 사용할 수 있습니다. 예를 들어 구독자가 리소스에 대한 권한 부여를 거부하면 MVPD가 다음과 같은 메시지를 보낼 수 있습니다. &quot;현재 패키지의 이 채널에 대한 액세스 권한이 없습니다. 패키지를 업그레이드하려면 **여기**.&quot; 이 콜백을 통해 Adobe Pass Authentication에서 메시지를 표시하거나 무시할 수 있는 옵션이 있는 프로그래머에게 전달합니다. Adobe Pass 인증은 이 매개 변수를 사용하여 오류가 발생했을 수 있는 조건에 대한 알림을 제공할 수도 있습니다. 예를 들어 &quot;공급자의 인증 서비스와 통신하는 동안 네트워크 오류가 발생했습니다.&quot;와 같습니다.

**트리거 기준:** ` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ), `getAuthorization:, `[getAuthorization:withData:](#getAuthZ)

[맨 위로...](#apis)

</br>

### 로그아웃 {#logout}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 이 메서드는 애플리케이션에서 호출하여 로그아웃 흐름을 시작합니다. 로그아웃은 사용자가 Adobe Pass 인증 서버와 MVPD의 서버 모두에서 로그아웃해야 하므로 일련의 HTTP 리디렉션 작업의 결과입니다. AccessEnabler 라이브러리에서 발급한 간단한 HTTP 요청으로는 이 흐름을 완료할 수 없으므로 `UIWebView/WKWebView or SFSafariViewController` http 리디렉션 작업을 수행하려면 컨트롤러를 인스턴스화해야 합니다.

로그아웃 흐름은 사용자가 와 상호 작용할 필요가 없다는 점에서 인증 흐름과 다릅니다 `UIWebView/WKWebView or SFSafariViewController`  어떤 식으로든 컨트롤러입니다. 따라서 Adobe은 로그아웃 프로세스 중에 컨트롤을 보이지 않게(즉, 숨겨지도록) 만들 것을 권장합니다.

인증 흐름과 유사한 패턴이 사용됩니다. iOS AccessEnabler가 `navigateToUrl:` 콜백 또는 `navigateToUrl:useSVC:` 을(를) 만들려면 `UIWebView/WKWebView or SFSafariViewController` 콜백에 제공된 URL을 컨트롤러 및 `url` 매개 변수. 백엔드 서버에 있는 로그아웃 끝점의 URL입니다. tvOS AccessEnabler 의 경우 `navigateToUrl:` 콜백 또는 `navigateToUrl:useSVC:` callback이 호출됩니다.

여러 리디렉션을 거치는 동안 애플리케이션이 의 활동을 모니터링해야 합니다. `UIWebView/WKWebView or SFSafariViewController `특정 사용자 지정 URL을 로드하는 순간을 제어하고 감지합니다. 이 특정 사용자 지정 URL은 실제로 유효하지 않으며 제어자가 실제로 로드하기 위한 것이 아닙니다. 응용 프로그램에서 로그아웃 흐름이 완료되었으며 컨트롤러를 닫는 것이 안전하다는 신호로만 해석해야 합니다. 컨트롤러가 특정 사용자 정의 URL을 로드하면 애플리케이션이 컨트롤러를 닫고 AccessEnabler를 호출해야 합니다 `handleExternalURL:url `API 메서드. 응용 프로그램에서 `SFSafariViewController `컨트롤러 특정 사용자 지정 URL은 `application's custom scheme` (예:`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), 그렇지 않은 경우 이 특정 사용자 지정 URL은 `ADOBEPASS_REDIRECT_URL `상수(즉, `adobepass://ios.app`).

그러면 AccessEnabler에서 [`setAuthenticationStatus()`](#setAuthNStatus) 로그아웃 흐름의 성공을 나타내는 상태 코드가 0인 콜백입니다.

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

**트리거된 콜백:** `navigateToUrl:, `[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[맨 위로...](#apis)

</br>

### getSelectedProvider {#getSelProv}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 이 메서드를 사용하여 현재 선택한 공급자를 결정합니다.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 호출: 현재 선택한 MVPD를 확인합니다.</th>
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

**트리거 기준:** [`getSelectedProvider`](#getSelProv)

[맨 위로...](#apis)

</br>

### getMetadata: {#getMeta}

**파일:** AccessEnabler/headers/AccessEnabler.h

**설명:** 이 방법을 사용하면 AccessEnabler 라이브러리에서 메타데이터로 노출된 정보를 검색할 수 있습니다. 애플리케이션은 사전 기반의 제공을 통해 이 데이터에 액세스할 수 있습니다 *key* 입력 매개 변수.

프로그래머가 사용할 수 있는 메타데이터에는 두 가지 유형이 있습니다.

* 정적 메타데이터(인증 토큰 TTL, 인증 토큰 TTL 및 장치 ID)
* 사용자 메타데이터(사용자 ID, 우편 번호와 같은 사용자별 정보, 인증 및 권한 부여 흐름 동안 MVPD에서 사용자의 장치로 전달될 수 있음)

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

* *키 사전*: 다음 형식의 사전 데이터 구조:
   * 키가 `METADATA_OPCODE_KEY` 및 값: `METADATA_AUTHENTICATION`그런 다음 인증 토큰 만료 시간을 얻기 위해 쿼리가 수행됩니다.
   * 키가 `METADATA_OPCODE_KEY` 및 값: `METADATA_AUTHORIZATION` **및**\
     키: `METADATA_RESOURCE_ID_KEY` 값이 특정 리소스 ID이면 지정된 리소스와 연결된 인증 토큰의 만료 시간을 얻기 위해 쿼리가 수행됩니다.
   * 키가 `METADATA_OPCODE_KEY` 및 값: `METADATA_DEVICE_ID`그런 다음 현재 장치 id를 얻기 위해 쿼리가 수행됩니다. 이 기능은 기본적으로 비활성화되어 있으며 프로그래머는 사용 권한 및 요금에 대한 정보를 Adobe에 문의해야 합니다.
   * 키가 `METADATA_OPCODE_KEY` 및 값: `METADATA_USER_META` **및** 키: `METADATA_USER_META_KEY` 그리고 값은 메타데이터의 이름이며, 그러면 사용자 메타데이터에 대해 쿼리가 수행됩니다. 사용 가능한 사용자 메타데이터 유형 목록:
      * `zip` - 우편 번호 목록
      * `householdID` - 세대 식별자. MVPD가 하위 계정을 지원하지 않는 경우에는 다음과 같습니다. `userID`.
      * `maxRating` - 사용자에 대한 최대 자녀 보호 등급 컬렉션
      * `userID` - 사용자 식별자. MVPD가 하위 계정을 지원하고 사용자가 주 계정이 아닌 경우 `userID` 이(가) 다음과 다름 `householdID.`
      * `channelID` - 사용자가 볼 수 있는 채널 목록입니다.

  >[!NOTE]
  >
  >프로그래머가 사용할 수 있는 실제 사용자 메타데이터는 MVPD가 사용할 수 있도록 하는 내용에 따라 다릅니다. 이 목록은 새 메타데이터를 사용할 수 있고 Adobe Pass 인증 시스템에 추가됨에 따라 확장됩니다.

**트리거된 콜백:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**추가 정보:** [사용자 메타데이터](/help/authentication/user-metadata.md)

[맨 위로...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 호출 후 AccessEnabler에 의해 트리거된 콜백[getAuthentication()](#getAuthN) 현재 요청자가 SSO를 지원하는 하나 이상의 MVPD를 지원하는 경우.

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

**트리거 기준:** [`getAuthentication`](#getAuthN)

**추가 정보:** [iOS/tvOS 단일 사인온](#presentTvDialog)

[맨 위로...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 사용자가 Apple SSO 대화 상자를 닫은 후 AccessEnabler에 의해 트리거된 콜백입니다.

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

**트리거 기준:** 사용자 작업

**추가 정보:** [iOS/tvOS 단일 사인온](#presentTvDialog)

[맨 위로...](#apis)

</br>

### setMetaStatus:encrypted:forKey:andArguments: {#setMetaStatus}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 다음을 통해 요청된 메타데이터를 전달하는 AccessEnabler에 의해 트리거된 콜백 [`getMetadata:`](#getMeta) 호출합니다.

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

* *메타데이터*: 요청한 메타데이터. 이 값은 `NSString` 정적 메타데이터(인증 TTL, 권한 부여 TTL, 장치 ID)의 경우.  사용자별 메타데이터를 요청할 때 복잡한 개체입니다. 이 복잡한 객체는 일반적으로 JSON 페이로드의 Objective-C 표현입니다(예: &#39;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;buildings&quot;). [&quot;150&quot;, &quot;320&quot;]}&#39;는 Objective-C에서 NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;buildings&quot; -> NSArray(&quot;150&quot;, &quot;320&quot;))로 번역됩니다.   샘플 메타데이터 JSON 개체:

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

* *암호화됨*: 검색된 메타데이터의 암호화 여부를 지정하는 부울 값입니다. 이 매개 변수는 사용자 메타데이터 요청에만 중요하며 항상 암호화되지 않은 상태로 수신되는 정적 메타데이터(예: 인증 TTL)에는 의미가 없습니다. 이 매개 변수가 true로 설정되면 프로그래머가 화이트리스트에 있는 개인 키(에서 요청자 ID 서명에 사용되는 동일한 개인 키)를 사용하여 RSA 암호 해독을 수행하여 암호화되지 않은 사용자 메타데이터 값을 가져옵니다. [`setRequestor:setSignedRequestorId:`](#setReq) 및 `setRequestor:setSignedRequestorId:serviceProviders: `호출).

* *key*: 메타데이터 검색 요청을 작성하는 데 사용되는 키입니다.

* *인수*: (으)로 전달된 동일한 사전 [`getMetadata:`](#getMeta) 호출합니다. 애플리케이션이 요청을 응답과 일치시킬 수 있도록 하기 위해 제공됩니다.

**트리거 기준:** [`getMetadata:`](#getMeta)

**추가 정보:** [사용자 메타데이터](/help/authentication/user-metadata.md)


[맨 위로...](#apis)

</br>

### MVPD {#mvpd}

**파일:** AccessEnabler/headers/model/MVPD.h

**설명** MVPD 개체에 대해 설명합니다. MVPD의 속성에 대한 정보를 얻는 데 사용할 수 있습니다.

**가용성:** v1.0+ [boardingStatus 속성은 v2.2에서 사용할 수 있습니다.]

**속성**:

* (NSString) ID - MVPD ID입니다.
* (NSString) displayName - MVPD 이름입니다. [선택기에 표시하는 데 사용해야 합니다.]
* (NSString) logoURL - MVPD 로고 주소.
* (BOOL) enablePlatformServices - true인 경우 MVPD는 다음과 같은 SSO 서비스를 지원합니다. [APPLE SSO](#presentTvDialog).
* (NSString) boardingStatus - 3개의 값을 가질 수 있습니다.
   * nil - MVPD가 Apple SSO를 지원하지 않습니다.
   * 선택기 - MVPD가 Apple 선택기에 나타날 수 있지만 인증 흐름은 Adobe에 의해 수행됩니다.
   * 지원됨 - MVPD는 Apple에서 완전히 지원되며 Apple의 SSO 토큰을 사용하게 됩니다.

[맨 위로...](#apis)

</br>

## 이벤트 추적 {#tracking}

AccessEnabler는 자격 흐름과 관련이 없는 추가 콜백을 트리거합니다. 구현 [`sendTrackingData()`](#sendTracking) callback 함수는 선택 사항이지만 응용 프로그램에서 특정 이벤트를 추적하고 인증/권한 부여 시도 성공/실패 횟수와 같은 통계를 컴파일할 수 있도록 합니다.

### sendTrackingData:forEventType: {#sendTracking}

**파일:** AccessEnabler/headers/EntitlementDelegate.h

**설명** 인증/인증 흐름의 완료/실패와 같은 다양한 이벤트 발생에 대해 애플리케이션에 보내는 AccessEnabler 신호를 통해 트리거되는 콜백입니다. Adobe Pass Authentication 1.6에서는 디바이스 유형, AccessEnabler 클라이언트 유형 및 운영 체제를 보고합니다. [`sendTrackingData()`](#sendTracking). 다음 [`sendTrackingData()`](#sendTracking) 콜백은 이전 버전과 호환됩니다.

**콜백: 이벤트 추적**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**가용성:** v1.0+

**참고:** 장치 유형 및 운영 체제는 공개 Java 라이브러리 ( )를 사용하여 파생됩니다.<http://java.net/projects/user-agent-utils>)와 사용자 에이전트 문자열. 이 정보는 운영 지표를 장치 범주로 분류하는 거친 방법으로만 제공되지만, 잘못된 결과에 대해서는 Adobe이 책임을 지지 않을 수 있습니다. 그에 따라 새로운 기능을 사용하십시오.

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

* *이벤트*: 추적 중인 이벤트 코드입니다. 가능한 추적 이벤트 유형에는 세 가지가 있습니다.
   * **authorizationDetection:** 인증 토큰 요청이 반환될 때마다(이벤트는 `TRACKING_AUTHORIZATION`)
   * **authenticationDiscovery:** 인증 확인이 발생할 때마다(이벤트는 `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** 사용자가 MVPD 선택 양식에서 MVPD를 선택하면(이벤트는 다음과 같음) `TRACKING_GET_SELECTED_PROVIDER`)
* *데이터*: 보고된 이벤트와 관련된 추가 데이터입니다. 이 데이터는 값 목록 형태로 표시됩니다.

**트리거 기준:** `checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN), `checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ), `getAuthorization:, `[getAuthorization:withData:](#getAuthZ), `setSelectedProvider:`

의 값 해석을 위한 지침 *데이터* 배열:

* TrackingEventType용 `TRACKING_AUTHENTICATION:`
   * **0** - 토큰 요청의 성공 여부(true/false) 및 성공 여부:
   * **1** - MVPD ID 문자열
   * **2** - GUID(md5 해시됨)
   * **3** - 토큰이 이미 캐시에 있습니다(true/false).
   * **4** - 장치 유형
   * **5** - AccessEnabler 클라이언트 유형
   * **6** - 운영 체제 유형

* TrackingEventType용 `TRACKING_AUTHORIZATION:`
   * **0** - 토큰 요청의 성공 여부(true/false) 및 성공 여부:
   * **1** - MVPD ID
   * **2** - GUID(md5 해시됨)
   * **3** - 토큰이 이미 캐시에 있습니다(true/false).
   * **4** - 오류
   * **5** - 세부 정보
   * **6** - 장치 유형
   * **7** - AccessEnabler 클라이언트 유형
   * **8** - 운영 체제 유형
* TrackingEventType용 `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - 현재 선택한 MVPD의 ID
   * **1** - 장치 유형
   * **2** - AccessEnabler 클라이언트 유형
   * **3** - 운영 체제 유형

</br>
