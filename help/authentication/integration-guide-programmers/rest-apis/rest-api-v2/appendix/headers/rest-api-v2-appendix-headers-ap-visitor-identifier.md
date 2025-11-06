---
title: 헤더 - AP-Visitor-Identifier
description: REST API V2 - 헤더 - AP-Visitor-Identifier
exl-id: 216f398b-1cfa-4453-a81d-963675b33ec2
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---

# 헤더 - AP-Visitor-Identifier {#header-ap-visitor-identifier}

>[!NOTE]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

<b>AP-Visitor-Identifier</b> 요청 헤더에 Adobe Experience Cloud 솔루션에서 방문자를 고유하게 식별하는 데 클라이언트 응용 프로그램에 필요한 `ECID`이(가) 포함되어 있습니다.

Adobe Pass 인증에서 ECID를 사용하는 방법에 대한 자세한 내용은 [Adobe Pass 인증에서 Experience Cloud ID 사용](../../../../features-premium/analytics/exp-cloud-id-authn.md) 설명서를 참조하십시오.

## 구문 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Visitor-Identifier</b>: &lt;visitor_identifier&gt;</td>
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

## 예 {#examples}

```JSON
AP-Visitor-Identifier: "THE_ECID_VALUE"
```
