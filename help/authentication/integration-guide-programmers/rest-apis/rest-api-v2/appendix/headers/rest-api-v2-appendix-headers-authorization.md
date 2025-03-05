---
title: 헤더 - 인증
description: REST API V2 - 헤더 - 인증
exl-id: 86917d7e-ffd9-4d34-8f9c-5a50083f85e6
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---


# 헤더 - 인증 {#header-authorization}

>[!NOTE]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

<b>인증</b> 요청 헤더에 클라이언트 응용 프로그램에서 Adobe Pass으로 보호된 API에 액세스하는 데 필요한 `Bearer` 액세스 토큰이 포함되어 있습니다.

Adobe Pass으로 보호된 API에 액세스하는 메커니즘에 대한 자세한 내용은 [동적 클라이언트 등록 개요](../../../rest-api-dcr/dynamic-client-registration-overview.md) 설명서를 참조하십시오.

## 구문 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>인증</b>: 전달자 &lt;access_token&gt;</td>
   </tr>
   <tr>
      <td>헤더 유형</td>
      <td>요청 헤더</td>
   </tr>
   <tr>
      <td>표준</td>
      <td>예</td>
   </tr>
</table>

## 지시문 {#directives}

<b>&lt;access_token></b>

액세스 토큰 값은 [액세스 토큰 검색](../../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API 설명서에 설명된 대로 Adobe Pass에서 가져와야 하는 제한된 TTL(예: 24시간)을 갖는 불투명 값입니다.

## 예시 {#examples}

```JSON
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI0NmY0MGZiMy01NmJkLTQyYTktOTExYS02YmZmNmEyZmY0
                      MDciLCJuYmYiOjE3MjM1NjE4ODUsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpO
                      mNsaWVudDp2MiIsImV4cCI6MTcyMzU4MzQ4NSwiaWF0IjoxNzIzNTYxODg1fQ.aZUZqwN2fCqNXgX
                      SdKFG9_HcqHjha66G6HmsfTJYcZc12iuLxMu7TT7MbhWVz3kW1jRqgJv8PHhrFSBL5_dgJ1PRSuDg
                      97ZK1secgMKwk46vKZVdtx7LF5t3jGVzQTwN4RqChqyvkW2o67KxVk5xarwJtwB2fwhX_732CYDcv
                      1gWOTLx4xyH5IVvg-P_aImyveG0D-x65I2nOKXaROVvv-kYE6B9OQv_-JBGj72R_yS2AyJQC0R_im
                      0h5S4YvL-c2UZrYK7pvdZq-xAj0uW1wad7PLZjl8yL5CWUz9vzQk2Cmj8adsydjb0u0P3aFrJ0HE9
                      ISqtRvjf4plR1TGWgw6
```
