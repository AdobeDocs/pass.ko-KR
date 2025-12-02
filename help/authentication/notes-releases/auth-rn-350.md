---
title: Adobe Pass Authentication 3.5.0 릴리스 노트
description: 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 알아봅니다.
source-git-commit: 6ff46a124f5f3c78173028ae3efed68d71ee6e41
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---


# Adobe Pass Authentication 3.5.0 릴리스 노트

마지막 업데이트: 2025년 12월 9일 화:00:00 GMT+0000(협정 세계시)

* 항목:
* 인증

>[!IMPORTANT]
>
> [제품 알림](https://experienceleague.adobe.com/ko/docs/pass/authentication/product-announcements) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

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
