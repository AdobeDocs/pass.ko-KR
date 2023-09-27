---
title: 로그아웃 시작
description: 로그아웃 시작
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 0%

---

# 로그아웃 시작 {#initiate-logout}

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

저장소에서 AuthN 및 AuthZ 토큰을 제거합니다.


| 엔드포인트 | 호출됨  </br>작성자: | 입력   </br>매개 변수 | HTTP  </br>방법 | 응답 | HTTP  </br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/logout | 스트리밍 앱</br></br>또는</br></br>프로그래머 서비스 | 1. 요청자</br>2.  deviceId(필수)</br>3.  device_info/X-Device-Info (필수)</br>4.  _deviceType_</br> 5.  _deviceUser_ (사용하지 않음)</br>6.  _appId_ (사용하지 않음) | DELETE | 없음 | 204 |


| 입력 매개 변수 | 설명 |
| --- | --- |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |
| deviceId | 장치 ID 바이트입니다. |
| device_info/</br></br>X-Device-Info | 스트리밍 장치 정보입니다.</br></br>**참고**: URL 매개 변수로 device_info를 전달할 수 있지만, 이 매개 변수의 잠재적 크기와 GET URL 길이 제한으로 인해 http 헤더에서 X-Device-Info로 전달해야 합니다. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | 디바이스 유형(예: Roku, PC).</br></br>이 매개 변수가 올바르게 설정되면 ESM은 다음과 같은 지표를 제공합니다. [장치 유형별 분류](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) 예: Roku, AppleTV, Xbox 등에 대해 다양한 유형의 분석을 수행할 수 있도록 Clientless를 사용할 때</br></br>다음을 참조하십시오 [전달 지표에서 클라이언트 없는 장치 유형 매개 변수 사용의 이점&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**참고**: device_info가 이 매개 변수를 대체합니다. |
| _deviceUser_ | 장치 사용자 식별자.</br></br>**참고**: 사용하는 경우 deviceUser는 [등록 코드 만들기](/help/authentication/registration-code-request.md) 요청. |
| _appId_ | 애플리케이션 ID/이름입니다. </br></br>**참고**: device_info가 이 매개 변수를 대체합니다. 사용하는 경우 `appId` 은(는) 과(와) 동일한 값을 가져야 합니다. [등록 코드 만들기](/help/authentication/registration-code-request.md) 요청. |

>[!IMPORTANT]
> 
>로그아웃 호출에는 현재 다음과 같은 제한이 있습니다. 스토리지(예: 프로그래머/Adobe Pass 인증 측)에서 AuthN 및 AuthZ 토큰이 지워지지만 **다음이 아님** mvpd 로그아웃 끝점을 호출합니다.
