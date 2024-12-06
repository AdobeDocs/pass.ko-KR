---
title: Adobe Pass Authentication 2.66 릴리스 노트
description: Adobe Pass Authentication 2.66 릴리스 노트
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# Adobe Pass Authentication 2.66 릴리스 노트 {#authn-266-rn}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-266}

* [빌드 번호](#build-number-266)
* [릴리스 개요](#release-overview-266)

### 빌드 번호 {#build-number-266}

Adobe Pass 인증: adobe-pass-**2.66.0.1**
릴리스 날짜: **2023/11/07 - 2023/07/13**

### 릴리스 개요 {#release-overview-266}

이 릴리스에서는 새로운 REST API에 대한 내부 업데이트를 계속 진행했습니다.

#### 버그 수정 {#release-overview-bugfixes-266}

* 로그아웃 요청에서 RelayState 매개 변수가 누락된 SAML 기반 MVPD의 로그아웃 흐름을 수정했습니다. 릴리스 이후 구성 업데이트를 타겟팅하여 영향을 받는 MVPD의 로그아웃 흐름을 복원합니다.
* SOAP 인증 종단점에 대한 구성에서 SSL 인증서를 업데이트하는 기능이 추가되었습니다.
* 일부 ESM 보고서의 프로그래머 필드에 잘못된 데이터가 기록되었던 코너 사례를 수정했습니다.
