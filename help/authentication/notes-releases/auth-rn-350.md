---
title: Adobe Pass Authentication 3.5.0 릴리스 노트
description: 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 알아봅니다.
exl-id: b196f636-26a5-4974-903e-40b5f8b93a24
source-git-commit: 1cbddf081fc7d57a187c9701e4ade8593baf8759
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Adobe Pass Authentication 3.5.0 릴리스 노트

마지막 업데이트: 2025년 12월 9일 화:00:00 GMT+0000(협정 세계시)

* 항목:
* 인증

>[!IMPORTANT]
>
> [제품 알림](https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-350}

* [빌드 번호](#build-number-350)
* [릴리스 개요](#release-overview-350)

### 빌드 번호 {#build-number-350}

Adobe Pass 인증: adobe-pass-**3.5.0**\
릴리스 날짜: **25/09/2025 - 2025/12/11**

### 릴리스 개요 {#release-overview-350}

#### REST API v2

* 고객이 구현 유형(SDK, REST V1 또는 REST V2)에 따라 이벤트를 구분하는 보고서를 작성할 수 있도록 ESM(&quot;api&quot;)에 새 차원을 추가했습니다.

#### 버그 수정

* TVE 대시보드에 설정된 사용자 지정 성능 저하 메시지가 REST API V2 오류 세부 정보에 표시되지 않는 문제가 해결되었습니다.
* REST API V2에서 인증된 프로필이 만료될 때 `authenticated_profile_expired` 오류 코드가 반환되지 않는 문제를 해결했습니다.
* REST API V2에서 인증 지연 계산 및 preflight TTL 값이 잘못되는 문제를 해결했습니다.
* DCR 토큰이 만료될 때 일관되지 않은 응답 형식이 반환되는 문제가 해결되었습니다.

## 유지 보수 업데이트 - 2026년 2월 {#maintenance-update-february-2026}

Adobe Pass 인증: adobe-pass-**3.5.0.5**\
릴리스 날짜: **2/24/2026 - 02/26/2026**

이 유지 보수 업데이트 릴리스에는 시스템 안정성과 보안을 강화하는 중요한 개선 사항이 포함되어 있습니다.

### 개선 사항

* REST API V2의 프록시화된 MVPD 구성에 대한 인증 저하 처리가 개선되어 MVPD 서비스 중단 중에 보다 일관된 동작이 보장됩니다.
* 보안 제어를 강화하고 전체 시스템 무결성을 향상시키기 위해 URL 매개 변수 유효성 검사 및 리디렉션 처리가 개선되었습니다.
