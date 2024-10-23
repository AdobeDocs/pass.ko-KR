---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass 인증
user-guide-description: Adobe Pass 인증은 TV Everywhere용 권한 부여 솔루션으로 리소스에 대한 액세스를 요청하는 사용자에게 자격이 있는지 여부를 결정하기 위한 모듈식 프레임워크를 제공합니다.
source-git-commit: 2f5e511f774e1a2d8b8b60084844edfe27be6c76
workflow-type: tm+mt
source-wordcount: '1181'
ht-degree: 2%

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
   + REST API V1 {#rest-api-v1}
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
   + REST API V2 {#rest-api-v2}
      + [REST API V2 개요](./rest-api-v2/rest-api-v2-overview.md)
      + [REST API V2 용어집](./rest-api-v2/rest-api-v2-glossary.md)
      + API {#rest-api-v2-apis}
         + [REST API V2 API 개요](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
         + 구성 {#rest-api-v2-configuration-apis}
            + [특정 서비스 공급자에 대한 구성 검색](./rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
         + 세션 {#rest-api-v2-sessions-apis}
            + [인증 세션 만들기](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
            + [인증 세션 다시 시작](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
            + [인증 세션 검색](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
            + [사용자 에이전트에서 인증 수행](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
         + 프로필 {#rest-api-v2-profiles-apis}
            + [프로필 검색](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
            + [특정 mvpd에 대한 프로필 검색](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
            + [특정 코드에 대한 프로필 검색](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
         + 결정 {#rest-api-v2-decisions-apis}
            + [특정 mvpd를 사용하여 권한 부여 결정 검색](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
            + [특정 mvpd를 사용하여 사전 인증 결정 검색](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
         + {#rest-api-v2-logout-apis} 로그아웃
            + [특정 mvpd에 대한 로그아웃 시작](./rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
         + 파트너 SSO(Single Sign-On) {#rest-api-v2-partner-single-sign-on-apis}
            + [파트너 인증 요청 검색](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
            + [파트너 인증 응답을 사용하여 프로필 검색](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
      + 흐름 {#rest-api-v2-flows}
         + [REST API V2 흐름 개요](./rest-api-v2/flows/rest-api-v2-flows-overview.md)
         + 기본 액세스 흐름 {#rest-api-v2-basic-access-flows}
            + [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
            + [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
            + [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
            + [보조 애플리케이션 내에서 수행되는 기본 인증 흐름](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
            + [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
            + [기본 애플리케이션 내에서 수행되는 기본 사전 인증 흐름](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
            + [기본 애플리케이션 내에서 수행되는 기본 로그아웃 흐름](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
         + 액세스 흐름 {#rest-api-v2-degraded-access-flows} 성능 저하
            + [액세스 흐름 성능 저하](rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
         + 임시 액세스 흐름 {#rest-api-v2-temporary-access-flows}
            + [임시 액세스 흐름](rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
         + SSO(Single Sign-On) 액세스 흐름 {#rest-api-v2-single-sign-on-access-flows}
            + [파트너 흐름을 사용한 SSO(Single Sign-On)](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
            + [플랫폼 ID 흐름을 사용한 SSO(Single Sign-On)](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
            + [서비스 토큰 흐름을 사용한 SSO(Single Sign-On)](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
            + [단일 로그아웃 흐름](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
      + Cookbook {#rest-api-v2-cookbooks}
         + [REST API V2 Cookbook(클라이언트-서버)](rest-api-v2/cookbooks/rest-api-v2-cookbooks-client-server.md)
      + 부록 {#rest-api-v2-appendix}
         + 헤더 {#rest-api-v2-appendix-headers}
            + [헤더 - 인증](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
            + [헤더 - AP-Device-Identifier](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
            + [헤더 - X-Device-Info](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
            + [헤더 - AD-Service-Token](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
            + [헤더 - Adobe-주체-토큰](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
            + [헤더 - AP-Partner-Framework-Status](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
            + [헤더 - AP-TempPass-Identity](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + AccessEnabler SDK {#accessenabler-sdk}
      + JavaScript SDK {#javascriptsdk}
         + [JavaScript SDK 개요](javascript-sdk-overview.md)
         + [JavaScript SDK Cookbook](javascript-sdk-cookbook.md)
         + [JavaScript SDK API 참조](javascript-sdk-api-reference.md)
         + 지침 {#js-sdk-guidelines}
            + [새로 고침 없이 로그인 및 로그아웃](refreshless-login-and-logout.md)
         + JavaScript API {#javascript-sdk-api}
            + [사전 승인](preauthorize-api-javascript-sdk.md)
      + iOS/tvOS SDK {#ios-sdk}
         + [iOS/tvOS SDK 개요](iostvos-sdk-overview.md)
         + [iOS/tvOS SDK Cookbook](iostvos-sdk-cookbook.md)
         + [iOS/tvOS SDK API 참조](iostvos-sdk-api-reference.md)
         + 지침 {#ios-tvos-sdk-guidelines}
            + [iOS/tvOS 애플리케이션 등록](iostvos-application-registration.md)
            + 마이그레이션 지침 {#migration-guidelines}
               + [iOS/tvOS v3.x 마이그레이션 안내서](iostvos-v3x-migration-guide.md)
            + [iOS/tvOS 스토리지 무결성 검사](iostvos-sdk-storage-integrity-checks.md)
         + iOS/tvOS API {#ios-tvos-sdk-api}
            + [사전 승인](preauthorize-api-ios-tvos-sdk.md)
      + Android SDK {#androidsdk}
         + [Android SDK 개요](android-sdk-overview.md)
         + [Android SDK Cookbook](android-sdk-cookbook.md)
         + [Android SDK API 참조](android-sdk-api-reference.md)
         + 지침 {#androidguidelines}
            + [Android 애플리케이션 등록](android-application-registration.md)
            + [Android SDK(동적 클라이언트 등록 포함)](android-sdk-with-dynamic-client-registration.md)
         + Android API{#android-sdk-api}
            + [사전 승인](preauthorize-api-android-sdk.md)
      + Amazon FireOS SDK {#fireossdk}
         + [Amazon FireOS SSO - 프로그래머 시작 안내서](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [Clientless API Cookbook을 사용한 Amazon FireOS SSO](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Amazon FireOS 기술 개요](amazon-fireos-technical-overview.md)
         + [Amazon FireOS 통합 Cookbook](amazon-fireos-integration-cookbook.md)
         + [Amazon FireOS API 참조](amazon-fireos-native-client-api-reference.md)
         + [Amazon FireOS 애플리케이션 등록](amazon-fireos-application-registration.md)
         + [Dynamic Client Registration이 포함된 FireOS SDK](fireos-sdk-with-dynamic-client-registration.md)
   + 플랫폼 SSO {#platform-sso}
      + Apple SSO {#apple-sso}
         + [Apple SSO 개요](apple-sso-overview.md)
         + [Apple SSO Cookbook (REST API)](apple-sso-cookbook-rest-api.md)
         + [Apple SSO Cookbook(iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md)
      + Roku SSO {#roku-sso}
         + [Roku SSO](roku-sso-overview.md)
   + 콘텐츠 메타데이터 {#content-metadata}
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
      + [암호화를 위한 사용자 메타데이터 인증서](user-metadata-certificate.md)
   + [Preflight 인증](preflight-authz.md)
   + {#error-reportn} 보고 오류
      + [오류 보고](error-reporting.md)
      + [향상된 오류 코드](enhanced-error-codes.md)
   + 클라이언트 등록 {#dcr-api}
      + [동적 클라이언트 등록 개요](./dcr-api/dynamic-client-registration-overview.md)
      + API {#dcr-api-apis}
         + [클라이언트 자격 증명 검색](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
         + [액세스 토큰 검색](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
      + 흐름 {#dcr-api-flows}
         + [동적 클라이언트 등록 흐름](./dcr-api/flows/dynamic-client-registration-flow.md)
   + 서비스 {#degrn-service} 저하
      + [저하 API 개요](degradation-api-overview.md)
   + 개인 정보 준비 {#privacy-readiness}
      + [개인 정보 지원 개요](privacy-supp-overview.md)
      + [개인 정보 보호 요청을 하는 방법](make-privacy-req.md)
+ 팁 및 문제 해결 {#tips-troubleshoot}
   + [선택 대화 상자에서 MVPD 허용](allow-mvpd-selectn-dialog.md)
   + [MVPD가 선택 대화 상자를 표시하지 않도록 합니다.](prevent-mvpd-selectn-dialog.md)
+ {#support} 지원
   + [에스컬레이션 절차](escalation-procedures.md)
   + [Adobe Pass Adobe PayTV 패스 모니터링](monitoring-adobe-pay-tv-pass.md)
   + [최소 시스템 요구 사항](minimum-system-requirements.md)
+ 릴리스 정보 {#release-notes}
   + [Adobe Pass Authentication 3.0.3 릴리스 노트](auth-rn-303.md)
   + [Adobe Pass Authentication 3.0 릴리스 노트](auth-rn-300.md)
   + [Adobe Pass Authentication 2.70 릴리스 노트](auth-rn-270.md)
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
   + JavaScript SDK 릴리스 노트 {#release-notes-javascript}
      + [Adobe Pass Authentication JavaScript 4.7.0 릴리스 노트](authn-rn-javascript-470.md)
      + [Adobe Pass Authentication JavaScript 4.6.0 릴리스 노트](authn-rn-javascript-460.md)
      + [Adobe Pass Authentication JavaScript 4.4.0 릴리스 노트](authn-rn-javascript-440.md)
      + [Adobe Pass Authentication JavaScript 4.2.0 릴리스 노트](authn-rn-javascript-420.md)
      + [Adobe Pass Authentication JavaScript 4.1.1 릴리스 노트](authn-rn-javascript-411.md)
      + [Adobe Pass Authentication JavaScript 4.1.0 릴리스 노트](authn-rn-javascript-410.md)
      + [Adobe Pass Authentication JavaScript 4.0.0 릴리스 노트](authn-rn-javascript-400.md)
      + [Adobe Pass Authentication JavaScript 3.5.0 릴리스 노트](authn-rn-javascript-350.md)
   + iOS/tvOS SDK 릴리스 노트 {#release-notes-ios}
      + [Adobe Pass 인증 iOS / tvOS 3.9.2 릴리스 노트](authn-rn-ios-tvos-392.md)
      + [Adobe Pass 인증 iOS / tvOS 3.8.4 릴리스 노트](authn-rn-ios-tvos-384.md)
      + [Adobe Pass 인증 iOS / tvOS 3.8.3 릴리스 노트](authn-rn-ios-tvos-383.md)
      + [Adobe Pass 인증 iOS / tvOS 3.8.2 릴리스 노트](authn-rn-ios-tvos-382.md)
      + [Adobe Pass 인증 iOS / tvOS 3.8.1 릴리스 노트](authn-rn-ios-tvos-381.md)
      + [Adobe Pass Authentication iOS / tvOS 3.7.0 릴리스 노트](authn-rn-ios-tvos-370.md)
   + Android SDK 릴리스 노트 {#release-notes-android}
      + [Adobe Pass Authentication Android 3.7.3 릴리스 노트](authn-rn-android-373.md)
+ 기술 노트 {#tech-notes}
   + Adobe Pass 인증 SDK {#primetime-authentication-sdks}
      + [인증서 Q&amp;A](certificates-qa.md)
      + JavaScript SDK {#javascript}
         + [추적 방지 평가 - Apple Safari](tracking-prevention-assessment-apple-safari.md)
         + [추적 방지 평가 - Google Chrome](tracking-prevention-assessment-google-chrome.md)
         + [쿠키 업데이트 - SameSite 및 보안 플래그](cookies-updates-samesite-and-secure-flags.md)
      + Android SDK {#android}
         + [Android 10 앱에서 Enabler Android SDK SSO(Single Sign-On) 액세스](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass 인증 및 Android 6 &quot;Marshmallow&quot; 새 권한 모델](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS SDK {#iostvos}
         + [iOS SDK 3.1+에서 WKWebView 지원](wkwebview-support-on-ios-sdk-31.md)
         + [iOS SDK 3.2+에서 SFSafariViewController 지원](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [Adobe Pass Authentication Access Enabler 사용 시 iOS에서 SSO](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS 인증 오류 - adobepass.ios.app을 찾을 수 없음](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [iOS에서 임시 패스 재설정](reset-temp-pass-on-ios.md)
         + [콘솔 앱 로그를 사용하여 AccessEnabler iOS/tvOS SDK 디버깅](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [AccessEnabler iOS/tvOS 3.7.0 업그레이드 경로](accessenabler-iostvos-370-upgrade-path.md)
   + 인증 환경 {#primetime-authentication-environments} 전달
      + [Adobe 환경 이해](understanding-the-adobe-environments.md)
      + [사전 영업 시 환경 설정 및 테스트](setting-up-your-environment-and-testing-in-prequal.md)
      + [Adobe API 테스트 사이트를 사용하여 인증 및 권한 부여 흐름을 테스트하는 방법](test-authn-authz-flows-using-adobes-api-test-site.md)
   + 클라이언트 없는 API {#clientless-api}
      + [Clientless API 구현 - 가능한 이유/원인이 있는 오류 코드/메시지](clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [장치 ID가 없는 클라이언트 없는 API 흐름](clientless-api-flow-in-the-absence-of-device-id.md)
      + [Clientless: /authenticate 요청에서 &#39;&amp;&#39;reg_code를 사용하지 마십시오.](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Xbox 360 및 XboxOne Clientless에서 프로그래머를 위해 Adobe Pass 자격 부여 서비스 활성화](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [클라이언트 없는 장치 유형 및 지표](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + 사용자 환경 {#user-exp}
      + [iFrame에서 팝업으로 MVPD 로그인 페이지를 마이그레이션하는 방법](migr-mvpd-login-iframe-popup.md)
      + [Preflight 기능: 문제를 활성화, 문제 해결 또는 확인하는 방법](preflight-feature.md)
   + 도구 및 유틸리티 {#tools-and-utilities}
      + [Charles Proxy 사용](using-charles-proxy.md)
   + 개념 {#concepts}
      + [사용자 ID 이해](understanding-user-ids.md)
+ [TVE 대시보드 사용 안내서](tve-dashboard/old-tve-dashboard/tve-dashboard-user-guide.md)
+ 새 TVE 대시보드 사용 안내서 {#user-guide}
   + [TVE 대시보드 개요](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-overview.md)
   + [환경](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-environments.md)
   + [변경 사항 검토 및 푸시](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [대시보드](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-home.md)
   + [채널](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md)
   + [프로그래머](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPD](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-mvpds.md)
   + [통합](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md)
   + [보고서](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-reports.md)
   + [변경 로그](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-changes-log.md)
+ [용어집](glossary.md)
