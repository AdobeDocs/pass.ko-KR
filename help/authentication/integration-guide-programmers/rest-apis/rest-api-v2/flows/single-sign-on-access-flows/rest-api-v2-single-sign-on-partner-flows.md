---
title: SSO(Single Sign-On) - 파트너 - 플로우
description: REST API V2 - Single Sign-On - 파트너 - 흐름
exl-id: 5735d67f-a311-4d03-ad48-93c0fcbcace5
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '1454'
ht-degree: 0%

---

# 파트너 흐름을 사용한 SSO(Single Sign-On) {#single-sign-on-partner-flows}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> REST API V2 구현은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md) 설명서에 의해 제한됩니다.

>[!MORELIKETHIS]
>
> [REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)도 방문하십시오.

Partner 메서드를 사용하면 Adobe Pass 서비스를 사용할 때 여러 애플리케이션이 파트너 프레임워크 상태 페이로드를 사용하여 장치 수준에서 SSO(Single Sign-On)를 수행할 수 있습니다.

애플리케이션은 Adobe Pass 시스템 외부의 파트너별 프레임워크 또는 라이브러리를 사용하여 파트너 프레임워크 상태 페이로드를 검색합니다.

응용 프로그램은 이 파트너 프레임워크 상태 페이로드를 지정하는 모든 요청에 대해 `AP-Partner-Framework-Status` 헤더의 일부로 포함합니다.

`AP-Partner-Framework-Status` 헤더에 대한 자세한 내용은 [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 설명서를 참조하십시오.

Adobe Pass 인증 REST API V2는 iOS, iPadOS 또는 tvOS에서 실행되는 클라이언트 애플리케이션의 최종 사용자를 위한 Partner SSO(Single Sign-On)를 지원합니다.

Apple 플랫폼용 SSO(Single Sign-On)에 대한 자세한 내용은 [Apple SSO Cookbook(REST API V2)](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md) 설명서를 참조하십시오.

## 파트너 인증 요청 검색 {#retrieve-partner-authentication-request}

### 전제 조건 {#prerequisites-retrieve-partner-authentication-request}

파트너 인증 요청을 검색하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* 파트너 프레임워크는 MVPD을 선택해야 합니다.
* 스트리밍 애플리케이션은 파트너 프레임워크에서 파트너 프레임워크 상태 정보를 가져와 Adobe Pass 서버에 전달해야 합니다.
* 스트리밍 애플리케이션은 Adobe Pass 서버에서 파트너 인증 요청을 가져와 파트너 프레임워크에 전달해야 합니다.

>[!IMPORTANT]
>
> 가정
> 
> <br/>
> 
> * 파트너 프레임워크는 MVPD 선택을 위한 사용자 상호 작용을 지원합니다.
> * 파트너 프레임워크는 선택한 MVPD을 인증하기 위한 사용자 상호 작용을 지원합니다.
> * 파트너 프레임워크는 사용자 권한 및 공급자 정보를 제공합니다.

### 워크플로 {#workflow-retrieve-partner-authentication-request}

다음 다이어그램과 같이 파트너 인증 요청을 검색하려면 해당 단계를 수행하십시오.

![파트너 인증 요청 검색](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-partner-authentication-request-flow.png)

*파트너 인증 요청 검색*

1. **파트너 프레임워크 상태 검색:** 스트리밍 애플리케이션은 Adobe Pass 시스템 외부에서 파트너 프레임워크를 호출하여 사용자 권한 및 공급자 정보를 얻습니다.

1. **파트너 프레임워크 상태 정보 반환:** 스트리밍 응용 프로그램에서 응답 데이터의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   * 사용자 권한 액세스 상태가 부여됩니다.
   * 사용자 공급자 매핑 식별자가 존재하며 유효합니다.
   * 사용자 공급자 프로필의 만료 날짜(사용 가능한 경우)가 유효합니다.

1. **파트너 인증 요청 검색:** 스트리밍 응용 프로그램은 세션 파트너 끝점을 호출하여 인증 세션을 시작하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [파트너 인증 요청 검색](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) API 설명서를 참조하십시오.
   >
   > * `serviceProvider` 및 `partner`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` 및 `AP-Partner-Framework-Status`과(와) 같은 모든 _필수_ 헤더
   > * 모든 _선택적_ 헤더 및 매개 변수
   >
   > <br/>
   >
   > 스트리밍 애플리케이션은 요청을 하기 전에 파트너 프레임워크 상태에 대한 유효한 값이 포함되어 있는지 확인해야 합니다.
   >
   > <br/>
   > 
   > `AP-Partner-Framework-Status` 헤더에 대한 자세한 내용은 [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 설명서를 참조하십시오.

1. **다음 작업을 나타냅니다.** 세션 파트너 끝점 응답에는 스트리밍 응용 프로그램에서 다음 작업에 대해 안내하는 데 필요한 데이터가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 세션 응답에 제공된 정보에 대한 자세한 내용은 [파트너 인증 요청 가져오기](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) API 설명서를 참조하십시오.
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
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../../features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   >
   > 세션 파트너 끝점이 요청 데이터를 확인하여 파트너 SSO(Single Sign-On) 조건이 충족되는지 확인합니다.
   >
   >  * Adobe Pass 서버의 파트너 SSO(Single Sign-On) 구성이 유효하고 활성화되어 있어야 합니다.
   >  * [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 헤더를 통해 받은 파트너 프레임워크 상태 페이로드가 유효해야 합니다.
   >
   > <br/>
   >
   > 파트너 SSO(Single Sign-On) 유효성 검사에 실패하면 응답은 기본적으로 기본 인증 플로우로 설정됩니다.

1. **파트너 인증 응답을 사용하여 프로필 검색 흐름을 진행합니다.** 세션 파트너 끝점 응답에 다음 데이터가 포함되어 있습니다.
   * `actionName` 특성이 &quot;partner_profile&quot;로 설정되어 있습니다.
   * `actionType` 특성이 &quot;direct&quot;로 설정되어 있습니다.
   * `authenticationRequest - type` 특성에는 MVPD 로그인을 위해 파트너 프레임워크에서 사용하는 보안 프로토콜이 포함됩니다(현재 SAML로만 설정됨).
   * `authenticationRequest - request` 특성에 파트너 프레임워크에 전달되는 SAML 요청이 포함되어 있습니다.
   * `authenticationRequest - attributesNames` 특성에 파트너 프레임워크에 전달되는 SAML 특성이 포함되어 있습니다.

   Adobe Pass 백엔드가 유효한 프로필을 식별하지 않고 파트너 SSO(Single Sign-On) 유효성 검사가 성공하면 스트리밍 애플리케이션은 MVPD으로 인증 흐름을 시작하기 위해 파트너 프레임워크에 전달할 작업 및 데이터가 포함된 응답을 수신합니다.

   파트너 인증 응답을 사용한 프로필 검색 흐름에 대한 자세한 내용은 [파트너 인증 응답을 사용한 프로필 검색](#retrieve-profile-using-partner-authentication-response) 섹션을 참조하십시오.

1. **기본 인증 흐름을 계속 진행합니다.** 세션 파트너 끝점 응답에 다음 데이터가 포함되어 있습니다.
   * `actionName` 특성이 &quot;authenticate&quot; 또는 &quot;resume&quot;으로 설정되어 있습니다.
   * `actionType` 특성이 &quot;interactive&quot; 또는 &quot;direct&quot;로 설정되어 있습니다.

   Adobe Pass 백엔드가 유효한 프로필을 식별하지 못하고 파트너 SSO(Single Sign-On) 유효성 검사에 실패하면 Adobe Pass 서버는 기본 인증 플로우로 돌아갑니다.

   기본 인증 흐름에 대한 자세한 내용은 다음 문서를 참조하십시오.
   * [기본 응용 프로그램 내에서 인증 수행](../basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [사전 선택된 mvpd로 보조 응용 프로그램 내에서 인증 수행](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [미리 선택된 mvpd 없이 보조 응용 프로그램 내에서 인증 수행](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **의사 결정 흐름을 진행합니다.** 세션 파트너 끝점 응답에 다음 데이터가 포함되어 있습니다.
   * `actionName` 특성이 &quot;authorize&quot;로 설정되어 있습니다.
   * `actionType` 특성이 &quot;direct&quot;로 설정되어 있습니다.

   Adobe Pass 백엔드가 유효한 프로필을 식별하는 경우 스트리밍 애플리케이션은 이미 후속 의사 결정 흐름에 사용할 수 있는 프로필이 있으므로 선택한 MVPD으로 다시 인증할 필요가 없습니다.

   >[!IMPORTANT]
   >
   > 스트리밍 애플리케이션은 요청을 하기 전에 파트너 프레임워크 상태에 대한 유효한 값이 포함되어 있는지 확인해야 합니다.
   >
   > <br/>
   > 
   > `AP-Partner-Framework-Status` 헤더에 대한 자세한 내용은 [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 설명서를 참조하십시오.

## 파트너 인증 응답을 사용하여 프로필 검색 {#retrieve-profile-using-partner-authentication-response}

### 전제 조건 {#prerequisites-retrieve-profile-using-partner-authentication-response}

파트너 인증 응답을 사용하여 프로필을 검색하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* 파트너 프레임워크는 선택한 MVPD을 사용하여 인증을 수행해야 합니다.
* 스트리밍 애플리케이션은 파트너 프레임워크에서 파트너 프레임워크 상태 정보와 함께 파트너 인증 응답을 가져와 Adobe Pass 서버에 전달해야 합니다.

>[!IMPORTANT]
>
> 가정
>
> * 파트너 프레임워크는 MVPD 선택을 위한 사용자 상호 작용을 지원합니다.
> * 파트너 프레임워크는 선택한 MVPD을 인증하기 위한 사용자 상호 작용을 지원합니다.
> * 파트너 프레임워크는 사용자 권한 및 공급자 정보를 제공합니다.

### 워크플로 {#workflow-retrieve-profile-using-partner-authentication-response}

다음 다이어그램과 같이 파트너 인증 응답을 사용하여 프로필 검색 플로우를 구현하려면 해당 단계를 수행하십시오.

![파트너 인증 응답을 사용하여 프로필 검색](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-profile-using-partner-authentication-response-flow.png)

*파트너 인증 응답을 사용하여 인증된 프로필 검색*

1. **파트너 프레임워크로 MVPD 인증 완료:** 인증 흐름이 성공하면 MVPD과의 파트너 프레임워크 상호 작용에서 파트너 프레임워크 상태 정보와 함께 반환되는 파트너 인증 응답(SAML 응답)이 생성됩니다.

1. **파트너 인증 응답 반환:** 스트리밍 응용 프로그램에서 응답 데이터의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   * 사용자 권한 액세스 상태가 부여됩니다.
   * 사용자 공급자 매핑 식별자가 존재하며 유효합니다.
   * 사용자 공급자 프로필의 만료 날짜(사용 가능한 경우)가 유효합니다.

1. **파트너 인증 응답을 사용하여 프로필 검색:** 스트리밍 애플리케이션은 프로필 파트너 끝점을 호출하여 프로필을 만들고 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [파트너 인증 응답을 사용하여 프로필 검색](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) API 설명서를 참조하십시오.
   >
   > * `serviceProvider`, `partner` 및 `SAMLResponse`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` 및 `AP-Partner-Framework-Status`과(와) 같은 모든 _필수_ 헤더
   > * 모든 _선택적_ 헤더 및 매개 변수
   >
   > <br/>
   > 
   > 스트리밍 애플리케이션은 요청을 하기 전에 파트너 프레임워크 상태에 대한 유효한 값이 포함되어 있는지 확인해야 합니다.
   >
   > <br/>
   > 
   > `AP-Partner-Framework-Status` 헤더에 대한 자세한 내용은 [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 설명서를 참조하십시오.

1. **파트너 프로필 만들기 및 저장:** Adobe Pass 서버는 모든 조건이 충족되는지 확인한 후 파트너 프로필을 만들고 저장합니다.

1. **파트너 프로필에 대한 정보를 반환합니다.** 프로필 끝점 응답에 &quot;appleSSO&quot;로 설정된 `type` 특성을 포함하여 파트너 프로필에 대한 정보가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 프로필 응답에 제공된 정보에 대한 자세한 내용은 [파트너 인증 응답을 사용하여 프로필 검색](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) API 설명서를 참조하십시오.
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
   > 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../../features-standard/error-reporting/enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   >
   > 프로필 파트너 엔드포인트는 요청 데이터의 유효성을 검사하여 파트너 SSO(Single Sign-On) 조건이 충족되는지 확인합니다.
   >
   >  * Adobe Pass 서버의 파트너 SSO(Single Sign-On) 구성이 유효하고 활성화되어 있어야 합니다.
   >  * [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 헤더를 통해 받은 파트너 프레임워크 상태 페이로드가 유효해야 합니다.
   >
   > <br/>
   >
   > 파트너 SSO(Single Sign-On) 유효성 검사에 실패하면 응답은 기본적으로 기본 프로필 검색 플로우로 설정됩니다.

1. **결정 흐름을 진행합니다.** 스트리밍 응용 프로그램은 후속 결정 흐름을 계속할 수 있습니다.

   >[!IMPORTANT]
   >
   > 스트리밍 애플리케이션은 요청을 하기 전에 파트너 프레임워크 상태에 대한 유효한 값이 포함되어 있는지 확인해야 합니다.
   >
   > <br/>
   > 
   > `AP-Partner-Framework-Status` 헤더에 대한 자세한 내용은 [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) 설명서를 참조하십시오.
