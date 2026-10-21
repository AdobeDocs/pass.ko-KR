---
title: Adobe Pass Authentication 3.10.0 릴리스 노트
description: Adobe Pass Authentication 3.10.0 릴리스 노트
hold: true
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: b754c97fc3c045e9511d07e22e0cc5387306a67d
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%
---
# Adobe Pass Authentication 3.10.0 릴리스 노트 {#authn-3100-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-3100}

* [빌드 번호](#build-number-3100)
* [릴리스 개요](#release-overview-3100)

### 빌드 번호 {#build-number-3100}

Adobe Pass 인증: adobe-pass-**3.10.0**\
릴리스 날짜: **10/27/2026 - 10/29/2026**

### 릴리스 개요 {#release-overview-3100}

이 릴리스는 REST API V2 및 MVPD 구성 관리를 위한 SSO(Single Sign-On) 제어, 사용자 메타데이터 개선 및 안정성 개선에 중점을 둡니다.

#### 개선 사항

* 서비스 토큰 SSO(Single Sign-On)에 대한 통합 수준 제어를 추가했습니다. 통합에 대해 명시적으로 활성화된 경우에만 서비스 토큰 SSO가 사용됩니다.
* 인증, 권한 부여 및 프리플라이트 중에 사용자 지정 사용자 메타데이터(예: `packageType`)에 대한 지원을 추가했습니다. 이제 REST API V2 권한 부여 및 Preflight 결정에 메타데이터 액세스 및 암호화 설정을 준수하면서, 캐시된 응답을 포함하는 구성된 사용자 메타데이터가 포함됩니다.

#### 버그 수정

* SAML을 통해 OAuth2 MVPD를 사용하는 통합에 대해 잘못된 SAML 응답 오류가 발생하여 REST API V2 Apple SSO 프로필 요청이 실패하는 문제를 해결했습니다.
* 동일한 리소스에 대한 프리플라이트 및 권한 부여 결정이 서로의 캐시된 만료 시간 및 사용자 메타데이터를 덮어쓸 수 있는 문제를 수정했습니다.
* 프록시화된 MVPD를 삭제하면 참조되지 않은 플랫폼 설정이 남아서 후속 구성이 저장되지 않는 문제가 해결되었습니다.
