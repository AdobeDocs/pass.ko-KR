---
title: 인증 토큰 검색
description: 인증 토큰 검색
exl-id: 0b010958-efa8-4dd9-b11b-5d10f51f5680
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 0%

---

# (레거시) 인증 토큰 검색 {#retrieve-authorization-token}

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

인증(AuthZ) 토큰을 검색합니다.


| 엔드포인트 | 호출자: </br>명 | 입력   </br>매개 변수 | HTTP </br>메서드 | 응답 | HTTP </br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authz</br></br>예: </br></br>&lt;SP_FQDN>/api/v1/tokens/authz | 스트리밍 앱</br></br>또는</br></br>프로그래머 서비스 | &#x200B;1. 요청자(필수)</br>2.  deviceId(필수)</br>3.  리소스(필수)</br>4.  device_info/X-Device-Info(필수)</br>5.  _deviceType_</br> 6.  _deviceUser_(사용하지 않음)</br>7.  _appId_(더 이상 사용되지 않음) | GET | &#x200B;1. 성공</br>2.  인증 토큰 </br>    찾을 수 없음 또는 만료됨:   </br>    XML 설명 이유 </br>    인증 토큰을 찾을 수 없음</br>3.  인증 토큰 </br>    찾을 수 없음: </br>    XML 설명</br>4.  인증 토큰 </br>    만료됨: </br>    XML 설명 | 200 - 성공 </br>412 - AuthN 없음</br></br>404 - AuthZ 없음</br></br>410 - AuthZ 만료됨 |

{style="table-layout:auto"}

</br>

| 입력 매개 변수 | 설명 |
| --- | --- |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |
| deviceId | 장치 ID 바이트입니다. |
| 리소스 | resourceId(또는 MRSS 조각)가 포함된 문자열은 사용자가 요청한 콘텐츠를 식별하고 MVPD 인증 종단점에서 인식합니다. |
| device_info/</br></br>X-Device-Info | 스트리밍 장치 정보입니다.</br></br>**참고**: 이 매개 변수는 URL 매개 변수로 device_info를 전달할 수 있지만, 이 매개 변수의 잠재적 크기와 GET URL 길이 제한으로 인해 http 헤더에 X-Device-Info로 전달해야 합니다. </br></br>자세한 내용은 [장치 및 연결 정보 전달](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)을 참조하세요. |
| _deviceType_ | 디바이스 유형(예: Roku, PC).</br></br>이 매개 변수가 올바르게 설정된 경우 ESM은 Clientless를 사용할 때 [장치 유형별로 분류된](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) 지표를 제공하므로 Roku, AppleTV 및 Xbox와 같은 다양한 유형의 분석을 수행할 수 있습니다.</br></br>참조, [전달 지표에서 클라이언트 없는 장치 유형 매개 변수를 사용할 때의 이점&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**참고**: device_info가 이 매개 변수를 대체합니다. |
| _deviceUser_ | 장치 사용자 식별자. |
| _appId_ | 애플리케이션 ID/이름입니다. </br></br>**참고**: device_info가 이 매개 변수를 대체합니다. |

{style="table-layout:auto"}


### 샘플 응답 {#response}



#### 성공

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authorization>
        <expires>1348148289000</expires>
        <mvpd>sampleMvpdId</mvpd>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <proxyMvpd>sampleProxyMvpdId</proxyMvpd>
    </authorization>
```



**JSON:**

```JSON
    {
        "mvpd": "sampleMvpdId",
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348148289000",
        "proxyMvpd": "sampleProxyMvpdId"
    }
```

</br>


#### 인증 토큰을 찾을 수 없거나 만료됨:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>412</status>
        <message>User not authenticated</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 412,
        "message": "User not authenticated",
        "details": null
    }
```

</br>


#### 인증 토큰을 찾을 수 없음:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>404</status>
        <message>Not found</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 404,
        "message": "Not Found",
        "details": null
    }
```

</br>



#### 인증 토큰 만료됨:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>410</status>
        <message>Gone</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 410,
        "message": "Gone",
        "details": null
    }
```
