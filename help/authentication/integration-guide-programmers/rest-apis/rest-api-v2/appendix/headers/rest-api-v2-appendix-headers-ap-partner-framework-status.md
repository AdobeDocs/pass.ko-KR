---
title: 헤더 - AP-Partner-Framework-Status
description: REST API V2 - 헤더 - AP-Partner-Framework-Status
exl-id: f589d948-e23e-43d4-81c2-8db0e7a40e93
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---

# 헤더 - AP-Partner-Framework-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

<b>AP-Partner-Framework-Status</b> 요청 헤더에 SSO(Single Sign-On)를 위해 파트너 프레임워크에서 가져온 상태 정보가 포함되어 있습니다.

## 구문 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status</b>: &lt;partner_framework_status_information&gt;</td>
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

<b>&lt;partner_framework_status_information></b>

다음 특성을 포함하는 JSON 요소의 `Base64-encoded` 값:

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">속성</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         필수 속성입니다.
         <br/><br/>
         파트너 프레임워크에서 반환되고 애플리케이션에서 처리되는 사용자 권한 상태 정보입니다.
         <br/><br/>
         다음 속성이 있는 JSON 요소입니다.
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">속성</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>accessStatus</td>
               <td>
                  필수 속성입니다.
                  <br/><br/>
                  가능한 값은 다음과 같습니다.
                  <br/>
                  <ul>
                     <li><b>허가됨</b><br/>사용자가 응용 프로그램에서 구독 정보에 액세스하도록 허용했습니다.</li>
                     <li><b>거부됨</b><br/>사용자가 구독 정보에 액세스하기 위한 응용 프로그램을 거부했습니다.</li>
                     <li><b>보류 중</b><br/>사용자가 응용 프로그램이 구독 정보에 액세스할 수 있도록 허용하지 않았습니다.</li>
                     <li><b>notDetermined</b><br/>응용 프로그램에서 구독 정보에 액세스할 수 없습니다.</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>오류</td>
               <td>
                  선택적 속성입니다.
                  <br/><br/>
                  사용자 권한 상태 정보를 쿼리하는 동안 파트너 프레임워크 오류가 트리거되는 경우 이를 사용하여 파트너 프레임워크 오류를 전달할 수 있습니다.
                  <br/><br/>
                  다음 속성이 있는 JSON 요소입니다.
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">속성</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>코드</td>
                        <td>파트너 프레임워크에서 정의한 오류를 고유하게 식별하는 문자열입니다.</td>
                     </tr>
                     <tr>
                        <td>메시지</td>
                        <td>파트너 프레임워크에서 정의한 오류에 대한 설명을 포함하는 문자열입니다.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
   <tr>
      <td>frameworkProviderInfo</td>
      <td>
         필수 속성입니다.
         <br/><br/>
         파트너 프레임워크에서 반환되고 애플리케이션에서 처리되는 공급자 로그인 상태 정보입니다.
         <br/><br/>
         다음 속성이 있는 JSON 요소입니다.
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">속성</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>id</td>
               <td>
                  필수 속성입니다.
                  <br/><br/>
                  파트너 프레임워크 수준에서 인증 흐름 동안 사용된 MVPD을 식별하는 mappingId입니다.
               </td>
            </tr>
            <tr>
               <td>expirationDate</td>
               <td>
                  필수 속성입니다.
                  <br/><br/>
                  사용자가 파트너 프레임워크 수준에서 지원되는 MVPD을 사용하여 성공적으로 기록한 경우 인증된 사용자 프로필의 만료 날짜입니다.
               </td>
            </tr>
            <tr>
               <td>오류</td>
               <td>
                  선택적 속성입니다.
                  <br/><br/>
                  공급자 로그인 상태 정보를 쿼리하는 동안 파트너 프레임워크 오류가 트리거되는 경우 이를 사용하여 파트너 프레임워크 오류를 전달할 수 있습니다.
                  <br/><br/>
                  다음 속성이 있는 JSON 요소입니다.
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">속성</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>코드</td>
                        <td>파트너 프레임워크에서 정의한 오류를 고유하게 식별하는 문자열입니다.</td>
                     </tr>
                     <tr>
                        <td>메시지</td>
                        <td>파트너 프레임워크에서 정의한 오류에 대한 설명을 포함하는 문자열입니다.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
</table>

## 예시 {#examples}

```JSON
// Partner framework status information
// {
//    "frameworkPermissionInfo": {
//        "accessStatus": "....",
//        "error": {
//            "code" : "....",
//            "message" : "...."
//        }
//     },
//    "frameworkProviderInfo" : {
//        "id" : "....",
//        "expirationDate" : "....",
//        "error" : {
//            "code" : "...",
//            "message" : "....."
//        }
//     }
// }  
 
// Base64-encoded
// ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAg
// ImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAg
// ICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAg
// ICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIs
// CiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
 
AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAgImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAgICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAgICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
```
