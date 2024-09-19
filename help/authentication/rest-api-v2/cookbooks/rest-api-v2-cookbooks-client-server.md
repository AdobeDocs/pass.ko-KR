---
title: REST API V2 Cookbook(클라이언트-서버)
description: REST API V2 Cookbook(클라이언트-서버)
source-git-commit: 837276ce85445da5c3877592b194e37adf35fa32
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 0%

---


# REST API V2 Cookbook(클라이언트-서버) {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> REST API V2 구현은 [조절 메커니즘](/help/authentication/throttling-mechanism.md) 설명서에 의해 제한됩니다.

## 클라이언트측 애플리케이션에서 REST API V2를 구현하는 절차 {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

Adobe Pass REST API V2를 구현하려면 단계로 그룹화된 아래 단계를 수행해야 합니다.

## A. 등록 단계 {#registration-phase}

### 1단계: 애플리케이션 등록 {#step-1-register-your-application}

애플리케이션이 Adobe Pass REST API V2를 호출하려면 API 보안 계층에 필요한 액세스 토큰이 필요합니다.

액세스 토큰을 가져오려면 응용 프로그램에서 [Dynamic Client Registration](./dynamic-client-registration.md)에 설명된 단계를 수행해야 합니다.

## B. 인증 단계 {#authentication-phase}

### 2단계: 인증된 기존 프로필 확인 {#step-2-check-for-existing-authenticated-profiles}

스트리밍 응용 프로그램에서 기존 인증된 프로필을 확인합니다. <b>/api/v2/{serviceProvider}/profiles</b><br>
([인증된 프로필 검색](./apis/profiles-apis/rest-api-v2-retrieve-authenticated-profiles.md))

* 프로필을 찾을 수 없고 스트리밍 애플리케이션이 TempPass 플로우를 구현하는 경우
   * [임시 액세스 흐름](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)을 구현하는 방법에 대한 설명서를 따르십시오.
* 프로필을 찾을 수 없는 경우 스트리밍 애플리케이션은 인증 흐름을 구현합니다.
   * 스트리밍 응용 프로그램이 serviceProvider에 사용할 수 있는 MVPD 목록을 검색합니다. <b>/api/v2/{serviceProvider}/구성</b><br>
([사용 가능한 MVPD 목록 검색](./apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * 스트리밍 애플리케이션은 MVPD의 목록에 필터링을 구현하고 다른 항목을 숨기고 의도한 MVPD만 표시할 수 있습니다(TempPass, 테스트 MVPD, 개발 중인 MVPD 등)
   * 스트리밍 애플리케이션 디스플레이 선택기, 사용자가 MVPD를 선택합니다.
   * 스트리밍 응용 프로그램에서 세션을 만듭니다. <b>/api/v2/{serviceProvider}/세션</b><br>
([인증 세션 만들기](./apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * 인증에 사용할 코드 및 URL이 반환됩니다
      * 프로필이 발견되면 스트리밍 응용 프로그램이 <a href="#preauthorization-phase">C로 진행될 수 있습니다. 사전 인증 단계</a> 또는 <a href="#authorization-phase">D. 인증 단계</a>

### 3단계: 사용자 인증 {#step-3-authenticate-the-user}

브라우저 또는 두 번째 화면 웹 기반 애플리케이션 사용:

* 옵션 1. 스트리밍 애플리케이션은 브라우저 또는 웹 보기를 열고 인증할 URL을 로드하며 사용자는 자격 증명을 제출해야 하는 MVPD 로그인 페이지에 도달합니다
   * 사용자 로그인/암호 입력, 최종 리디렉션에 성공 페이지가 표시됨
* 옵션 2. 스트리밍 애플리케이션은 브라우저를 열 수 없으며 CODE만 표시합니다. <b>코드, 빌드 및 열기 URL(<b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>)을 입력하도록 사용자에게 요청하려면 별도의 웹 응용 프로그램을 개발해야 합니다</b>
   * 사용자 로그인/암호 입력, 최종 리디렉션에 성공 페이지가 표시됨

### 4단계: 인증된 프로필 확인 {#step-4-check-for-authenticated-profiles}

스트리밍 애플리케이션은 브라우저 또는 두 번째 화면에서 완료하기 위해 MVPD를 사용한 인증을 확인합니다.

* <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>에서 15초마다 폴링하는 것이 좋습니다.
([특정 MVPD에 대해 인증된 프로필 검색](.apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * MVPD 선택기가 두 번째 화면 애플리케이션에 표시되므로 스트리밍 애플리케이션에서 MVPD를 선택하지 않으면 코드 <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>에서 폴링이 발생합니다
([특정 코드에 대해 인증된 프로필 검색](./apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* 폴링은 30분을 초과할 수 없습니다. 30분에 도달하고 스트리밍 애플리케이션이 여전히 활성 상태인 경우 새 세션을 시작해야 하며 새 코드와 URL이 반환됩니다
* 인증이 완료되면 인증된 프로필로 반환은 200입니다.
* 스트리밍 응용 프로그램이 <a href="#preauthorization-phase">C로 진행될 수 있습니다. 사전 인증 단계</a> 또는 <a href="#authorization-phase">D. 인증 단계</a>

## C. 사전 인증 단계 {#preauthorization-phase}

### 5단계: 사전 승인된 리소스 확인 {#step-5-check-for-preauthorized-resources}

스트리밍 애플리케이션은 인증된 사용자가 사용할 수 있는 비디오를 표시하도록 준비하며 다음을 확인할 수 있습니다.
이러한 리소스에 대한 액세스 권한.

* 애플리케이션이 인증된 사용자 패키지에서 사용할 수 없는 리소스를 필터링하려는 경우 단계는 선택 사항이며 실행됩니다
* <b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br> 호출
([특정 MVPD를 사용하여 사전 권한 부여 결정 검색](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))

## D. 인증 단계 {#authorization-phase}

### 6단계: 승인된 리소스 확인 {#step-6-check-for-authorized-resources}

스트리밍 애플리케이션은 사용자가 선택한 비디오/에셋/리소스의 재생을 준비한다.

* 모든 재생 시작에 단계가 필요합니다.
* <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br> 호출
([특정 MVPD를 사용하여 권한 부여 결정 검색](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * 결정 = &#39;허용&#39; , 스트리밍 장치 스트리밍 시작
   * decision = &#39;거부&#39;, 스트리밍 장치는 사용자에게 해당 비디오에 대한 액세스 권한이 없다고 알립니다.

## E. 로그아웃 단계 {#logout-phase}

### 7단계: 로그아웃 {#step-7-logout}

스트리밍 장치: 사용자가 MVPD에서 로그아웃하려고 함

* <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br> 호출
([특정 MVPD에 대한 로그아웃 시작](.apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* response actionType=&#39;interactive&#39; 및 url이 있으면 브라우저/초 화면에서 url을 열어 MVPD로 로그아웃을 완료합니다
