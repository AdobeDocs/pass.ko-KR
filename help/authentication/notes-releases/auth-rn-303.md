---
title: Adobe Pass Authentication 3.0.3 릴리스 노트
description: Adobe Pass Authentication 3.0.3 릴리스 노트
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Adobe Pass Authentication 3.0.3 릴리스 노트 {#authn-303-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-303}

* [빌드 번호](#build-number-303)
* [릴리스 개요](#release-overview-303)

### 빌드 번호 {#build-number-303}

Adobe Pass 인증: adobe-pass-**3.0.3**

릴리스 날짜: **20/29/10 - 2024/10/31**

### 릴리스 개요 {#release-overview-303}

#### REST API v2

##### 코드

* REST API V2 개선 사항(Adobe Pass 3.0 메이저 릴리스 이후 [REST API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* 인증 코드 유효성에 대한 정보를 반환하기 위해 `notBefore`에 `notAfter` 및 `/sessions/{code}` 필드를 추가했습니다.
* 플랫폼 식별이 개선되었습니다.

##### 설명서

* 새 REST API v2로 시작하려면 [REST API v2 개요](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) 문서를 참조하세요.

##### 도구

* 새 REST API v2를 사용하려면 [Adobe Developer](https://developer.adobe.com/adobe-pass) 웹 사이트의 새 Adobe Pass 인증 페이지를 참조하세요.
