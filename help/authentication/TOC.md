---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass 인증
user-guide-description: Adobe Pass 인증은 TV Everywhere용 권한 부여 솔루션으로 리소스에 대한 액세스를 요청하는 사용자에게 자격이 있는지 여부를 결정하기 위한 모듈식 프레임워크를 제공합니다.
source-git-commit: ffedb5db269644c8d9c81480d27dff43bd4eb5d6
workflow-type: tm+mt
source-wordcount: '1240'
ht-degree: 2%

---


# Adobe Pass 인증 도움말 {#authentication}

+ [Adobe Pass 인증](home.md)
+ [제품 공지](product-announcements.md)
+ 제품 릴리스 {#product-releases}
   + 2024 {#2024}
      + [Adobe Pass Authentication 3.0.3 릴리스 노트](notes-releases/auth-rn-303.md)
      + [Adobe Pass Authentication 3.0 릴리스 노트](notes-releases/auth-rn-300.md)
      + [Adobe Pass Authentication 2.70 릴리스 노트](notes-releases/auth-rn-270.md)
      + [Adobe Pass Authentication 2.69 릴리스 노트](notes-releases/auth-rn-269.md)
      + [Adobe Pass Authentication JavaScript 4.7.0 릴리스 노트](notes-releases/authn-rn-javascript-470.md)
      + [Adobe Pass 인증 iOS / tvOS 3.9.2 릴리스 노트](notes-releases/authn-rn-ios-tvos-392.md)
      + [Adobe Pass 인증 iOS / tvOS 3.8.4 릴리스 노트](notes-releases/authn-rn-ios-tvos-384.md)
   + 2023년 {#2023}
      + [Adobe Pass Authentication 2.68 릴리스 노트](notes-releases/auth-rn-268.md)
      + [Adobe Pass Authentication 2.67 릴리스 노트](notes-releases/auth-rn-267.md)
      + [Adobe Pass Authentication 2.66 릴리스 노트](notes-releases/auth-rn-266.md)
      + [Adobe Pass Authentication 2.65.1 릴리스 노트](notes-releases/auth-rn-2651.md)
      + [Adobe Pass Authentication 2.65 릴리스 노트](notes-releases/auth-rn-265.md)
      + [Adobe Pass Authentication 2.64.1 릴리스 노트](notes-releases/auth-rn-2641.md)
      + [Adobe Pass 인증 iOS / tvOS 3.8.3 릴리스 노트](notes-releases/authn-rn-ios-tvos-383.md)
      + [Adobe Pass 인증 iOS / tvOS 3.8.2 릴리스 노트](notes-releases/authn-rn-ios-tvos-382.md)
      + [Adobe Pass 인증 iOS / tvOS 3.8.1 릴리스 노트](notes-releases/authn-rn-ios-tvos-381.md)
      + [Adobe Pass Authentication Android 3.7.3 릴리스 노트](notes-releases/authn-rn-android-373.md)
   + 2022년 {#2022}
      + [Adobe Pass Authentication 2.64 릴리스 노트](notes-releases/auth-rn-264.md)
      + [Adobe Pass Authentication 2.63 릴리스 노트](notes-releases/auth-rn-263.md)
      + [Adobe Pass Authentication 2.62.1 릴리스 노트](notes-releases/auth-rn-2621.md)
      + [Adobe Pass Authentication JavaScript 4.6.0 릴리스 노트](notes-releases/authn-rn-javascript-460.md)
   + 2021 {#2021}
      + [Adobe Pass Authentication JavaScript 4.4.0 릴리스 노트](notes-releases/authn-rn-javascript-440.md)
      + [Adobe Pass 인증 iOS / tvOS 3.7.0 릴리스 노트](notes-releases/authn-rn-ios-tvos-370.md)
+ 킥스타트 {#kickstart}
   + [기술 문서](kickstart/technical-paper.md)
   + [프로그래머 개요](kickstart/programmer-overview.md)
   + [MVPD 개요](kickstart/mvpd-overview.md)
   + [프로그래머 킥스타트 안내서](kickstart/programmer-kickstart-guide.md)
   + [MVPD 킥스타트 안내서](kickstart/mvpd-kickstart-guide.md)
   + [지원 절차 FAQ](kickstart/support-procedures-faqs.md)
+ 프로그래머 {#integration-guide-programmers}을(를) 위한 통합 안내서
   + REST API {#rest-apis}
      + REST API DCR {#rest-api-dcr}
         + [동적 클라이언트 등록 개요](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
         + [동적 클라이언트 등록 용어집](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)
         + [동적 클라이언트 등록 FAQ](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)
         + API {#rest-api-dcr-apis}
            + [클라이언트 자격 증명 검색](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
            + [액세스 토큰 검색](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
         + 흐름 {#rest-api-dcr-flows}
            + [동적 클라이언트 등록 흐름](integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)
      + REST API V2 {#rest-api-v2}
         + [REST API V2 개요](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
         + [REST API V2 용어집](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
         + [REST API V2 FAQ](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)
         + API {#rest-api-v2-apis}
            + [REST API V2 API 개요](integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
            + 구성 {#rest-api-v2-configuration-apis}
               + [특정 서비스 공급자에 대한 구성 검색](integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
            + 세션 {#rest-api-v2-sessions-apis}
               + [인증 세션 만들기](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
               + [인증 세션 다시 시작](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
               + [인증 세션 검색](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
               + [사용자 에이전트에서 인증 수행](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
            + 프로필 {#rest-api-v2-profiles-apis}
               + [프로필 검색](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
               + [특정 mvpd에 대한 프로필 검색](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
               + [특정 코드에 대한 프로필 검색](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
            + 결정 {#rest-api-v2-decisions-apis}
               + [특정 mvpd를 사용하여 권한 부여 결정 검색](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
               + [특정 mvpd를 사용하여 사전 인증 결정 검색](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
            + {#rest-api-v2-logout-apis} 로그아웃
               + [특정 mvpd에 대한 로그아웃 시작](integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
            + 파트너 SSO(Single Sign-On) {#rest-api-v2-partner-single-sign-on-apis}
               + [파트너 인증 요청 검색](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
               + [파트너 인증 응답을 사용하여 프로필 검색](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
         + 흐름 {#rest-api-v2-flows}
            + [REST API V2 흐름 개요](integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
            + 기본 액세스 흐름 {#rest-api-v2-basic-access-flows}
               + [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
               + [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
               + [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
               + [보조 애플리케이션 내에서 수행되는 기본 인증 흐름](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
               + [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
               + [기본 애플리케이션 내에서 수행되는 기본 사전 인증 흐름](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
               + [기본 애플리케이션 내에서 수행되는 기본 로그아웃 흐름](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
            + 액세스 흐름 {#rest-api-v2-degraded-access-flows} 성능 저하
               + [액세스 흐름 성능 저하](integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
            + 임시 액세스 흐름 {#rest-api-v2-temporary-access-flows}
               + [임시 액세스 흐름](integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
            + SSO(Single Sign-On) 액세스 흐름 {#rest-api-v2-single-sign-on-access-flows}
               + [파트너 흐름을 사용한 SSO(Single Sign-On)](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
               + [플랫폼 ID 흐름을 사용한 SSO(Single Sign-On)](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
               + [서비스 토큰 흐름을 사용한 SSO(Single Sign-On)](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
               + [단일 로그아웃 흐름](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
         + Cookbook {#rest-api-v2-cookbooks}
            + [REST API V2 Cookbook(클라이언트-서버)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
            + [REST API V2 Cookbook(서버 간)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
         + 부록 {#rest-api-v2-appendix}
            + 헤더 {#rest-api-v2-appendix-headers}
               + [헤더 - 인증](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
               + [헤더 - AP-Device-Identifier](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
               + [헤더 - X-Device-Info](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
               + [헤더 - AD-Service-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
               + [헤더 - Adobe-주체-토큰](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
               + [헤더 - AP-Partner-Framework-Status](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
               + [헤더 - AP-TempPass-Identity](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + 표준 기능 {#standard-features}
      + 권한 {#entitlements}
         + [결정](integration-guide-programmers/features-standard/entitlements/decisions.md)
         + [미디어 토큰](integration-guide-programmers/features-standard/entitlements/media-tokens.md)
         + [사용자 메타데이터](integration-guide-programmers/features-standard/entitlements/user-metadata.md)
      + 오류 보고 {#error-reporting}
         + [향상된 오류 코드](integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)
      + SSO(Single Sign-On) 액세스 {#sso-access}
         + 파트너 SSO(Single Sign-On) {#partner-sso}
            + Apple Single Sign-On {#apple-sso}
               + [Apple SSO 개요](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
               + [Apple SSO Cookbook(REST API V2)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
         + Platform Single Sign-On {#platform-sso}
            + Amazon Single Sign-On {#amazon-sso}
               + [Amazon SSO Cookbook(REST API V2)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
            + Roku Single Sign-On {#roku-sso}
               + [Roku SSO 개요](integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)
      + 홈 기반 인증 액세스 {#hba-access}
         + [홈 기반 인증(HBA)](integration-guide-programmers/features-standard/hba-access/home-based-authentication.md)
      + 개인 정보 보호 지원 {#privacy-support}
         + [개인 정보 지원 개요](integration-guide-programmers/features-premium/privacy-support/privacy-supp-overview.md)
         + [개인 정보 보호 요청을 하는 방법](integration-guide-programmers/features-premium/privacy-support/make-privacy-req.md)
   + 프리미엄 기능 {#features-premium}
      + 임시 액세스 {#temporary-access}
         + [TempPass 기능](integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
      + 성능이 저하된 액세스 {#degraded-access}
         + [성능 저하 기능](integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
      + ESM {#esm}
         + [권한 부여 서비스 모니터링 개요](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)
         + [권한 부여 서비스 모니터링 API](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)
         + [서버측 지표](integration-guide-programmers/features-premium/esm/understanding-serverside-metrics.md)
      + Analytics {#analytics}
         + [Adobe Pass 인증 서버측 데이터를 Adobe Analytics에 통합](integration-guide-programmers/features-premium/analytics/integrate-authn-servr-data-analytics.md)
         + [Adobe Pass 인증에서 Experience Cloud ID 사용](integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)
   + 레거시 {#legacy}
      + (레거시) REST API V1 {#rest-api-v1}
         + [(기존) REST API V1 개요](integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
         + [(기존) REST API V1 참조](integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
         + (기존) API {#rest-api-v1-apis}
            + [(기존) 등록 코드 요청](integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)
            + [(기존) 등록 레코드 반환](integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md)
            + [(기존) 등록 레코드 삭제](integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md)
            + [(레거시) MVPD 목록 제공](integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)
            + [(레거시) 인증 시작](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)
            + [(기존) 인증 토큰 확인](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)
            + [(레거시) 인증 토큰 검색](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)
            + [(레거시) 인증 시작](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)
            + [(레거시) 인증 토큰 검색](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md)
            + [(레거시) 짧은 미디어 토큰 받기](integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)
            + [(레거시) 두 번째 화면 웹 앱에서 인증 흐름 확인](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md)
            + [(기존) 사전 승인된 리소스 목록 검색](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md)
            + [(레거시) 두 번째 화면 웹 앱에서 사전 승인된 리소스 목록 검색](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
            + [(기존) 로그아웃 시작](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)
            + [(기존) 사용자 메타데이터](integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)
            + [(레거시) 프로필 요청 검색](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md)
            + [(기존) 토큰 교환](integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)
            + [(레거시) 임시 패스 및 프로모션 임시 패스에 대한 무료 미리보기](integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md)
         + (레거시) Cookbook {#rest-api-v1-cookbooks}
            + [(기존) REST API V1 Cookbook(클라이언트-서버)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-clienttoserver.md)
            + [(기존) REST API V1 Cookbook(서버 간)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)
      + (레거시) SDK {#sdks}
         + (기존) JavaScript SDK {#javascript-sdk}
            + [(기존) JavaScript SDK 개요](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-overview.md)
            + [(기존) JavaScript SDK Cookbook](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-cookbook.md)
            + [(기존) JavaScript SDK API 참조](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
            + [(기존) JavaScript SDK API 사전 인증](integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
            + (레거시) 지침 {#javascript-sdk-guidelines}
               + [(기존) 새로 고침 없이 로그인 및 로그아웃](integration-guide-programmers/legacy/sdks/javascript-sdk/refreshless-login-and-logout.md)
         + (기존) iOS/tvOS SDK {#ios-tvos-sdk}
            + [(기존) iOS/tvOS SDK 개요](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-overview.md)
            + [(기존) iOS/tvOS SDK Cookbook](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)
            + [(기존) iOS/tvOS SDK API 참조](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
            + [(기존) iOS/tvOS SDK API 사전 인증](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
            + (레거시) 지침 {#ios-tvos-sdk-guidelines}
               + [(기존) iOS/tvOS 애플리케이션 등록](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)
               + [(기존) iOS/tvOS v3.x 마이그레이션 안내서](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-v3x-migration-guide.md)
               + [(기존) iOS/tvOS 스토리지 무결성 검사](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-storage-integrity-checks.md)
         + (기존) Android SDK {#android-sdk}
            + [(기존) Android SDK 개요](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-overview.md)
            + [(기존) Android SDK Cookbook](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-cookbook.md)
            + [(기존) Android SDK API 참조](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md)
            + [(기존) Android SDK API 사전 인증](integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)
            + (레거시) 지침 {#android-sdk-guidelines}
               + [(기존) Android 애플리케이션 등록](integration-guide-programmers/legacy/sdks/android-sdk/android-application-registration.md)
               + [(기존) Android SDK(동적 클라이언트 등록 기능 포함)](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-with-dynamic-client-registration.md)
         + (레거시) FireOS SDK {#fireos-sdk}
            + [(기존) Amazon FireOS 기술 개요](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-technical-overview.md)
            + [(기존) Amazon FireOS 통합 Cookbook](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-integration-cookbook.md)
            + [(기존) Amazon FireOS API 참조](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)
            + [(기존) Amazon FireOS 애플리케이션 등록](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-application-registration.md)
            + [(기존) Dynamic Client Registration이 포함된 FireOS SDK](integration-guide-programmers/legacy/sdks/fireos-sdk/fireos-sdk-with-dynamic-client-registration.md)
            + [(레거시) Amazon FireOS SSO - 프로그래머 시작 안내서](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-firetv-sso-programmer-kickoff-guide.md)
      + (레거시) 클라이언트 정보 {#client-information}
         + [(기존) 클라이언트 정보(장치, 연결 및 애플리케이션) 전달](integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)
      + (레거시) 오류 보고 {#error-reporting}
         + [(기존) 오류 보고](integration-guide-programmers/legacy/error-reporting/error-reporting.md)
      + (기존) SSO(Single Sign-On) 액세스 {#sso-access}
         + [(기존) 단일 사인온 지원](integration-guide-programmers/legacy/sso-access/sso-support.md)
         + [(기존) 수동 인증을 통한 SSO](integration-guide-programmers/legacy/sso-access/sso-passive-authn.md)
         + [(기존) Amazon SSO Cookbook(REST API V1)](integration-guide-programmers/legacy/sso-access/amazon-sso-cookbook-rest-api-v1.md)
         + [(기존) Apple SSO Cookbook(REST API V1)](integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)
         + [(기존) Apple SSO Cookbook(iOS/tvOS SDK)](integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md)
      + (레거시) TVE 대시보드 {#tve-dashboard}
         + [(기존) TVE 대시보드 사용 안내서](integration-guide-programmers/legacy/tve-dashboard/tve-dashboard-user-guide.md)
      + (레거시) 기술 노트 {#tech-notes}
         + (레거시) REST API V1 {#rest-api-v1}
            + [(기존) 클라이언트 없는 API 구현 - 가능한 이유/원인이 있는 오류 코드/메시지](integration-guide-programmers/legacy/notes-technical/clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
            + [(기존) 장치 ID가 없는 클라이언트 없는 API 흐름](integration-guide-programmers/legacy/notes-technical/clientless-api-flow-in-the-absence-of-device-id.md)
            + [(기존) 클라이언트 없음: /authenticate 요청에서 &#39;&amp;&#39;reg_code를 사용하지 마십시오.](integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)
            + [(레거시) Xbox 360 및 XboxOne Clientless에서 프로그래머를 위해 Adobe Pass Entitlement Services 활성화](integration-guide-programmers/legacy/notes-technical/enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
            + [(기존) 클라이언트 없는 장치 유형 및 지표](integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
         + (레거시) SDK {#sdks}
            + [(기존) 인증서 Q&amp;A](integration-guide-programmers/legacy/notes-technical/certificates-qa.md)
            + [(기존) 사용자 ID 이해](integration-guide-programmers/legacy/notes-technical/understanding-user-ids.md)
            + (기존) JavaScript SDK {#javascript-sdk}
               + [(기존) 추적 방지 평가 - Apple Safari](integration-guide-programmers/legacy/notes-technical/tracking-prevention-assessment-apple-safari.md)
               + [(기존) 추적 방지 평가 - Google Chrome](integration-guide-programmers/legacy/notes-technical/tracking-prevention-assessment-google-chrome.md)
               + [(기존) 쿠키 업데이트 - SameSite 및 Secure 플래그](integration-guide-programmers/legacy/notes-technical/cookies-updates-samesite-and-secure-flags.md)
               + [(레거시) 디버깅 팁](integration-guide-programmers/legacy/notes-technical/appendix-b-debugging-tips.md)
            + (기존) Android SDK {#android-sdk}
               + [(기존) Android 10 앱에서 Access Enabler Android SDK SSO(Single Sign-On)](integration-guide-programmers/legacy/notes-technical/access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
               + [(기존) Adobe Pass 인증 및 Android 6 &quot;Marshmallow&quot;의 새로운 권한 모델](integration-guide-programmers/legacy/notes-technical/adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
            + (기존) iOS/tvOS SDK {#ios-tvos-sdk}
               + [(기존) iOS SDK 3.1+에서 WKWebView 지원](integration-guide-programmers/legacy/notes-technical/wkwebview-support-on-ios-sdk-31.md)
               + [(기존) iOS SDK 3.2+에서 SFSafariViewController 지원](integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)
               + [(레거시) Adobe Pass 인증 액세스 Enabler 사용 시 iOS에서 SSO](integration-guide-programmers/legacy/notes-technical/sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
               + [(기존) iOS 인증 오류 - adobepass.ios.app을 찾을 수 없음](integration-guide-programmers/legacy/notes-technical/ios-authentication-error-adobepassiosapp-cannot-be-found.md)
               + [(레거시) iOS에서 임시 패스 재설정](integration-guide-programmers/legacy/notes-technical/reset-temp-pass-on-ios.md)
               + [(레거시) 콘솔 앱 로그를 사용하여 AccessEnabler iOS/tvOS SDK 디버깅](integration-guide-programmers/legacy/notes-technical/debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
               + [(기존) AccessEnabler iOS/tvOS 3.7.0 업그레이드 경로](integration-guide-programmers/legacy/notes-technical/accessenabler-iostvos-370-upgrade-path.md)
         + (레거시) 사용자 경험 {#user-experience}
            + [(기존) iFrame에서 팝업으로 MVPD 로그인 페이지를 마이그레이션하는 방법](integration-guide-programmers/legacy/notes-technical/migr-mvpd-login-iframe-popup.md)
            + [(기존) Preflight 기능: 문제를 활성화, 문제 해결 또는 확인하는 방법](integration-guide-programmers/legacy/notes-technical/preflight-feature.md)
            + [(레거시) 선택 대화 상자에서 MVPD 허용](integration-guide-programmers/legacy/notes-technical/allow-mvpd-selectn-dialog.md)
            + [(레거시) MVPD가 선택 대화 상자를 표시하지 않도록 방지](integration-guide-programmers/legacy/notes-technical/prevent-mvpd-selectn-dialog.md)
         + (레거시) {#troubleshooting} 문제 해결
            + [(기존) Charles Proxy 사용](integration-guide-programmers/legacy/notes-technical/using-charles-proxy.md)
            + [(기존) Adobe Pass Adobe PayTV Pass 모니터링](integration-guide-programmers/legacy/notes-technical/monitoring-adobe-pay-tv-pass.md)
            + [(레거시) Adobe API 테스트 사이트를 사용하여 인증 및 권한 부여 흐름을 테스트하는 방법](integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)
   + [프로그래머 통합 안내서 개요](integration-guide-programmers/programmer-integration-guide-overview.md)
   + [프로그래머 권한 흐름](integration-guide-programmers/entitlement-flow.md)
   + [프로그래머 활용 사례](integration-guide-programmers/programmer-use-cases.md)
   + [조절 메커니즘](integration-guide-programmers/throttling-mechanism.md)
   + [최소 시스템 요구 사항](integration-guide-programmers/minimum-system-requirements.md)
+ MVPD {#integration-guide-mvpds}에 대한 통합 안내서
   + [통합 기능](integration-guide-mvpds/mvpd-integr-features.md)
   + [인증](integration-guide-mvpds/authn-usecase.md)
   + [OAuth 2.0 프로토콜을 사용한 인증](integration-guide-mvpds/authn-oauth2-protocol.md)
   + [인증](integration-guide-mvpds/authz-usecase.md)
   + [Preflight 인증](integration-guide-mvpds/mvpd-preflight-authz.md)
   + [MVPD 로그아웃](integration-guide-mvpds/usecase-mvpd-logout.md)
   + [컨텐츠 메타데이터 교환](integration-guide-mvpds/mvpd-content-metadata-exchange.md)
   + [사용자 메타데이터 교환](integration-guide-mvpds/mvpd-user-metadata-exchng.md)
   + [프록시 MVPD 웹 서비스](integration-guide-mvpds/proxy-mvpd-webserv.md)
   + [프록시 MVPD SAML 통합](integration-guide-mvpds/proxy-mvpd-saml-int.md)
   + [서비스 공급자 범위 지정](integration-guide-mvpds/serv-provider-scoping.md)
   + [MVPD 허용 IP 주소](integration-guide-mvpds/mvpd-listing-ip-addres.md)
+ TVE 대시보드 {#user-guide-tve-dashboard}에 대한 사용 안내서
   + [TVE 대시보드 개요](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
   + [환경](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
   + [변경 사항 검토 및 푸시](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [대시보드](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
   + [채널](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
   + [프로그래머](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPD](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
   + [통합](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
   + [보고서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
   + [변경 로그](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)
+ 기술 노트 {#tech-notes}
   + 환경 {#environments}
      + [Adobe 환경 이해](notes-technical/environments/understanding-the-adobe-environments.md)
      + [사전 영업 시 환경 설정 및 테스트](notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)
