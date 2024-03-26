---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass 인증
user-guide-description: Adobe Pass 인증은 TV Everywhere용 권한 부여 솔루션으로, 리소스에 대한 액세스를 요청하는 사용자에게 자격이 있는지 여부를 결정하기 위한 모듈식 프레임워크를 제공합니다.
source-git-commit: 73d87de7db950f23973ad31b61409a1b711fc916
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 3%

---


# Adobe Pass 인증 도움말 {#authentication}

+ [Adobe Pass 인증 개요](home.md)
+ Adobe Pass 인증 개념 {#authentication-concepts}
   + [기술 문서](technical-paper.md)
   + [프로그래머를 위한 개요](programmer-overview.md)
   + [MVPD 개요](mvpd-overview.md)
+ 킥스타트 가이드 {#kickstart-guides}
   + [프로그래머 킥스타트 안내서](programmer-kickstart-guide.md)
   + [MVPD 킥스타트 안내서](mvpd-kickstart-guide.md)
+ 프로그래머 통합 안내서 {#programmer-integration-guide}
   + [프로그래머 통합 안내서 개요](programmer-integration-guide-overview.md)
   + [프로그래머 권한 흐름](entitlement-flow.md)
   + [프로그래머 활용 사례](programmer-use-cases.md)
   + [클라이언트 정보 전달(장치, 연결 및 애플리케이션)](passing-client-information-device-connection-and-application.md)
   + [조절 메커니즘](throttling-mechanism.md)
   + 나머지 API {#restapi}
      + [REST API 개요](rest-api-overview.md)
      + [REST API Cookbook(서버 간)](rest-api-cookbook-servertoserver.md)
      + [REST API Cookbook(클라이언트-서버)](rest-api-cookbook-clienttoserver.md)
      + Rest API 참조 {#rest-api-reference}
         + [REST API 참조](rest-api-reference.md)
         + [등록 코드 요청](registration-code-request.md)
         + [등록 레코드 반환](return-registration-record.md)
         + [등록 레코드 삭제](delete-registration-record.md)
         + [MVPD 목록 제공](provide-mvpd-list.md)
         + [인증 시작](initiate-authentication.md)
         + [인증 토큰 확인](check-authentication-token.md)
         + [인증 토큰 검색](retrieve-authentication-token.md)
         + [인증 시작](initiate-authorization.md)
         + [인증 토큰 검색](retrieve-authorization-token.md)
         + [짧은 미디어 토큰 가져오기](obtain-short-media-token.md)
         + [두 번째 화면 웹 앱별 인증 흐름 확인](check-authentication-flow-by-second-screen-web-app.md)
         + [사전 승인된 리소스 목록 검색](retrieve-list-of-preauthorized-resources.md)
         + [두 번째 화면 웹 앱으로 사전 승인된 리소스 목록 검색](retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
         + [로그아웃 시작](initiate-logout.md)
         + [사용자 메타데이터](user-metadata.md)
         + [프로필 요청 검색](retrieve-profilerequest.md)
         + [토큰 교환](token-exchange.md)
         + [임시 패스 및 프로모션 임시 패스에 대한 무료 미리보기](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + AccessEnabler SDK {#accessenabler-sdk}
      + JavaScript SDK {#javascriptsdk}
         + [JavaScript SDK 개요](javascript-sdk-overview.md)
         + [JavaScript SDK Cookbook](javascript-sdk-cookbook.md)
         + [JavaScript SDK API 참조](javascript-sdk-api-reference.md)
         + 지침 {#js-sdk-guidelines}
            + [새로 고침 없이 로그인 및 로그아웃](refreshless-login-and-logout.md)
         + JavaScript API {#js-api}
            + [사전 승인](js-preauthorize.md)
      + iOS/tvOs SDK {#ios-sdk}
         + [iOS/tvOS SDK 개요](iostvos-sdk-overview.md)
         + [iOS/tvOS SDK Cookbook](iostvos-sdk-cookbook.md)
         + [iOS/tvOS SDK API 참조](iostvos-sdk-api-reference.md)
         + 지침 {#ios-tvos-sdk-guidelines}
            + [iOS/tvOS 애플리케이션 등록](iostvos-application-registration.md)
            + 마이그레이션 지침 {#migration-guidelines}
               + [iOS/tvOS v3.x 마이그레이션 안내서](iostvos-v3x-migration-guide.md)
            + [iOS/tvOS 스토리지 무결성 검사](iostvos-sdk-storage-integrity-checks.md)
         + iOS/tvOS API {#ios-tvos-api}
            + [사전 승인](preauthorize.md)
      + Android SDK {#androidsdk}
         + [Android SDK 개요](android-sdk-overview.md)
         + [Android SDK Cookbook](android-sdk-cookbook.md)
         + [Android SDK API 참조](android-sdk-api-reference.md)
         + 지침 {#androidguidelines}
            + [Android 애플리케이션 등록](android-application-registration.md)
            + [Dynamic Client Registration이 포함된 Android SDK](android-sdk-with-dynamic-client-registration.md)
         + Android API{#androidapi}
            + [사전 승인](preauthorize-android.md)
      + Amazon Fireos SDK {#fireossdk}
         + [Amazon FireOS SSO - 프로그래머 시작 안내서](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [Clientless API Cookbook을 사용한 Amazon FireOS SSO](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Amazon FireOS 기술 개요](amazon-fireos-technical-overview.md)
         + [Amazon FireOS 통합 Cookbook](amazon-fireos-integration-cookbook.md)
         + [Amazon FireOS API 참조](amazon-fireos-native-client-api-reference.md)
         + [Amazon FireOS 애플리케이션 등록](amazon-fireos-application-registration.md)
         + [Dynamic Client Registration이 포함된 FireOS SDK](fireos-sdk-with-dynamic-client-registration.md)
   + 플랫폼 SSO {#platform-sso}
      + APPLE SSO {#apple-sso}
         + [Apple SSO 개요](apple-sso-overview.md)
         + [Apple SSO Cookbook (REST API)](apple-sso-cookbook-rest-api.md)
         + [Apple SSO Cookbook(iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md)
      + Roku SSO {#roku-sso}
         + [Roku SSO](roku-sso-overview.md)
   + 컨텐츠 메타데이터 {#content-metadata}
      + [보호된 리소스 식별](identify-protected-resources.md)
   + Content Server 통합 {#content-serv-int}
      + [미디어 토큰 검증기를 통합하는 방법](media-token-verifier-int.md)
   + 부록 {#appendices}
      + [디버깅 팁](appendix-b-debugging-tips.md)
+ MVPD 통합 안내서 {#mvpd-int-guide}
   + [통합 기능](mvpd-integr-features.md)
   + [인증](authn-usecase.md)
   + [OAuth 2.0 프로토콜을 사용한 인증](authn-oauth2-protocol.md)
   + [인증](authz-usecase.md)
   + [Preflight 인증](mvpd-preflight-authz.md)
   + [MVPD 로그아웃](usecase-mvpd-logout.md)
   + [컨텐츠 메타데이터 교환](mvpd-content-metadata-exchange.md)
   + [사용자 메타데이터 교환](mvpd-user-metadata-exchng.md)
   + [프록시 MVPD 웹 서비스](proxy-mvpd-webserv.md)
   + [프록시 MVPD SAML 통합](proxy-mvpd-saml-int.md)
   + [서비스 공급자 범위 지정](serv-provider-scoping.md)
   + [MVPD 허용 IP 주소](mvpd-listing-ip-addres.md)
+ Adobe Pass 인증 기능 {#auth-features}
   + Adobe Analytics 통합 {#analytics-int}
      + [Adobe Pass 인증 서버측 데이터를 Adobe Analytics에 통합](integrate-authn-servr-data-analytics.md)
      + [Adobe Pass 인증에서 Experience Cloud ID 사용](exp-cloud-id-authn.md)
   + 권한 부여 서비스 모니터링 {#entitlement-service-monitoring}
      + [권한 부여 서비스 모니터링 개요](entitlement-service-monitoring-overview.md)
      + [권한 부여 서비스 모니터링 API](entitlement-service-monitoring-api.md)
      + [서버측 지표](understanding-serverside-metrics.md)
   + 임시 통과 {#temp-pass}
      + [임시 통과](temp-pass.md)
      + [프로모션 임시 패스](promotional-temp-pass.md)
      + [임시 통과 재설정](reset-temp-pass.md)
   + SSO(Single Sign-On) {#sso}
      + [단일 사인온 지원](sso-support.md)
      + [수동 인증을 통한 SSO](sso-passive-authn.md)
   + 홈 기반 인증 {#home-based-auth}
      + [TV Everywhere를 위한 홈 기반 인증](home-based-authn-tve.md)
      + [MVPD에 대한 HBA 상태](hba-status-mvpds.md)
   + 사용자 메타데이터 {#user-metadat}
      + [사용자 메타데이터](user-metadata-feature.md)
   + [Preflight 인증](preflight-authz.md)
   + 오류 보고 {#error-reportn}
      + [오류 보고](error-reporting.md)
      + [향상된 오류 코드](enhanced-error-codes.md)
   + 클라이언트 등록 {#client-regn}
      + [동적 클라이언트 등록](dynamic-client-registration.md)
      + [Dynamic Client Registration API](dynamic-client-registration-api.md)
      + [동적 클라이언트 등록 관리](dynamic-client-registration-management.md)
   + 저하 서비스 {#degrn-service}
      + [저하 API 개요](degradation-api-overview.md)
   + 개인 정보 보호 준비 {#privacy-readiness}
      + [개인 정보 지원 개요](privacy-supp-overview.md)
      + [개인 정보 보호 요청을 하는 방법](make-privacy-req.md)
+ 팁 및 문제 해결 {#tips-troubleshoot}
   + [선택 대화 상자에서 MVPD 허용](allow-mvpd-selectn-dialog.md)
   + [MVPD가 선택 대화 상자를 표시하지 않도록 합니다.](prevent-mvpd-selectn-dialog.md)
+ 지원 {#support}
   + [에스컬레이션 절차](escalation-procedures.md)
   + [Adobe Pass Adobe PayTV 패스 모니터링](monitoring-adobe-pay-tv-pass.md)
   + [최소 시스템 요구 사항](minimum-system-requirements.md)
+ 릴리스 정보 {#release-notes}
   + [Adobe Pass Authentication 2.69 릴리스 노트](auth-rn-269.md)
   + [Adobe Pass Authentication 2.68 릴리스 노트](auth-rn-268.md)
   + [Adobe Pass Authentication 2.67 릴리스 노트](auth-rn-267.md)
   + [Adobe Pass Authentication 2.66 릴리스 노트](auth-rn-266.md)
   + [Adobe Pass Authentication 2.65.1 릴리스 노트](auth-rn-2651.md)
   + [Adobe Pass Authentication 2.65 릴리스 노트](auth-rn-265.md)
   + [Adobe Pass Authentication 2.64.1 릴리스 노트](auth-rn-2641.md)
   + [Adobe Pass Authentication 2.64 릴리스 노트](auth-rn-264.md)
   + [Adobe Pass Authentication 2.63 릴리스 노트](auth-rn-263.md)
   + [Adobe Pass Authentication 2.62.1 릴리스 노트](auth-rn-2621.md)
   + JavaScript SDK 릴리스 노트  {#release-notes-javascript}
      + [Adobe Pass 인증 JavaScript 4.7.0 릴리스 노트](authn-rn-javascript-470.md)
      + [Adobe Pass 인증 JavaScript 4.6.0 릴리스 노트](authn-rn-javascript-460.md)
      + [Adobe Pass 인증 JavaScript 4.4.0 릴리스 노트](authn-rn-javascript-440.md)
      + [Adobe Pass 인증 JavaScript 4.2.0 릴리스 노트](authn-rn-javascript-420.md)
      + [Adobe Pass 인증 JavaScript 4.1.1 릴리스 노트](authn-rn-javascript-411.md)
      + [Adobe Pass 인증 JavaScript 4.1.0 릴리스 노트](authn-rn-javascript-410.md)
      + [Adobe Pass 인증 JavaScript 4.0.0 릴리스 노트](authn-rn-javascript-400.md)
      + [Adobe Pass 인증 JavaScript 3.5.0 릴리스 노트](authn-rn-javascript-350.md)
   + iOS/tvOS SDK 릴리스 노트  {#release-notes-ios}
      + [Adobe Pass 인증 iOS / tvOS 3.9.2 릴리스 노트](authn-rn-ios-tvos-392.md)
      + [Adobe Pass 인증 iOS / tvOS 3.8.4 릴리스 노트](authn-rn-ios-tvos-384.md)
      + [Adobe Pass 인증 iOS / tvOS 3.8.3 릴리스 노트](authn-rn-ios-tvos-383.md)
      + [Adobe Pass 인증 iOS / tvOS 3.8.2 릴리스 노트](authn-rn-ios-tvos-382.md)
      + [Adobe Pass 인증 iOS / tvOS 3.8.1 릴리스 노트](authn-rn-ios-tvos-381.md)
      + [Adobe Pass Authentication iOS / tvOS 3.7.0 릴리스 노트](authn-rn-ios-tvos-370.md)
   + Android SDK 릴리스 노트 {#release-notes-android}
      + [Adobe Pass 인증 Android 3.7.3 릴리스 노트](authn-rn-android-373.md)
+ 기술 노트 {#tech-notes}
   + Adobe Pass 인증 SDK {#primetime-authentication-sdks}
      + [인증서 Q&amp;A](certificates-qa.md)
      + JavaScript SDK {#javascript}
         + [추적 방지 평가 - Apple Safari](tracking-prevention-assessment-apple-safari.md)
         + [추적 방지 평가 - Google Chrome](tracking-prevention-assessment-google-chrome.md)
         + [쿠키 업데이트 - SameSite 및 보안 플래그](cookies-updates-samesite-and-secure-flags.md)
      + Android SDK {#android}
         + [Android 10 앱에서 Enabler Android SDK SSO(Single Sign-On) 액세스](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass 인증 및 Android 6 &quot;Marshmallow&quot; 새로운 권한 모델](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOs SDK {#iostvos}
         + [iOS SDK 3.1+에서 WKWebView 지원](wkwebview-support-on-ios-sdk-31.md)
         + [iOS SDK 3.2+에서 SFSafariViewController 지원](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [Adobe Pass Authentication Access Enabler 사용 시 iOS에서 SSO](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS 인증 오류 - adobepass.ios.app을 찾을 수 없음](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [iOS에서 임시 패스 재설정](reset-temp-pass-on-ios.md)
         + [콘솔 앱 로그를 사용하여 AccessEnabler iOS/tvOS SDK 디버깅](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [AccessEnabler iOS/tvOS 3.7.0 업그레이드 경로](accessenabler-iostvos-370-upgrade-path.md)
   + 인증 환경 전달 {#primetime-authentication-environments}
      + [Adobe 환경 이해](understanding-the-adobe-environments.md)
      + [사전 영업 시 환경 설정 및 테스트](setting-up-your-environment-and-testing-in-prequal.md)
      + [Adobe API 테스트 사이트를 사용하여 인증 및 권한 부여 흐름을 테스트하는 방법](test-authn-authz-flows-using-adobes-api-test-site.md)
   + 클라이언트 없는 API {#clientless-api}
      + [Clientless API 구현 - 가능한 이유/원인이 있는 오류 코드/메시지](clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [장치 ID가 없는 클라이언트 없는 API 흐름](clientless-api-flow-in-the-absence-of-device-id.md)
      + [Clientless: /authenticate 요청에서 &#39;&amp;&#39;reg_code를 사용하지 마십시오.](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Xbox 360 및 XboxOne Clientless에서 프로그래머를 위해 Adobe Pass 자격 부여 서비스 활성화](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [클라이언트 없는 장치 유형 및 지표](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + 사용자 경험 {#user-exp}
      + [iFrame에서 팝업으로 MVPD 로그인 페이지를 마이그레이션하는 방법](migr-mvpd-login-iframe-popup.md)
      + [Preflight 기능: 문제를 활성화, 문제 해결 또는 확인하는 방법](preflight-feature.md)
   + 도구 및 유틸리티 {#tools-and-utilities}
      + [Charles Proxy 사용](using-charles-proxy.md)
   + 개념 {#concepts}
      + [사용자 ID 이해](understanding-user-ids.md)
+ [TVE 대시보드 사용 안내서](tve-dashboard-user-guide.md)
+ [용어집](glossary.md)