---
title: 두 번째 화면 웹 앱별 인증 흐름 확인
description: 두 번째 화면 웹 앱별 인증 흐름 확인
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 0%

---

# (레거시) 두 번째 화면 웹 앱에서 인증 흐름 확인 {#check-authentication-flow-by-second-screen-web-app}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

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

두 번째 화면 로그인 웹 앱에서는 이 API를 사용하여 Adobe Pass 인증이 MVPD에서 성공적으로 로그인되었음을 확인해야 합니다. 최종 사용자가 장치 콘솔로 이동하여 워크플로우를 계속하도록 지시하는 성공 메시지를 표시하기 전에 이 API를 호출하는 것이 좋습니다.


| 엔드포인트 | 호출자: </br>명 | 입력   </br>매개 변수 | HTTP </br>메서드 | 응답 | HTTP </br>응답 |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{registration code} | 로그인 웹 앱 | 1. 등록 코드 </br>    (경로 구성 요소)</br>2.  요청자 </br>    (필수) | GET | 실패한 경우 오류 세부 정보가 포함된 XML 또는 JSON. | 200 - 성공   </br>403 - 금지됨 |

</br>

| 입력 매개 변수 | 설명 |
| ----------------- | --------------------------------------------------------------------------------------------- |
| 등록 코드 | 인증 흐름 시작 시 사용자가 제공한 등록 코드 값입니다. |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |


### 샘플 응답(오류의 경우) {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

### [REST API 참조로 돌아가기](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
