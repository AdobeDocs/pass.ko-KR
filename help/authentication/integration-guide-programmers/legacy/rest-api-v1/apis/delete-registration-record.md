---
title: 등록 레코드 삭제
description: 등록 리소스 삭제
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 1%

---

# (기존) 등록 레코드 삭제 {#delete-registration-record}

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


## 설명 {#delete-record}

정규 코드 레코드를 삭제하고 재사용을 위해 정규 코드를 해제합니다.

| 엔드포인트 | 호출자: </br>명 | 입력   </br>매개 변수 | HTTP </br>메서드 | 응답 | HTTP </br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>예:</br></br>&lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | 스트리밍 앱</br></br>또는</br></br>프로그래머 서비스 | 1. 요청자 ID </br>    (경로 구성 요소)</br>2.  등록 코드 </br>    (경로 구성 요소) | DELETE | 없음 | 204 |

{style="table-layout:auto"}

</br>

| 입력 매개 변수 | 설명 |
| --- | --- |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |
| 등록 코드 | 스트리밍 장치에 표시될(인증 흐름에 입력될) 등록 코드 값입니다. |

{style="table-layout:auto"}

</br>

### [REST API 참조로 돌아가기](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
