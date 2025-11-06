---
title: Adobe Pass Authentication JavaScript 4.0.0 릴리스 노트
description: Adobe Pass Authentication JavaScript 4.0.0 릴리스 노트
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.0.0 릴리스 노트 {#javascript-sdk-400-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 빌드 번호 {#build-number-400}

Adobe Pass 인증: JavaScript 4.0.0

릴리스 날짜: **07/05/2018**

## 릴리스 개요 {#release-overview-400}

* 플랫폼 간의 통합 보안 메커니즘인 동적 클라이언트 등록 메커니즘에 대한 지원이 구현되었습니다. 플랫폼 간 보안 액세스를 관리하는 것은 프로그래머 스스로 TVE 대시보드를 통해 수행할 수 있습니다.
* 모든 기능에 대한 모든 타사 쿠키와 거의 모든 자사 쿠키의 사용을 제거합니다(자사 쿠키가 여전히 사용되는 개별화는 제외 - 향후 제거됨). 따라서 타사 쿠키에 관련된 새로운 브라우저 정책 또는 일반적인 쿠키(예: )와 더 호환되고 향후 보장됩니다. Safari의 ITP). 이전 3.x 릴리스는 자사 쿠키만 사용하여 행복한 흐름에 대해 예상대로 작동하지만, 새 브라우저 버전의 릴리스와 함께 변경될 수 있습니다.
* 프리플라이트 인증 검사에 대한 캐싱 비활성화를 지원합니다.
* 이 버전은 이전 버전과 호환되지 않으며, 새 경로 https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js에서 호스팅됩니다. 이 새로운 JS SDK의 이점을 활용하려면 새 동적 클라이언트 등록 메커니즘을 사용하여 웹 애플리케이션을 등록하고 새 JS SDK URL을 가리키도록 프로그래머로부터 마이그레이션 프로세스가 필요합니다.

## 릴리스 패키지 {#release-package-400}

프로덕션 URL은 https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js입니다.

스테이징 URL은 https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js입니다.
