---
title: Adobe Pass Authentication 2.65.1 릴리스 노트
description: Adobe Pass Authentication 2.65.1 릴리스 노트
exl-id: 28d112db-b038-4d11-93c5-d6ab67a29700
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 0%

---

# Adobe Pass Authentication 2.65.1 릴리스 노트 {#authn-2651-rn}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-2651}

* [빌드 번호](#build-number-2651)
* [릴리스 개요](#release-overview-2651)

### 빌드 번호 {#build-number-2651}

Adobe Pass 인증: adobe-pass-**2.65.1**
릴리스 날짜: **2023/06/20 - 2023/06/22**

### 릴리스 개요 {#release-overview-2651}

이번 릴리스에는 다음과 같은 사항이 추가됩니다.

이 릴리스부터 전체 에코시스템을 잘못된 사용으로부터 보호하기 위해 모든 API에 속도 제한 규칙을 추가하는 기술 지원을 도입할 예정입니다.

초기 목표는 적절한 제한 값을 설정하기 위해 애플리케이션 동작을 모니터링하는 것이며, 이 릴리스의 일부로 제한이 적용되지 않습니다. 특정 애플리케이션에서 생성된 트래픽이 특정 제한을 초과하는 경우 각 고객과 협력하여 구현을 검증합니다.

모든 고객의 애플리케이션에서 생성된 트래픽의 유효성을 검사한 후 속도 제한 규칙이 올해 말에 적용됩니다.
