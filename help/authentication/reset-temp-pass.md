---
title: 임시 통과 재설정
description: 임시 통과 재설정
source-git-commit: 4ae0b17eff2dfcf0aaa5d11129dfd60743f6b467
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 임시 통과 재설정 {#reset-temp-pass}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.
>
>임시 패스 재설정 API를 사용하려면 다음 작업을 수행해야 합니다.
>- 지원 팀에 등록된 응용 프로그램에 대한 소프트웨어 설명을 요청하십시오.
>- 다음을 기반으로 액세스 토큰 얻기 [동적 클라이언트 등록](dynamic-client-registration.md)
> 

에게 **특정 임시 패스 재설정**, Adobe Pass 인증은 프로그래머에게 *공용* 웹 API:

- **환경:** 임시 패스 네트워크 호출 재설정을 수신할 Adobe Pay-TV Pass 서버 끝점을 지정합니다. 가능한 값: **프리쿠알** (*mgmt-prequal.auth.adobe.com*), **릴리스** (*mgmt.auth.adobe.com*) 또는 **사용자 정의** (Adobe 내부 테스트용으로 예약됨).
- **OAuth2 액세스 토큰:** oauth2 토큰은 프로그래머가 Adobe Pay-TV 인증을 받도록 인증하는 데 필요합니다. 이러한 토큰은에서 얻을 수 있습니다. [동적 클라이언트 등록](dynamic-client-registration.md).
- **임시 통과 ID:** 재설정할 임시 통과 MVPD에 대한 고유 ID입니다.(프로그래머는 여러 임시 패스 MVPD를 사용할 수 있으며 특정 MVPD를 재설정하려고 함)
- **일반 키:** 일부 임시 통과 MVPD(즉, [프로모션 임시 패스](promotional-temp-pass.md)).

위의 모든 매개 변수(제외) *일반 키*)은 필수입니다. 다음은 매개 변수 및 관련 네트워크 호출의 예입니다(예제는 *curl *명령의 형태임).

- **환경:** 릴리스(*mgmt.auth.adobe.com*)
- **OAuth2 액세스 토큰:** &lt;access_token> 출처: [동적 클라이언트 등록](dynamic-client-registration.md)
- **프로그래머 ID:** 참조
- **임시 통과 ID:** TempPassREF
- **일반 키:** null(제공된 값 없음)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

에 대한 DELETE HTTP 요청이 수행됩니다. **/reset** 끝점, 전달 *OAuth2 액세스 토큰* Authorization 헤더 및 *장치 ID*, *요청자 ID* 및 *임시 패스 ID(MVPD ID)* 를 매개 변수로 사용하십시오.

프로그래머가 다음에 대한 값을 제공하는 경우 *일반 키*&#x200B;을 사용하여 다른 HTTP 호출이 수행됩니다(이번에는 **/reset/generic** endpoint), 전달 *일반 키* 의 내부 *key* 요청 매개 변수.

예를 들어, *일반 키* (이러한 기능을 지원하는 Temp Pass MVPD의 경우) 전자 메일 주소 해시에 대해 다음과 같은 HTTP 호출이 발생합니다(전자 메일은 다음과 같음). `user@domain.com` SHA-256 해시는 `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


에게 **모든 장치에 대한 특정 임시 패스 재설정**, Adobe Pass 인증은 프로그래머에게 *공용* 웹 API:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>위의 URL은 이전 재설정 API보다 우선합니다. 이전 재설정 API(v1)는 더 이상 지원되지 않습니다.

- **프로토콜:** HTTPS
- **호스트:**
   - 릴리스 - mgmt.auth.adobe.com
   - Prequal - mgmt-prequal.auth.adobe.com
- **경로:** /reset-tempass/v3/reset
- **쿼리 매개 변수:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
- **헤더:** 인증: 전달자 &lt;access_token_here>
- **응답:**
   - 성공 - HTTP 204
   - 실패:
      - 잘못된 요청에 대한 HTTP 400
      - 액세스가 거부된 경우 HTTP 401. 클라이언트가 새 access_token을 요청해야 합니다.
      - 클라이언트 ID가 더 이상 요청을 수행할 수 없는 경우 HTTP 403. 새 클라이언트 자격 증명을 생성해야 합니다.


For example:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
