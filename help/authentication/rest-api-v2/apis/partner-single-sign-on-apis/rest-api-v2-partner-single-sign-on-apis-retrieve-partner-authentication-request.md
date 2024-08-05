---
title: 파트너 인증 요청 검색
description: REST API V2 - 파트너 인증 요청 검색
source-git-commit: 4598aaa0827b943de83a9e7d847227edf6b0b387
workflow-type: tm+mt
source-wordcount: '1104'
ht-degree: 0%

---


# 파트너 인증 요청 검색 {#retrieve-partner-authentication-request}

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
      <td>/api/v2/{serviceProvider}/sessions/sso/{partner}</td>
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
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        MVPD 로그인을 수행하는 애플리케이션의 원본 도메인입니다.
        <br/><br/>
        스트리밍 장치 플랫폼에서 값을 제공하는 데 제한이 있는 경우 애플리케이션은 인증 세션을 다시 시작하고 유효한 값을 제공해야 합니다.
        <br/><br/>
        이는 응답이 스트리밍 애플리케이션이 기본 인증 플로우를 진행해야 함을 나타내는 폴백 시나리오의 경우에 사용됩니다.
      </td>
      <td><i>필수</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        MVPD에 대한 인증 흐름이 완료될 때 사용자 에이전트가 탐색하는 최종 리디렉션 URL입니다.
        <br/><br/>
        값은 URL로 인코딩되어야 합니다.
        <br/><br/>
        스트리밍 장치 플랫폼에서 값을 제공하는 데 제한이 있는 경우 애플리케이션은 인증 세션을 다시 시작하고 유효한 값을 제공해야 합니다.
        <br/><br/>
        이는 응답이 스트리밍 애플리케이션이 기본 인증 플로우를 진행해야 함을 나타내는 폴백 시나리오의 경우에 사용됩니다.
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
      <td>200</td>
      <td>확인</td>
      <td>
        응답 본문에는 인증을 수행하는 데 필요한 다음 작업에 대한 정보가 포함되어 있습니다.
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
      <td>200</td>
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
      <td style="background-color: #DEEBFF;"></td>
      <td>
         다음 속성이 있는 JSON 개체:
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">속성</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  인증 흐름을 완료하기 위해 스트리밍 장치가 수행해야 하는 작업입니다.
                  <br/><br/>
                  가능한 값은 다음과 같습니다.
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">값</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">partner_profile</td>
                        <td>스트리밍 디바이스는 제공된 파트너 인증 요청을 사용하여, 프로필을 검색하는데 활용될 수 있는 파트너 인증 응답을 획득할 수 있다.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">인증</td>
                        <td>
                            파트너 SSO(Single Sign-On) 플로우가 진행될 수 없는 경우, 스트리밍 디바이스는 기본 인증 플로우로 폴백될 수 있다.
                            <br/><br/>
                            스트리밍 장치 또는 다른 장치는 사용자 에이전트에서 제공된 URL을 열어야 합니다.
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">다시 시작</td>
                        <td>
                            파트너 SSO(Single Sign-On) 플로우가 진행될 수 없는 경우, 스트리밍 디바이스는 기본 인증 플로우로 폴백될 수 있다.
                            <br/><br/>
                            스트리밍 장치 또는 다른 장치는 누락된 매개 변수를 제공하고 코드를 사용하여 인증 세션을 다시 시작해야 합니다.
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">authorize</td>
                        <td>스트리밍 디바이스는 결정 흐름들을 직접 진행할 수 있다.</td>
                     </tr>
                  </table>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  'actionName' 특성에 지정된 작업으로 흐름을 계속하려면 스트리밍 장치가 수행해야 하는 상호 작용 유형입니다.
                  <br/><br/>
                  가능한 값은 다음과 같습니다.
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">값</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">직접</td>
                        <td>이 흐름은 클라이언트 구현에 사용할 수 있는 HTTP 클라이언트를 사용하여 제공된 URL을 직접 호출하여 계속됩니다.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">인터랙티브한</td>
                        <td>흐름은 사용자 에이전트를 사용하여 제공된 URL을 탐색하는 방식으로 계속됩니다.</td>
                     </tr>
                  </table>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>
                    기본 인증 흐름을 완료하기 위해 제공해야 하는 누락된 매개 변수입니다.
                    <br/><br/>
                    이 필드는 파트너 SSO(Single Sign-On) 흐름을 진행할 수 없는 경우에 표시됩니다.
               </td>
               <td>선택 사항</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>클라이언트 애플리케이션이 탐색해야 하는 URL입니다.</td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">코드</td>
               <td>
                    인증 세션을 다시 시작하는 데 보조 응용 프로그램에서 사용할 수 있는 인증 코드입니다.
                    <br/><br/>
                    이 필드는 파트너 SSO(Single Sign-On) 흐름을 진행할 수 없는 경우에 표시됩니다.
               </td>
               <td>선택 사항</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">authenticationRequest</td>
               <td>
                    Adobe Pass 인증 시스템 외부의 파트너와의 인증 흐름에 사용될 파트너 인증 요청입니다.
                    <br/><br/>
                    이 필드는 파트너 SSO(Single Sign-On) 흐름을 진행할 수 있는 경우에 표시됩니다.
                    <br/><br/>
                    다음 속성이 있는 JSON 개체:
                    <table>
                        <tr>
                            <th style="background-color: #EFF2F7; width: 30%;">속성</th>
                            <th style="background-color: #EFF2F7;"></th>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">유형</td>
                            <td>
                                MVPD에서 지원하는 프로토콜 유형을 나타냅니다.
                                <br/><br/>
                                가능한 값은 다음과 같습니다.
                                <table>
                                    <tr>
                                        <th style="background-color: #EFF2F7; width: 30%;">값</th>
                                        <th style="background-color: #EFF2F7;"></th>
                                    </tr>
                                    <tr>
                                        <td style="background-color: #DEEBFF;">saml</td>
                                        <td>MVPD는 SAML 프로토콜을 지원합니다.</td>
                                    </tr>
                                </table>
                            </td>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">요청</td>
                            <td>SAML 요청.</td>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">속성</td>
                            <td>SAML 요청 속성입니다.</td>
                        </tr>
                    </table>
               </td>
               <td>선택 사항</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>사용자 활동 추적에 사용할 수 있는 불투명 식별자입니다.</td>
               <td><i>필수</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>온보딩 프로세스 중 ID 공급자와 연결된 내부 고유 식별자입니다.</td>
               <td>선택 사항</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>온보딩 프로세스 중 서비스 공급자와 연결된 내부 고유 식별자입니다.</td>
               <td><i>필수</i></td>
            </tr>
         </table>
      </td>
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

### 1. 유효한 파트너 SSO 활성화됨

>[!BEGINTABS]

>[!TAB 요청]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 응답]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"partner_profile",
   "actionType":"direct",
   "url":"/v2/REF30/profiles/sso/Apple/Cablevision",
   "sessionId":"83c046be-ea4b-4581-b5f2-13e56e69dee9",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30",
   "authenticationRequest":{
      "type":"saml",
      "request":"PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRG...."
   }
}    
```

>[!ENDTABS]

### 2. 성능이 저하된 MVPD

>[!BEGINTABS]

>[!TAB 요청]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 응답]

```JSON
HTTP/1.1 200 OK
 
{          
   "actionName" : "authorize",
   "actionType" : "direct",
   "url" : "/api/v2/REF30/decisions",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30",
   "sessionId":"14d4f239-e3b1-4a4a-b8b3-6395b968a260"
}
```

>[!ENDTABS]

### 3. 비활성화된 통합

>[!BEGINTABS]

>[!TAB 요청]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 응답]

```JSON
HTTP/1.1 403
Content-Type: application/json; charset=utf-8
 
{
    "errors" : [
        {
            "code": "unknown_integration",
            "message": "The integration between the specified programmer and identity provider doesn't exist or it's disabled. Use the TVE Dashboard to register or enable the required integration.",
            "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "action": "none"        
        } 
    ]
}
```

>[!ENDTABS]

### 4. Partner SSO가 활성화되지 않음(전달 구성 또는 VSA에서) - 일반 인증으로 폴백하고 필요한 모든 매개 변수가 있습니다.

>[!BEGINTABS]

>[!TAB 요청]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status : ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 응답]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"authenticate",
   "actionType":"interactive",
   "url":"/v2/authenticate/REF30/OKTWW2W",
   "code":"OKTWW2W",
   "sessionId":"748f0b9e-a2ae-46d5-acd9-4b4e6d71add7",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30"
}  
```

>[!ENDTABS]

### 5. 파트너 SSO가 활성화되지 않음(전달 구성 또는 VSA에서) - 일반 인증으로 폴백, 필수 매개 변수 누락, 인증 세션을 다시 시작해야 함

>[!BEGINTABS]

>[!TAB 요청]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:
        
domainName=adobe.com
```

>[!TAB 응답]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"resume",
   "actionType":"direct",
   "missingParameters":[
      "redirectUrl"
   ],
   "url":"/v2/REF30/sessions/SB7ZRIO",
   "code":"SB7ZRIO",
   "sessionId":"1476173f-5088-43b8-b7c3-8cf3a185de0a",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30"
}
```

>[!ENDTABS]
