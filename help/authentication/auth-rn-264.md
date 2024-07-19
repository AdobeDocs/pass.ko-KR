---
title: Adobe Pass Authentication 2.64 릴리스 노트
description: Adobe Pass Authentication 2.64 릴리스 노트
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---

# Adobe Pass Authentication 2.64 릴리스 노트 {#authn-264-rn}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#ss-web-clients}

* [빌드 번호](#build-no-264)
* [새로운 기능](#new-featres-264)
* [버그 수정](#bug-fixes-264)

### 빌드 번호 {#build-no-264}

Adobe Pass 인증: adobe-pass-**2.64**

릴리스 날짜: **11/08/2022 - 11/10/2022**

### 새로운 기능 {#new-featres-264}

* 인프라 업데이트, 서버 응답 시간 단축, 시스템 전반적인 성능 향상
* 새로운 플랫폼 식별 메커니즘에 대한 개선 사항.
* SAML 어설션에 &quot;in_response_to&quot; 매개 변수가 없는 MVPD의 원치 않는 인증 응답을 차단하는 기능입니다.

### 버그 수정 {#bug-fixes-264}

* 일부 기존 TempPass 토큰의 잘못된 포맷을 수정했습니다.
* 두 번째 화면 인증 흐름에서 사소한 문제가 해결되었습니다.
