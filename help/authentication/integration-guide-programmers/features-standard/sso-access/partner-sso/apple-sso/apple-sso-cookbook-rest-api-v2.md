---
title: Apple SSO Cookbook(REST API V2)
description: Apple SSO Cookbook(REST API V2)
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '3442'
ht-degree: 0%

---

# Apple SSO Cookbook(REST API V2) {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

Adobe Pass 인증 REST API V2는 iOS, iPadOS 또는 tvOS에서 실행되는 클라이언트 애플리케이션의 최종 사용자를 위한 Partner SSO(Single Sign-On)를 지원합니다.

이 문서는 높은 수준의 보기를 제공하는 기존 [REST API V2 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)와 파트너 흐름을 사용하여 [Single Sign-On을 구현하는 방법을 설명하는 문서의 확장 역할을 합니다](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).

## 파트너 흐름을 사용하는 Apple Single Sign-On {#cookbook}

### 전제 조건 {#prerequisites}

파트너 흐름을 사용하여 Apple Single Sign-On을 계속 진행하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* 스트리밍 애플리케이션은 Adobe Pass 인증 백엔드가 장치 플랫폼 및 해당 기능을 식별할 수 있도록 `X-Device-Info` 및/또는 `User-Agent` 헤더에 필요한 모든 데이터를 수집해야 합니다. `X-Device-Info` 헤더에 대한 자세한 내용은 [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) 설명서를 참조하십시오.

* 스트리밍 애플리케이션은 디바이스 레벨에 저장된 사용자의 가입 정보에 대한 액세스를 요청해야 하며, 이 경우 사용자는 디바이스의 카메라 또는 마이크에 대한 액세스를 제공하는 것과 유사하게 진행할 수 있는 애플리케이션 권한을 부여해야 합니다. 이 권한은 Apple의 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount)를 사용하여 응용 프로그램별로 요청해야 하며 장치에서 사용자 선택을 저장합니다.

  Apple SSO(Single Sign-On) 사용자 환경의 이점을 설명하여 구독 정보에 대한 액세스 권한을 부여하지 않으려는 사용자에게 인센티브를 제공하는 것이 좋습니다. 하지만 사용자가 iOS 및 iPadOS의 *`Settings -> TV Provider`* 또는 tvOS의 *`Settings -> Accounts -> TV Provider`*&#x200B;이나 애플리케이션 설정(TV 공급자 권한 액세스)으로 이동하여 결정을 변경할 수 있습니다.

  스트리밍 응용 프로그램은 사용자 인증을 요구하기 전에 언제든지 [사용자의 구독 정보에 액세스할 수 있는 권한](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)을 확인할 수 있으므로 응용 프로그램이 포그라운드 상태에 들어갈 때 사용자의 권한을 요청할 수 있습니다.

>[!IMPORTANT]
>
> 가정
>
> <br/>
>
> * 스트리밍 응용 프로그램이 프로그래머에게 적용되는 [온보딩 필수 구성 요소](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md#apple-sso-prerequisites-programmer)를 완료했으며 Apple SSO(Single Sign-On) 사용자 환경을 사용하도록 설정하는 데 필요합니다.

### 워크플로 {#workflow}

다음 다이어그램과 같이 파트너 흐름을 사용하여 Apple Single Sign-On을 구현하려면 주어진 단계를 수행하십시오.

![파트너 흐름을 사용하는 Apple Single Sign-On](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

*파트너 흐름을 사용하는 Apple Single Sign-On*

+++이. 등록 단계

1. **클라이언트 자격 증명 검색:** 스트리밍 응용 프로그램은 클라이언트 등록 끝점을 호출하여 클라이언트 자격 증명을 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [클라이언트 자격 증명 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) API 설명서를 참조하십시오.
   >
   > * `software_statement`과(와) 같은 모든 _필수_ 매개 변수
   > * `Content-Type`, `X-Device-Info`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **클라이언트 자격 증명 반환:** 클라이언트 등록 끝점 응답에 수신한 매개 변수 및 헤더와 관련된 클라이언트 자격 증명에 대한 정보가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 클라이언트 자격 증명 응답에 제공된 정보에 대한 자세한 내용은 [클라이언트 자격 증명 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) API 설명서를 참조하십시오.
   >
   > <br/>
   >
   > 클라이언트 레지스터는 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   >
   > <br/>
   >
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [클라이언트 자격 증명 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) API 설명서를 준수하는 추가 정보가 제공됩니다.

   >[!TIP]
   >
   > 제안: 클라이언트 자격 증명은 캐시되어야 하며 무기한 사용될 수 있습니다.

1. **액세스 토큰 검색:** 스트리밍 응용 프로그램은 클라이언트 토큰 끝점을 호출하여 액세스 토큰을 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [액세스 토큰 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#request) API 설명서를 참조하십시오.
   >
   > * `client_id`, `client_secret` 및 `grant_type`과(와) 같은 모든 _필수_ 매개 변수
   > * `Content-Type`, `X-Device-Info`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **액세스 토큰 반환:** 클라이언트 토큰 끝점 응답에 수신된 매개 변수 및 헤더와 연결된 액세스 토큰에 대한 정보가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 액세스 토큰 응답에 제공된 정보에 대한 자세한 내용은 [액세스 토큰 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) API 설명서를 참조하십시오.
   >
   > <br/>
   >
   > 클라이언트 토큰은 기본 조건이 충족되는지 확인하기 위해 요청 데이터의 유효성을 검사합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   >
   > <br/>
   >
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [액세스 토큰 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#error) API 설명서를 준수하는 추가 정보가 제공됩니다.

   >[!TIP]
   >
   > 제안: 액세스 토큰은 지정된 기간(예: 24시간 TTL(Time-to-Live)) 내에서만 캐시되고 사용되어야 합니다. 스트리밍 애플리케이션은 만료 후 새로운 액세스 토큰을 요청해야 합니다.

+++

+++나. 인증 단계 확인

1. **파트너 프레임워크 상태 검색:** 스트리밍 응용 프로그램이 Apple에서 개발한 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount)를 호출하여 사용자 권한 및 공급자 정보를 얻습니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount) 설명서를 참조하십시오.
   >
   > <br/>
   >
   > * 스트리밍 애플리케이션은 사용자의 구독 정보에 액세스할 수 있는 [권한](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)을 확인하고 사용자가 이를 허용한 경우에만 진행해야 합니다.
   > * 스트리밍 응용 프로그램에서 `VSAccountManager`에 대한 [위임](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)을 제공해야 합니다.
   > * 스트리밍 애플리케이션은 구독자 계정 정보에 대한 [요청](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)을 제출해야 합니다.
   > * 스트리밍 응용 프로그램은 [메타데이터](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 정보를 대기 및 처리해야 합니다.
   >
   > <br/>
   >
   > 스트리밍 응용 프로그램은 `VSAccountMetadataRequest` 개체의 [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) 속성에 대해 `false`과(와) 같은 부울 값을 지정해야 이 단계에서 사용자를 중단할 수 없음을 나타냅니다.

1. **파트너 프레임워크 상태 정보 반환:** 스트리밍 응용 프로그램에서 응답 데이터의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   * 사용자 권한 액세스 상태가 부여됩니다.
   * 사용자 공급자 매핑 식별자가 존재하며 유효합니다.
   * 사용자 공급자 프로필의 만료 날짜(사용 가능한 경우)가 유효합니다.

1. **프로필 검색:** 스트리밍 애플리케이션은 프로필 끝점에 요청을 보내 모든 프로필 정보를 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [프로필 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) API 설명서를 참조하십시오.
   >
   > * `serviceProvider`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization`, `AP-Device-Identifier` 및 `AP-Partner-Framework-Status`과(와) 같은 모든 _필수_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더
   >
   > <br/>
   >
   > 스트리밍 애플리케이션은 검색된 응답이 &quot;appleSSO&quot; 유형 프로필을 포함할 수 있도록 파트너 프레임워크 상태에 대한 유효한 값을 포함해야 합니다.
   >
   > <br/>
   >
   > `AP-Partner-Framework-Status` 헤더에 대한 자세한 내용은 [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 설명서를 참조하십시오.

1. **찾은 프로필에 대한 정보를 반환합니다.** Profiles 끝점 응답에는 받은 매개 변수 및 헤더와 연결된 찾은 프로필에 대한 정보가 포함되어 있습니다.

1. **프로필을 선택하고 결정 흐름을 진행합니다.** 프로필 끝점 응답에 프로필이 포함되어 있으면 스트리밍 응용 프로그램은 내부 논리(최종 사용자와 상호 작용하여)를 사용하여 사용 가능한 프로필 중 하나를 선택하여 후속 결정 흐름을 계속합니다.

1. **파트너 인증 흐름을 계속 진행합니다.** Profiles 끝점 응답에 프로필이 없으면 스트리밍 응용 프로그램은 파트너 인증 흐름을 계속 진행합니다.

+++

+++C. 파트너 인증 단계

1. **구성 검색:** 스트리밍 응용 프로그램은 구성 끝점에 요청을 보내 활성 통합을 갖는 MVPD 목록을 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 서비스 공급자에 대한 구성 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request) API 설명서를 참조하십시오.
   >
   > * `serviceProvider`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization`, `AP-Device-Identifier` 및 `X-Device-Info`과(와) 같은 모든 _필수_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **반환 구성:** 구성 끝점 응답에 서비스 공급자와 활성 통합을 하는 MVPD에 대한 정보가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 구성 응답에 제공된 정보에 대한 자세한 내용은 [특정 서비스 공급자에 대한 구성 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response) API 설명서를 참조하십시오.
   >
   > <br/>
   >
   > 구성 종단점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   >
   > <br/>
   >
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)를 준수하는 추가 정보가 제공됩니다

   >[!IMPORTANT]
   >
   > 스트리밍 애플리케이션은 계속 진행할 때 각 MVPD에 대해 제공된 다음 세부 정보를 처리해야 합니다.
   >
   > * `enablePlatformServices`: MVPD가 현재 Apple Single Sign-On을 지원하는지 여부를 나타냅니다.
   > * `displayInPlatformPicker`: MVPD를 Apple 선택기에 표시할 수 있는지 여부를 나타냅니다.
   > * `boardingStatus`: MVPD가 Apple Single Sign-On에 온보딩되었는지 여부를 나타냅니다.

1. **파트너 프레임워크 상태 검색:** 스트리밍 응용 프로그램이 Apple에서 개발한 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount)를 호출하여 사용자 권한 및 공급자 정보를 얻습니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount) 설명서를 참조하십시오.
   >
   > <br/>
   >
   > * 스트리밍 애플리케이션은 사용자의 구독 정보에 액세스할 수 있는 [권한](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)을 확인하고 사용자가 이를 허용한 경우에만 진행해야 합니다.
   > * 스트리밍 응용 프로그램에서 `VSAccountManager`에 대한 [위임](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)을 제공해야 합니다.
   > * 스트리밍 애플리케이션은 구독자 계정 정보에 대한 [요청](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)을 제출해야 합니다.
   > * 스트리밍 응용 프로그램은 [메타데이터](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 정보를 대기 및 처리해야 합니다.
   >
   > <br/>
   >
   > 스트리밍 응용 프로그램에서 `VSAccountMetadataRequest` 개체의 [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) 속성에 대해 `true`과(와) 같은 부울 값을 지정해야 이 단계에서 사용자가 TV 공급자를 선택하기 위해 중단할 수 있음을 나타냅니다.

1. **파트너 프레임워크 상태 정보 반환:** 스트리밍 응용 프로그램에서 응답 데이터의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   * 사용자 권한 액세스 상태가 부여됩니다.
   * 사용자 공급자 매핑 식별자가 존재하며 유효합니다.
   * 사용자 공급자 프로필의 만료 날짜(사용 가능한 경우)가 유효합니다.

1. **파트너 인증 요청 검색:** 스트리밍 응용 프로그램은 세션 파트너 끝점을 호출하여 인증 세션을 시작하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [파트너 인증 요청 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request) API 설명서를 참조하십시오.
   >
   > * `serviceProvider` 및 `partner`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` 및 `AP-Partner-Framework-Status`과(와) 같은 모든 _필수_ 헤더
   > * 모든 _선택적_ 헤더 및 매개 변수
   >
   > <br/>
   >
   > 스트리밍 애플리케이션은 검색된 응답이 파트너 인증 요청(SAML 요청)을 포함할 수 있도록 파트너 프레임워크 상태에 대한 유효한 값을 포함하는지 확인해야 합니다.
   >
   > <br/>
   >
   > `AP-Partner-Framework-Status` 헤더에 대한 자세한 내용은 [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 설명서를 참조하십시오.

1. **다음 작업을 나타냅니다.** 세션 파트너 끝점 응답에는 스트리밍 응용 프로그램에서 다음 작업에 대해 안내하는 데 필요한 데이터가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 세션 응답에 제공된 정보에 대한 자세한 내용은 [파트너 인증 요청 가져오기](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response) API 설명서를 참조하십시오.
   >
   > <br/>
   >
   > 세션 파트너 엔드포인트는 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   >
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   >
   > 세션 파트너 끝점이 요청 데이터를 확인하여 파트너 SSO(Single Sign-On) 조건이 충족되는지 확인합니다.
   >
   >  * Adobe Pass 서버의 파트너 SSO(Single Sign-On) 구성이 유효하고 활성화되어 있어야 합니다.
   >  * [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 헤더를 통해 받은 파트너 프레임워크 상태 페이로드가 유효해야 합니다.
   >
   > <br/>
   >
   > 파트너 SSO(Single Sign-On) 유효성 검사에 실패하면 응답은 기본적으로 기본 인증 플로우로 설정됩니다.

1. **의사 결정 흐름을 진행합니다.** 세션 파트너 끝점 응답에 다음 데이터가 포함되어 있습니다.
   * `actionName` 특성이 &quot;authorize&quot;로 설정되어 있습니다.
   * `actionType` 특성이 &quot;direct&quot;로 설정되어 있습니다.

   Adobe Pass 백엔드가 유효한 프로필을 식별하는 경우 스트리밍 애플리케이션은 이미 후속 결정 흐름에 사용할 수 있는 프로필이 있으므로 선택한 MVPD로 다시 인증할 필요가 없습니다.

1. **기본 인증 흐름을 계속 진행합니다.** 세션 파트너 끝점 응답에 다음 데이터가 포함되어 있습니다.
   * `actionName` 특성이 &quot;authenticate&quot; 또는 &quot;resume&quot;으로 설정되어 있습니다.
   * `actionType` 특성이 &quot;interactive&quot; 또는 &quot;direct&quot;로 설정되어 있습니다.

   Adobe Pass 백엔드가 유효한 프로필을 식별하지 못하고 파트너 SSO(Single Sign-On) 유효성 검사에 실패하면 Adobe Pass 서버는 기본 인증 플로우로 돌아갑니다.

   기본 인증 흐름에 대한 자세한 내용은 다음 문서를 참조하십시오.
   * [기본 응용 프로그램 내에서 인증 수행](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [사전 선택된 mvpd로 보조 응용 프로그램 내에서 인증 수행](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [미리 선택된 mvpd 없이 보조 응용 프로그램 내에서 인증 수행](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **파트너 인증 응답 흐름을 사용하여 프로필 검색을 계속합니다.** 세션 파트너 끝점 응답에 다음 데이터가 포함되어 있습니다.
   * `actionName` 특성이 &quot;partner_profile&quot;로 설정되어 있습니다.
   * `actionType` 특성이 &quot;direct&quot;로 설정되어 있습니다.
   * `authenticationRequest - type` 특성에는 MVPD 로그인을 위해 파트너 프레임워크에서 사용하는 보안 프로토콜이 포함되어 있습니다(현재 SAML로만 설정됨).
   * `authenticationRequest - request` 특성에 파트너 프레임워크에 전달되는 SAML 요청이 포함되어 있습니다.
   * `authenticationRequest - attributesNames` 특성에 파트너 프레임워크에 전달되는 SAML 특성이 포함되어 있습니다.

   Adobe Pass 백엔드가 유효한 프로필을 식별하지 못하고 파트너 SSO(Single Sign-On) 유효성 검사가 통과하면 스트리밍 애플리케이션은 MVPD로 인증 흐름을 시작하기 위해 파트너 프레임워크에 전달할 작업 및 데이터가 포함된 응답을 수신합니다.

1. **파트너 프레임워크로 MVPD 인증 완료:** 이전 단계에서 얻은 파트너 인증 요청(SAML 요청)을 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount)에 전달합니다. 인증 흐름이 성공하면 MVPD와 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount) 상호 작용에서 파트너 프레임워크 상태 정보와 함께 반환되는 파트너 인증 응답(SAML 응답)이 생성됩니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount) 설명서를 참조하십시오.
   >
   > <br/>
   >
   > * 스트리밍 애플리케이션은 사용자의 구독 정보에 액세스할 수 있는 [권한](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)을 확인하고 사용자가 이를 허용한 경우에만 진행해야 합니다.
   > * 스트리밍 응용 프로그램에서 `VSAccountManager`에 대한 [위임](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)을 제공해야 합니다.
   > * 스트리밍 애플리케이션은 구독자 계정 정보에 대한 [요청](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)을 제출해야 하며 이전 단계에서 얻은 파트너 인증 요청(SAML 요청)을 포함해야 합니다.
   > * 스트리밍 응용 프로그램은 [메타데이터](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 정보를 대기 및 처리해야 합니다.
   >
   > <br/>
   >
   > 스트리밍 응용 프로그램에서 `VSAccountMetadataRequest` 개체의 [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) 속성에 대해 `true`과(와) 같은 부울 값을 지정해야 이 단계에서 선택한 TV 공급자로 인증하기 위해 사용자를 중단할 수 있음을 나타냅니다.

1. **파트너 인증 응답 반환:** 스트리밍 응용 프로그램에서 응답 데이터의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   * 사용자 권한 액세스 상태가 부여됩니다.
   * 사용자 공급자 매핑 식별자가 존재하며 유효합니다.
   * 사용자 공급자 프로필의 만료 날짜(사용 가능한 경우)가 유효합니다.
   * 파트너 인증 응답(SAML 응답)이 있고 유효합니다.

1. **파트너 인증 응답을 사용하여 프로필 검색:** 스트리밍 애플리케이션은 프로필 파트너 끝점을 호출하여 프로필을 만들고 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [파트너 인증 응답을 사용하여 프로필 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request) API 설명서를 참조하십시오.
   >
   > * `serviceProvider`, `partner` 및 `SAMLResponse`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` 및 `AP-Partner-Framework-Status`과(와) 같은 모든 _필수_ 헤더
   > * 모든 _선택적_ 헤더 및 매개 변수
   >
   > <br/>
   >
   > 스트리밍 애플리케이션은 검색된 응답이 &quot;appleSSO&quot; 유형 프로필을 포함할 수 있도록 파트너 프레임워크 상태 및 파트너 인증 응답(SAML 응답)에 대한 유효한 값을 포함해야 합니다.
   >
   > <br/>
   >
   > `AP-Partner-Framework-Status` 헤더에 대한 자세한 내용은 [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 설명서를 참조하십시오.

1. **파트너 프로필에 대한 정보를 반환합니다.** 프로필 끝점 응답에 &quot;appleSSO&quot;로 설정된 `type` 특성을 포함하여 파트너 프로필에 대한 정보가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 프로필 응답에 제공된 정보에 대한 자세한 내용은 [파트너 인증 응답을 사용하여 프로필 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response) API 설명서를 참조하십시오.
   >
   > <br/>
   >
   > 프로필 파트너 엔드포인트는 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   >
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   >
   > 프로필 파트너 엔드포인트는 요청 데이터의 유효성을 검사하여 파트너 SSO(Single Sign-On) 조건이 충족되는지 확인합니다.
   >
   >  * Adobe Pass 서버의 파트너 SSO(Single Sign-On) 구성이 유효하고 활성화되어 있어야 합니다.
   >  * [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 헤더를 통해 받은 파트너 프레임워크 상태 페이로드가 유효해야 합니다.
   >
   > <br/>
   >
   > 파트너 SSO(Single Sign-On) 유효성 검사에 실패하면 응답은 기본적으로 기본 프로필 검색 플로우로 설정됩니다.

1. **결정 흐름을 진행합니다.** 스트리밍 응용 프로그램은 후속 결정 흐름을 계속할 수 있습니다.

+++

+++ D. 결정 단계

1. **파트너 프레임워크 상태 검색:** 스트리밍 응용 프로그램이 Apple에서 개발한 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount)를 호출하여 사용자 권한 및 공급자 정보를 얻습니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount) 설명서를 참조하십시오.
   >
   > <br/>
   >
   > * 스트리밍 애플리케이션은 사용자의 구독 정보에 액세스할 수 있는 [권한](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)을 확인하고 사용자가 이를 허용한 경우에만 진행해야 합니다.
   > * 스트리밍 응용 프로그램에서 `VSAccountManager`에 대한 [위임](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)을 제공해야 합니다.
   > * 스트리밍 애플리케이션은 구독자 계정 정보에 대한 [요청](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)을 제출해야 합니다.
   > * 스트리밍 응용 프로그램은 [메타데이터](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 정보를 대기 및 처리해야 합니다.
   >
   > <br/>
   >
   > 스트리밍 응용 프로그램은 `VSAccountMetadataRequest` 개체의 [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) 속성에 대해 `false`과(와) 같은 부울 값을 지정해야 이 단계에서 사용자를 중단할 수 없음을 나타냅니다.

   >[!TIP]
   > 
   > 제안: 스트리밍 애플리케이션은 파트너 프레임워크 상태 정보에 캐시된 값을 사용할 수 있습니다. 애플리케이션이 배경에서 전경 상태로 전환할 때 새로 고치는 것이 좋습니다.

1. **파트너 프레임워크 상태 정보 반환:** 스트리밍 응용 프로그램에서 응답 데이터의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   * 사용자 권한 액세스 상태가 부여됩니다.
   * 사용자 공급자 매핑 식별자가 존재하며 유효합니다.
   * 사용자 공급자 프로필의 만료 날짜(사용 가능한 경우)가 유효합니다.

1. **사전 권한 부여 결정 검색:** 스트리밍 응용 프로그램은 의사 결정 사전 권한 부여 끝점을 호출하여 리소스 목록에 대한 사전 권한 부여 결정을 얻는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 mvpd를 사용하여 사전 권한 부여 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) API 설명서를 참조하십시오.
   >
   > * `serviceProvider`, `mvpd` 및 `resources`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization` 및 `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더
   >
   > <br/>
   >
   > 스트리밍 애플리케이션은 선택한 프로필이 &quot;appleSSO&quot; 유형 프로필인 경우 추가적으로 요청하기 전에 파트너 프레임워크 상태에 대한 유효한 값을 포함하는지 확인해야 합니다.
   >
   > <br/>
   >
   > `AP-Partner-Framework-Status` 헤더에 대한 자세한 내용은 [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 설명서를 참조하십시오.

1. **미리 권한 부여 결정 반환:** 미리 권한 부여 결정 끝점 응답에 각 리소스에 대한 `Permit` 또는 `Deny` 결정이 포함되어 있습니다.
   * `Permit` 결정은 리소스를 재생할 수 있음을 의미합니다. 사전 인증 흐름은 리소스를 재생하는 데 사용되면 안 되므로 응답에는 미디어 토큰이 포함되지 않습니다.
   * `Deny` 결정은 리소스를 재생할 수 없음을 의미합니다. 응답에는 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 오류 페이로드가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 의사 결정 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd를 사용하여 사전 인증 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) API 설명서를 참조하십시오.
   >
   > <br/>
   >
   > 결정 사전 인증 끝점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   >
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **파트너 프레임워크 상태 검색:** 스트리밍 응용 프로그램이 Apple에서 개발한 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount)를 호출하여 사용자 권한 및 공급자 정보를 얻습니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount) 설명서를 참조하십시오.
   >
   > <br/>
   >
   > * 스트리밍 애플리케이션은 사용자의 구독 정보에 액세스할 수 있는 [권한](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)을 확인하고 사용자가 이를 허용한 경우에만 진행해야 합니다.
   > * 스트리밍 응용 프로그램에서 `VSAccountManager`에 대한 [위임](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)을 제공해야 합니다.
   > * 스트리밍 애플리케이션은 구독자 계정 정보에 대한 [요청](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)을 제출해야 합니다.
   > * 스트리밍 응용 프로그램은 [메타데이터](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 정보를 대기 및 처리해야 합니다.
   >
   > <br/>
   >
   > 스트리밍 응용 프로그램은 `VSAccountMetadataRequest` 개체의 [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) 속성에 대해 `false`과(와) 같은 부울 값을 지정해야 이 단계에서 사용자를 중단할 수 없음을 나타냅니다.

   >[!TIP]
   >
   > 제안: 스트리밍 애플리케이션은 파트너 프레임워크 상태 정보에 캐시된 값을 사용할 수 있습니다. 애플리케이션이 배경에서 전경 상태로 전환할 때 새로 고치는 것이 좋습니다.

1. **파트너 프레임워크 상태 정보 반환:** 스트리밍 응용 프로그램에서 응답 데이터의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   * 사용자 권한 액세스 상태가 부여됩니다.
   * 사용자 공급자 매핑 식별자가 존재하며 유효합니다.
   * 사용자 공급자 프로필의 만료 날짜(사용 가능한 경우)가 유효합니다.

1. **권한 부여 결정 검색:** 스트리밍 응용 프로그램은 결정 권한 부여 끝점을 호출하여 특정 리소스에 대한 권한 부여 결정을 얻는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 mvpd를 사용하여 권한 부여 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request) API 설명서를 참조하십시오.
   >
   > * `serviceProvider`, `mvpd` 및 `resources`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization` 및 `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더
   >
   > <br/>
   >
   > 스트리밍 애플리케이션은 선택한 프로필이 &quot;appleSSO&quot; 유형 프로필인 경우 추가적으로 요청하기 전에 파트너 프레임워크 상태에 대한 유효한 값을 포함하는지 확인해야 합니다.
   >
   > <br/>
   >
   > `AP-Partner-Framework-Status` 헤더에 대한 자세한 내용은 [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 설명서를 참조하십시오.

1. **반환 권한 부여 결정:** Decisions Authorize 끝점 응답에 특정 리소스에 대한 `Permit` 또는 `Deny` 결정이 포함되어 있습니다.
   * `Permit` 결정은 리소스를 재생할 수 있음을 의미합니다. 응답에는 미디어 토큰이 포함됩니다.
   * `Deny` 결정은 리소스를 재생할 수 없음을 의미합니다. 응답에는 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 오류 페이로드가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 의사 결정 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd를 사용하여 인증 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) API 설명서를 참조하십시오.
   >
   > <br/>
   >
   > 결정 권한 부여 끝점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   >
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

+++

+++ D. 로그아웃 단계

1. **Adobe Pass 로그아웃 시작:** 스트리밍 애플리케이션은 Adobe Pass 로그아웃 끝점을 호출하여 로그아웃 흐름을 시작하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 mvpd에 대한 로그아웃 시작](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request) API 설명서를 참조하십시오.
   >
   > * `serviceProvider`, `mvpd` 및 `redirectUrl`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization`, `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **다음 작업을 나타냅니다.** Adobe Pass Logout 끝점 응답에는 다음 작업에 대해 스트리밍 응용 프로그램을 안내하는 데 필요한 데이터가 포함되어 있습니다.
   * 로그아웃 흐름을 완료하려면 사용자가 파트너(시스템) 수준과 상호 작용해야 하므로 `url` 특성이 없습니다.
   * `actionName` 특성이 &quot;partner_logout&quot;으로 설정되어 있습니다.
   * `actionType` 특성이 &quot;partner_interactive&quot;로 설정되어 있습니다.

   >[!IMPORTANT]
   >
   > 로그아웃 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd에 대한 로그아웃 시작](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response) API 설명서를 참조하십시오.
   >
   > <br/>
   >
   > Adobe Pass Logout 끝점은 요청 데이터를 확인하여 기본 조건이 충족되는지 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   >
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

   >[!IMPORTANT]
   > 
   > 스트리밍 애플리케이션은 사용자가 파트너 수준에서 계속 로그아웃할 것임을 표시해야 합니다.

+++
