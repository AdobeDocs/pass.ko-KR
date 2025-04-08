---
title: REST API V2 개요
description: REST API V2 개요
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: a02ba4ca1b6579781e40ecd0d12dbfdd23ea7398
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# REST API V2 개요 {#rest-api-v2-overview}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

TVE 애플리케이션의 비용 효율성을 개선하시겠습니까?

여러 플랫폼에서 TVE 애플리케이션을 지원하는 데 필요한 개발 시간과 리소스를 줄이시겠습니까?

플랫폼 간에 일관된 사용자 경험을 보장하시겠습니까?

유지 관리 노력을 줄이고 업데이트, 버그 수정 또는 개선 사항 제공을 단순화하시겠습니까?

## REST API V2 소개 {#rest-api-v2-introduction}

Adobe Pass Authentication은 사용자 경험을 향상하고 Pass 서비스와의 통합을 단순화하기 위해 설계된 REST API V2 출시를 기쁘게 발표합니다.

당사는 당사 플랫폼을 위한 주요 단계를 나타내고 새로운 기능 및 애플리케이션 흐름의 문을 여는 새로운 REST API의 가능성에 대해 기대하고 있습니다.

## 새로운 기능 {#rest-api-v2-whats-new}

모든 플랫폼에서 고유한 구현

이제 고객 애플리케이션은 여러 플랫폼에서 동일한 구현을 사용할 수 있으므로 새로운 기능을 시작하거나 라이브 앱을 유지 관리하는 것이 더 쉬워졌습니다.

### 크로스 디바이스 SSO {#rest-api-v2-cross-device-sso}

REST API V2를 사용하면 인증 세션을 다른 장치 간에 안전하게 전달할 수 있습니다. 디바이스 간 세션을 전달하기만 하면 사용자는 모바일 디바이스에서 인증하고 재인증 없이 TV로 연결된 디바이스에서 비디오를 스트리밍할 수 있습니다.

### 여러 활성 인증 세션 {#rest-api-v2-multiple-active-authentication-sessions}

이제 다양한 활성 MVPD 세션이 가능하며, 고객은 필요한 경우 TempPass와 일반 MVPD 통합 간을 전환할 수 있습니다.

### 향상된 보안 메커니즘 {#rest-api-v2-enhanced-security-mechanism}

Dynamic Client Registration은 모든 플로우 및 기능에서 사용되는 보안 메커니즘입니다. 고객의 애플리케이션을 보다 안전하고 세밀하게 제어할 수 있으며 모든 플랫폼에서 애플리케이션을 등록할 수 있습니다.

### 향상된 성능으로 응답 시간 단축 {#rest-api-v2-improved-performance}

향상된 캐싱 메커니즘을 통해 MVPD에 대한 트래픽을 줄여 응답 시간을 향상시키고 지연을 줄일 수 있습니다. 전체적으로 비디오가 시작될 때까지 API 호출 수가 줄어듭니다.

### 모든 흐름의 향상된 오류 코드 {#rest-api-v2-enhanced-error-codes}

이제 고급 오류 코드를 모든 전달 흐름에서 동일한 형식으로 사용할 수 있으며, 추가 세부 정보를 통해 애플리케이션이 전반적인 사용자 경험을 개선할 수 있습니다.

### 모든 인증 세션에 대한 제어 기능 향상 {#rest-api-v2-improved-control}

새로운 REST API V2는 인증된 여러 세션에 대한 작업을 동시에 허용합니다.

### 유지 관리 비용 절감 {#rest-api-v2-reduce-maintenance-costs}

이제 모든 응답 및 오류 정보가 표준화되었습니다.

## 다음은 무엇입니까? {#rest-api-v2-whats-next}

2025년 말까지 지원을 계속할 계획이므로 현재 SDK 또는 REST 호출을 통해 API를 사용하는 모든 고객은 계속 그렇게 할 수 있습니다.

그러나 이후의 모든 개발은 REST API V2를 기반으로 구축됩니다. 최신 Adobe Pass 기능을 활용하려면 마이그레이션 프로세스를 시작하는 것이 좋습니다.

## 자세히 알아보시겠습니까? {#rest-api-v2-want-to-learn-more}

시작하려면 다음 공개 설명서를 참조하십시오.

- [웨비나](#rest-api-v2-webinar)
- [용어집](rest-api-v2-glossary.md)
- [체크리스트](rest-api-v2-checklist.md)
- [FAQ](rest-api-v2-faqs.md)
- [API](apis/rest-api-v2-apis-overview.md)
- [플로우](flows/rest-api-v2-flows-overview.md)
- 요리책
- 부록
- [최소 시스템 요구 사항](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

또한 전담 지원 팀이 귀하에게 필요한 질문이나 기술 지원을 도와드릴 수 있습니다.

### 웨비나 {#rest-api-v2-webinar}

새로운 REST API V2에 대한 웨비나를 시청하십시오. 여기에서는 새로운 기능에 대한 개요와 함께 작동 중인 REST API V2에 대한 라이브 데모도 제공했습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3457461/?quality=12&learn=on)

## REST API V2를 사용하시겠습니까? {#rest-api-v2-want-to-try}

이제 [Adobe Developer](https://developer.adobe.com/adobe-pass/) 웹 사이트에서 제품 전용 페이지를 통해 REST API V2를 살펴볼 수 있습니다.
