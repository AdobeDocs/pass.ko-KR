---
title: REST API V2 Cookbook(서버 간)
description: REST API V2 Cookbook(서버 간)
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '1578'
ht-degree: 0%

---

# REST API V2 Cookbook(서버 간) {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> REST API V2 구현은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md) 설명서에 의해 제한됩니다.

이 Cookbook 문서의 목적은 서버 간 아키텍처에서 REST API V2를 사용하여 Adobe Pass 인증을 구현하기 위한 모범 사례를 자세히 설명하는 것입니다. 프로덕션 환경 및 운영에 대한 기본 요구 사항, 단계별 흐름 구현 및 일반적인 고려 사항을 제공합니다.

## 구성 요소 {#components}

작동하는 서버 간 솔루션에서는 다음 구성 요소가 포함됩니다.

| 유형 | 구성 요소 | 설명 |
|---------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 스트리밍 장치 | 스트리밍 앱 | 사용자의 스트리밍 장치에 상주하며 인증된 비디오를 재생하는 프로그래머 애플리케이션. |
|                           | \[선택 사항\] AuthN 모듈 | 스트리밍 장치에 사용자 에이전트(즉, 웹 브라우저)가 있는 경우 AuthN 모듈은 MVPD IdP에서 사용자를 인증합니다. |
| \[선택 사항\] AuthN 장치 | Autn 앱 | 스트리밍 장치에 사용자 에이전트(즉, 웹 브라우저)가 없는 경우, AuthN 애플리케이션은 웹 브라우저를 사용하여 별도의 사용자의 장치에서 액세스되는 프로그래머 웹 애플리케이션입니다. |
| 프로그래머 인프라 | 프로그래머 서비스 | 인증 및 권한 부여 결정을 얻기 위해 스트리밍 장치와 Adobe Pass 서비스를 연결하는 서비스입니다. |
| Adobe 인프라 | Adobe Pass 서비스 | MVPD IdP 및 AuthZ 서비스와 통합되고 인증 및 권한 부여 결정을 제공하는 서비스입니다. |
| MVPD 인프라 | MVPD IdP | 사용자의 ID를 확인하기 위해 자격 증명 기반 인증 서비스를 제공하는 MVPD 종단점입니다. |
|                           | MVPD AuthZ 서비스 | 사용자의 구독, 자녀 보호 등에 따라 권한 부여 결정을 제공하는 MVPD 종단점입니다. |

플로우에 사용되는 추가 용어는 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)에 정의되어 있습니다.

다음 다이어그램은 전체 흐름을 보여 줍니다.

![REST API V2 Cookbook(서버 간)](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

### 서버 간 아키텍처에서 REST API V2를 구현하는 단계 {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

Adobe Pass REST API V2를 구현하려면 단계로 그룹화된 아래 단계를 수행해야 합니다.

## 0. 전제 조건 {#prerequisites}

서버 간 구현에서 스트리밍 앱 및 프로그래머 서비스는 프로그래머 서비스가 다음을 수행할 수 있도록 프로토콜을 설정해야 합니다.

* 장치에서 스트리밍 앱을 고유하게 식별합니다.
* 스트리밍 앱을 대신하여 활동하며 Adobe Pass 서비스와 통신합니다.
* IP 주소, 소스 포트, 장치 정보와 같은 스트리밍 앱 및 장치에 대한 정보를 수집하고 저장하여 Adobe Pass에 전달합니다.
* 스트리밍 앱에 의사 결정 및 지침을 반환합니다.

장치 ID, 클라이언트 ID, 클라이언트 암호(아래에 정의됨)와 같은 매개 변수는 스트리밍 앱 또는 프로그래머 서비스에 저장할 수 있습니다.

## A. 등록 단계 {#registration-phase}

### 1단계: 애플리케이션 등록 {#step-1-register-your-application}

애플리케이션이 Adobe Pass REST API V2를 호출하려면 API 보안 계층에 필요한 액세스 토큰이 필요합니다.

서버 간 구현의 경우 프로그래머 서비스는 애플리케이션 인스턴스를 대신하여 등록할 수 있지만 각 스트리밍 장치에 대해 클라이언트 자격 증명(클라이언트 ID 및 클라이언트 암호) 값을 얻어야 합니다.

액세스 토큰을 가져오려면 프로그래머 서비스가 스트리밍 앱을 대신하여 작동할 수 있으며 [동적 클라이언트 등록](../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) 설명서에 설명된 단계를 따라야 합니다.

## B. 인증 단계 {#authentication-phase}

### 2단계: 인증된 기존 프로필 확인 {#step-2-check-for-existing-authenticated-profiles}

프로그래머 서비스는 스트리밍 앱 대신 기존 인증된 프로필을 확인합니다. `/api/v2/{serviceProvider}/profiles`([인증된 프로필 검색](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)).

* 프로필을 찾을 수 없고 스트리밍 애플리케이션이 TempPass 플로우를 구현하는 경우
   * [임시 액세스 흐름](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)을 구현하는 방법에 대한 설명서를 따르십시오.
* 프로필을 찾을 수 없는 경우 스트리밍 애플리케이션은 인증 흐름을 구현합니다
   * <b>2단계.a:</b> 프로그래머 서비스에서 serviceProvider에 사용할 수 있는 MVPD 목록을 검색합니다. <b>/api/v2/{serviceProvider}/configuration</b><br>
([사용 가능한 MVPD 목록 검색](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * 프로그래머 서비스는 MVPD의 목록에 필터링을 구현하고 다른 항목을 숨기고 의도한 MVPD만 표시할 수 있습니다(TempPass, 테스트 MVPD, 개발 중인 MVPD 등)
   * 프로그래머 서비스는 스트리밍 앱에서 선택기를 표시할 필터링된 MVPD 목록을 반환해야 합니다. 사용자가 MVPD을 선택합니다
   * 스트리밍 앱에서 선택한 MVPD을 사용하여 프로그래머 서비스에서 세션을 만듭니다. <b>/api/v2/{serviceProvider}/세션</b><br>
([인증 세션 만들기](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * 인증에 사용할 코드 및 URL이 반환됩니다
      * 프로필이 있으면 프로그래머 서비스가 <a href="#preauthorization-phase">C로 진행할 수 있습니다. 사전 인증 단계</a>
   * 프로그래머 서비스는 코드 및 URL을 스트리밍 앱에 반환해야 합니다

### 3단계: 사용자 인증 {#step-3-authenticate-the-user}

브라우저 또는 두 번째 화면 웹 기반 애플리케이션 사용:

* 옵션 1. 스트리밍 앱은 브라우저나 웹 보기를 열고 인증할 URL을 로드할 수 있으며 사용자가 자격 증명을 제출해야 하는 MVPD 로그인 페이지에 도달합니다
   * 사용자가 로그인/암호 입력, 최종 리디렉션에 성공 페이지가 표시됨
* 옵션 2. 스트리밍 앱은 브라우저를 열 수 없으며 코드만 표시합니다. <b>사용자에게 코드, 빌드 및 URL 열기(<b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>)를 입력하도록 요청하려면 별도의 웹 응용 프로그램인 AuthN_APP을 개발해야 합니다</b>
   * 사용자가 로그인/암호 입력, 최종 리디렉션에 성공 페이지가 표시됨

### 4단계: 인증된 프로필 확인 {#step-4-check-for-authenticated-profiles}

프로그래머 서비스는 브라우저 또는 두 번째 화면에서 완료하기 위해 MVPD과의 인증을 확인합니다

* <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>에서 15초마다 폴링하는 것이 좋습니다.
([특정 MVPD에 대해 인증된 프로필 검색](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * 두 번째 화면 애플리케이션에 MVPD 선택기가 표시되므로 MVPD이 스트리밍 애플리케이션에서 선택되지 않은 경우 CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>에서 폴링이 수행되어야 합니다
([특정 코드에 대해 인증된 프로필 검색](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* 폴링은 30분을 초과할 수 없습니다. 30분에 도달하고 스트리밍 애플리케이션이 여전히 활성 상태인 경우 새 세션을 시작해야 하며 새 코드와 URL이 반환됩니다
* 인증이 완료되면 인증된 프로필로 반환은 200입니다
* 프로그래머 서비스가 <a href="#preauthorization-phase">C(으)로 진행될 수 있습니다. 사전 인증 단계</a>

## C. 사전 인증 단계 {#preauthorization-phase}

### 5단계: 사전 승인된 리소스 확인 {#step-5-check-for-preauthorized-resources}

사용자에 대한 유효한 인증 프로필이 있는 프로그래머 서비스에서는 사용 가능한 비디오에 대한 액세스를 확인하고 목록을 스트리밍 애플리케이션에 전달하여 표시할 수 있습니다.

* 응용 프로그램에서 인증된 사용자 패키지에서 사용할 수 없는 리소스를 필터링하려는 경우 단계는 선택 사항이며 실행됩니다
* <b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br> 호출
([특정 MVPD을 사용하여 사전 권한 부여 결정 검색](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))

## D. 인증 단계 {#authorization-phase}

### 6단계: 승인된 리소스 확인 {#step-6-check-for-authorized-resources}

스트리밍 앱은 사용자가 선택한 비디오/에셋/리소스를 재생할 준비를 합니다.

* 모든 재생 시작에 단계가 필요합니다.
* 스트리밍 앱은 이 정보를 프로그래머 서비스에 전달합니다.
* 스트리밍 앱 대신 프로그래머 서비스에서 <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>을(를) 호출하십시오.
([특정 MVPD을 사용하여 권한 부여 결정 검색](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * decision = &#39;Permit&#39;, Programmer Service가 스트리밍 앱에서 스트리밍을 시작하도록 지시합니다.
   * decision = &#39;Deny&#39;, Programmer Service는 사용자에게 해당 비디오에 대한 액세스 권한이 없음을 알리도록 스트리밍 앱에 지시합니다.
   * 이 과정에서 프로그래머 서비스는 다른 비즈니스 규칙을 평가하고 스트리밍 앱에 적절한 결정을 반환할 수 있습니다

## E. 로그아웃 단계 {#logout-phase}

### 7단계: 로그아웃 {#step-7-logout}

스트리밍 앱: 사용자가 MVPD에서 로그아웃하려고 합니다.

* 스트리밍 앱은 이 특정 앱에 대해 MVPD에서 로그아웃해야 한다고 프로그래머 서비스에 알립니다.
* 프로그래머 서비스는 인증된 사용자에 대해 저장하는 정보를 정리할 수 있다
* 프로그래머 서비스 호출 <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([특정 MVPD에 대한 로그아웃 시작](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* response actionType=&#39;interactive&#39; 및 url이 있으면 프로그래머 서비스는 url을 스트리밍 앱으로 반환합니다
* 기존 기능에 따라 스트리밍 앱은 브라우저에서 URL을 열 수 있습니다(일반적으로 인증에 사용되는 것과 동일한 것)
* 스트리밍 앱에 브라우저가 없거나 인증 시 인스턴스와 다른 경우 MVPD 세션이 브라우저 캐시에서 지속되지 않았으므로 흐름을 중단할 수 있습니다.

## 환경 및 기능 요구 사항{#environments}

프로그래머는 프로덕션 환경과 스테이징 환경을 두 개 이상 만들어야 합니다.

### 프로덕션

프로덕션 환경은 가용성이 높고 큰 스파이크나 예기치 않은 스파이크(예: 라이브 스포츠, 최신 뉴스)에 맞게 확장되어야 합니다.

Adobe Pass 서비스는 미국 전역에 지리적으로 분산된 여러 데이터 센터에서 실행됩니다. Adobe Pass 서비스에서 최고의 응답 시간(즉, 가장 낮은 지연 시간)을 달성하려면 프로그래머는 지리적으로 분산된 유사한 서비스 인프라도 만들어야 합니다.

프로그래머 서비스는 Adobe이 트래픽 경로를 다시 지정해야 하는 경우 DNS 캐시를 최대 30초로 제한해야 합니다. 이 문제는 데이터 센터를 사용할 수 없는 경우 발생할 수 있습니다.

프로그래머는 프로덕션 환경의 공용 IP 범위를 제공해야 합니다. 이러한 IP는 Adobe Pass 인프라의 허용 목록에 입력되어 액세스하고 Adobe의 API 페어 사용 정책에 의해 관리됩니다.

### 스테이징

스테이징 환경은 최소한일 수 있지만 모든 시스템 구성 요소와 비즈니스 논리를 포함해야 합니다. 프로덕션과 유사하게 작동하고 프로덕션 외부에서 릴리스를 테스트할 수 있도록 해야 합니다. 이상적으로 스테이징 환경은 프로그래머가 사용하기 위해 Adobe Pass 테스트 환경에 연결할 수 있고 필요할 때 Adobe을 통해 연결할 수 있으므로 테스트 및 문제 해결에 도움이 될 수 있습니다.

### 기능 요구 사항

프로그래머 서비스는 플로우를 실행하는 장치에 대한 정확한 장치 식별 정보를 전달해야 합니다. 또한 프로그래머 서비스는 연결 소스 포트(장치 정보 필드)와 함께 흐름을 실행할 장치의 IP(x-forwarded-for 헤더에서)를 전달해야 합니다.

프로그래머 서비스는 개별 MVPD 또는 통합 앱에 필요한 데이터 및 형식을 전송해야 합니다(예: 장치 IP, 소스 포트, 장치 정보, MRSS, ECID와 같은 선택적 데이터). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.

프로그래머 서비스는 캐싱 시 인증 프로필 및 결정 유효성을 준수하고 알림 시 인증 또는 결정을 무효화해야 합니다.

프로그래머 서비스는 암호화된 사용자 메타데이터의 경우 Adobe과 공유된 인증서를 유지 관리해야 합니다.

## 관련 정보 {#related}

* [REST API V2 참조](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
