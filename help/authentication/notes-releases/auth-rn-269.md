---
title: Adobe Pass Authentication 2.69 릴리스 노트
description: Adobe Pass Authentication 2.69 릴리스 노트
exl-id: d031c4c5-dbd5-4a77-b298-a53b992cc4c5
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Adobe Pass Authentication 2.69 릴리스 노트 {#authn-269-rn}

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-269}

* [빌드 번호](#build-number-269)
* [릴리스 개요](#release-overview-269)

### 빌드 번호 {#build-number-269}

Adobe Pass 인증: adobe-pass-**2.69**

릴리스 날짜: **2/27/2024 - 02/29/2024**

### 릴리스 개요 {#release-overview-269}

#### 기타

* 보안 취약점이 패치되었습니다.
* DCR(Dynamic Client Registration)을 사용하여 임시 패스 보안 계층을 재설정하도록 개선되었습니다.
   * 자세한 내용은 다음을 참조하십시오. [TempPass 기능](../integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
* 플랫폼 식별 보고 기능이 개선되었습니다.

#### REST API

* 새로운 REST API에 대한 지속적인 개발.
   * 예정된 전용 릴리스에는 별도의 알림에서 발표될 새 엔드포인트와 흐름이 도입됩니다.
   * 이러한 새 API 사용에 대한 설명서 업데이트가 진행 중입니다.

#### TVE 대시보드

* 새로운 TVE Dashboard에 대한 지속적인 개발.
   * 예정된 전용 릴리스에는 별도의 알림에 발표될 새 TVE 대시보드가 도입됩니다.
   * 이 새로운 TVE 대시보드의 사용에 대한 설명서 업데이트가 진행 중입니다.

#### JavaScript SDK 4.7.0

* 보안 취약성으로 인해 더 이상 사용되지 않는 Access Enabler JavaScript SDK 버전 2.0.1이 제거되었습니다.
   * 자세한 내용은 다음 링크를 참조하십시오. [Adobe Pass 인증 JavaScript 4.7.0 릴리스 노트](authn-rn-javascript-470.md)
