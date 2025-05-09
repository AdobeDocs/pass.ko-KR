---
title: 헤더 - Adobe-Subject-Token
description: REST API V2 - 헤더 - Adobe-Subject-Token
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 1%

---

# 헤더 - Adobe-Subject-Token {#header-adobe-subject-token}

>[!NOTE]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

<b>Adobe-Subject-Token</b> 요청 헤더에 Adobe Pass 인증 시스템 외부에서 실행 중인 ID 서비스 또는 라이브러리에서 얻은 고유한 플랫폼 식별자 `JWS` 또는 `JWE`이(가) 포함되어 있습니다.

이 헤더는 Platform Identity 메서드를 활용하는 SSO(Single Sign-On) 지원 흐름에서 사용하도록 설계되었습니다.

플랫폼 ID 메서드를 활용하는 SSO(Single Sign-On) 사용 흐름에 대한 자세한 내용은 [플랫폼 ID 흐름을 사용하는 Single Sign-On](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) 설명서를 참조하십시오.

## 구문 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Adobe-Subject-Token</b>: &lt;unique_platform_identifier&gt;</td>
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

<b>unique_platform_identifier</b>

고유한 플랫폼 식별자 정보를 포함하는 서명되거나 암호화된 JSON 웹 토큰(`JWT`)인 JSON 웹 서명(`JWS`) 또는 JSON 웹 암호화(`JWE`)입니다.

이 기능은 다음 플랫폼에 사용할 수 있습니다.

* [Amazon SSO Cookbook(REST API V2)](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## 예시 {#examples}

다음 플랫폼에 대해 설명된 예를 참조하십시오.

* [Amazon SSO Cookbook(REST API V2)](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
