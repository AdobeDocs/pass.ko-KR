---
title: Adobe Pass Authentication 3.0 릴리스 노트
description: Adobe Pass Authentication 3.0 릴리스 노트
exl-id: 9284151a-8458-44a3-937b-35f379ca0e4e
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Adobe Pass Authentication 3.0 릴리스 노트 {#authn-300-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-300}

* [빌드 번호](#build-number-300)
* [릴리스 개요](#release-overview-300)

### 빌드 번호 {#build-number-300}

Adobe Pass 인증: adobe-pass-**3.0**

릴리스 날짜: **09/10/2024 - 09/12/2024**

### 릴리스 개요 {#release-overview-300}

#### REST API v2

##### 코드

* adobe pass-**3.0** 릴리스부터 신규 및 기존 클라이언트 애플리케이션을 새로운 REST API v2로 통합하거나 마이그레이션하여 최신 Adobe Pass 기능을 활용할 수 있습니다.

##### 설명서

* 새 REST API v2로 시작하려면 다음 문서를 참조하십시오.
   * [REST API v2 - API - 개요](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [REST API v2 - 흐름 - 개요](../integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
* REST API v1의 공개 문서에 대한 URL이 변경되었습니다. 다음 문서를 참조하십시오.
   * [REST API v1 - API - 개요](../integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
   * [REST API v1 - API - 참조](../integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)

##### 도구

* 새 REST API v2를 사용하려면 [Adobe Developer](https://developer.adobe.com/adobe-pass) 웹 사이트의 새 Adobe Pass 인증 페이지를 참조하세요.

#### 버그 수정

* 로그아웃 요청에 있을 때 리디렉션 URL 매개 변수가 사용되지 않는 문제를 해결했습니다.
