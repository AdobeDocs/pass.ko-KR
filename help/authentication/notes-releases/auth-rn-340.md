---
title: Adobe Pass Authentication 3.4.0 릴리스 노트
description: Adobe Pass Authentication 3.4.0 릴리스 노트
exl-id: 7a9b8c6d-4e5f-4a3b-8c7d-9e0f1a2b3c4d
source-git-commit: 58da8137988f0146716b56ac7a960c683b204d53
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Adobe Pass Authentication 3.4.0 릴리스 노트 {#authn-340-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-340}

* [빌드 번호](#build-number-340)
* [릴리스 개요](#release-overview-340)

### 빌드 번호 {#build-number-340}

Adobe Pass 인증: adobe-pass-**3.4.0**
릴리스 날짜: **2025/16/09 - 2025/09/18**

### 릴리스 개요 {#release-overview-340}

#### REST API v2

* 사용자 식별 및 추적 기능을 개선하기 위해 [ECID(Experience Cloud ID)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md)에 대한 지원을 추가했습니다.
* REST API V2 [구성 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)에 대해 헤더 AP-Device-Identifier 및 X-Device-Info를 옵션으로 변경했습니다.

#### 버그 수정

* REST API V2 [Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) 응답의 토큰에서 MVPD ID와 프록시 MVPD ID가 역전되는 문제가 수정되었습니다.
* 요청자 ID가 REST API V2 [Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) 응답의 토큰에 없는 문제가 수정되었습니다.
* REST API V2 [의사 결정 사전 승인](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API에 너무 많은 리소스를 전달하면 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) **too_many_resources** 대신 응답에 빈 의사 결정 목록이 발생하는 문제가 해결되었습니다.

#### 기타

* 보안 취약점이 패치되었습니다.