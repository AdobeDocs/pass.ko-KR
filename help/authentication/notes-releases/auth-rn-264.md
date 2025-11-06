---
title: Adobe Pass Authentication 2.64 릴리스 노트
description: Adobe Pass Authentication 2.64 릴리스 노트
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Adobe Pass Authentication 2.64 릴리스 노트 {#authn-264-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-264}

* [빌드 번호](#build-number-264)
* [릴리스 개요](#release-overview-264)

### 빌드 번호 {#build-number-264}

Adobe Pass 인증: adobe-pass-**2.64**

릴리스 날짜: **11/08/2022 - 11/10/2022**

### 릴리스 개요 {#release-overview-264}

* 인프라 업데이트, 서버 응답 시간 단축, 시스템 전반적인 성능 향상
* 새로운 플랫폼 식별 메커니즘에 대한 개선 사항.
* SAML 어설션에 &quot;in_response_to&quot; 매개 변수가 없는 MVPD의 원치 않는 인증 응답을 차단하는 기능입니다.

#### 버그 수정

* 일부 기존 TempPass 토큰의 잘못된 포맷을 수정했습니다.
* 두 번째 화면 인증 흐름에서 사소한 문제가 해결되었습니다.
