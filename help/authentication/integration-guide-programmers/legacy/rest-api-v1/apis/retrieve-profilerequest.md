---
title: Platform SSO 프로필 요청 검색
description: Platform SSO 프로필 요청 검색
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 0%

---

# (레거시) Platform SSO 프로필 요청 검색 {#retrieve-platform-sso-profile-request}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

>[!NOTE]
>
> REST API 구현은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md)에 의해 제한됩니다.

## REST API 끝점 {#clientless-endpoints}

&lt;레지스트리_FQDN>:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 설명 {#description}

이 리소스는 요청자 ID 및 MVPD 튜플에 대한 프로필 요청을 생성합니다.


| 엔드포인트 | 호출자: </br>명 | 입력   </br>매개 변수 | HTTP </br>메서드 | 응답 | HTTP </br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | 스트리밍 앱</br></br>또는</br></br>프로그래머 서비스 | 1. 요청자(경로 매개 변수)</br>2. mvpd(경로 매개 변수)</br>3. deviceType(필수) | GET | 클라이언트 애플리케이션에 대한 실제 페이로드가 불투명하기 때문에 응답 Content-Type은 application/octet-stream이 됩니다.</br></br>프로필 SSO를 가져오려면 응용 프로그램에서 </br></br>SSO 엔진에 응답을 전달해야 합니다. | 200 - 성공   </br>400 - 잘못된 요청 |


| 입력 매개 변수 | 설명 |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |
| mvpd | 이 작업이 유효한 MVPD ID입니다. |
| deviceType | 프로필 요청을 가져오려는 Apple 플랫폼입니다.  **iOS** 또는 **tvOS**&#x200B;입니다. |
