---
title: Adobe Pass Authentication 3.9.0 릴리스 노트
description: Adobe Pass Authentication 3.9.0 릴리스 노트
hold: true
source-git-commit: 5ca8f29764a07ddb68abb36accb12cfb3b68b72d
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Adobe Pass Authentication 3.9.0 릴리스 노트 {#authn-390-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-390}

* [빌드 번호](#build-number-390)
* [릴리스 개요](#release-overview-390)

### 빌드 번호 {#build-number-390}

Adobe Pass 인증: adobe-pass-**3.9.0.1**\
릴리스 날짜: **09/08/2026 - 09/10/2026**

### 릴리스 개요 {#release-overview-390}

이 릴리스는 REST API V2 및 ESM 지표 개선 사항에 중점을 둡니다.

#### 개선 사항

* OAuth2로 구성된 MVPD에 대해 올바른 인증 요청이 반환되도록 REST API V2 Partner Single Sign-On이 개선되었습니다.
* 권한 부여가 실패할 때 빈 응답 대신 명확한 오류 응답을 반환하도록 REST API V2 의사 결정이 개선되었습니다.
* 시각적으로 모호한 문자를 방지하고 코드를 보다 쉽게 읽고 올바르게 입력할 수 있도록 등록 코드 생성이 개선되었습니다.
* Preflight AuthZ 지표를 지원하는 ESM 대시보드 개선 사항.
