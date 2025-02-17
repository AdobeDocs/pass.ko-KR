---
title: Adobe Pass Authentication 2.64.1 릴리스 노트
description: Adobe Pass Authentication 2.64.1 릴리스 노트
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---

# Adobe Pass Authentication 2.64.1 릴리스 노트 {#authn-264-rn}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 서버측 및 웹 클라이언트 {#server-side-web-clients-2641}

* [빌드 번호](#build-number-2641)
* [릴리스 개요](#release-overview-2641)

### 빌드 번호 {#build-number-2641}

Adobe Pass 인증: adobe-pass-**2.64.1**

릴리스 날짜: **01/31/2023 - 02/02/2023**

### 릴리스 개요 {#release-overview-2641}

이번 릴리스에는 다음과 같은 사항이 추가됩니다.

* SAML 어설션에 &quot;in_response_to&quot; 매개 변수가 없는 경우 MVPD의 원치 않는 인증 응답을 차단하는 기능입니다.
* 보안 요구 사항을 준수하기 위해 redirect_url 매개 변수에 대한 유효성 검사가 개선되었습니다.
* 초기 디바이스에서 제공된 정보를 사용하여, 제2 스크린 인증 요청에 대한 디바이스 정보의 개선된 로깅이 제공된다.
