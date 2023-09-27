---
title: Platform SSO 토큰을 Adobe 토큰으로 교환
description: Platform SSO 토큰을 Adobe 토큰으로 교환
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 2%

---

# Platform SSO 토큰을 Adobe 토큰으로 교환 {#exchange-a-platform-sso-token-for-an-adobe-token}

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

Platform SSO 프로필을 Adobe 토큰으로 &quot;교환&quot;할 수 있습니다.

| 엔드포인트 | 호출됨  </br>작성자: | 입력   </br>매개 변수 | HTTP  </br>방법 | 응답 | HTTP  </br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/tokens/authn | 스트리밍 앱</br></br>또는</br></br>프로그래머 서비스 | 1. 요청자(필수)</br>    </br>2.  deviceId(필수)</br>    </br>3.  mvpd(필수)</br>    </br>4.  deviceType(필수)</br>    </br>5.  SAMLResponse(필수)</br>    </br>6.  deviceUser(사용되지 않음)</br>    </br>7.  appId(사용하지 않음) | POST | 성공적인 응답은 토큰이 성공적으로 만들어졌으며 인증 흐름에 사용할 준비가 되었음을 나타내는 204 컨텐츠 없음이 됩니다. | 204 - 콘텐츠 없음   </br>400 - 잘못된 요청 |


| 입력 매개 변수 | 설명 |
| --- | --- |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |
| deviceId | 장치 ID 바이트입니다. |
| mvpd | 이 작업이 유효한 MVPD ID. |
| deviceType | 프로필 요청을 가져오려는 Apple 플랫폼입니다.  다음 중 하나 **iOS** 또는 **tvOS**. |
| SAMLResponse | Platform SSO에서 반환된 실제 프로필입니다. |
| _deviceUser_ | 장치 사용자 식별자. |
| _appId_ | 애플리케이션 ID/이름입니다. |
