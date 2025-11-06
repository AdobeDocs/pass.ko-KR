---
title: Roku SSO Cookbook(REST API V2)
description: Roku SSO Cookbook(REST API V2)
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Roku SSO Cookbook(REST API V2) {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

Adobe Pass 인증 REST API V2는 RokuOS에서 실행되는 클라이언트 애플리케이션의 최종 사용자를 위한 Platform SSO(Single Sign-On)를 지원합니다.

이 문서는 높은 수준의 보기를 제공하는 기존 [REST API V2 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)에 대한 확장 역할을 하며, [플랫폼 ID 흐름을 사용하여 Single Sign-On을 구현하는 방법](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)을 설명하는 문서입니다.

## 플랫폼 ID 흐름을 사용한 Roku 단일 사인온 {#cookbook}

Adobe Pass 인증은 Roku와 협력하여 로그인 사용자 경험을 개선하고 TV 가입자를 위한 TV Everywhere 애플리케이션에서 SSO(Single Sign-On)를 용이하게 합니다.

### 사전 요구 사항 {#prerequisites}

플랫폼 ID 흐름을 사용하여 Roku 단일 사인온을 계속 진행하기 전에 Roku SSO가 활성화되어 있는지 확인하십시오. 프로그래머 또는 MVPD이 SSO를 비활성화하도록 요청하지 않는 한, Roku SSO는 기본적으로 활성화됩니다.

각 프로그래머는 [Adobe Pass TVE 대시보드](https://experience.adobe.com/pass/authentication)를 통해 특정 통합을 위해 Roku 플랫폼에서 SSO(Single Sign-On)를 활성화하거나 비활성화할 수 있습니다.

### 워크플로 {#workflow}

**클라이언트-서버**

REST API V2를 통합하기 위해 클라이언트-서버 아키텍처를 활용하는 프로그래머 애플리케이션의 경우 Roku SSO가 수정 없이 원활하게 작동합니다.

RokuOS는 Adobe Pass 인증 종단점으로 전송되는 모든 요청에 두 개의 HTTP 헤더를 자동으로 추가합니다.

**서버 간**

서버 간 아키텍처를 사용하여 REST API V2를 통합하는 프로그래머 애플리케이션의 경우, 프로그래머는 Roku 팀과 조정하여 해당 도메인으로 향하는 모든 API 흐름에 이러한 헤더를 포함하도록 구성해야 합니다.

교차 애플리케이션 및 교차 장치 SSO를 활성화하려면 애플리케이션에서 전달할 때 장치 ID 대신 Roku에서 제공한 구독자 ID를 사용해야 합니다.

자세한 내용은 다음 설명서를 참조하십시오.

* [헤더 - X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [헤더 - AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

필요한 헤더의 형식에 대한 자세한 내용은 Adobe 담당자에게 문의하십시오.

### FAQ {#faqs}

* **SSO는 어떻게 작동합니까?**

  SSO는 동일한 Roku 사용자와 연결된 모든 Roku 장치에서 Adobe Pass 인증으로 구동되는 모든 프로그래머 애플리케이션에서 작동합니다. 모든 MVPD가 Roku SSO를 허용하지는 않습니다.


* **인증 TTL이 변경됩니까?**

  첫 번째 유효한 인증 토큰은 SSO 수행에 사용되며, 이 경우 SSO를 통해 인증될 다른 모든 애플리케이션은 만료될 때까지 동일한 TTL을 사용합니다. 따라서 한 애플리케이션에서 다른 애플리케이션으로 이동할 때 두 번째 애플리케이션은 인증하는 첫 번째 애플리케이션의 TTL을 공유합니다.


* **다른 Adobe 기능이 이전과 같이 작동합니까?**

  모든 Adobe Pass 인증 기능이 이전과 동일하게 작동합니다.


* **프로그래머 옵트인/옵트아웃 프로세스가 Roku 플랫폼에서 SSO의 혜택을 받고 있습니까?**

  이는 Adobe의 TVE Dashboard에서 구성이 변경됩니다. 각 프로그래머는 특정 통합을 위해 Roku 플랫폼에서 SSO를 활성화하거나 비활성화할 수 있습니다.


* **일반적인 문제는 무엇입니까?**

  프로그래머는 Adobe의 REST API를 기반으로 한 현재 구현이 Roku의 platform-SSO를 방해하지 않는지 확인해야 합니다.

  가능한 문제 및 해결 방법 목록은 아래를 참조하십시오.

| 문제 | 가능한 원인 | 가능한 해결 방법 |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Adobe으로 전송된 Roku SSO 헤더 없음 | Adobe Pass 인증 도메인 호출에 HTTPS 대신 HTTP 사용 | HTTPS 사용 |
| MVPD 로고가 표시되지 않음 / SSO 토큰에 대해 업데이트되지 않음 | UI는 로컬 스토리지에 의존 | 애플리케이션은 인증을 확인한 후 UI(및 필요한 경우 로컬 스토리지)를 업데이트해야 합니다. |
| AuthZ가 없을 때 로그아웃이 트리거됨 | 응용 프로그램 디자인 | 백그라운드에서 로그아웃을 수행하지 않도록 애플리케이션을 업데이트해야 합니다. |
