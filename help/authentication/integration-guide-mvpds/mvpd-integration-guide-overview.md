---
title: MVPD 통합 안내서
description: MVPD 통합 안내서
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 07bb12f7983f39b58e1b9795fdaa1bec4f68e674
workflow-type: tm+mt
source-wordcount: '1307'
ht-degree: 0%

---

# MVPD 통합 안내서 {#mvpd-integration-guide}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 통합 안내서는 Adobe® 패스 인증과 통합할 예정인 멀티채널 비디오 프로그래밍 배포자(MVPD)를 위한 것입니다.

TV Everywhere(TVE)는 유료 TV 업계의 혁신적인 이니셔티브로, 가입자가 집에서든 이동 중이든 여러 디바이스에서 이미 지불한 콘텐츠에 액세스할 수 있도록 합니다. TVE는 유료 TV 사업자들에게 기존 고객과의 관계를 강화하고 새로운 고객들에게 문호를 개방할 수 있는 중요한 기회를 제공한다. 그러나 이러한 기회에는 도전이 수반됩니다.

TVE 생태계에서 **프로그래머**&#x200B;는 콘텐츠를 제공하고 **MVPD**(다중 채널 비디오 프로그래밍 배포자)는 시청자가 적격 구독자인지 확인하는 데 필요한 고객 데이터를 관리합니다. 한 명의 프로그래머로 인증과 승인을 조정하는 것은 관리하기 쉬울 수 있지만 수십 명 또는 수백 명의 프로그래머를 사용하면 상당한 복잡성을 초래할 수 있습니다.

여기에서 **Adobe® 인증 전달**&#x200B;이 프로세스를 간소화합니다. MVPD는 전체 TVE 에코시스템에 대한 액세스 권한을 얻기 위해 Adobe Pass과의 통합이 간소화되어야 합니다. 제공되는 통합 프레임워크를 통해 마켓 출시 시간을 단축하고, 사기 행위를 완화할 수 있는 안전한 환경을 제공하며, 여러 플랫폼에서 더 많은 TV 콘텐츠를 제공하여 고객 경험을 향상시킬 수 있습니다.

## TV Everywhere용 Adobe Pass 인증 {#adobe-pass-authentication-for-tv-everywhere}

Adobe Pass 인증은 신속한 백엔드(서버 간) 통합이 가능하도록 설계된 SaaS(Software as a Service) 솔루션으로 작동하며, 멀티채널 비디오 프로그래밍 배포자(MVPD)와 프로그래머 모두의 비즈니스 규칙을 준수합니다.

### 표준 및 프로토콜 {#standards-protocols}

Adobe Pass 인증은 TV Everywhere(TVE)의 새롭게 부상하는 표준 및 프로토콜과 완전히 일치하여 생태계 전반에서 원활한 통합 및 규정 준수를 지원합니다.

* **CableLabs OLCA(온라인 콘텐츠 액세스) 사양**\
  Adobe Pass 인증은 온라인 소스에서 유료 TV 고객에게 비디오 컨텐츠를 제공하기 위한 기술 요구 사항 및 아키텍처를 정의하는 CableLabs OLCA 사양을 준수합니다. Adobe은 2011년 6월 CableLabs 상호 운용성 테스트 프로젝트에 적극적으로 참여하여 서비스 공급자 구현을 위한 테스트 프로세스를 성공적으로 통과했습니다.

Adobe Pass 인증은 여러 프로토콜(예: SAML, OAuth 2.0 등)을 지원하도록 설계되었으며, 이러한 유연성은 진화하는 요구 사항에 따라 사용자 지정 프로토콜을 포함하여 향후 확장을 수행할 수 있습니다.

대부분의 통합에서는 인증을 위한 기본 표준인 SAML(Security Assertion Markup Language) 프로토콜을 사용합니다. Adobe Pass 인증은 SAML 프레임워크 내에서 프록시 서비스 제공업체 역할을 하며 Adobe 공통 도메인에서 보안 토큰으로 SAML 인증 응답을 유지합니다.

결론적으로, Adobe Pass 인증은 OLCA 표준과 밀접하게 일치하도록 설계된 프로토콜과 관계없습니다. 이러한 표준은 프로그래머, MVPD 및 서비스 제공자를 위한 공통 프레임워크를 구축하여 TVE 기능 구현에 대한 일관된 접근 방식을 보장합니다.

### 통합 및 지원 {#integration-support}

Adobe Pass 인증은 MVPD 기술 팀과 협력하여 다음과 같이 특정 요구 사항에 맞게 통합을 구성합니다.

* **표준 통합**\
  설명서 및 기본 이메일 지원을 포함하여 무료로 제공됩니다.

* **향상된 지원**\
  중요한 사용자 지정 또는 신속한 타임라인의 경우 지원 비용이 적용될 수 있습니다.

Adobe Pass 인증은 다음과 같이 MVPD 비즈니스 논리를 효율적으로 처리할 수 있도록 지원합니다.

* **자체 포함된 비즈니스 논리**\
  인증 요청 중에 MVPD에서 전체적으로 적용하는 비즈니스 논리의 경우 Adobe에서 필요한 데이터를 제공합니다. 이 데이터에는 요청을 하는 사용자의 장치에 대한 고유 장치 ID 및 IP 주소가 포함될 수 있지만 이에 제한되지 않습니다.

* **사용자 지정 속성 지원**\
  사용자 개입 또는 Adobe 관련 처리가 필요한 비즈니스 로직의 경우 Adobe에서 각 MVPD에 대한 사용자 지정 구성을 유지 관리할 수 있습니다. 이러한 정책을 사용하면 권한 부여 워크플로의 특정 지점에서 트리거할 수 있는 사전 정의된 워크플로를 사용할 수 있습니다. 자세한 내용은 Adobe 담당자에게 문의하십시오.

## 권한 흐름 {#entitlement-flow}

권한 흐름은 프로그래머(TVE) 애플리케이션이 보호된 콘텐츠를 스트리밍하기 위해 완료해야 하는 일련의 단계입니다. 이 흐름에는 MVPD과의 상호 작용과 관련된 몇 가지 단계가 포함됩니다.

* [인증 단계](#authentication-phase)
* (선택 사항) 사전 인증 단계
* [인증 단계](#authorization-phase)
* 로그아웃 단계

프로그래머(TVE) 애플리케이션에 대한 사용자의 초기 방문 시 자격 흐름은 요약된 시퀀스를 따릅니다. 그러나, 후속 방문에서, 애플리케이션은 인증의 상태 및 적용 가능한 시청 정책들에 기초하여 특정 단계들을 우회할 수 있다.

>[!NOTE]
>
> 이 문서에서는 Adobe Pass 인증이 지원하는 다양한 플랫폼(브라우저, 모바일 장치, TV 연결 장치 등)에서 실행되는 애플리케이션 유형을 포괄적으로 지칭하는 데 TVE(프로그래머) 애플리케이션이 사용됩니다.

### 인증 단계 {#authentication-phase}

다음 단계는 SAML 통합의 경우 높은 수준의 단계에 대해 간략히 설명합니다.

1. **프로그래머 응용 프로그램(웹 사이트) 로드**\
   사용자가 Adobe Pass 인증 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)을(를) 통합하는 프로그래머 애플리케이션(웹 사이트)으로 이동합니다.

1. **보호된 콘텐츠 요청**\
   사용자가 보호된 콘텐츠에 액세스하려고 하면 프로그래머 애플리케이션은 사용자가 선택할 수 있는 MVPD 목록을 표시합니다.

1. **인증 요청 초기화**\
   MVPD 선택 시 사용자는 Adobe Pass 인증 서버로 리디렉션됩니다. 여기서, 선택된 MVPD에 대한 암호화된 SAML 인증 요청은 SAML 통합의 경우에 생성된다. 이 요청은 프로그래머를 대신하여 MVPD으로 전송됩니다. MVPD 시스템에 따라 사용자의 브라우저가 MVPD의 로그인 페이지로 리디렉션되거나 로그인 iFrame이 프로그래머 애플리케이션 내에 포함됩니다.

1. **MVPD 로그인**\
   MVPD은 요청을 수락하고 리디렉션 또는 iFrame을 통해 로그인 인터페이스를 제공합니다.

1. **사용자 로그인 및 유효성 검사**\
   사용자가 MVPD 자격 증명으로 로그인합니다. MVPD은 사용자의 구독 상태를 확인하고 자체 HTTP 세션을 설정합니다.

1. **Adobe Pass 인증에 대한 MVPD 응답**\
   유효성 검사가 완료되면 MVPD은 SAML 응답(암호화됨)을 생성하여 Adobe Pass 인증으로 다시 보냅니다.

1. **프로필 생성**\
   Adobe Pass 인증은 SAML 응답을 확인하고, 캐시되는 사용자 프로필을 생성하고, 사용자를 프로그래머 애플리케이션(웹 사이트)으로 다시 리디렉션합니다.

### 인증 단계 {#authorization-phase}

**높은 수준의 단계**

다음 단계는 상위 레벨 단계에 대한 개요입니다.

1. **리소스 식별자 처리**\
   보호된 콘텐츠는 [리소스 식별자](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)로 식별됩니다. 이 식별자는 간단한 문자열이거나 더 복잡한 구조일 수 있습니다. 이 식별자는 사전 정의되어 있으며 프로그래머와 MVPD이 동의합니다. 프로그래머 응용 프로그램에서 Adobe Pass 인증 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)에 리소스 식별자를 보냅니다.

1. **MVPD 인증 확인**\
   Adobe Pass 인증 서버는 표준화된 프로토콜을 사용하여 MVPD의 인증 엔드포인트와 통신합니다.

1. **Adobe Pass 인증에 대한 MVPD 응답**\
   유효성 검사가 완료되면 MVPD은 사용자에게 콘텐츠에 액세스할 권한이 있는지 여부를 확인하고 응답을 다시 Adobe Pass 인증으로 보냅니다.

1. **결정 및 미디어 토큰 생성**\
   Adobe Pass 인증은 응답을 확인하고 캐시되는 [결정](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)을 생성하며 미디어 토큰이 포함된 결정을 프로그래머 애플리케이션(웹 사이트)으로 다시 반환합니다.

1. **콘텐츠 액세스 확인**\
   프로그래머 응용 프로그램은 [미디어 토큰 검증기](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)를 사용하여 올바른 사용자가 올바른 콘텐츠에 액세스하고 있는지 확인합니다. 유효성을 검사하면 사용자에게 보호된 콘텐츠를 볼 수 있는 액세스 권한이 부여됩니다.

## 권한 이해 {#understanding-entitlements}

Adobe Pass 인증 솔루션은 인증 및 권한 부여 워크플로의 성공적인 완료 시 생성되는 특정 데이터인 권한 생성을 중심으로 다룹니다. 이러한 권한은 보호된 콘텐츠에 대한 액세스 권한을 부여하지만 사용 기간은 제한됩니다. 권한이 만료되면 인증 또는 권한 부여 프로세스를 다시 시작하여 갱신해야 합니다.

권한에 대한 자세한 내용은 다음 문서를 참조하십시오.

* **프로필**

  인증에 성공하면 Adobe Pass 인증은 요청하는 애플리케이션, 장치 및 서비스 공급자 식별자(요청자 식별자)와 연결된 인증된 프로필(&quot;long-lived&quot;)을 만듭니다.

* **[사용자 메타데이터](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  인증에 성공하면(그리고 경우에 따라 인증 후에도) Adobe Pass 인증은 MVPD에서 사용자 메타데이터를 수신하여 이를 요청하는 애플리케이션에 노출할 수 있습니다.

* **[결정](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  승인이 성공하면 Adobe Pass 인증은 요청하는 애플리케이션, 디바이스, 서비스 공급자 식별자(요청자 식별자) 및 특정 보호된 리소스(리소스 식별자)와 관련된 권한 부여 결정(&quot;long-lived&quot;)을 만듭니다.

* **[미디어 토큰](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  인증에 성공하면 Adobe Pass 인증은 성공적인 재생 요청과 연결된 미디어 토큰(&quot;단기&quot;)을 만들고 사기(예: 스트림 복사)를 줄이기 위한 업계 모범 사례에 대한 지원을 제공합니다.

프로필 및 의사 결정에 대한 TTL(time-to-live) 값은 프로그래머 및 유료 TV 공급자 간의 합의에 따라 설정되며, 프로그래머는 관련 있는 모든 사람에게 가장 적합한 값에 동의합니다.
