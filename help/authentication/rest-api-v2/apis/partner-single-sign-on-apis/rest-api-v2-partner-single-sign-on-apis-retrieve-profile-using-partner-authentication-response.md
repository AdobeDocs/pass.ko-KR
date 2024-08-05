---
title: 파트너 인증 응답을 사용하여 프로필 검색
description: REST API V2 - 파트너 인증 응답을 사용하여 프로필 검색
source-git-commit: 4598aaa0827b943de83a9e7d847227edf6b0b387
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 1%

---


# 파트너 인증 응답을 사용하여 프로필 검색 {#retrieve-profile-using-partner-authentication-response}

>[!NOTE]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> REST API V2 구현은 [조절 메커니즘](/help/authentication/throttling-mechanism.md) 설명서에 의해 제한됩니다.

## 요청 {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">경로</td>
      <td>/api/v2/{serviceProvider}/profiles/sso/{partner}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">방법</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">경로 매개 변수</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>온보딩 프로세스 중 서비스 공급자와 연결된 내부 고유 식별자입니다.</td>
      <td><i>필수</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">파트너</td>
      <td>Adobe Pass 인증 흐름과 통합된 SSO(Single Sign-On) 프레임워크를 제공하는 파트너(예: Apple)의 이름.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">본문 매개 변수</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">SAMLResponse</td>
      <td>
        파트너 프로필을 만들고 저장하는 데 필요한 사용자 메타데이터가 포함된 파트너 인증 응답입니다.
        <br/><br/>
        이 값은 Base64로 인코딩한 다음 URL로 인코딩해야 합니다.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">인증</td>
      <td>전달자 토큰 페이로드의 생성은 <a href="../../../dynamic-client-registration-api.md">동적 클라이언트 등록</a> 문서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
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
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>장치 식별자 페이로드의 생성은 <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> 문서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         장치 정보 페이로드의 생성은 <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> 설명서에 설명되어 있습니다.
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
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        Partner 메서드에 대한 SSO(Single Sign-On) 페이로드의 생성은 <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a> 문서에 설명되어 있습니다.
        <br/><br/>
        파트너를 사용한 Single Sign-On 사용 흐름에 대한 자세한 내용은 <a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-partner-flows.md">파트너 흐름을 사용한 Single Sign-On</a> 설명서를 참조하십시오.</td>
      <td>선택 사항</td>
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 10%;">코드</th>
      <th style="background-color: #EFF2F7; width: 20%;">텍스트</th>
      <th style="background-color: #EFF2F7;">설명</th>
   </tr>
   <tr>
      <td>201</td>
      <td>생성됨</td>
      <td>
        응답 본문에 유효한 프로필 맵이 포함되어 있으며, 이 맵은 비어 있을 수 있습니다.
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
        액세스 토큰이 잘못되었습니다. 클라이언트가 새 액세스 토큰을 얻은 후 다시 시도하십시오. 자세한 내용은 <a href="../../../dynamic-client-registration-api.md">동적 클라이언트 등록</a> 설명서를 참조하십시오.
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">헤더</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">본문</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">프로필</td>
      <td>
         키, 값 쌍의 맵이 포함된 JSON.
         <br/><br/>
         키 요소는 다음 값으로 정의됩니다.
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">값</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>온보딩 프로세스 중 ID 공급자와 연결된 내부 고유 식별자입니다.</td>
               <td><i>필수</i></td>
            </tr>
         </table>
         값 요소는 다음 속성으로 정의됩니다.
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">속성</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>프로필이 유효하지 않은 타임스탬프.</td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>프로필이 유효하지 않은 타임스탬프.</td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">발급자</td>
               <td>
                  프로필을 소유하는 엔티티입니다.
                  <br/><br/>
                  가능한 값:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">값</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">Apple</td>
                        <td>
                            프로필은 다음과 같은 결과로 생성되었습니다.
                            <ul>
                                <li>파트너 Apple을 사용한 SSO(Single Sign-On)</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               </td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">유형</td>
               <td>
                  프로필의 유형입니다.
                  <br/><br/>
                  가능한 값:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">값</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">appleSSO</td>
                        <td>
                            프로필은 다음과 같은 결과로 생성되었습니다.
                            <ul>
                                <li>파트너 Apple을 사용한 SSO(Single Sign-On)</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               </td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">속성</td>
               <td>
                    사용자 메타데이터 속성 목록입니다.
                    <br/><br/>
                    이러한 속성은 다음과 같을 수 있습니다.
                    <ul>
                        <li>필수, 예: 'userId'</li>
                        <li>'zip', 'householdId', 'maxRating' 등과 같이 필수가 아닙니다.</li>
                    </ul>
                    속성의 값은 다음과 같을 수 있습니다.
                    <ul>
                        <li>단순</li>
                        <li>목록</li>
                        <li>맵</li>
                    </ul>
               </td>
               <td><i>필수</i></td>
            </tr>
         </table>
      </td>
      <td><i>필수</i></td>
</table>

### 오류 {#error}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">본문</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">오류</td>
      <td>오류는 <a href="../../../enhanced-error-codes.md">향상된 오류 코드</a> 설명서를 준수하는 추가 정보를 제공합니다.</td>
      <td><i>필수</i></td>
   </tr>
</table>

## 샘플 {#samples}

### 1. Apple SSO 활성화 및 유효한 SAML 응답

>[!BEGINTABS]

>[!TAB 요청]

```JSON
POST /api/v2/REF30/profiles/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB 응답]

```JSON
HTTP/1.1 200 OK
 
{
    "profiles" : {
        "Cablevision" : {
            "notBefore" : 1623943955,
            "notAfter" : 1623951155,
            "issuer" : "Apple",
            "type" : "appleSSO",
            "attributes" : {
                "userId" : {
                    "value" : "BASE64_value_userId",
                    "state" : "plain"
                },
                "householdId" : {
                    "value" : "BASE64_value_householdId",
                    "state" : "plain"
                },
                "zip" : {
                    "value" : "BASE64_value_zip",
                    "state" : "enc"
                }       
            }
        }
     }
}  
```

>[!ENDTABS]

### 2. 통합 저하를 위한 AppleSSO 프로필

>[!BEGINTABS]

>[!TAB 요청]

```JSON
POST /api/v2/REF30/profiles/sso/Apple HTTP/1.1
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB 응답]

```JSON
HTTP/1.1 200 OK
 
{
   "profiles":{
      "WOW":{
         "notBefore":1706636062704,
         "notAfter":1706696062704,
         "issuer":"Adobe",
         "type":"degraded",
         "attributes":{
            "userID":{
               "value":"95cf93bcd183214ac9e4433153cb8a9d180a463128c0a5d26f202e8c",
               "state":"plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### 3. SAML 응답이 유효하지 않은 경우 Apple SSO 흐름

>[!BEGINTABS]

>[!TAB 요청]

```JSON
POST /api/v2/REF30/profiles/sso/Apple HTTP/1.1 
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:
        
SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB 응답]

```JSON
HTTP/1.1 403 OK
Content-Type: application/json; charset=utf-8
    
{
    "errors" : [
        {
            "code": "invalid_mvpd_response",
            "message": "The saml mvpd response is not valid",
            "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "action": "none"        
        } 
    ]
}   
```

>[!ENDTABS]
