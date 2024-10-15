---
title: 특정 mvpd에 대한 로그아웃 시작
description: REST API V2 - 특정 mvpd에 대한 로그아웃 시작
exl-id: 2482de87-b3d4-4ea8-bd4a-25bf10017e01
source-git-commit: ca8eaff83411daab5f136f01394e1d425e66f393
workflow-type: tm+mt
source-wordcount: '941'
ht-degree: 1%

---

# 특정 mvpd에 대한 로그아웃 시작 {#initiate-logout-for-specific-mvpd}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> REST API V2 구현은 [조절 메커니즘](/help/authentication/throttling-mechanism.md) 설명서에 의해 제한됩니다.

## 요청 {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">경로</td>
      <td>/api/v2/{serviceProvider}/logout/{mvpd}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">방법</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">경로 매개 변수</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>온보딩 프로세스 중 서비스 공급자와 연결된 내부 고유 식별자입니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>온보딩 프로세스 중 ID 공급자와 연결된 내부 고유 식별자입니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">쿼리 매개변수</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        MVPD에 대한 로그아웃 흐름이 완료되면 사용자 에이전트가 탐색하는 최종 리디렉션 URL입니다.
        <br/><br/>
        값은 URL로 인코딩되어야 합니다.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">인증</td>
      <td>전달자 토큰 페이로드의 생성은 <a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">인증</a> 헤더 설명서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>장치 식별자 페이로드 생성은 <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> 헤더 문서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         장치 정보 페이로드의 생성은 <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> 헤더 설명서에 설명되어 있습니다.
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
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         스트리밍 장치의 IP 주소입니다.
         <br/><br/>
         특히 스트리밍 장치가 아닌 프로그래머 서비스에서 호출하는 경우 항상 서버 대 서버 구현에 사용하는 것이 좋습니다.
         <br/><br/>
         클라이언트 대 서버 구현의 경우, 스트리밍 장치의 IP 주소가 암묵적으로 전송됩니다.
      </td>
      <td>선택 사항</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Adobe-주체-토큰</td>
      <td>
        플랫폼 ID 메서드에 대한 Single Sign-On 페이로드의 생성은 <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-주체-토큰</a> 헤더 설명서에 설명되어 있습니다.
        <br/><br/>
        플랫폼 ID를 사용한 Single Sign-On 사용 흐름에 대한 자세한 내용은 <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">플랫폼 ID 흐름을 사용한 Single Sign-On</a> 설명서를 참조하십시오.
      </td>
      <td>선택 사항</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">광고 서비스 토큰</td>
      <td>
        서비스 토큰 메서드에 대한 Single Sign-On 페이로드의 생성은 <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a> 헤더 설명서에 설명되어 있습니다.
        <br/><br/>
        서비스 토큰을 사용한 Single Sign-On 사용 흐름에 대한 자세한 내용은 <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md">서비스 토큰 흐름을 사용한 Single Sign-On</a> 설명서를 참조하십시오.
      </td>
      <td>선택 사항</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         클라이언트 애플리케이션에서 허용하는 미디어 유형입니다.
         <br/><br/>
         지정하면 application/json이어야 합니다.
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

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">코드</th>
      <th style="background-color: #EFF2F7;">텍스트</th>
      <th style="background-color: #EFF2F7;">설명</th>
   </tr>
   <tr>
      <td>200</td>
      <td>확인</td>
      <td>
        응답 본문에는 삭제된 각 프로필에 대한 로그아웃 플로우를 완료하기 위해 클라이언트가 수행해야 하는 작업 목록이 포함되어 있습니다.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>잘못된 요청</td>
      <td>
        요청이 잘못되었습니다. 클라이언트가 요청을 수정하고 다시 시도하십시오. 응답 본문에는 <a href="../../../enhanced-error-codes.md">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>승인되지 않음</td>
      <td>
        액세스 토큰이 잘못되었습니다. 클라이언트가 새 액세스 토큰을 얻은 후 다시 시도하십시오. 자세한 내용은 <a href="../../../dcr-api/dynamic-client-registration-overview.md">동적 클라이언트 등록 개요</a> 설명서를 참조하십시오.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>메서드가 허용되지 않음</td>
      <td>
        HTTP 메서드가 잘못되었습니다. 클라이언트가 요청한 리소스에 대해 허용되는 HTTP 메서드를 사용하고 다시 시도하십시오. 자세한 내용은 <a href="#request">요청</a> 섹션을 참조하세요.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>내부 서버 오류</td>
      <td>
        서버 측에서 문제가 발생했습니다. 응답 본문에는 <a href="../../../enhanced-error-codes.md">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
</table>

### 성공 {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">상태</td>
      <td>200</td>
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
      <td style="background-color: #DEEBFF;">로그아웃</td>
      <td>
         키, 값 쌍의 맵이 포함된 JSON.
         <br/><br/>
         키 요소는 다음 값으로 정의됩니다.
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">값</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>온보딩 프로세스 중 ID 공급자와 연결된 내부 고유 식별자입니다.</td>
               <td><i>필수</i></td>
         </table>
         값 요소는 다음 속성으로 정의됩니다.
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">속성</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  로그아웃 흐름을 완료하기 위해 스트리밍 장치가 수행해야 하는 작업입니다.
                  <br/><br/>
                  가능한 값은 다음과 같습니다.
                  <ul>
                    <li><b>로그아웃</b><br/>스트리밍 장치는 사용자 에이전트에서 제공된 URL을 열어야 합니다.<br/>이 작업은 다음 시나리오에 적용됩니다. 로그아웃 끝점을 사용하여 MVPD에서 로그아웃합니다.</li>
                    <li><b>완료</b><br/>스트리밍 장치는 후속 작업을 수행할 필요가 없습니다.<br/>이 작업은 다음 시나리오에 적용됩니다. 로그아웃 끝점 없이 MVPD에서 로그아웃하고(더미 로그아웃 기능), 액세스 성능이 저하된 동안에는 로그아웃하고, 임시 액세스 중에는 로그아웃합니다.</li>
                    <li><b>잘못됨</b><br/>스트리밍 장치는 후속 작업을 수행할 필요가 없습니다.<br/>이 작업은 다음 시나리오에 적용됩니다. 유효한 프로필이 없으면 MVPD에서 로그아웃합니다.</li>
                  </ul>  
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  'actionName' 특성에 지정된 작업으로 흐름을 계속하려면 스트리밍 장치가 수행해야 하는 상호 작용 유형입니다.
                  <br/><br/>
                  가능한 값은 다음과 같습니다.
                  <ul>
                    <li><b>대화식</b><br/>이 형식은 'actionName' 특성의 다음 값에 적용됩니다. <b>로그아웃</b>.</li>
                    <li><b>없음</b><br/>이 유형은 'actionName' 특성의 다음 값에 적용됩니다. <b>완료</b>, <b>잘못됨</b>.</li>
                  </ul>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>온보딩 프로세스 중 ID 공급자와 연결된 내부 고유 식별자입니다.</td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>
                  MVPD 끝점으로 로그아웃 흐름을 수행하는 데 사용되는 URL입니다.
                  <br/><br/>
                  다음 'actionName' 속성 값은 표시되지 않습니다.
                  <ul>
                    <li><b>완료</b></li>
                    <li><b>잘못됨</b></li>
                  </ul>
               </td>
               <td>선택 사항</td>
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
      <td>400, 401, 405, 500</td>
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
      <td style="background-color: #DEEBFF;"></td>
      <td>응답 본문은 <a href="../../../enhanced-error-codes.md">향상된 오류 코드</a> 설명서를 준수하는 추가 오류 정보를 제공할 수 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
</table>

## 샘플 {#samples}

### 1. 로그아웃 끝점이 있는 특정 mvpd에 대한 로그아웃 시작

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/v2/REF30/logout/Cablevision?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)  
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "logouts": {
        "Cablevision": {
            "actionName": "logout",
            "actionType": "interactive",
            "mvpd": "Cablevision",
            "url": "https://sp.auth.adobe.com/adobe-services/logout?noflash=true&mso_id=Cablevision&requestor_id=REF30&redirect_url=http%3A%2F%2Fadobe.com"
        }
    }
}
```

>[!ENDTABS]

### 2. 로그아웃 끝점이 없는 특정 mvpd에 대한 로그아웃 시작

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/v2/REF30/logout/Dish?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "logouts": {
        "Dish": {
            "actionName": "complete",
            "actionType": "none",
            "mvpd": "Dish"
       }
    }
}
```

>[!ENDTABS]

### 3. 서비스 토큰 방법을 사용하여 단일 사인온을 통해 얻은 프로필을 포함하는 특정 mvpd에 대한 단일 로그아웃 시작

>[!IMPORTANT]
>
> 가정
>
> <br/>
>
> * MVPD에 로그아웃 끝점이 있습니다.

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/v2/REF30/logout/Spectrum?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AD-Service-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJZemsxTXpNMk4yWXRZMk0wTWkwME1X .....
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
   "logouts": {
      "Spectrum": {
         "actionName": "logout",
         "actionType": "interactive",
         "mvpd": "Spectrum",
         "url": "/adobe-services/logout?requestor_id=REF30&mso_id=Spectrum&pt_device_id=c54fa2c80652b10abea58c...."
      }
   }
}
```

>[!ENDTABS]

### 4. 플랫폼 ID 방법을 사용하여 단일 사인온을 통해 얻은 프로필을 포함하는 특정 mvpd에 대한 단일 로그아웃 시작

>[!IMPORTANT]
>
> 가정
>
> <br/>
>
> * MVPD에 로그아웃 끝점이 있습니다.

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/v2/REF30/logout/Comcast_SSO?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Adobe-Subject-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJObUZtWmpjek5XVXROVFJoWWkwME5ERmlMV0V6Wm .....
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
   "logouts": {
      "Comcast_SSO": {
         "actionName": "logout",
         "actionType": "interactive",
         "mvpd": "Comcast_SSO",
         "url": "/adobe-services/logout?requestor_id=REF30&mso_id=Comcast_SSO&pt_device_id=c54fa2c80652b10abea58c...."
      }
   }
}
```

>[!ENDTABS]

### 5. 성능 저하가 적용되는 동안 특정 mvpd에 대한 로그아웃 시작

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/v2/REF30/logout/${degradedMvpd}?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "logouts": {
        "ATTOTT": {
            "actionName": "complete",
            "actionType": "none",
            "mvpd": "${degradedMvpd}",
        }
    }
}
```

>[!ENDTABS]

### 6. 기본 또는 프로모션 TempPass 로그아웃 시작(필수 아님)

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/v2/logout/REF30/TempPass_5mins?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "logouts": {
        "TempPass_5mins": {
            "actionName": "complete",
            "actionType": "none",
            "mvpd": "TempPass_5mins"
        }
    }
}
```

>[!ENDTABS]
