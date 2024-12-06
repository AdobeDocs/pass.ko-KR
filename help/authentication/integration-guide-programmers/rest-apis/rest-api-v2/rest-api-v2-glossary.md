---
title: REST API V2 용어집
description: REST API V2 용어집
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1964'
ht-degree: 0%

---

# REST API V2 용어집 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 문서는 Adobe Pass 인증 REST API V2 설명서를 통합할 때 사용되는 용어에 대한 정의를 제공하며 기존 [용어집](/help/authentication/kickstart/glossary.md)에 대한 재정의 역할을 합니다.

## 용어집 용어 {#glossary-terms}

### {#a}

#### 액세스 토큰 {#access-token}

액세스 토큰은 보호된 API에 대한 액세스를 보장하기 위한 [DCR(Dynamic Client Registration)](#dcr) 프로세스의 결과로 Adobe Pass 인증에 의해 생성된 토큰입니다.

#### 인증 {#authentication}

인증은 사용자가 [MVPD](#mvpd)로 사용자 구독을 확인한 후 보호된 콘텐츠([resource](#resource))에 대한 액세스 권한을 얻기 위해 [프로그래머](#programmer)에게 ID를 증명하도록 하는 프로세스입니다.

#### 인증 코드 {#code}

인증 코드는 사용자가 [인증](#authentication) 프로세스를 시작할 때 생성된 고유한 값을 저장하고, 인증 프로세스가 완료될 때까지 사용자의 [인증 세션](#session)을(를) 고유하게 식별하는 Adobe Pass 인증 개념입니다.

인증 코드는 [기본(프로그래머) 응용 프로그램](#primary-application) 또는 [보조(프로그래머) 응용 프로그램](#secondary-application)에서 모두 사용하여 [인증](#authentication) 프로세스를 완료하거나 [인증 세션](#session)에 대한 정보를 검색하거나 사용자 [프로필](#profile)에 액세스할 수 있습니다.

등록 코드를 사용한 이전 용어와 동의어입니다.

#### 인증 세션 {#session}

인증 세션은 [프로그래머](#programmer) 응용 프로그램에서 시작(또는 계속)한 사용자 인증 프로세스에 대한 정보를 저장하는 Adobe Pass 인증 개념이며 [인증 코드](#code)에 의해 고유하게 식별됩니다.

사용자가 이미 인증된 경우 인증 세션에서 [프로그래머](#programmer) 응용 프로그램이 [권한 부여](#entitlement) 흐름의 다음 단계로 [권한 부여](#authorization) 프로세스를 진행하도록 지정할 수도 있습니다.

#### 인증 {#authorization}

권한 부여는 사용자가 [MVPD](#mvpd)에 대한 사용자 권한을 확인한 후 소유한 [MVPD](#mvpd) 구독을 기반으로 [프로그래머](#programmer) 카탈로그에서 보호된 콘텐츠([resource](#resource))에 액세스할 수 있도록 하는 프로세스입니다.

### C {#c}

#### 클라이언트 자격 증명 {#client-credentials}

클라이언트 자격 증명은 [DCR(Dynamic Client Registration)](#dcr) 프로세스 중에 생성된 고유한 값 집합이며 [액세스 토큰](#access-token)을(를) 얻는 데 사용됩니다.

#### 구성 {#configuration}

구성은 [프로그래머](#programmer) 및 [MVPD](#mvpd) 통합 설정에 대한 정보를 저장하는 Adobe Pass 인증 개념으로, 사용자에게 활성 통합 목록에서 [TV 공급자](#tv-provider)를 선택하도록 요청할 때 [인증](#authentication) 프로세스 중에 사용할 수 있습니다.

#### 사용자 지정 체계 {#custom-scheme}

사용자 지정 구성표는 Adobe Pass [TVE Dashboard](#tve-dashboard)에서 생성 및 다운로드할 수 있는 [프로그래머](#programmer) 응용 프로그램을 참조하는 고유한 값이며 iOS 장치에서 실행되는 응용 프로그램에서 최종 리디렉션으로 사용됩니다.

### D {#d}

#### DCR {#dcr}

DCR(Dynamic Client Registration)은 [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591)에서 정의한 인증 메커니즘이며 [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)에서 설명한 OAuth 2.0 인증 프레임워크를 기반으로 합니다.

DCR은 보호된 API에 대한 액세스를 추가로 활성화할 수 있는 Adobe Pass 인증 서비스로 [프로그래머](#programmer)에게 전달됩니다.

자세한 내용은 [동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) 설명서를 참조하십시오.

#### 결정 {#decision}

이 결정은 [프로그래머](#programmer) 보호된 콘텐츠에 대한 사용자 액세스를 허용 또는 거부하기 위한 [MVPD](#mvpd) [권한 부여](#authorization) 또는 [사전 권한 부여](#preauthorization) 프로세스 조회에 대한 정보를 저장하는 Adobe Pass 인증 개념입니다.

#### 저하 {#degradation}

성능 저하가 [MVPD](#mvpd)에서 서비스 중단이 발생한 경우에도 사용자가 보호된 콘텐츠에 액세스할 수 있는 Adobe Pass 인증 기능입니다.

자세한 내용은 [성능 저하 API 개요](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md) 설명서를 참조하십시오.

### 전자 {#e}

#### 권한 부여 {#entitlement}

권한 부여는 사용자가 [인증](#authentication), [사전 권한 부여](#preauthorization), [권한 부여](#authorization), 마지막으로 [로그아웃](#logout)에 이르기까지 보호된 콘텐츠에 액세스할 수 있는 흐름과 다른 단계를 거치는 데 도움이 되는 기능을 통합한 Adobe Pass 인증 개념입니다.

#### 향상된 오류 코드 {#enhanced-error-code}

향상된 오류 코드는 요청을 처리하는 동안 발생한 오류에 대한 추가 정보를 제공하는 Adobe Pass 인증 개념입니다.

자세한 내용은 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) 설명서를 참조하세요.

### 시간 {#h}

#### HBA {#hba}

홈 기반 인증(HBA)은 소비자가 홈 네트워크에 연결된 일부 장치의 [TV Everywhere(TVE)](#tve) 콘텐츠에 대한 액세스 권한을 자동으로 부여 받는 프로세스입니다. 이 프로세스는 구독 계약 내 위치의 일부입니다.

### 내가 {#i}

#### ID 공급자 {#identity-provider}

ID 공급자는 [TV Everywhere(TVE)](#tve)의 컨텍스트에서 케이블, 위성 또는 인터넷 기반 서비스를 통해 소비자에게 ID 서비스를 제공하는 회사입니다.

[MVPD](#mvpd) 및 [TV 공급자](#tv-provider)와 동의어입니다.

### L {#l}

#### 로그아웃 {#logout}

로그아웃은 사용자가 Adobe Pass 인증 내에서 인증된 [프로필](#profile)을(를) 종료하고 사용자의 상태를 반영하도록 [프로그래머](#programmer) 응용 프로그램을 업데이트할 수 있는 프로세스입니다.

### M {#m}

#### 미디어 토큰 {#media-token}

미디어 토큰은 보호된 콘텐츠에 대한 액세스를 제공하기 위한 권한 부여 [결정](#decision)의 결과로 Adobe Pass 인증에 의해 생성된 토큰입니다.

미디어 토큰이 [Programmer](#programmer)에 전달되며, 유효성을 검사하여 해당 [resource](#resource)에 대한 액세스 보안을 유지합니다.

단기 인증 토큰을 사용한 이전 용어의 동의어입니다.

#### 미디어 토큰 검증기 {#media-token-verifier}

미디어 토큰 검증기는 [미디어 토큰](#media-token)의 신뢰성을 확인하는 역할을 하는 Adobe Pass 인증에서 배포한 라이브러리입니다.

자세한 내용은 [미디어 토큰 검증기 통합](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md) 설명서를 참조하십시오.

#### MVPD {#mvpd}

다채널 비디오 프로그래밍 디스트리뷰터(MVPD)는 케이블, 위성 또는 인터넷 기반 서비스를 통해 소비자에게 텔레비전 서비스를 제공하는 회사이다.

MVPD는 MVPD와 Adobe 사이의 온보딩 프로세스 동안 정의된 고유 값에 의해 식별된다.

[TV 공급자](#tv-provider) 및 [ID 공급자](#identity-provider)와 동의어입니다.

### P {#p}

#### 파트너 {#partner}

파트너는 SSO(Single Sign-On) 사용자 환경을 활성화하기 위해 [프로그래머](#programmer)에게 서비스 또는 프레임워크를 제공하는 회사입니다.

파트너는 파트너와 Adobe 간의 온보딩 프로세스 중에 정의된 고유한 값(예: &quot;apple&quot;)으로 식별됩니다.

#### 사전 인증 {#preauthorization}

사전 인증은 사용자가 [MVPD](#mvpd)를 사용하여 사용자 권한을 확인한 후 액세스 권한이 있는 [프로그래머](#programmer) 카탈로그에서 [리소스](#resource)의 하위 집합을 미리 볼 수 있는 프로세스입니다.

[Preflight](#preflight)의 동의어입니다.

#### Preflight {#preflight}

Preflight는 사용자가 [MVPD](#mvpd)를 사용하여 사용자 권한을 확인한 후 액세스 권한이 있는 [프로그래머](#programmer) 카탈로그에서 [리소스](#resource)의 하위 집합을 미리 볼 수 있는 프로세스입니다.

[사전 인증](#preauthorization)과 동의어입니다.

#### 기본(프로그래머) 애플리케이션 {#primary-application}

기본 응용 프로그램이 [인증](#authentication)을 시작하는 [프로그래머](#programmer) 응용 프로그램을 참조하지만 [사용자 에이전트](#user-agent)를 사용하여 [MVPD](#mvpd) 로그인 페이지로 이동하여 프로세스를 완료할 수 없습니다.

#### 프로필 {#profile}

프로필은 사용자의 인증 시작 날짜 및 종료 날짜, [사용자의 메타데이터](#user-metadata)에 대한 정보를 인증 획득 방법을 나타내는 다른 필드(예: &quot;일반&quot;, &quot;성능 저하&quot;, &quot;임시&quot;, &quot;Single Sign-On&quot; 등)와 함께 저장하는 Adobe Pass 인증 개념입니다.

이전에 사용한 인증 토큰의 동의어입니다.

#### 프로그래머 {#programmer}

프로그래머는 다양한 플랫폼에서 소유한 채널(브랜드)을 통해 소비자에게 콘텐츠를 제공하는 회사다.

프로그래머는 Adobe Pass 인증과의 통합에서 여러 소유 채널(브랜드)을 [서비스 공급자](#service-provider)(으)로 그룹화합니다.

#### 프록시 MVPD {#proxy-mvpd}

프록시 MVPD는 다른 MVPD에 대한 ID 서비스를 제공하는 회사로 Adobe Pass 인증과 직접 통합됩니다.

#### 프록시화된 MVPD {#proxied-mvpd}

프록시화된 MVPD는 Adobe Pass 인증과 직접 통합되지 않지만 [프록시 MVPD](#proxy-mvpd)을(를) 통해 통합됩니다.

#### 플랫폼 ID {#platform-identity}

플랫폼 ID는 사용자의 장치에 바인딩된 서비스 또는 프레임워크(라이브러리)에서 생성된 고유한 플랫폼 식별자 페이로드이며, SSO(Single Sign-On) 사용자 환경을 활성화하기 위해 [프로그래머](#programmer)에게 제공됩니다.

자세한 내용은 [플랫폼 ID 흐름을 사용한 Single Sign-On](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) 설명서를 참조하세요.

### R {#r}

#### 등록된 응용 프로그램 {#registered-application}

등록된 응용 프로그램은 [DCR(Dynamic Client Registration)](#dcr) 프로세스를 진행해야 하는 [Programmer](#programmer) 응용 프로그램에 대한 정보를 저장하는 Adobe Pass 인증 개념입니다.

#### 리소스 {#resource}

리소스가 사용자가 [프로그래머](#programmer) 카탈로그에서 액세스하려는 보호된 콘텐츠입니다.

리소스는 프로그래머와 MVPD 간에 합의된 고유한 값으로 식별됩니다.

자세한 내용은 [보호된 리소스 식별](/help/authentication/integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md) 설명서를 참조하세요.

### S {#s}

#### SAML {#saml}

SAML(Security Assertion Markup Language)은 당사자 간에, 특히 [ID 공급자](#identity-provider)와(과) [서비스 공급자](#sp) 간에 인증 및 권한 부여 데이터를 교환하기 위한 개방형 표준입니다.

#### 보조(프로그래머) 애플리케이션 {#secondary-application}

보조 응용 프로그램은 [사용자 에이전트](#user-agent)를 사용하여 [MVPD](#mvpd) 로그인 페이지로 이동하여 [인증](#authentication) 프로세스를 완료할 수 있는 [프로그래머](#programmer) 응용 프로그램을 참조합니다.

보조 애플리케이션은 기본 애플리케이션과 동일한 디바이스 또는 다른 (보조) 디바이스에서 실행될 수 있으며, 이 경우 로그인 경험을 &#39;제2 화면 인증&#39; 사용자 경험이라고 합니다.

#### 서비스 토큰 {#service-token}

서비스 토큰은 사용자에 바인딩된 서비스 또는 프레임워크(라이브러리)에서 생성된 고유한 사용자 식별자이며 SSO(Single Sign-On) 사용자 경험을 활성화하기 위해 [프로그래머](#programmer)에게 제공됩니다.

자세한 내용은 [서비스 토큰 흐름을 사용하는 Single Sign-On](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) 설명서를 참조하세요.

#### 서비스 공급자 {#service-provider}

서비스 공급자가 [프로그래머](#programmer)가 소유한 채널(브랜드)입니다.

서비스 공급자는 프로그래머와 Adobe 간 온보딩 프로세스 중에 정의된 고유 값으로 식별됩니다.

[요청자 ID](/help/authentication/kickstart/glossary.md#requestor-id)를 사용한 이전 용어와 동의어입니다.

#### 소프트웨어 구문 {#software-statement}

소프트웨어 문은 Adobe Pass [TVE Dashboard](#tve-dashboard)에서 다운로드할 수 있는 JSON 웹 토큰(JWT)이며 [DCR(Dynamic Client Registration)](#dcr) 프로세스의 일부로 사용됩니다.

#### SLO {#slo}

SLO(Single Logout)는 사용자가 [SSO(Single Sign-On)](#sso)에 포함된 모든 응용 프로그램에서 로그아웃할 수 있는 프로세스입니다.

#### SP {#sp}

SP(서비스 공급자)는 [MVPD](#mvpd)과의 통합에서 [프로그래머](#programmer)을(를) 대신하여 Adobe Pass 인증이 수행하는 역할을 참조합니다.

#### SSO {#sso}

SSO(Single Sign-On)는 사용자가 한 번 인증하고 각 응용 프로그램을 인증하지 않고도 여러 [프로그래머](#programmer) 응용 프로그램에 액세스할 수 있는 프로세스입니다.

### T {#t}

#### TempPass 기본 {#temp-pass-basic}

기본 TempPass는 사용자가 [MVPD](#mvpd)(으)로 인증할 필요 없이 제한된 시간 동안 보호된 콘텐츠에 액세스할 수 있는 Adobe Pass 인증 기능입니다.

자세한 내용은 [Temp Pass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md) 설명서를 참조하세요.

#### TempPass 프로모션 {#temp-pass-promotional}

프로모션 TempPass는 사용자가 [MVPD](#mvpd)로 인증할 필요 없이 최대 리소스 수와 제한된 시간 동안 보호된 콘텐츠에 액세스할 수 있는 Adobe Pass 인증 기능입니다.

자세한 내용은 [프로모션 임시 패스](/help/authentication/integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md) 설명서를 참조하십시오.

#### TL {#ttl}

TTL(time to live)은 기본 엔터티가 유효한 시간을 나타내는 값입니다.

TTL은 [액세스 토큰](#access-token), [프로필](#profile), 권한 부여 [결정](#decision) 또는 [미디어 토큰](#media-token)에 대해 언급될 수 있습니다.

#### TVE {#tve}

TV Everywhere(TVE)는 소비자가 좋아하는 TV 프로그램, 영화 및 기타 컨텐츠를 스마트폰, 태블릿, 노트북 등과 같은 여러 디바이스에서 액세스할 수 있도록 하는 업계 틈새입니다.

#### TVE 대시보드 {#tve-dashboard}

TV Everywhere(TVE) 대시보드는 [프로그래머](#programmer)에게 구성 및 데이터를 관리할 수 있도록 제공되는 Adobe Pass 인증 도구입니다.

자세한 내용은 [TVE 대시보드 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) 설명서를 참조하십시오.

#### TV 공급자 {#tv-provider}

TV 제공자는 케이블, 위성 또는 인터넷 기반 서비스를 통해 소비자에게 TV 서비스를 제공하는 회사이다.

TV 제공자는 TV 제공자와 Adobe 사이의 온보딩 프로세스 동안 정의된 고유 값에 의해 식별된다.

[MVPD](#mvpd) 및 [ID 공급자](#identity-provider)와 동의어입니다.

### U {#u}

#### 사용자 에이전트 {#user-agent}

사용자 에이전트는 웹을 탐색하고 [MVPD](#mvpd) 로그인 페이지를 렌더링할 수 있는 브라우저나 유사한 구성 요소(플랫폼별)를 참조합니다.

#### 사용자 메타데이터 {#user-metadata}

사용자 메타데이터는 [MVPD](#mvpd)에 의해 유지되고 [프로필](#profile)의 일부로 Adobe Pass 인증에서 제공하는 사용자별 특성(예: 우편 번호, 보호자 등급, 사용자 ID 등)을 참조합니다.

자세한 내용은 [사용자 메타데이터](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md) 설명서를 참조하세요.

### V {#v}

#### VSA {#vsa}

VSA(비디오 구독자 계정)는 SSO(Single Sign-On) 사용자 환경을 활성화하기 위해 [프로그래머](#programmer)에게 제공되는 Apple 개발 프레임워크입니다.

자세한 내용은 [비디오 구독자 계정 프레임워크](https://developer.apple.com/documentation/videosubscriberaccount) 및 [파트너 흐름을 사용하는 Single Sign-On](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) 설명서를 참조하십시오.
