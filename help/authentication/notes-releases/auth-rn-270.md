---
title: Adobe Pass Authentication 2.70 릴리스 노트
description: Adobe Pass Authentication 2.70 릴리스 노트
exl-id: 81713f8e-bc51-4057-9b00-6a2d6c83cd02
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Adobe Pass Authentication 2.70 릴리스 노트 {#pt-authn-270-rn}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-270}

* [빌드 번호](#build-number-270)
* [새로운 기능](#new-features-270)

### 빌드 번호 {#build-number-270}

Adobe Pass 인증: adobe-pass-**2.70**
릴리스 날짜: **04/23/2024 - 04/25/2024**

### 새로운 기능 {#new-features-270}

#### 기타 {#misc}

* 보안 취약점이 패치되었습니다.
* API 서비스 저하가 개선되었습니다.
   * DCR을 성능 저하 API에 대한 보안 메커니즘으로 사용합니다.
   * 자세한 내용은 [저하 API 개요](../integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md)를 참조하십시오.

#### REST API {#rest-apis}

* 새로운 REST API에 대한 지속적인 개발.
   * 예정된 전용 릴리스에는 별도의 알림에서 발표될 새 엔드포인트와 흐름이 도입됩니다.
   * 이러한 새 API 사용에 대한 설명서 업데이트가 진행 중입니다.
