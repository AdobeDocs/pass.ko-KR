---
title: 조절 메커니즘
description: Adobe Pass 인증에 사용되는 조절 메커니즘에 대해 알아봅니다. 이 페이지에서 이 메커니즘에 대한 개요를 살펴보십시오.
exl-id: f00f6c8e-2281-45f3-b592-5bbc004897f7
source-git-commit: 83998257b25465c109cac56ae753291d1572696c
workflow-type: tm+mt
source-wordcount: '1141'
ht-degree: 0%

---

# 조절 메커니즘 {#throttling-mechanism}

모든 패스 인증 고객은 지침 및 비즈니스 사례에 따라 각 사용자에 대한 패스 인증 API에 액세스할 수 있어야 합니다.

Pass Authentication에는 고객의 사용자 간에 리소스를 균등하게 분배하기 위한 조절 메커니즘이 도입되어 있습니다.

이 메커니즘은 다음과 같은 몇 가지 이유로 중요합니다.

- 다중 임차인 아키텍처의 골든 규칙 중 하나는 한 사용자의 행동이 다른 사용자에게 영향을 미치지 않아야 한다는 것입니다.
- API와 통합할 때 오류를 범하기 쉽기 때문에 속도 제한은 API에 중요합니다. API는 프로그래밍 방식으로 전송되므로 의도한 것보다 실수로 더 많은 요청을 보내는 것이 비교적 쉽습니다.

## 메커니즘 개요 {#mechanism-overview}

### 클라이언트 구현

패스 인증은 API와 상호 작용하기 위한 지침과 SDK를 제공하지만 고객이 이를 사용하는 방식은 제어하지 않습니다. 일부 구현은 기본적인 구현이 있을 수 있으며, 실수로 또는 실수로 다른 사용자에게 속도 저하 또는 용량 문제가 발생할 수 있는 경우 불필요한 API 요청으로 서비스를 쇄도할 수 있습니다.

서비스 자체는 모든 합리적인 용량을 처리할 수 있어야 합니다. 그러나 아무리 확장 가능하고 성능이 뛰어난 서비스라 할지라도 항상 한계가 있습니다. 따라서 서비스에는 특정 시간 간격 내에 허용되는 호출 수에 대해 구성된 제한이 있어야 합니다.

### 조절 기능 도입

인증 전달은 사용자 식별과 사전 정의된 값이 있는 토큰 버킷 속도 제한 알고리즘을 기반으로 각 사용자의 API 액세스를 제어합니다.

### 장치 식별 메커니즘

제안된 조절 메커니즘은 &quot;X-Forwarded-For&quot; 헤더를 통해 식별된 디바이스를 개별적으로 사용합니다. 각 장치에 대해 동일한 방식으로 제한이 적용됩니다.

### 필수 업데이트

서버 간 구현은 &quot;X-Forwarded-For&quot; 헤더 메커니즘을 사용하여 클라이언트의 IP 주소를 전달해야 합니다.

X-Forwarded-For 헤더를 전달하는 방법에 대한 자세한 내용은 [여기](rest-api-cookbook-servertoserver.md)에서 확인할 수 있습니다.

### 실제 제한 및 끝점

현재, 기본 제한은 초기 버스트가 10개이며, 초당 최대 1개의 요청을 허용합니다(식별된 클라이언트의 첫 번째 상호 작용에 대해 1회 허용하며 초기화를 성공적으로 완료할 수 있도록 해야 함). 이는 모든 고객의 일반적인 비즈니스 사례에 영향을 미치지 않습니다.

제한 메커니즘은 다음 종단점에서 활성화됩니다.

- /o/client/register
- /o/client/token
- /o/client/scopes
- /o/client/validate
- /api/v2/
- /api/v1/tokens/usermetadata
- /api/v1/tokens/authn
- /api/v1/tokens/authz
- /api/v1/tokens/media
- /api/v1/config/
- /api/v1/checkauthn
- /api/v1/logout
- /api/v1/authorize
- /api/v1/사전 승인
- /api/v1/mediatoken
- /api/v1/authenticate/freepreview
- /api/v1/authenticate/
- /api/v1/.+/profile-requests/.+
- /api/v1/identities
- /adobe-services/config/
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### SDK 구현 명확화

Adobe Pass 인증 제공 SDK를 사용하는 클라이언트는 끝점과 명시적으로 상호 작용하지 않으므로 이 섹션에서는 알려진 함수, 스로틀 응답이 있을 때 작동하는 방식 및 취해야 하는 작업을 제공합니다.

#### setRequestor

SDK에서 `setRequestor` 함수를 사용하여 스로틀 제한에 도달하면 SDK는 `errorHandler` 콜백을 통해 CFG429 오류 코드를 반환합니다.

#### getAuthorization

SDK에서 `getAuthorization` 함수를 사용하여 스로틀 제한에 도달하면 SDK는 `errorHandler` 콜백을 통해 Z100 오류 코드를 반환합니다.

#### checkPreauthorizedResources

SDK에서 `checkPreauthorizedResources` 함수를 사용하여 스로틀 제한에 도달하면 SDK는 `errorHandler` 콜백을 통해 P100 오류 코드를 반환합니다.

#### getMetadata

SDK에서 `getMetadata` 함수를 사용하여 스로틀 제한에 도달하면 SDK는 `setMetadataStatus` 콜백을 통해 빈 응답을 반환합니다.

각 특정 구현에 대한 자세한 내용은 특정 SDK 설명서를 참조하십시오.

- [JavaScript SDK API 참조](javascript-sdk-api-reference.md)
- [Android SDK API 참조](android-sdk-api-reference.md)
- [iOS/tvOS API 참조](iostvos-sdk-api-reference.md)

### API 응답 변경 및 응답

제한을 위반했다고 식별하면 이 요청을 특정 응답 상태(HTTP 429 요청이 너무 많음)로 표시하여 시간 간격 동안 사용자 장치에 할당된 모든 토큰(IP 주소)을 소비했음을 나타냅니다.

전송률 조절은 처음 429개의 응답 중 1초 후에 만료됩니다. 429 응답을 수신하는 각 애플리케이션은 새 요청을 생성하기 전에 최소 1초 동안 대기해야 합니다.

모든 고객 애플리케이션은 &quot;429 Too Many Requests&quot; 응답을 적절히 처리해야 합니다.

다음은 샘플 429 응답 메시지입니다.

```
HTTP/2 429
date: Tue, 20 Feb 2024 11:21:53 GMT
content-type: text/html
content-length: 166
set-cookie: AWSALB=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/
set-cookie: AWSALBCORS=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/; SameSite=None; Secure
server: openresty
access-control-allow-credentials: true
access-control-allow-methods: POST,GET,OPTIONS,DELETE
access-control-allow-headers: ap_11,ap_42,ap_z,ap_19,ap_21,ap_23,authorization,content-type,pass_sfp,AP-Session-Identifier,AP-Device-Identifier,AP-SDK-Identifier,X-Device-Info
access-control-expose-headers: pass_sfp,Authzf-Error-Code,Authzf-Sub-Error-Code,Authzf-Error-Details
p3p: CP="NOI DSP COR CURa ADMa DEVa OUR BUS IND UNI COM NAV STA"

<html>
<head><title>429 Too Many Requests</title></head>
<body>
<center><h1>429 Too Many Requests</h1></center>
<hr><center>openresty</center>
</body>
</html>
```

## 영향 및 필요한 변경 사항

### X-Forwarded-For 헤더 전달

사용자 지정 구현(서버 간 구현 포함)을 사용하여 인증 전달 API와 상호 작용하는 고객은 사용자 IP 주소를 캡처하고 X-Forwarded-For 헤더를 사용하여 인증 전달 API에 올바르게 전달할 수 있는지 확인해야 합니다.

자세한 내용은 [여기](rest-api-cookbook-servertoserver.md)를 참조하세요.

### 새 응답 코드에 대한 반응

인증 전달 API와 상호 작용하기 위해 사용자 지정 구현(서버 간 구현 포함)을 사용하는 고객은 429 너무 많은 요청을 받은 후 수행된 모든 후속 호출에 최소 1초 대기 기간이 포함되어 있는지 확인해야 합니다. 이 대기 기간을 통해 이 메커니즘을 변경하고 유효한 비즈니스 응답을 얻을 수 있습니다.

## 조절에 대한 시나리오 예

| 첫 번째 요청 이후 시간 | 응답 수신됨 | 설명 |
|--------------------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------|
| 초 0 | 호출이 성공 상태 코드를 수신함 | 제한에서 1개의 호출 사용됨 |
| 두 번째 0.3 | 호출이 성공 상태 코드를 수신함 | 제한에서 1개의 호출이 소비되고 버스트로 1개의 호출이 표시됨 |
| 두 번째 0.6 | 호출이 성공 상태 코드를 수신함 | 제한에서 1개의 호출이 소비되고 2개의 호출이 버스트로 표시됨 |
| 두 번째 0.9 | 호출이 성공 상태 코드를 수신함 | 제한에서 1개의 호출이 소비되고 3개의 호출이 버스트로 표시됨 |
| 두 번째 1.2 | 호출이 성공 상태 코드를 수신함 | 제한에서 2개의 호출이 소비되고 3개의 호출이 버스트로 표시됨 |
| 두 번째 1.3 | 호출이 성공 상태 코드를 수신함 | 제한에서 2개의 호출이 소비되고 버스트로 표시된 4개의 호출 |
| 두 번째 1.4 | 호출이 성공 상태 코드를 수신함 | 제한에서 2개의 호출이 소비되고 5개의 호출이 버스트로 표시됨 |
| 초 1.5 | 호출이 성공 상태 코드를 수신함 | 제한에서 2개의 호출이 소비되고 6개의 호출이 버스트로 표시됨 |
| 두 번째 1.6 | 호출이 성공 상태 코드를 수신함 | 제한에서 2개의 호출이 소비되고 7개의 호출이 버스트로 표시됨 |
| 두 번째 1.7 | 호출이 성공 상태 코드를 수신함 | 제한에서 2개의 호출이 소비되고 8개의 호출이 버스트로 표시됨 |
| 두 번째 1.8 | 호출이 성공 상태 코드를 수신함 | 제한에서 2개의 호출이 소비되고 9개의 호출이 버스트로 표시됨 |
| 두 번째 2.1 | 호출이 성공 상태 코드를 수신함 | 제한에서 3개의 호출이 소비되고 9개의 호출이 버스트로 표시됨 |
| 두 번째 2.2 | 호출이 성공 상태 코드를 수신함 | 제한에서 3개의 호출이 소비되고 버스트로 표시된 10개의 호출이 있습니다. |
| 두 번째 2.4 | 호출이 429 상태 코드를 수신함 | 제한에서 3개의 호출이 소비되고, 버스트로 표시된 10개의 호출과 &#39;429개의 요청이 너무 많음&#39;을 수신함 |
| 두 번째 2.6 | 호출이 429 상태 코드를 수신함 | 제한에서 3개의 호출이 소비되고, 버스트로 표시된 10개의 호출과 2개의 호출이 &#39;429개의 요청이 너무 많음&#39;을 수신함 |
| 두 번째 2.8 | 호출이 429 상태 코드를 수신함 | 제한에서 3개의 호출이 소비되고, 버스트로 표시된 10개의 호출과 3개의 호출이 &#39;429개의 요청이 너무 많음&#39;을 수신함 |
| 두 번째 3.1 | 호출이 성공 상태 코드를 수신함 | 제한에서 4개의 호출이 소비되고, 버스트로 표시된 10개의 호출과 3개의 호출이 &#39;429개의 요청이 너무 많음&#39;을 수신함 |
