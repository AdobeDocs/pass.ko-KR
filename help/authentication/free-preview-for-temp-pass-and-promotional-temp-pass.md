---
title: 임시 패스 및 프로모션 임시 패스에 대한 무료 미리보기
description: 임시 패스 및 프로모션 임시 패스에 대한 무료 미리보기
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '409'
ht-degree: 0%

---

# 임시 패스 및 프로모션 임시 패스에 대한 무료 미리보기 {#free-preview-for-temp-pass-and-promotional-temp-pass}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!NOTE]
>
> REST API 구현은 [조절 메커니즘](/help/authentication/throttling-mechanism.md)에 의해 제한됩니다.

## REST API 끝점 {#clientless-endpoints}

&lt;레지스트리_FQDN>:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 설명 {#description}

두 번째 화면을 사용하지 않고도 임시 패스 및 프로모션 임시 패스에 대한 인증 토큰을 만들 수 있습니다.


| 엔드포인트 | 호출자: </br>명 | 입력   </br>매개 변수 | HTTP </br>메서드 | 응답 | HTTP </br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/인증/미리보기 | 스트리밍 앱</br></br>또는</br></br>프로그래머 서비스 | 1. requestor_id(필수)</br>    </br>2.  deviceId(필수)</br>    </br>3.  mso_id(필수)</br>    </br>4.  domain_name(필수)</br>    </br>5.  device_info/X-Device-Info(필수)</br>6.  deviceType</br>    </br>7.  deviceUser(사용되지 않음)</br>    </br>8.  appId(사용되지 않음)</br>    </br>9.  generic_data (선택 사항) | POST | 성공적인 응답은 토큰이 성공적으로 만들어졌으며 인증 흐름에 사용할 준비가 되었음을 나타내는 204 컨텐츠 없음이 됩니다. | 204 - 콘텐츠 없음   </br>400 - 잘못된 요청 |

<div>


| 입력 매개 변수 | 설명 |
| --- | --- |
| requestor_id | 이 작업이 유효한 Programmer requestorId입니다. |
| deviceId | 장치 ID 바이트입니다. |
| mso_id | 이 작업이 유효한 MVPD ID. |
| domain_name | 토큰이 부여될 도메인 이름입니다. 인증 토큰이 부여될 때 서비스 공급자의 도메인과 비교됩니다. |
| device_info/</br></br>X-Device-Info | 스트리밍 장치 정보입니다.</br></br>**참고**: 이 매개 변수는 URL 매개 변수로 device_info를 전달할 수 있지만, 이 매개 변수의 잠재적 크기와 GET URL 길이 제한으로 인해 http 헤더에 X-Device-Info로 전달해야 합니다. </br></br>자세한 내용은 [장치 및 연결 정보 전달](/help/authentication/passing-client-information-device-connection-and-application.md)을 참조하세요. |
| _deviceType_ | 디바이스 유형(예: Roku, PC).</br></br>이 매개 변수가 올바르게 설정된 경우 ESM은 Clientless를 사용할 때 [장치 유형별로 분류된](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) 지표를 제공하므로 Roku, AppleTV, Xbox 등의 다양한 분석 유형을 수행할 수 있습니다.</br></br>See [Benefits of using clientless device type parameters ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**참고**: device_info가 이 매개 변수를 대체합니다. |
| _deviceUser_ | 장치 사용자 식별자.</br></br>**참고**: 사용하는 경우 deviceUser의 값은 [등록 코드 만들기](/help/authentication/registration-code-request.md) 요청과 동일해야 합니다. |
| _appId_ | 애플리케이션 ID/이름입니다. </br></br>**참고**: device_info가 이 매개 변수를 대체합니다. 사용하는 경우 `appId`은(는) [등록 코드 만들기](/help/authentication/registration-code-request.md) 요청과 동일한 값을 가져야 합니다. |
| generic_data | 프로모션 임시 패스에 대한 토큰 범위를 제한하는 데 사용됩니다. |


### [REST API 참조로 돌아가기](/help/authentication/rest-api-reference.md)
