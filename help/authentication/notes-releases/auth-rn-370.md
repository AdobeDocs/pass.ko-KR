---
title: Adobe Pass Authentication 3.7.0 릴리스 노트
description: Adobe Pass Authentication 3.7.0 릴리스 노트
source-git-commit: 89b5fbd8e8510cbf84ce7908e8cf86551e7a0cb9
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Adobe Pass Authentication 3.7.0 릴리스 노트 {#authn-370-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-370}

* [빌드 번호](#build-number-370)
* [릴리스 개요](#release-overview-370)

### 빌드 번호 {#build-number-370}

Adobe Pass 인증: adobe-pass-**3.7.0.2**\
릴리스 날짜: **05/12/2026 - 05/14/2026**

### 릴리스 개요 {#release-overview-370}

이 릴리스는 MVPD 통합 업그레이드, 버그 수정 및 TVE 대시보드 개선 사항에 중점을 둡니다.

#### MVPD 통합

* OAuth2 기반 MVPD 인증에 대한 PKCE 지원이 추가되었습니다.

#### 개선 사항

* TVE Dashboard 버전 1.5.1이 릴리스되었습니다.

#### 버그 수정

* Apple SSO에서 특정 MVPD 구성 불일치를 간과하는 문제를 해결했습니다.
* 응답 헤더의 잘못된 문자로 인해 권한 부여 거부 시 HTTP 500 오류가 발생하는 문제를 해결했습니다.
