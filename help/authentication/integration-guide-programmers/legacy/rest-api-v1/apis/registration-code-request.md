---
title: 등록 페이지
description: 등록 페이지
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---

# (기존) 등록 페이지 {#registration-page}

## REST API 끝점 {#clientless-endpoints}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

>[!NOTE]
>
> REST API 구현은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md)에 의해 제한됩니다.

&lt;레지스트리_FQDN>:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

<br>

## 설명 {#create-reg-code-svc}

임의로 생성된 등록 코드 및 로그인 페이지 URI를 반환합니다.

| 엔드포인트 | 호출자: <br>명 | 입력   <br>매개 변수 | HTTP <br>메서드 | 응답 | HTTP <br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode<br>예:<br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | 스트리밍 앱<br>또는<br>프로그래머 서비스 | &#x200B;1. 요청자 <br>    (경로 구성 요소)<br>2.  deviceId(해시됨)   <br>    (필수)<br>3.  device_info/X-Device-Info(필수)<br>4.  mvpd(선택 사항)<br>5.  ttl(선택 사항)<br> | POST | 실패한 경우 등록 코드 및 정보 또는 오류 세부 정보가 포함된 XML 또는 JSON. 아래 샘플을 참조하십시오. | 201 |

{style="table-layout:auto"}

| 입력 매개 변수 | 유형 | 설명 |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 인증 | 헤더 <br> 값: 전달자 &lt;access_token> | DCR 액세스 토큰 |
| Accept | 헤더 <br> 값: application/json | 클라이언트가 이해할 수 있는 콘텐츠 유형 표시 |
| 요청자 | 쿼리 매개 변수 | 이 작업이 유효한 Programmer requestorId입니다. |
| deviceId | 쿼리 매개 변수 | 장치 ID 바이트입니다. |
| device_info/<br>X-Device-Info | device_info: 본문 <br> X-Device-Info: 헤더 | 스트리밍 장치 정보입니다.<br>**참고**: 이 매개 변수는 URL 매개 변수로 device_info를 전달할 수 있지만, 이 매개 변수의 잠재적 크기와 GET URL 길이 제한으로 인해 http 헤더에 X-Device-Info로 전달해야 합니다. <br>자세한 내용은 [장치 및 연결 정보 전달](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)을 참조하세요. |
| mvpd | 쿼리 매개 변수 | 이 작업이 유효한 MVPD ID입니다. |
| ttl | 쿼리 매개 변수 | 이 regcode가 유지되어야 하는 시간(초)입니다.<br>**참고**: ttl에 허용되는 최대값은 36000초(10시간)입니다. 값이 높을수록 400 HTTP 응답(잘못된 요청)이 발생합니다. `ttl`을(를) 비워 두면 Adobe Pass 인증은 기본값으로 30분을 설정합니다. |
| _deviceType_ | 쿼리 매개 변수 | 사용 중단됨. 사용해서는 안 됩니다. |
| _deviceUser_ | 쿼리 매개 변수 | 사용 중단됨. 사용해서는 안 됩니다. |
| _appId_ | 쿼리 매개 변수 | 사용 중단됨. 사용해서는 안 됩니다. |

{style="table-layout:auto"}

>[!CAUTION]
>
>**스트리밍 장치 IP 주소**
><br>
>클라이언트-서버 구현의 경우 스트리밍 장치 IP 주소는 이 호출과 함께 암시적으로 전송됩니다.  **regcode** 호출이 스트리밍 장치가 아닌 프로그래머 서비스인 서버 간 구현의 경우 스트리밍 장치 IP 주소를 전달하려면 다음 헤더가 필요합니다.
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>여기서 `<streaming\_device\_ip>`은(는) 스트리밍 장치 공용 IP 주소입니다.
><br><br>
>예: <br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
<br>

### 응답 JSON


#### 등록 코드 JSON 샘플

```JSON
{
  "id": "ef5a79e8-7c8a-41d6-a45a-e378c6c7c8b5",
  "code": "IYQD5JQ",
  "requestor": "sampleRequestorId",
  "mvpd": "sampleMvpdId",
  "generated": 1704963921144,
  "expires": 1704965721144,
  "info": {
    "deviceId": "c28tZGV2aWQtMDAz",
    "deviceInfo": "eyJ0eXBlIjoiU2V0VG9wQm94IiwibW9kZWwiOiJBRlRNTSIsInZlcnNpb24iOnsibWFqb3IiOjAsIm1pbm9yIjowLCJwYXRjaCI6MCwicHJvZmlsZSI6IiJ9LCJoYXJkd2FyZSI6eyJuYW1lIjoiQUZUTU0iLCJ2ZW5kb3IiOiJVbmtub3duIiwidmVyc2lvbiI6eyJtYWpvciI6MCwibWlub3IiOjAsInBhdGNoIjowLCJwcm9maWxlIjoiIn0sIm1hbnVmYWN0dXJlciI6IlJva3UifSwib3BlcmF0aW5nU3lzdGVtIjp7Im5hbWUiOiJBbmRyb2lkIiwiZmFtaWx5IjoiQW5kcm9pZCIsInZlbmRvciI6IkFtYXpvbiIsInZlcnNpb24iOnsibWFqb3IiOjcsIm1pbm9yIjoxLCJwYXRjaCI6MiwicHJvZmlsZSI6IiJ9fSwiYnJvd3NlciI6eyJuYW1lIjoiQ2hyb21lIiwidmVuZG9yIjoiR29vZ2xlIiwidmVyc2lvbiI6eyJtYWpvciI6MTEyLCJtaW5vciI6MCwicGF0Y2giOjU2MTUsInByb2ZpbGUiOiIifSwidXNlckFnZW50IjoiTW96aWxsYS81LjAgKExpbnV4OyBBbmRyb2lkIDcuMS4yOyBBRlRNTSBCdWlsZC9OUzYyOTc7IHd2KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBWZXJzaW9uLzQuMCBDaHJvbWUvMTEyLjAuNTYxNS4xOTcgTW9iaWxlIFNhZmFyaS81MzcuMzYgQWRvYmVQYXNzTmF0aXZlRmlyZVRWLzMuMC44Iiwib3JpZ2luYWxVc2VyQWdlbnQiOiJNb3ppbGxhLzUuMCAoTGludXg7IEFuZHJvaWQgNy4xLjI7IEFGVE1NIEJ1aWxkL05TNjI5Nzsgd3YpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIFZlcnNpb24vNC4wIENocm9tZS8xMTIuMC41NjE1LjE5NyBNb2JpbGUgU2FmYXJpLzUzNy4zNiBBZG9iZVBhc3NOYXRpdmVGaXJlVFYvMy4wLjgifSwiZGlzcGxheSI6eyJ3aWR0aCI6MCwiaGVpZ2h0IjowLCJwcGkiOjAsIm5hbWUiOiJESVNQTEFZIiwidmVuZG9yIjpudWxsLCJ2ZXJzaW9uIjpudWxsLCJkaWFnb25hbFNpemUiOm51bGx9LCJhcHBsaWNhdGlvbklkIjpudWxsLCJjb25uZWN0aW9uIjp7ImlwQWRkcmVzcyI6IjE5My4xMDUuMTQwLjEzMSIsInBvcnQiOiI5OTM0Iiwic2VjdXJlIjpmYWxzZSwidHlwZSI6bnVsbH19",
    "userAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "originalUserAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "authorizationType": "OAUTH2",
    "sourceApplicationInformation": {
      "id": "14138364-application-id",
      "name": "application name",
      "version": "1.0.0"
    }
  }
}
```

<br>

| 요소 이름 | 설명 |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| id | 등록 코드 서비스에서 생성된 UUID |
| 코드 | 등록 코드 서비스에서 생성한 등록 코드 |
| 요청자 | 요청자 ID |
| mvpd | Mvpd ID |
| 생성됨 | 등록 코드 생성 타임스탬프(1970년 1월 1일 GMT 이후 밀리초) |
| 만료 | 등록 코드가 만료되는 타임스탬프(1970년 1월 1일 GMT 이후 밀리초) |
| deviceId | Base64 고유 장치 ID |
| 정보:deviceId | Base64 장치 유형 |
| 정보:deviceInfo | Base64 User-Agent, X-Device-Info 또는 device_info에서 받은 정보를 기반으로 표준화된 장치 정보 작성 |
| 정보:userAgent | 애플리케이션에서 보낸 사용자 에이전트 |
| 정보:originalUserAgent | 애플리케이션에서 보낸 사용자 에이전트 |
| 정보:authorizationType | DCR을 사용한 호출에 대한 OAUTH2 |
| 정보:sourceApplicationInformation | DCR에 구성된 애플리케이션 정보 |

{style="table-layout:auto"}


<br>

### 오류 메시지 JSON 응답 샘플(#error-sample-response)

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

