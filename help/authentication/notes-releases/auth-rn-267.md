---
title: Adobe Pass Authentication 2.67 릴리스 노트
description: Adobe Pass Authentication 2.67 릴리스 노트
exl-id: d899fe96-a273-4681-90a5-bde54cc2f3b3
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 0%

---

# Adobe Pass Authentication 2.67 릴리스 노트 {#authn-267-rn}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-267}

* [빌드 번호](#build-number-267)
* [릴리스 개요](#release-overview-267)

### 빌드 번호 {#build-number-267}

Adobe Pass 인증: adobe-pass-**2.67.0.1**

릴리스 날짜: **09/12/2023 - 09/14/2023**

### 릴리스 개요 {#release-overview-267}

* 새 REST API에 대한 지속적인 내부 업데이트.
* 지속적인 내부 아키텍처 개선.

#### MVPD 업데이트

* Adobe과의 **DirecTV 푸에르토리코** 통합에 대한 업데이트입니다. 자세한 내용은 TAM에 문의하십시오.

#### 버그 수정

* REST API를 사용하는 응용 프로그램과 FireTV SDK 간에 Adobe-Subject-Token 헤더를 사용하여 얻은 SSO를 차단하는 문제를 해결했습니다.
