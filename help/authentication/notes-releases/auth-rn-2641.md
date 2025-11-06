---
title: Adobe Pass Authentication 2.64.1 릴리스 노트
description: Adobe Pass Authentication 2.64.1 릴리스 노트
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 0%

---

# Adobe Pass Authentication 2.64.1 릴리스 노트 {#authn-264-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-2641}

* [빌드 번호](#build-number-2641)
* [릴리스 개요](#release-overview-2641)

### 빌드 번호 {#build-number-2641}

Adobe Pass 인증: adobe-pass-**2.64.1**

릴리스 날짜: **01/31/2023 - 02/02/2023**

### 릴리스 개요 {#release-overview-2641}

이번 릴리스에는 다음과 같은 사항이 추가됩니다.

* SAML 어설션에 &quot;in_response_to&quot; 매개 변수가 없는 경우 MVPD의 원치 않는 인증 응답을 차단하는 기능입니다.
* 보안 요구 사항을 준수하기 위해 redirect_url 매개 변수에 대한 유효성 검사가 개선되었습니다.
* 초기 디바이스에서 제공된 정보를 사용하여, 제2 스크린 인증 요청에 대한 디바이스 정보의 개선된 로깅이 제공된다.
