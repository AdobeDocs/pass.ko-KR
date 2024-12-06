---
title: 인증 토큰 확인
description: 인증 토큰 확인
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# 인증 토큰 확인 {#check-authentication-token}

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

장치에 만료되지 않은 인증 토큰이 있는지 여부를 나타냅니다.

| 엔드포인트 | 호출자: </br>명 | 입력   </br>매개 변수 | HTTP </br>메서드 | 응답 | HTTP </br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/checkauthn | 스트리밍 앱</br></br>또는</br></br>프로그래머 서비스 | 1. 요청자(필수)</br>2.  deviceId(필수)</br>3.  device_info/X-Device-Info(필수)</br>4.  _deviceType_ </br>5  _deviceUser_(사용하지 않음)</br>6.  _appId_(더 이상 사용되지 않음) | GET | 실패한 경우 오류 세부 정보가 포함된 XML 또는 JSON. | 200 - 성공   </br>403 - 성공 없음 |

{style="table-layout:auto"}


| 입력 매개 변수 | 설명 |
| --- | --- |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |
| deviceId | 장치 ID 바이트입니다. |
| device_info/</br></br>X-Device-Info | 스트리밍 장치 정보입니다.</br></br>**참고**: 이 매개 변수는 URL 매개 변수로 device_info를 전달할 수 있지만, 이 매개 변수의 잠재적 크기와 GET URL 길이 제한으로 인해 http 헤더에 X-Device-Info로 전달해야 합니다. </br></br><!--See the full details in [Passing Device and Connection Information](/help/authentication/passing-client-information-device-connection-and-application.md)(/help/authentication/passing-client-information-device-connection-and-application.md)-->. |
| _deviceType_ | 디바이스 유형(예: Roku, PC).</br></br>이 매개 변수가 올바르게 설정된 경우 ESM은 Clientless를 사용할 때 [장치 유형별로 분류된](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) 지표를 제공하므로 Roku, AppleTV, Xbox 등의 다양한 분석 유형을 수행할 수 있습니다.</br></br>자세한 내용은 [Adobe Pass 인증 지표에서 Clientless deviceType 매개 변수를 사용할 때의 이점&#x200B;](/help/authentication/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**참고**&#x200B;을 참조하십시오. device_info가 이 매개 변수를 대체합니다. |
| _deviceUser_ | 장치 사용자 식별자. |
| _appId_ | 애플리케이션 ID/이름입니다.</br>**참고**: device_info가 이 매개 변수를 대체합니다. |

{style="table-layout:auto"}


## 응답(실패한 경우) {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [REST API 참조로 돌아가기](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
