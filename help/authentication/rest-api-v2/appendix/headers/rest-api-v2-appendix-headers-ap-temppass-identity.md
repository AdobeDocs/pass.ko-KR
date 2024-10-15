---
title: 헤더 - AP-TempPass-Identity
description: REST API V2 - 헤더 - AP-TempPass-Identity
exl-id: a6238a58-a3f1-495d-a9d1-82475f5ffc60
source-git-commit: ca8eaff83411daab5f136f01394e1d425e66f393
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 2%

---

# 헤더 - AP-TempPass-Identity {#header-ap-temppass-identity}

>[!NOTE]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

<b>AP-TempPass-Identity</b> 요청 헤더에 프로모션 TempPass를 얻는 데 사용되는 사용자 ID 정보가 포함되어 있습니다.

## 구문 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-TempPass-Identity</b>: &lt;user_identity_information&gt;</td>
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

<b>&lt;user_identity_information></b>

프로모션 임시 액세스 권한을 부여해야 하는 최종 사용자와 연결된 사용자 ID 정보에 대한 `Base64-encoded` 값입니다.

## 예시 {#examples}

```JSON
// Identity
// {"email": "example@domain.com"}

// Base64-encoded
// eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==

X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
