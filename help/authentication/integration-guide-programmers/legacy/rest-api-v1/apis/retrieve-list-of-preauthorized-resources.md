---
title: 사전 승인된 리소스 목록 검색
description: 사전 승인된 리소스 목록 검색
exl-id: 3821378c-bab5-4dc9-abd7-328df4b60cc3
source-git-commit: 1c357b918fa4f6d4b92a9055de018c55ee5861e0
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 0%

---

# (기존) 사전 승인된 리소스 목록 검색 {#retrieve-list-of-preauthorized-resources}

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

사전 승인된 리소스 목록을 가져오기 위해 Adobe Pass 인증에 대한 요청입니다.

API에는 스트리밍 앱 또는 프로그래머 서비스용 세트와 두 번째 화면 웹 앱용 세트, 이렇게 두 개의 API 세트가 있습니다. 이 페이지에서는 스트리밍 앱 또는 프로그래머 서비스용 API에 대해 설명합니다.


| 엔드포인트 | 호출자: </br>명 | 입력   </br>매개 변수 | HTTP </br>메서드 | 응답 | HTTP </br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/사전 인증 | 스트리밍 앱</br></br>또는</br></br>프로그래머 서비스 | 1. 요청자(필수)</br>2.  deviceId(필수)</br>3.  리소스(필수)</br>4.  device_info/X-Device-Info(필수)</br>5.  _deviceType_</br> 6.  _deviceUser_(사용하지 않음)</br>7.  _appId_(더 이상 사용되지 않음) | GET | 개별 사전 인증 결정 또는 오류 세부 정보가 포함된 XML 또는 JSON입니다. 아래 샘플을 참조하십시오. | 200 - 성공</br></br>400 - 잘못된 요청</br></br>401 - 권한 없음</br></br>405 - 메서드가 허용되지 않음 </br></br>412 - 사전 조건 실패</br></br>500 - 내부 서버 오류 |


| 입력 매개 변수 | 설명 |
| --- | --- |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |
| deviceId | 장치 ID 바이트입니다. |
| 리소스 | 사용자가 액세스할 수 있고 MVPD 권한 부여 종단점에서 인식하는 콘텐츠를 식별하는, 쉼표로 구분된 resourceId 목록이 포함된 문자열입니다. |
| device_info/</br></br>X-Device-Info | 스트리밍 장치 정보입니다.</br></br>**참고**: 이 매개 변수는 URL 매개 변수로 device_info를 전달할 수 있지만, 이 매개 변수의 잠재적 크기와 GET URL 길이 제한으로 인해 http 헤더에 X-Device-Info로 전달해야 합니다. </br></br>자세한 내용은 [장치 및 연결 정보 전달](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)을 참조하세요. |
| _deviceType_ | 디바이스 유형(예: Roku, PC).</br></br>이 매개 변수가 올바르게 설정된 경우 ESM은 Clientless를 사용할 때 [장치 유형별로 분류된](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) 지표를 제공하므로 Roku, AppleTV 및 Xbox와 같은 다양한 유형의 분석을 수행할 수 있습니다.</br></br>참고: [전달 지표에서 클라이언트 없는 장치 형식 매개 변수를 사용할 때의 이점&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**참고**: `device_info`이(가) 이 매개 변수를 대체합니다. |
| _deviceUser_ | 장치 사용자 식별자. |
| _appId_ | 애플리케이션 ID/이름입니다. </br></br>**참고**: device_info가 이 매개 변수를 대체합니다. |



### 샘플 응답 {#sample-response}



**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
  <resource>
    <id>TestStream1</id>
    <authorized>true</authorized>
  </resource>
  <resource>
    <id>TestStream2</id>
    <authorized>false</authorized>
    <error>
      <status>403</status>
      <code>authorization_denied_by_mvpd</code>
      <message>User not authorized</message>
      <details>Your subscription package does not include the "TestStream3" channel.</details>
      <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
      <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
      <action>none</action>
    </error>
  </resource>
</resources>
```

</br>

**JSON:**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
