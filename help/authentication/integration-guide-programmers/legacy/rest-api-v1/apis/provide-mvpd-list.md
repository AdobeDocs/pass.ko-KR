---
title: MVPD 목록 제공
description: MVPD 목록 제공
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 2%

---

# (레거시) MVPD 목록 제공 {#provide-mvpd-list}

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

요청자에 대해 구성된 MVPD 목록을 반환합니다.

| 엔드포인트 | 호출자: </br>명 | 입력   </br>매개 변수 | HTTP </br>메서드 | 응답 | HTTP </br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br>예: </br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestorId | Adobe Pass 인증 | 1. 요청자</br>    (경로 구성 요소)</br>_2.  deviceType(더 이상 사용되지 않음)_ | GET | MVPD 목록이 포함된 XML 또는 JSON. | 200 |

{style="table-layout:auto"}


| 입력 매개 변수 | 설명 |
| --------------- | ------------------------------------------------------------- |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |
| *deviceType* | 장치 유형. |

{style="table-layout:auto"}

### 샘플 응답 {#sample-response}

/config 서블릿에 대한 기존 MVPD XML 응답과 동일

참고: Platform SSO를 사용하도록 구성된 모든 MVPD는 해당 노드(JSON/XML) 내에 다음과 같은 추가 속성을 갖게 됩니다.

* 이 MVPD이 Platform SSO를 통해 통합되었는지 여부를 나타내는 **enablePlatformServices(부울):** 플래그
* **boardingStatus(문자열):** MVPD에서 Platform SSO를 완전히 지원하는지(지원됨) 또는 MVPD이 플랫폼 선택기(PICKER)에만 나타나는지 여부를 나타내는 플래그입니다.
* **displayInPlatformPicker(부울):** 이 MVPD이 플랫폼 선택기에 표시되어야 합니다.
* **platformMappingId(문자열):** 플랫폼에서 인식하는 이 MVPD의 식별자입니다.
* **requiredMetadataFields(문자열 배열):** 성공적인 로그인 시 사용할 수 있어야 하는 사용자 메타데이터 필드
