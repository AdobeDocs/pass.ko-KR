---
title: 임시 통과 재설정
description: 임시 통과 재설정
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# 임시 통과 재설정 {#reset-temp-pass}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> Reset Temp Pass API를 사용하기 전에 다음 사전 요구 사항이 충족되는지 확인하십시오.
>
> * [클라이언트 자격 증명 검색](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API 설명서에 설명된 대로 클라이언트 자격 증명을 가져옵니다.
> * [액세스 토큰 검색](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API 설명서에 설명된 대로 액세스 토큰을 가져옵니다.
>
> 등록된 응용 프로그램을 만들고 소프트웨어 문을 다운로드하는 방법에 대한 자세한 내용은 [동적 클라이언트 등록 개요](./dcr-api/dynamic-client-registration-overview.md) 설명서를 참조하십시오.

**특정 임시 패스 재설정**&#x200B;을 위해 Adobe Pass 인증은 프로그래머에게 *public* 웹 API를 제공합니다.

* **환경:**&#x200B;은(는) 임시 패스 네트워크 호출 재설정을 수신할 Adobe Pay-TV Pass 서버 끝점을 지정합니다. 가능한 값: **Prequal** (*mgmt* prequal.auth.adobe.com*), **Release** (*mgmt.auth.adobe.com*) 또는 **Custom**(Adobe 내부 테스트용으로 예약됨).
* **OAuth2 액세스 토큰:** OAuth2 토큰은 Adobe Pay-TV 인증을 위해 프로그래머를 인증하는 데 필요합니다. 이러한 토큰은 [액세스 토큰 검색](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API 설명서에 설명된 대로 가져올 수 있습니다.
* **임시 패스 ID:** 임시 패스 MVPD를 다시 설정할 고유 ID입니다.(프로그래머는 여러 임시 패스 MVPD를 사용할 수 있으며 특정 MVPD를 재설정하려고 함)
* **일반 키:** 일부 임시 패스 MVPD(예: [프로모션 임시 패스](promotional-temp-pass.md)).

위의 모든 매개 변수(*일반 키* 제외)는 필수입니다. 다음은 매개 변수 및 관련 네트워크 호출의 예입니다(예제는 *curl *명령의 형태임).

* **환경:** 릴리스(*mgmt.auth.adobe.com*)
* [액세스 토큰 검색](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API 설명서에 설명된 대로 **OAuth2 액세스 토큰:** &lt;access_token>
* **프로그래머 ID:** 참조
* **임시 통과 ID:** TempPassREF
* **일반 키:** null(제공된 값 없음)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

인증 헤더의 *OAuth2 액세스 토큰* 및 매개 변수로 *장치 ID*, *요청자 ID* 및 *임시 패스 ID(MVPD ID)*&#x200B;을(를) 전달하여 **/reset** 끝점에 DELETE HTTP 요청이 이루어집니다.

프로그래머가 *일반 키*&#x200B;에 대한 값을 제공하면 다른 HTTP 호출이 수행되며(이번에는 **/reset/generic** 끝점에 대해), *key* 요청 매개 변수 내에 *일반 키*&#x200B;을(를) 전달합니다.

예를들어, *일반 키*을(를) 전자 메일 주소 해시로 설정합니다.
이러한 기능을 지원하는 임시 통과 MVPD는
다음 HTTP 호출(전자 메일은 SHA-256의 `user@domain.com`입니다.)
해시는 `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`입니다.

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


**모든 장치에 대한 특정 임시 패스를 다시 설정**&#x200B;하기 위해 Adobe Pass 인증은 프로그래머에게 *public* 웹 API를 제공합니다.

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>위의 URL은 이전 재설정 API보다 우선합니다. 이전 재설정 API(v1)는 더 이상 지원되지 않습니다.

* **프로토콜:** HTTPS
* **호스트:**
   * 릴리스 - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **경로:** /reset-tempass/v3/reset
* **쿼리 매개 변수:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **헤더:** 인증: 전달자 &lt;access_token_here>
* **응답:**
   * 성공 - HTTP 204
   * 실패:
      * 잘못된 요청에 대한 HTTP 400
      * 액세스가 거부된 경우 HTTP 401. 클라이언트가 새 access_token을 요청해야 합니다.
      * 클라이언트 ID가 더 이상 요청을 수행할 수 없는 경우 HTTP 403. 새 클라이언트 자격 증명을 생성해야 합니다.


For example:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
