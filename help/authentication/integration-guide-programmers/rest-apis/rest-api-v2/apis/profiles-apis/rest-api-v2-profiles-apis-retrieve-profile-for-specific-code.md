---
title: 특정 코드에 대한 프로필 검색
description: REST API V2 - 특정 코드에 대한 프로필 검색
exl-id: d6ead7d5-de5f-4033-8115-980953a370c0
source-git-commit: be2b75d3dcde92c0b83700705892403291dcab2e
workflow-type: tm+mt
source-wordcount: '756'
ht-degree: 1%

---

# 특정 코드에 대한 프로필 검색 {#retrieve-profile-for-specific-code}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> REST API V2 구현은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md) 설명서에 의해 제한됩니다.

>[!MORELIKETHIS]
>
> [REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)도 방문하십시오.

## 요청 {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">경로</td>
      <td>/api/v2/{serviceProvider}/profiles/code/{code}</td>
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
      <td style="background-color: #DEEBFF;">코드</td>
      <td>스트리밍 장치에서 인증 세션을 만든 후 얻은 인증 코드입니다.</td>
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
      <td style="background-color: #DEEBFF;">AP-TempPass-Identity</td>
      <td>사용자 고유 식별자 페이로드 생성은 <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md">AP-TempPass-Identity</a> 헤더 문서에 설명되어 있습니다.</td>
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
        응답 본문에 유효한 프로필 맵이 포함되어 있으며, 이 맵은 비어 있을 수 있습니다.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>잘못된 요청</td>
      <td>
        요청이 잘못되었습니다. 클라이언트가 요청을 수정하고 다시 시도하십시오. 응답 본문에는 <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>승인되지 않음</td>
      <td>
        액세스 토큰이 잘못되었습니다. 클라이언트가 새 액세스 토큰을 얻은 후 다시 시도하십시오. 자세한 내용은 <a href="../../../rest-api-dcr/dynamic-client-registration-overview.md">동적 클라이언트 등록 개요</a> 설명서를 참조하십시오.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>금지됨</td>
      <td>
        임시 액세스 TTL(time-to-live)이 만료되었거나 최대 리소스 수를 초과했습니다. 클라이언트는 일반 MVPD을 사용하여 기본 인증 흐름을 시작하도록 사용자를 표시해야 합니다. 응답 본문에는 <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
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
        서버 측에서 문제가 발생했습니다. 응답 본문에는 <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
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
      <td style="background-color: #DEEBFF;">프로필</td>
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
            </tr>
         </table>
         값 요소는 다음 속성으로 정의됩니다.
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">속성</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>프로필이 유효하지 않은 타임스탬프(밀리초)입니다.</td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>프로필이 유효하지 않은 타임스탬프(밀리초)입니다.</td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">발급자</td>
               <td>
                  프로필을 소유하는 엔티티입니다.
                  <br/><br/>
                  가능한 값은 다음과 같습니다.
                  <ul>
                    <li><b>mvpd(예: Spectrum, Cablevision 등)</b><br/>기본 인증의 결과로 프로필이 만들어졌습니다.</li>
                    <li><b>Adobe</b><br/>액세스 성능 저하, 임시 액세스로 인해 프로필이 만들어졌습니다.</li>
                  </ul>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">유형</td>
               <td>
                  프로필의 유형입니다.
                  <br/><br/>
                  가능한 값은 다음과 같습니다.
                  <ul>
                    <li><b>일반</b><br/>기본 인증의 결과로 프로필이 만들어졌습니다.</li>
                    <li><b>성능 저하</b><br/>액세스 성능 저하로 인해 프로필이 만들어졌습니다.</li>
                    <li><b>임시</b><br/>임시 액세스로 인해 프로필이 만들어졌습니다.</li>
                  </ul>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">속성</td>
               <td>
                    키, 값 쌍의 맵이 포함된 JSON.
                    <br/><br/>
                    키 요소는 사용자 메타데이터 특성으로 정의되며 다음과 같은 작업을 수행할 수 있습니다.
                    <ul>
                        <li>필수, 예: 'userID'</li>
                        <li>'zip', 'householdID', 'maxRating' 등과 같이 필수가 아닙니다.</li>
                    </ul>
                    속성의 값은 다음과 같을 수 있습니다.
                    <ul>
                        <li>단순</li>
                        <li>목록</li>
                        <li>맵</li>
                    </ul>
                    사용자 메타데이터는 인증 흐름이 완료된 후에 사용할 수 있게 되지만 특정 메타데이터 속성은 MVPD 및 해당 특정 메타데이터 속성에 따라 인증 흐름 중에 업데이트될 수 있습니다.
               </td>
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
      <td>400, 401, 403, 405, 500</td>
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
      <td>응답 본문은 <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">향상된 오류 코드</a> 설명서를 준수하는 추가 오류 정보를 제공할 수 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
</table>

## 샘플 {#samples}

### &#x200B;1. 기본 인증을 통해 얻은 특정 코드에 대한 프로필 검색

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/v2/REF30/profiles/code/XTC98W HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "Cablevision": {
            "notBefore": 1752149281000,
            "notAfter": 1783685280000,
            "issuer": "Cablevision",
            "type": "regular",
            "attributes": {
                "userID": {
                    "value": "BASE64_value_userId",
                    "state": "plain"
                },
                "householdID": {
                    "value": "BASE64_value_householdId",
                    "state": "plain"
                },
                "zip": {
                    "value": "BASE64_value_zip",
                    "state": "enc"
                }
            }
        }
     }
}
```

>[!ENDTABS]

### &#x200B;2. 기본 TempPass가 선택된 동안 특정 코드에 대한 프로필 검색

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/v2/REF30/profiles/code/XTC98W HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 응답 - 사용 가능]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "TempPass_TEST40": {
            "notBefore": 1697718650206,
            "notAfter": 1697718710206,
            "issuer": "Adobe",
            "type": "temporary",
            "attributes": {
                "expiration_date": {
                    "value": 1697718710206,
                    "state": "plain"
                },
                "userID": {
                    "value": "temppass_0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!TAB 응답 - 기간 제한 초과]

```HTTPS
HTTP/1.1 403 Forbidden

Content-Type: application/json;charset=UTF-8

{
    "status": 403,
    "code": "temporary_access_duration_limit_exceeded",
    "message": "The temporary access duration limit has been exceeded.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "authentication"
}
```

>[!TAB 응답 - 잘못된 구성]

```HTTPS
HTTP/1.1 500 Internal Server Error

Content-Type: application/json;charset=UTF-8

{
    "status": 500,
    "code": "invalid_configuration_temporary_access",
    "message": "The temporary access configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "configuration"
}
```

>[!ENDTABS]

### &#x200B;3. 프로모션 TempPass가 선택된 동안 특정 코드에 대한 프로필 검색

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/v2/REF30/profiles/code/XTC98W HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-TempPass-Identity: eyJlbWFpbCI6ImZvb0BiYXIuY29tIn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 응답 - 사용 가능]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "flexibleTempPass": {
            "notBefore": 1697720528524,
            "notAfter": 1697720588524,
            "issuer": "Adobe",
            "type": "temporary",
            "attributes": {
                "remaining_resources": {
                    "value": 1,
                    "state": "plain"
                },
                "used_assets": {
                    "value": [
                        "res04",
                        "res02",
                        "res03",
                        "res01"
                    ],
                    "state": "plain"
                },
                "expiration_date": {
                    "value": 1697720528524,
                    "state": "plain"
                },
                "userID": {
                    "value": "temppass_0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!TAB 응답 - 기간 제한 초과]

```HTTPS
HTTP/1.1 403 Forbidden

Content-Type: application/json;charset=UTF-8

{
    "status": 403,
    "code": "temporary_access_duration_limit_exceeded",
    "message": "The temporary access duration limit has been exceeded.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "authentication"
}
```

>[!TAB 응답 - 리소스 제한 초과]

```HTTPS
HTTP/1.1 403 Forbidden

Content-Type: application/json;charset=UTF-8

{
    "status": 403,
    "code": "temporary_access_resources_limit_exceeded",
    "message": "The temporary access resources limit has been exceeded.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "authentication"
}
```

>[!TAB 응답 - 잘못된 구성]

```HTTPS
HTTP/1.1 500 Internal Server Error

Content-Type: application/json;charset=UTF-8

{
    "status": 500,
    "code": "invalid_configuration_temporary_access",
    "message": "The temporary access configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "configuration"
}
```

>[!TAB 응답 - 잘못된 ID]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{
    "status": 400,
    "code": "invalid_header_identity_for_temporary_access",
    "message": "The identity for temporary access header value is missing or invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### &#x200B;4. 저하가 적용되는 동안 특정 코드에 대한 프로필 검색

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/v2/REF30/profiles/code/XTC98W HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 응답 - AuthNaLl 저하]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "${degradedMvpd}": {
            "notBefore": 1697719042666,
            "notAfter": 1697719102666,
            "issuer": "Adobe",
            "type": "degraded",
            "attributes":
                "userID": {
                    "value": "95cf93bcd183214a0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!IMPORTANT]
>
> `95cf93bcd183214a`은(는) 성능 저하 관련 접두사입니다.

>[!ENDTABS]
