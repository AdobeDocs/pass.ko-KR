---
title: 헤더 - AP-Device-Identifier
description: REST API V2 - 헤더 - AP-Device-Identifier
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '77'
ht-degree: 1%

---


# 헤더 - AP-Device-Identifier {#header-ap-device-identifier}

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
