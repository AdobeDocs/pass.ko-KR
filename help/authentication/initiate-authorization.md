---
title: 인증 시작
description: 인증 시작
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# 인증 시작 {#initiate-authorization}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## REST API 끝점 {#clientless-endpoints}

&lt;reggie_fqdn>:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 설명 {#description}

인증 응답을 가져옵니다.

| 엔드포인트 | 호출됨  </br>작성자: | 입력   </br>매개 변수 | HTTP  </br>방법 | 응답 | HTTP  </br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authorize | 스트리밍 앱</br></br>또는</br></br>프로그래머 서비스 | 1. 요청자(필수)</br>2.  deviceId(필수)</br>3.  리소스(필수)</br>4.  device_info/X-Device-Info (필수)</br>5.  _deviceType_</br> 6.  _deviceUser_ (사용하지 않음)</br>7.  _appId_ (사용하지 않음)</br>8.  추가 매개 변수(선택 사항) | GET | 실패한 경우 인증 세부 정보 또는 오류 세부 정보가 포함된 XML 또는 JSON입니다. 아래 샘플을 참조하십시오. | 200 - 성공  </br>403 - 성공 없음 |

{style="table-layout:auto"}

</br>


| 입력 매개 변수 | 설명 |
| --- | --- |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |
| deviceId | 장치 ID 바이트입니다. |
| 리소스 | resourceId(또는 MRSS 조각)가 포함된 문자열은 사용자가 요청한 콘텐츠를 식별하며 MVPD 인증 종단점에서 인식됩니다. |
| device_info/</br></br>X-Device-Info | 스트리밍 장치 정보입니다.</br></br>**참고**: URL 매개 변수로 device_info를 전달할 수 있지만, 이 매개 변수의 잠재적 크기와 GET URL 길이 제한으로 인해 http 헤더에서 X-Device-Info로 전달해야 합니다. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | 디바이스 유형(예: Roku, PC).</br></br>이 매개 변수가 올바르게 설정되면 ESM은 다음과 같은 지표를 제공합니다. [장치 유형별 분류](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) 예: Roku, AppleTV, Xbox 등에 대해 다양한 유형의 분석을 수행할 수 있도록 Clientless를 사용할 때</br></br>다음을 참조하십시오 [전달 지표에서 클라이언트 없는 장치 유형 매개 변수의 이점&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**참고**: device_info가 이 매개 변수를 대체합니다. |
| _deviceUser_ | 장치 사용자 식별자. |
| _appId_ | 애플리케이션 ID/이름입니다. </br></br>**참고**: device_info가 이 매개 변수를 대체합니다. |
| 추가 매개 변수 | 호출에는 다음과 같은 다른 기능을 활성화하는 선택적 매개 변수도 포함될 수 있습니다.</br></br>* generic_data - 다음을 사용할 수 있습니다. [프로모션 TempPass](/help/authentication/promotional-temp-pass.md)</br></br>예: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**스트리밍 장치 IP 주소**</br>
>클라이언트-서버 구현의 경우 스트리밍 장치 IP 주소는 이 호출과 함께 암시적으로 전송됩니다.  서버 간 구현의 경우 **regcode** 호출은 스트리밍 장치가 아닌 프로그래머 서비스에서 수행되며, 스트리밍 장치 IP 주소를 전달하려면 다음 헤더가 필요합니다.</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>위치 `<streaming\_device\_ip>` 는 스트리밍 장치 공용 IP 주소입니다.</br></br>
>예 :</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### 샘플 응답 {#sample-response}

* **사례 1: 성공**
</br>
  * **XML:**
  </br>

    &quot;XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
    &quot;



* **JSON:**

  ```JSON
  {
    "mvpd": "sampleMvpdId",
    "resource": "sampleResourceId",
    "requestor": "sampleRequestorId",
    "expires": "1348148289000"
  }
  ```

>[!IMPORTANT]
>
>응답이 프록시 MVPD로부터 나오는 경우, 이는 이라는 추가 요소를 포함할 수 있다 `proxyMvpd`.



* **사례 2: 권한 부여 거부됨**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
