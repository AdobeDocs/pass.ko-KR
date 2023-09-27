---
title: 인증 토큰 검색
description: 인증 토큰 검색
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# 인증 토큰 검색 {#retrieve-authentication-token}

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

인증(AuthN) 토큰을 검색합니다.

| 엔드포인트 | 호출됨  </br>작성자: | 입력   </br>매개 변수 | HTTP  </br>방법 | 응답 | HTTP  </br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/tokens/authn</br></br>예:</br></br>&lt;sp_fqdn>/api/v1/tokens/authn | 스트리밍 앱</br></br>또는</br></br>프로그래머 서비스 | 1. 요청자(필수)</br>2.  deviceId(필수)</br>3.  device_info/X-Device-Info (필수)</br>4.  _deviceType_ (사용하지 않음)</br>5.  _deviceUser_ (사용하지 않음)</br>6.  _appId_ (사용하지 않음) | GET | 실패한 경우 인증 정보 또는 오류 세부 정보가 포함된 XML 또는 JSON. | 200 - 성공  </br>404 - 토큰을 찾을 수 없음  </br>410 - 토큰이 만료됨 |

{style="table-layout:auto"}


| 입력 매개 변수 | 설명 |
| --- | --- |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |
| deviceId | 장치 ID 바이트입니다. |
| device_info/</br></br>X-Device-Info | 스트리밍 장치 정보입니다.</br></br>**참고**: URL 매개 변수로 device_info를 전달할 수 있지만, 이 매개 변수의 잠재적 크기와 GET URL 길이 제한으로 인해 http 헤더에서 X-Device-Info로 전달해야 합니다. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | 디바이스 유형(예: Roku, PC).</br></br>**참고**: device_info가 이 매개 변수를 대체합니다. |
| _deviceUser_ | 장치 사용자 식별자.</br></br>**참고**: 사용하는 경우 deviceUser는 [등록 코드 만들기](/help/authentication/registration-code-request.md) 요청. |
| _appId_ | 애플리케이션 ID/이름입니다. </br></br>**참고**: device_info가 이 매개 변수를 대체합니다. 사용하는 경우 `appId` 은(는) 과(와) 동일한 값을 가져야 합니다. [등록 코드 만들기](/help/authentication/registration-code-request.md) 요청. |

{style="table-layout:auto"}

</br>

### 샘플 응답 {#response}



#### 성공

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authentication>
         <expires>1601114932000</expires>
         <userId>sampleUserId</userId>
         <mvpd>sampleMvpdId</mvpd>
         <requestor>sampleRequestor</requestor>
    </authentication>
```


**JSON:**

```JSON
    {
         "requestor": "sampleRequestor",
         "mvpd": "sampleMvpdId",
         "userId": "sampleUserId",
         "expires": "1601114932000"
    }
```





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
        "message": "Not Found"
    }
```
