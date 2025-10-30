---
title: 액세스 토큰 검색
description: Dynamic Client Registration API - 액세스 토큰 검색
exl-id: 23287acf-5d56-46f0-b65e-79bf7d667708
source-git-commit: 110e8519d6c042cc38de3fbefcd34297b6edcfad
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 1%

---

# 액세스 토큰 검색 {#retrieve-access-token}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> 동적 클라이언트 등록 API 구현은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md) 설명서에 의해 제한됩니다.

## 요청 {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">경로</td>
      <td>/o/client/token</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">방법</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">본문 매개 변수</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_id</td>
      <td>
            클라이언트 애플리케이션 식별자 문자열입니다.
            <br/><br/>
            클라이언트 식별자 문자열을 얻는 방법에 대한 자세한 내용은 <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">클라이언트 자격 증명 검색</a> API 설명서를 참조하십시오.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">클라이언트 암호</td>
      <td>
            클라이언트 응용 프로그램 암호 문자열입니다.
            <br/><br/>
            클라이언트 암호 문자열을 얻는 방법에 대한 자세한 내용은 <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">클라이언트 자격 증명 검색</a> API 설명서를 참조하십시오.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">grant_type</td>
      <td>
            클라이언트 애플리케이션이 클라이언트 토큰 엔드포인트에 사용할 수 있는 부여 유형 문자열(예: "client_credentials").
            <br/><br/>
            부여 유형 문자열을 가져오는 방법에 대한 자세한 내용은 <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">클라이언트 자격 증명 검색</a> API 설명서를 참조하십시오.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         전송 중인 리소스에 대해 허용되는 미디어 유형입니다.
         <br/><br/>
         application/x-www-form-urlencoded여야 합니다.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         장치 정보 페이로드의 생성은 <a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> 설명서에 설명되어 있습니다.
         <br/><br/>
         응용 프로그램의 장치 플랫폼에서 유효한 값의 명시적 제공을 허용하는 경우 항상 사용하는 것이 좋습니다.
         <br/><br/>
         제공되면 Adobe Pass 인증 백엔드는 명시적으로 설정된 값을 추출된 값과 묵시적으로(기본적으로) 병합합니다.
         <br/><br/>
         제공하지 않으면 Adobe Pass 인증 백엔드는 추출된 값을 묵시적으로(기본적으로) 사용합니다.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         클라이언트 애플리케이션에서 허용하는 미디어 유형입니다.
         <br/><br/>
         지정하면 application/json;charset=utf-8이어야 합니다.
      </td>
      <td>선택 사항</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>클라이언트 애플리케이션의 사용자 에이전트입니다.</td>
      <td>선택 사항</td>
   </tr>
</table>

## 응답 {#response}

### 성공 {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">상태</td>
      <td>201</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">본문</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         다음 속성이 있는 JSON 개체:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">속성</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">id</td>
               <td>사용자 활동 추적에 사용할 수 있는 불투명 식별자입니다.</td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">access_token</td>
               <td>클라이언트 애플리케이션이 인증 헤더에 사용해야 하는 액세스 토큰 값입니다.</td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">created_at</td>
               <td>액세스 토큰을 발급한 시간(밀리초).</td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">expires_in</td>
               <td>액세스 토큰이 만료될 때까지의 시간(초)입니다.</td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token_type</td>
               <td>토큰 유형(예: "bearer").</td>
               <td><i>필수</i></td>
            </tr>
         </table>
      </td>
      <td><i>필수</i></td>
</table>

### 오류 {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">상태</td>
      <td>400</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">본문</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">오류</td>
      <td>
        가능한 값은 다음과 같습니다.
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">값</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_request</td>
               <td>
                    다음 이유 중 하나로 인해 요청이 잘못되었습니다.
                    <ul>
                        <li>요청에서 필수 매개 변수를 누락했습니다.</li>
                        <li>요청에 지원되지 않는 매개 변수 값(권한 부여 유형 제외)이 포함되어 있습니다.</li>
                        <li>요청은 매개 변수를 반복합니다.</li>
                        <li>요청에는 여러 자격 증명이 포함됩니다.</li>
                        <li>요청은 클라이언트를 인증하기 위해 두 개 이상의 메커니즘을 사용합니다.</li>
                        <li>요청 형식이 잘못되었습니다.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_client</td>
               <td>클라이언트 자격 증명이 유효하지 않습니다. 클라이언트가 새 클라이언트 자격 증명을 획득하고 다시 시도해야 합니다. 자세한 내용은 <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">클라이언트 자격 증명 검색</a> API 설명서를 참조하십시오.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unauthorized_client</td>
               <td>클라이언트 응용 프로그램에 이 권한 부여 유형을 사용할 수 있는 권한이 없습니다.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unsupported_grant_type</td>
               <td>인증 서버에서 권한 부여 유형을 지원하지 않습니다.</td>
            </tr>
         </table>
      </td>
      <td><i>필수</i></td>
   </tr>
</table>

## 샘플 {#samples}

### 액세스 토큰 검색 {#samples-retrieve-access-token}

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
POST /o/client/token HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
    
Body:
    
client_id=s6BhdRkqt3&client_secret=t7AkePiru4&grant_type=client_credentials
```

>[!TAB 응답 - 성공]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
  "id": "a932f8f0-210a-41a4-b2a8-377751f6b76f",  
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "created_at": 1752148106221,
  "expires_in": 21600,
  "token_type": "bearer"
}
```

>[!TAB 응답 - 오류]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
