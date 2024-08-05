---
title: 헤더 - AD-Service-Token
description: REST API V2 - 헤더 - AD-Service-Token
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 1%

---


# 헤더 - AD-Service-Token {#header-ad-service-token}

## 개요 {#overview}

<b>AD-Service-Token</b> 요청 헤더에 Adobe Pass 인증 시스템 외부에서 실행 중인 ID 서비스에서 가져온 `JWS`(으)로 고유한 사용자 식별자가 포함되어 있습니다.

이 헤더는 서비스 토큰 방법을 활용하는 SSO(Single Sign-On) 지원 흐름에서 사용하도록 설계되었습니다.

서비스 토큰 메서드를 활용하는 SSO(Single Sign-On) 사용 흐름에 대한 자세한 내용은 [서비스 토큰 흐름을 사용하는 Single Sign-On](../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-service-token-flows.md) 설명서를 참조하십시오.

## 구문 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>광고 서비스 토큰</b>: &lt;unique_user_identifier&gt;</td>
   </tr>
   <tr>
      <td>헤더 유형</td>
      <td>요청 헤더</td>
   </tr>
   <tr>
      <td>표준</td>
      <td>아니요</td>
   </tr>
</table>

## 지시문 {#directives}

<b>unique_user_identifier</b>

고유 사용자 식별자 정보를 포함하는 서명된 JSON 웹 토큰(`JWT`)인 JSON 웹 서명(`JWS`)입니다.

`JWT`에 다음 특성이 있습니다.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">속성</th>
      <th style="background-color: #EFF2F7;">설명</th>
   </tr>
   <tr>
      <td>iss</td>
      <td>SSO(Single Sign-On)를 위해 응용 프로그램에 외부 ID 서비스를 제공하는 엔터티와 연결된 고유 식별자입니다.</td>
   </tr>
   <tr>
      <td>하위</td>
      <td>외부 ID 서비스에서 반환되는 사용자의 고유 식별자입니다.</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>Adobe 대상이며, "대상자"여야 합니다.</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>현재 JWT에 대해 타임스탬프에서 발행된 시간.</td>
   </tr>
   <tr>
      <td>만료</td>
      <td>현재 JWT에 대한 만료 타임스탬프입니다.</td>
   </tr>
</table>

`JWT`은(는) `SHA256withRSA` 알고리즘을 사용하여 서명해야 합니다.

`JWT`은(는) 외부 ID 서비스에서 관리하는 공개 키인 RSA 개인 키 쌍의 일부인 개인 키로 서명되어야 합니다.

앞서 설명한 개인 키로 서명된 `JWT`개의 토큰을 인식하려면 해당 쌍의 공개 키를 Adobe Pass 인증에 전달해야 합니다.

## 예시 {#examples}

```JSON
// JWT
// Header
// {
//  "alg": "RS256",
//  "kid": "qapEaY0hYNvphytwII3Sae_cAKyLS7GZOqtT_a4ajeo"
// }
// Payload data
// {
//  "sub": "Jane",
//  "name": "Jane Smith",
//  "iat": 1516239022,
//  "iss": "adobe",
//  "exp": 1720152820,
//  "aud": "adobe",
//  "jti": "3b2fb040-30a9-43d7-b647-d00ac495bab"
// }
 
// JWS
// eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA

AD-Service-Token: eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA
```
