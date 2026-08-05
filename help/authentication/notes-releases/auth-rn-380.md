---
title: Adobe Pass Authentication 3.8.0 릴리스 노트
description: Adobe Pass Authentication 3.8.0 릴리스 노트
hold: true
source-git-commit: ce9e8de3d69699d03cf68c86be1bb811967501dc
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# Adobe Pass Authentication 3.8.0 릴리스 노트 {#authn-380-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-380}

* [빌드 번호](#build-number-380)
* [릴리스 개요](#release-overview-380)

### 빌드 번호 {#build-number-380}

Adobe Pass 인증: adobe-pass-**3.8.0**\
릴리스 날짜: **08/11/2026 - 08/13/2026**

### 릴리스 개요 {#release-overview-380}

이 릴리스는 Adobe Pass 인증 서비스 전반에 걸친 안정성, 개선 사항 및 보안 업데이트에 중점을 둡니다.

#### 버그 수정

* deviceId의 특정 잘못된 문자로 인해 V2 API에서 HTTP 500 오류가 발생하는 문제를 해결했습니다.

#### 개선 사항

* 롤링 토큰 갱신을 지원하도록 새로 고침 토큰 처리가 개선되었습니다.
* 분석을 위한 보조 장치에서 visitorId 인식이 개선되었습니다.
* 보안 제어를 강화하고 전체 시스템 무결성을 향상하기 위해 URL 매개 변수 유효성 검사가 개선되었습니다.
* TVE Dashboard 버전 1.5.2(부수적 UI 개선 사항 포함).
