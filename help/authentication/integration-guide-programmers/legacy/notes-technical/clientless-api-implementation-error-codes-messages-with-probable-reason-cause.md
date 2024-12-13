---
title: 클라이언트리스 API 구현 - 오류 코드/가능한 이유/원인이 있는 메시지
description: 클라이언트리스 API 구현 - 오류 코드/가능한 이유/원인이 있는 메시지
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# (레거시) 클라이언트리스 API 구현 - 오류 코드/가능한 이유/원인이 있는 메시지 {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>이 페이지 상의 컨텐츠 는 정보 제공의 목적으로만 제공됩니다. 이 API를 사용하려면 Adobe Systems 의 최신 라이센스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> 제품 공지](/help/authentication/product-announcements.md) 페이지에 집계된 최신 Adobe Pass Authentication 제품 발표 및 서비스 해제 일정에 [대한 정보를 계속 확인하십시오.

</br>


## 오류: 승인되지 않음

### 원인:

1. POST 내에 인증 헤더 누락
1. 권한 부여 헤더 문제 - 요청 시간이 밀리초 단위인지 확인합니다.

## 오류: 인증 중 SC 400

### 원인:

1. 서버가 특정 요청자 및 환경에 대해 작성된 등록 코드를 찾지 못했습니다.
1. 도메인 간 스크립팅 문제가 발생할 수 있습니다
1. /etc/hosts 파일에 적절한 스푸핑을 추가해야 합니다.

## 오류: 400개의 잘못된 요청

### 원인:

1. POST/GET에 대한 URL 형식이 잘못되었습니다.
1. SAMLAssertionParserException – 암호화된 SAML 어설션을 Adobe Systems 측에서 해독할 수 없습니다.

## 오류: 403 사용할 수 없음

### 원인:

1. 빠른 요청이 너무 많음 - DoS 공격을 방지하기 위한 API 관리 기능입니다.
2. prequal 환경을 사용하는 경우 스푸핑을 추가하고, 그렇지 않으면 스푸핑이 / etc / hosts 파일에서 제거되었는지 확인하십시오.

## 오류: MVPD 페이지에 로그인할 수 없습니다.

### 원인:

1. 사용자 이름과 암호가 일치하지 않습니다.
2. 로그인이 비활성화되었을 수 있습니다.
3. 로그인 이유가 프로덕션용인지 스테이징용인지 확인


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
