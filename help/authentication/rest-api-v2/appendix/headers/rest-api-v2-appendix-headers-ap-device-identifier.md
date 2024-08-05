---
title: 헤더 - AP-Device-Identifier
description: REST API V2 - 헤더 - AP-Device-Identifier
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---


# 헤더 - AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

<b>AP-Device-Identifier</b> 요청 헤더에 클라이언트 응용 프로그램에서 만든 스트리밍 장치 식별자가 포함되어 있습니다.

## 구문 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>: &lt;type&gt; &lt;identifier&gt;</td>
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

<b>&lt;type></b>

장치 식별자 유형.

아래에 제시된 대로 지원되는 유형은 한 가지뿐입니다.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">유형</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>지문</td>
      <td>장치 식별자는 불투명 식별자로 구성되며 클라이언트 애플리케이션에 의해 생성된다.</td>
   </tr>
</table>


<b>&lt;식별자></b>

장치 식별자의 `Base64-encoded` 값입니다.

## 예 {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```
