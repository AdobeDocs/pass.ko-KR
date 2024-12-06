---
title: MVPD 개요
description: MVPD 개요
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2734'
ht-degree: 0%

---

# MVPD 개요 {#mvpd-overview}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 소개 {#intro}

이 개요는 멀티채널 MVPD(비디오 프로그래밍 디스트리뷰터)를 위한 것입니다. 킥스타트 및 통합 안내서를 포함한 추가 설명서는 이 문서 끝에 있는 관련 정보 섹션을 참조하십시오.



TV Everywhere(TVE)는 이제 유료 TV 가입자가 집 안팎에서 여러 디바이스를 통해 이미 지불한 콘텐츠에 액세스할 수 있도록 하는 업계 운동으로 잘 알려져 있습니다.  TVE는 유료 TV 공급업체에게 기존 고객 관계를 유지하고 새로운 고객 관계를 가능하게 하는 새로운 기회를 제공합니다. 그러나 이러한 기회들과 함께 도전이 옵니다. TVE 환경에서 프로그래머들은 콘텐츠를 제공하지만 MVPD들은 잠재 시청자들이 유효한 구독자임을 확인하기 위해 고객 정보를 보유한다.



한 프로그래머와 뷰어 인증 및 권한 부여를 조정하는 것은 간단할 수 있지만 수십 명 또는 수백 명의 다른 프로그래머와 조정하는 것은 점점 더 복잡해진다. 그러나 Adobe® Pass를 사용하면 MVPD는 NBCUniversal Media, Turner Broadcasting (TBS, TNT, CNN), Fox Broadcast Networks, Hulu 등과 같은 프로그래머를 포함하여 전체 TVE 생태계에 액세스하기 위해 하나의 간단한 통합만 구현하면 됩니다.  Adobe Pass 인증은 사용자 권한 부여를 간단하고 안전하게 결정하는 통합 프레임워크를 제공합니다.



결론: Adobe Pass 인증은 프로그래머와 MVPD 간의 권한 거래를 안전하게 중개하여 시청자가 구독 콘텐츠에 액세스할 수 있도록 합니다. 즉, Adobe Pass 인증을 통해 적합한 고객이 적합한 콘텐츠에 쉽고 빠르게 액세스할 수 있습니다.


Adobe Pass 인증을 통해 MVPD는 다음을 수신합니다.

프로그래머와의 쉬운 통합.  하나의 통합으로 여러 콘텐츠 소유자로부터 즉각적인 연결을 제공합니다.

향상된 고객 참여  고객이 여러 플랫폼 및 디바이스에서 콘텐츠를 볼 수 있도록 원활한 브랜드 경험을 지원합니다.

보안 인증.  승인된 사용자 및 장치에게만 프리미엄 콘텐츠에 대한 액세스 권한이 부여되는지 확인하고, (선택적으로) 가구 계정당 연결할 수 있는 장치 및 동시 스트림 수를 제한하십시오.

## FAQ {#faq}

Adobe Pass 인증은 얼마나 안전합니까? Adobe Pass 인증 아키텍처의 가장 중요한 우선 순위는 인증된 뷰어만 인증하고 프리미엄 콘텐츠에 대한 액세스 권한을 부여하도록 하는 것입니다. Adobe Pass 인증은 보는 장치에 대한 액세스를 단단히 바인딩하며, 특정 세대의 스트림, 세션 및/또는 장치를 제한하는 데 도움이 될 수 있습니다.


Flash Player이 필요합니까? TV Everywhere용 Adobe Pass 인증은 플레이어 및 플랫폼 종류를 불문하며 Silverlight 및 HTML5를 포함한 모든 재생 애플리케이션과 통합됩니다. 또한 Adobe Pass 인증은 iOS 및 Android을 실행하는 휴대폰 및 태블릿과 같은 장치에 대한 기본 지원을 제공합니다.


Adobe Pass 인증은 어떤 장치를 지원합니까? Adobe Pass 인증은 브라우저 내 보기 환경을 위한 HTML5 웹 키트를 사용하는 거의 모든 장치에서 지원됩니다. 또한 Adobe Pass 인증은 iOS, Android™, Xbox360(더 이상 사용되지 않음) 및 Adobe Air®(더 이상 사용되지 않음) 응용 프로그램을 포함하여 다양한 장치별 플랫폼에 대한 기본 SDK(소프트웨어 개발 키트)를 지속적으로 배포하고 있습니다. 가장 최근, Adobe Pass 인증은 브라우저 페이지를 렌더링할 수 없는 장치(예: &quot;스마트&quot; TV, 셋톱 박스 및 게임 콘솔)에 대해 클라이언트 없는 솔루션을 출시했습니다.  브라우저 페이지를 렌더링하는 기능은 MVPD로 사용자를 인증하기 위한 요구 사항입니다.


Adobe Pass 인증이 TV Everywhere의 새로운 표준을 지원합니까? Adobe Pass 인증은 온라인 소스에서 유료 TV 고객에게 비디오를 제공하기 위한 기술 요구 사항 및 아키텍처를 제공하는 CableLabs OLCA(온라인 컨텐츠 액세스) 사양을 준수합니다. Adobe은 2011년 6월에 공동 CableLabs 상호 작용 테스트 프로젝트에 참여했으며 서비스 공급자 구현을 위한 테스트 프로세스를 통과했습니다. Adobe Pass 인증이 인증을 위한 OLCA 사양에 대해 확인(완료 및 테스트)됩니다. 인증 구성 요소는 완료되었지만 테스트 검증은 현재 CableLabs 테스트 환경의 출시를 기다리고 있습니다. Adobe은 또한 OATC(Open Authentication Technical Consortium)의 활성 구성원이며, 해당 기구의 일부로 여러 분과위원회의 사양 입안 프로젝트에 참여하고 있습니다.



인증이란? 인증은 MVPD가 주어진 사용자가 알려진 고객임을 확인하는 프로세스입니다.



인증이란 무엇입니까? 인증은 MVPD가 인증된 사용자가 주어진 리소스에 대해 유효한 구독을 가지고 있음을 확인하는 프로세스입니다.



## 아키텍처 {#architecture}

Adobe Pass 인증은 MVPD와 프로그래머가 모두 요구하는 비즈니스 규칙에 따라 신속한 백엔드(서버 간) 통합이 가능한 호스팅 서비스입니다. 즉, 모든 당사자를 위한 빠른 시장 출시 시간, 사기를 방지하기 위한 보다 안전한 환경, 더 많은 플랫폼에서 더 많은 사람이 이용할 수 있는 더 많은 TV 콘텐츠로 우수한 고객 경험을 제공합니다.


Adobe Pass 인증은 SaaS(Software as a Service) 모델을 통해 제공되며, 콘텐츠에 대한 권한을 확인하기 위해 최종 사용자, MVPD 및 프로그래머 간에 보다 안전한 통신이 이루어지도록 합니다. 서비스의 핵심 구성 요소는 다음과 같습니다.

서버 측 - 호스팅된 Adobe Pass 인증 서버. 이것은 MVPD의 인증 시스템과 백 채널(서버 간) 통신을 수행하는 응용 프로그램 서버입니다.
클라이언트측:
클라이언트측 Access Enabler - Access Enabler는 프로그래머의 웹 페이지 또는 플레이어 애플리케이션에 로드되는 작은 파일입니다. 프로그래머의 콘텐츠 보기 애플리케이션에 자격 부여 API를 제공하고 Adobe Pass 인증 서버와 통신합니다.
클라이언트 없는 웹 서비스(웹이 아닌 장치의 경우) - 스마트 TV, 게임 콘솔 및 셋톱 박스와 같은 장치에 권한 부여 API를 제공하는 RESTful 웹 서비스.

>[!NOTE]
>
>MVPD로서 웹 서비스는 Adobe Pass 인증에서 인증 및 권한 부여에 대한 요청을 인식하고 필요한 데이터를 예상 형식으로 응답할 수 있어야 합니다.
>

Adobe Pass 인증을 사용하면 고객에게 SSO(Single Sign-On) 인증 및 인증이라고도 하는 통합 ID 관리를 제공할 수 있습니다. Adobe Pass 인증을 사용하면 MVPD에서 해당 인증이 계속 허용되는 한 구독자가 첫 번째 인증 후 다시 로그인할 필요가 없습니다. (일반적으로 30일) 이를 위해 Adobe Pass 인증은 고객을 위한 인증 토큰에 대한 공통 도메인을 제공합니다. 이 인증 상태 정보는 주어진 MVPD와 통합된 모든 참여 사이트에서 사용할 수 있습니다.


현재 대부분의 MVPD와의 Adobe Pass 인증 통합은 기본 인증 표준 중 하나인 SAML 프로토콜을 사용합니다. Adobe Pass 인증은 SAML 아키텍처에서 프록시 서비스 제공자로 작동하고 SAML 인증 응답을 Adobe 공통 도메인의 보안 토큰으로 유지합니다. Adobe Pass 인증은 SAML 2.0을 준수합니다. 그러나 이 시점에서 Adobe Pass 인증은 일반적으로 SAML SSO 솔루션에서 사용되지만 Adobe Pass 인증 아키텍처는 특정 프로토콜에 바인딩되지 않습니다. 따라서 OAuth 2.0 기반 프로토콜 또는 사용자 지정 프로토콜과 같은 새로운 프로토콜에 대한 지원은 시간이 지남에 따라 추가될 수 있습니다.


Adobe은 MVPD의 기술 팀과 협력하여 기존 통합 요구 사항을 충족하도록 Adobe Pass 인증을 구성합니다. &quot;표준&quot; 통합과 최소한의 지원 요구 사항(설명서 및 기본 이메일 지원)을 전제로 하는 경우 MVPD에 대한 통합은 무료입니다. MVPD가 상당한 지원을 요구하거나 시간이 확대되는 경우, 지원 비용이 청구되거나 공급자는 Synacor와 같은 솔루션을 잘 알고 있는 제3자와 협력할 수 있습니다.


Adobe Pass 인증은 다음과 같이 MVPD 비즈니스 논리를 효율적으로 처리할 수 있도록 지원합니다.

승인 요청이 수신될 때 MVPD에 의해 적용될 수 있는 자체 비즈니스 로직의 경우, Adobe은 MVPD가 승인 요청을 수신할 때 비즈니스 로직 집행을 지원하는 데 필요한 데이터를 제공합니다. 이 데이터에는 요청을 하는 사용자의 고유 장치 ID 및 장치의 IP 주소가 포함될 수 있지만 이에 제한되지 않습니다.

사용자 개입 및/또는 Adobe 솔루션에 의한 특정 처리가 필요한 비즈니스 논리의 경우, Adobe은 각 MVPD에 대한 일부 사용자 지정 속성을 유지할 수 있습니다. 이러한 MVPD별 구성/정책에는 최상위 워크플로우의 특정 지점에서 시작할 수 있는 미리 정의된 워크플로우가 포함됩니다. 사용자 지정 속성 지원에 대한 자세한 내용은 Adobe 담당자에게 문의하십시오.

다음 다이어그램은 이러한 Adobe Pass 인증 구성 요소와 MVPD 및 프로그래머의 관계를 보여 줍니다.

![](../assets/high-level-architecture-nflows.png)

*그림: 높은 수준의 아키텍처 및 흐름*

## Adobe Pass 인증 구성 요소 {#components}

다음은 Adobe Pass 인증 에코 시스템의 일부 주요 구성 요소에 대한 개요를 제공합니다. 여기에는 다음이 포함됩니다.

* [Access Enabler/클라이언트 없는 웹 서비스](#ae)
* [Adobe이 호스팅된 백엔드 서버](#backend)
* [토큰](#tokens)

### Access Enabler/클라이언트 없는 웹 서비스 {#ae}

Access Enabler는 사용자와의 모든 인증 및 권한 부여를 용이하게 하며 시스템에서 로컬로 실행됩니다. MVPD로 실제 권한 부여 워크플로를 처리하는 액세스 Enabler이며, 프로그래머는 상위 수준의 웹 페이지나 플레이어 애플리케이션에 대한 책임을 유지합니다.

클라이언트 없는 웹 서비스는 웹 페이지를 렌더링할 수 없는 장치에 대해 Adobe Pass 인증에서 제공합니다.  이러한 장치의 경우 권한 부여 프로세스가 시작되고 스마트 장치에서 컨텐츠를 보는 반면 MVPD를 사용한 인증은 웹 지원 장치(PC, 스마트폰 및 태블릿)에서 발생합니다.

Access Enabler:

* MVPD별 인증 및 권한 부여 워크플로를 시작합니다.
* 프로그래머 리소스/채널당 성공적인 인증 응답을 캐시하여 불필요한 요청 트래픽을 최소화합니다.
* 명시적 장치 등록과 같이, 각 MVPD와 관련된 사전 정의된 워크플로우에 대해 구성할 수 있습니다.
* 다음 양식으로 사용할 수 있습니다.
   * Flash Player 런타임에서 실행할 수 있는 SWF 파일입니다.
   * 브라우저에서 직접 실행되는 JS 파일
   * iOS, Android, Xbox 등 다양한 플랫폼을 위한 기본 Access Enabler.

### Adobe 호스팅 백엔드 서버 {#backend}

Adobe이 호스팅하는 Adobe Pass 인증 백엔드 서버:

* Adobe Pass 인증과 운영자 간의 서버 간 통신이 필요한 MVPD를 사용하여 인증 및 권한 부여 워크플로우를 규정합니다.
* 프로그래머 사이트 및 애플리케이션에 대한 구성을 유지 관리합니다.
* 다운로드 가능한 Access Enabler 구성 요소 파일을 호스팅합니다.
* 인증 및 인증 토큰을 생성합니다.

### 토큰 {#tokens}

Adobe Pass 인증 권한 부여 솔루션은 인증/권한 부여 워크플로가 성공적으로 완료되면 얻게 되는 특정 데이터 조각을 생성하는 데 중점을 둡니다. 이러한 데이터 조각을 토큰이라고 합니다. 이 솔루션은 수명이 제한되어 플랫폼에 종속된 위치에 안전하게 보관됩니다. 만료 시 인증 및/또는 권한 부여 워크플로의 다시 시작을 통해 토큰을 다시 발급해야 합니다.

인증/권한 부여 워크플로 중에 발행되는 세 가지 유형의 토큰이 있습니다. 두 가지 유형은 &quot;장기&quot;로, 사용자의 보기 경험에 연속성을 제공합니다. 세 번째, 단기 토큰은 스트림 리핑을 통해 사기 행위를 완화하기 위한 업계 모범 사례에 대한 지원을 제공합니다. 토큰의 TTL(&quot;time-to-live&quot;) 값은 MVPD와 프로그래머 간의 계약에 따라 설정됩니다. 비즈니스 및 고객에게 가장 적합한 TTL 값을 결정합니다.

**오래 지속된 인증 토큰**. 고객이 Adobe Pass 인증을 사용하여 MVPD 계정에 성공적으로 로그온하면 인증이 성공합니다. 그런 다음 Adobe Pass 인증은 요청 장치에 연결된 장기 인증(&quot;authN&quot;) 토큰과 MVPD에 따라 사용자를 익명으로 식별하는 글로벌 고유 식별자(&quot;GUID&quot;)를 생성합니다.

**오래 지속된 인증 토큰**. 인증에 성공하면 Adobe Pass 인증이 오래 지속되는 인증(&quot;authZ&quot;) 토큰을 만듭니다. 이 토큰은 요청 장치 및 특정 보호된 리소스(예: 채널, 시리즈 또는 에피소드)에 연결되어 있으므로 이동할 수 없습니다. Access Enabler는 장기 AuthZ 토큰을 사용하여 실제 보기 액세스에 사용되는 단기 미디어 토큰을 생성합니다.

**수명이 짧은 미디어 토큰**. 사용자에게 권한이 부여되면 Adobe Pass 인증은 authZ 토큰을 생성하고, 이 토큰을 사용하여 Adobe으로 서명되고 교환 중 변조를 방지하기 위해 암호화된 일회성 단기 미디어 토큰을 생성합니다. 단기 토큰은 Access Enabler API 또는 Clientless 웹 서비스를 통해 포함 사이트에 노출되므로 보호된 리소스에 대한 액세스를 제공하기 전에 프로그래머의 미디어 서버는 Adobe Pass 인증 구성 요소인 미디어 토큰 검증기를 사용하여 토큰을 검증해야 합니다.

## MVPD 통합 라이프사이클 {#lifecycle}

다음 그림은 Adobe Pass 인증과 MVPD 간의 통합의 라이프사이클을 보여줍니다.

![](../assets/mvpd-int-lifecycle.png)

*그림: MVPD 통합 라이프사이클*

## 권한 부여 순서도 {#chart}

다음 순서도는 Adobe Pass 인증을 사용하여 권한을 확인하는 전반적인 프로세스를 보여 줍니다.

![](../assets/authn-authz-entitlmnt-flow.png)

*그림: Adobe Pass 인증을 사용하여 자격 확인 프로세스*

## 인증 단계 {#authn-steps}

다음 단계에서는 Adobe Pass 인증 인증 흐름의 예를 보여 줍니다.  이것은 프로그래머가 사용자가 MVPD의 유효한 고객인지 여부를 결정하는 자격 프로세스의 일부입니다.  이 시나리오에서, 사용자는 MVPD에 대한 유효한 가입자이다.  사용자가 프로그래머 Flash 응용 프로그램을 사용하여 보호된 콘텐츠를 보려고 합니다.

1. 사용자가 프로그래머 Flash 응용 프로그램과 Adobe Pass Authentication Access Enabler 구성 요소를 사용자 컴퓨터에 로드하는 프로그래머 웹 페이지로 이동합니다. Flash 애플리케이션은 Access Enabler를 사용하여 프로그래머의 ID를 Adobe Pass 인증으로 설정하고 Adobe Pass 인증은 Access Enabler를 해당 프로그래머(&quot;요청자&quot;)에 대한 구성 및 상태 데이터로 채웁니다. Access Enabler는 다른 API 호출을 수행하기 전에 서버에서 이 데이터를 받아야 합니다.  기술 참고: 프로그래머는 Access Enabler의 `setRequestor()` 메서드를 사용하여 ID를 설정합니다. 자세한 내용은 [Programmer 통합 안내서](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md)를 참조하십시오.
1. 사용자가 프로그래머로 보호된 콘텐츠를 보려고 하면 프로그래머 애플리케이션은 MVPD의 목록을 사용자에게 제공하고, 사용자는 이 목록에서 공급자를 선택합니다.
1. 사용자는 Adobe Pass 인증 서버로 리디렉션되며, 여기서 사용자가 선택한 MVPD에 대한 암호화된 SAML 요청을 만듭니다. 이 요청은 프로그래머를 대신하여 MVPD에 인증 요청으로 보내진다. MVPD의 시스템에 따라 사용자의 브라우저는 로그인을 위해 MVPD의 사이트로 리디렉션되거나 프로그래머 앱에서 로그인 iFrame이 만들어집니다.
1. 두 경우(리디렉션 또는 iFrame) 모두 MVPD가 요청을 수락하고 해당 로그인 페이지를 표시합니다.
1. 사용자가 MVPD에 로그인하면 MVPD가 사용자의 결제 고객 상태를 확인한 다음 MVPD가 자체 HTTP 세션을 만듭니다.
1. 사용자가 인증되면 MVPD는 응답(SAML 및 암호화)을 만들어 MVPD가 다시 Adobe Pass 인증으로 보냅니다.
1. Adobe Pass 인증은 MVPD 응답을 받고, Adobe Pass 인증 HTTP 세션이 열려 있는지 확인하고, MVPD에서 [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) 응답을 확인하고, 프로그래머 사이트로 다시 리디렉션합니다.
1. 프로그래머 사이트가 다시 로드되고 Access Enabler가 다시 로드되며 프로그래머는 setRequestor()를 다시 호출합니다.  현재 구성이 변경되었기 때문에 setRequestor()에 대한 두 번째 호출이 필요합니다. 이제 Access Enabler에 AuthN 토큰이 서버에서 생성되기를 기다리고 있음을 알리는 플래그가 있습니다.
1. Access Enabler는 보류 중인 인증이 있음을 확인하고 Adobe Pass 인증 서버에서 토큰을 요청합니다. Flash Player의 DRM 기능을 호출하여 서버에서 토큰을 검색합니다.
1. AuthN 토큰은 프로그래머의 Flash Player LSO 캐시에 저장됩니다. 이제 인증이 완료되고 Adobe Pass 인증 서버에서 세션이 제거됩니다.

## 인증 단계 {#authz-steps}

다음 단계는 이전 섹션([인증 단계](#authn-steps))에서 계속됩니다.

1. 사용자가 프로그래머로 보호된 콘텐츠에 액세스하려고 하면 프로그래머 애플리케이션은 먼저 사용자의 로컬 컴퓨터 또는 장치에서 AuthN 토큰을 확인합니다.  해당 토큰이 없으면 위의 [인증 단계](#authn-steps)를 따릅니다.  AuthN 토큰이 있는 경우 인증 흐름은 프로그래머 애플리케이션에서 보호된 콘텐츠의 특정 항목에 대한 사용자의 보기 권한을 가져오도록 요청하여 Access Enabler에 대한 호출을 시작하는 것으로 진행됩니다.
1. 보호된 콘텐츠의 특정 항목은 &quot;리소스 식별자&quot;로 표시됩니다.  이는 단순한 문자열이거나 더 복잡한 구조일 수 있지만, 어떤 경우든 리소스 식별자의 특성은 프로그래머와 MVPD 간에 미리 합의됩니다.  프로그래머 애플리케이션은 리소스 식별자를 Access Enabler에 전달합니다.  Access Enabler는 사용자의 로컬 시스템 또는 디바이스에서 AuthZ 토큰을 확인합니다.  AuthZ 토큰이 없으면 Access Enabler가 요청을 백엔드 Adobe Pass 인증 서버에 전달합니다.
1. Adobe Pass 인증 서버는 표준화된 프로토콜을 사용하여 MVPD 인증 엔드포인트와 통신합니다.  MVPD의 응답이 사용자에게 보호된 콘텐츠를 볼 권한이 있음을 나타내는 경우 Adobe Pass 인증 서버는 AuthZ 토큰을 만들어 다시 Access Enabler에 전달합니다. Access Enabler는 사용자 컴퓨터에 AuthZ 토큰을 저장합니다.
1. 사용자의 시스템 또는 장치에 저장된 AuthZ 토큰을 사용하여 프로그래머 애플리케이션은 Adobe Pass 인증 서버에서 미디어 토큰을 얻기 위해 Access Enabler를 호출하고 해당 토큰을 프로그래머 애플리케이션에 제공합니다.
1. 마지막으로, 프로그래머 애플리케이션은 미디어 토큰 검증기 구성 요소를 사용하여 올바른 사용자가 올바른 콘텐츠를 보고 있는지 확인하고 미디어 토큰을 배치한 상태에서 사용자가 보호된 콘텐츠를 볼 수 있도록 합니다.

<!--
>![RELATEDINFORMATION]
>
>*   Kickstart Guides, [MVPD kickstart](/help/authentication/mvpd-kickstart-guide.md) and [programmer kickstart](/help/authentication/programmer-kickstart-guide.md). These guides explain the initial steps to take to begin integrating with Adobe Pass Authentication.
>
>*   [MVPD Integration Guide](/help/authentication/mvpd-kickstart-guide.md). This is a lower level technical guide for MVPDs, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>
>*   [Overview For Programmers](/help/authentication/programmer-overview.md). The same high level of conceptual information as in this MVPD overview, but directed toward the content providers (Programmers).
-->
