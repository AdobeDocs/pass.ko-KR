---
title: Adobe Pass Authentication JavaScript 4.4.0 릴리스 노트
description: Adobe Pass Authentication JavaScript 4.4.0 릴리스 노트
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.4.0 릴리스 노트 {#javascript-sdk-440-rn}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 빌드 번호 {#build-number-440}

Adobe Pass 인증: JavaScript 4.4.0

릴리스 날짜: **2021/06/22**

## 릴리스 개요 {#release-overview-440}

### 새로운 기능

Preflight 인증

* 새로운 사전 권한 부여 API - 사전 권한 부여 기능에 사용되는 새로운 API 호출로서, 사전 권한 흐름에 대한 향상된 오류 보고도 도입했습니다.
* 이 기능은 Primetime 인증 구성에서 활성화해야 하므로 요청 시 사용할 수 있습니다. 이 기능을 활성화하는 방법에 대한 자세한 내용은 TAM에 문의하십시오.
* checkPreauthorizedResources API를 사용하지 않습니다.

플랫폼 식별

* 모든 SDK 호출에 AP-SDK-Identifier 헤더를 추가하여 SDK 유형 및 버전을 보다 잘 식별합니다.

기타

* 내부 아키텍처 개선.

### 버그 수정

* setRequestor와 getAuthentication이 동시에 호출될 때 생성된 경합 조건을 수정합니다.
* 스테이징 환경에서 권한이 올바르게 로드되지 않는 문제를 해결했습니다.
* Safari 브라우저에서 백그라운드 로그아웃 흐름이 완료되지 않는 문제가 해결되었으므로 페이지 새로 고침이 발생할 때까지 사용자가 여전히 인증된 것으로 보입니다. 시간 제한이 도입되었으며, 현재 30초로 설정되었으므로 이 기간 동안 Primetime 인증 서버에서 응답이 없을 경우 SDK이 setAuthenticationStatus 콜백을 호출합니다.

## 릴리스 패키지 {#release-package-440}

프로덕션 URL은 https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js입니다.

스테이징 URL은 https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js입니다.
