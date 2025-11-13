---
title: Adobe Single Sign-On 서비스
description: 여러 디바이스와 애플리케이션 간에 원활한 인증을 가능하게 하는 Adobe Pass SSO 서비스에 대해 알아봅니다.
exl-id: a1ff85d4-f7d2-4dea-b82f-d29730d9012f
source-git-commit: 151c64276377be5ef21bca4c0d3eaa04ac3da495
workflow-type: tm+mt
source-wordcount: '3615'
ht-degree: 2%

---


# Adobe Single Sign-On 서비스 {#sso-service}

이 문서에서는 Adobe Single Sign-On 서비스의 사용 사례, 끝점 및 API에 대해 설명합니다.

**현재 수정 버전 - 1.0.0**

## 범위 {#scope}

Adobe Pass SSO 서비스를 통해 여러 디바이스와 애플리케이션에서 원활한 인증을 지원하여 보안 및 규정 준수 표준을 유지하면서 통합된 사용자 환경을 제공합니다. 이 서비스는 오늘날 멀티 디바이스 스트리밍 환경에서 플랫폼 간 인증이 점점 더 필요하다는 점을 해결합니다.

## 개요 {#overview}

### 정의 {#what-it-is}

사용자가 한 번 인증하고 다른 장치로 정보에 입각한 방식으로 인증 전송을 관리할 수 있는 포괄적인 단일 사인온 솔루션

### 현재 인증 문제 {#current-challenges}

* 사용자는 구독이 있는 모든 스트리밍 서비스를 인증해야 합니다.
* 사용자는 각 장치 또는 애플리케이션에서 별도로 인증해야 합니다.
* 일부 플랫폼의 인증 흐름에서 암호를 입력하는 데 어려움이 있어 포기 비율이 증가할 수 있습니다

### Adobe Pass SSO 서비스 기회 {#opportunity}

* 자체 인증을 필요로 하는 스트리밍 서비스의 수가 증가하고 있습니다.
* 원활한 크로스 디바이스 경험에 대한 수요 증가
* 가구당 스트리밍 장치의 수 증가
* 가구당 통합 인증 필요

### 비즈니스 이점 {#business-benefits}

#### 콘텐츠 공급자 {#content-providers}

* **사용자 참여 증가** - 원활한 경험으로 인해 세션 시간이 길어짐
* **마찰 감소** - 인증 장벽이 낮을수록 콘텐츠 소모가 증가합니다.
* **향상된 유지** - 사용자 경험이 개선되어 이탈이 줄어듭니다.
* **비용 절감** - 인증 문제와 관련된 지원 티켓 감소

#### 최종 사용자 {#end-users}

* **원활한 경험** - 한 번 인증하고 어디에서나 액세스합니다.
* **시간 절약** - 반복 로그인 프로세스 없음
* **장치 유연성** - 중단 없이 장치 간 전환
* **일관된 경험** - 모든 플랫폼에서 균일한 인증

#### IdPs(MVPD, Telcos 등) {#idps}

* 인증된 SSO를 사용하여 MVPD에 추가 장치 알림
* **향상된 사용자 만족도** - 향상된 인증 경험
* **지원 로드 감소** - 인증 관련 지원 호출 수 감소
* **경쟁 우위** - 경쟁사보다 사용자 경험 향상

## 사용 사례 {#use-cases}

### ID 매핑 {#identity-mapping}

이 서비스는 동일한 앱에서 D2C와 TVE 계정을 연결하는 기능을 빌드합니다.

더 많은 스트리밍 서비스는 서드파티(MVPD/virtual MVPD/Telco 등)에게 판매할 번들을 구축하는 것입니다. 사용자는 동일한 앱에서 여러 계정을 처리해야 할 수 있습니다. 원활한 인증 경험을 만들려면 서비스는 이러한 계정을 연결하여 로그인 경험을 단순화해야 합니다.

사용자는 D2C 계정으로 인증한 다음 다른 계정으로 한 번 인증합니다(예: MVPD). 당사의 서비스를 사용하면 이러한 계정이 연결되므로 앞으로 다른 장치에서 후속 인증은 D2C 계정으로만 수행할 수 있습니다.

### 크로스 디바이스 SSO {#cross-device-sso}

TV 연결 장치에서의 인증은 휴대 전화에서보다 더 번거로울 수 있다. 휴대폰에서 인증한 다음 해당 인증을 스마트 TV에 전달하는 것이 좋은 사용자 경험입니다.

## 주요 구성 요소 {#key-components}

* **서비스 토큰 API** - SSO(Single Sign-On) 메커니즘을 안전하게 관리하는 주요 구성 요소
* **목록 API** - 응용 프로그램을 통해 사용자가 에코시스템의 장치 목록을 이해할 수 있습니다.
* **API 연결** - 사용자가 에코시스템에 장치를 추가할 수 있도록 응용 프로그램에서 설정합니다.
* **API 연결 해제** - 응용 프로그램을 통해 사용자가 에코시스템에서 장치를 제거할 수 있습니다.

## 자세한 사용 사례 {#use-cases-detailed}

### D2C-TVE SSO {#d2c-tve-sso}

이 사용 사례를 통해 한 장치에서 D2C와 TVE(MVPD) 자격 증명 간에 연결하고 연결된 프로필을 동일한 애플리케이션 내의 다른 장치에서 활용할 수 있습니다.

![D2C-TVE SSO 흐름](../../../assets/sso_service_d2c_1.png)

![장치 간 SSO 흐름](../../../assets/sso_service_d2c_2.png)

### 크로스 디바이스 SSO {#cross-device-sso-detailed}

이 사용 사례를 사용하면 한 디바이스에서 이미 인증된 사용자가 다른 디바이스에 설치된 동일한 D2C 또는 TVE 애플리케이션(인증 및 권한 부여를 위해 Adobe Pass REST V2를 구현하는 데 필요한 애플리케이션)에서 인증된 프로필을 재사용할 수 있습니다.

## D2C-TVE 애플리케이션에서 Adobe SSO 서비스를 통합하는 방법 {#integration}

### 1단계 - 공통 식별자 가져오기 {#step-1}

Adobe SSO 서비스를 통합하려면 애플리케이션 구현에서 X-SSO-ID에서 공통 식별자 SSO 속성으로 사용할 고유한 영구 ID를 설정해야 합니다. 이는 D2C 서비스로 사용자를 인증함으로써 획득될 수 있고, 이 인증에 대한 속성을 보유한다.

### 2단계 - 서비스 토큰 가져오기 {#step-2}

POST /serviceToken 끝점에서 X-SSO-ID의 공통 식별자를 사용하면 다음 페이로드가 있는 서명된 JWT를 검색합니다.

```json
{
  "iss": "ssoservicetoken",
  "sub": "unique_common_identifier",
  "nbf": 1758093558,
  "exp": 1758097158,
  "iat": 1758093558
}
```

서비스 토큰에 &quot;iat&quot; - 발급 시간 및 &quot;exp&quot; - 만료 시간(서비스 토큰이 유효한 간격)이 있습니다. 만료 시 만료된 서비스 토큰과 함께 GET /serviceToken 끝점을 사용하여 새 JWT를 가져올 수 있습니다.

### 3단계 - TVE MVPD과 함께 Adobe Pass REST API V2를 사용하여 인증 {#step-3}

Adobe Pass을 사용한 인증은 서비스 토큰을 사용하여 구현해야 합니다. [REST API V2 - Single Sign-On 서비스 토큰 흐름](https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-flows/rest-api-v2-single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows)

### 4단계 - 다른 장치 연결 {#step-4}

인증된 애플리케이션에서 /link API를 사용하여 다른 디바이스에서 사용할 링크를 만들 수 있습니다

```json
{
    "status": "CREATED",
    "code": "228128",
    "notBefore": 1758094617220,
    "notAfter": 1758098217220
}
```

&quot;코드&quot;는 사용자가 보조 장치의 인증되지 않은 응용 프로그램에서 도입할 6자리 시퀀스 형식의 단기 링크 코드입니다

### 5단계 - Single Sign On 인증 검색 {#step-5}

다른 장치에서 사용자가 코드를 도입하면 애플리케이션은 다음과 같은 작업을 수행할 수 있습니다.

* D2C 서비스에서 id 검색
* REST V2 API를 사용하여 Adobe Pass에서 MVPD 프로필 검색

MVPD 프로필은 초기 인증을 받은 기간 동안 SSO를 통해 유효합니다.

MVPD이 프로필을 무효화하거나 사용자가 MVPD에서 로그아웃하도록 선택하는 경우 Adobe Pass REST API V2를 사용하는 애플리케이션에는 더 이상 프로필 레코드가 없으므로 사용자가 MVPD을 다시 인증해야 합니다.

### 6단계 - 다른 장치에서 단일 사인온 관리 {#step-6}

애플리케이션은 /list API를 사용하여 동일한 공통 식별자와 연결된 다른 모든 디바이스에 대한 정보를 가져올 수 있습니다.

언제든지 해당 장치를 /unlink API와 연결 해제하여 단일 사인온에서 장치를 제거할 수 있습니다.

## API {#apis}

### 서비스 토큰 API {#service-token-api}

#### 설명 {#service-token-description}

서비스 토큰 API는 여러 애플리케이션 또는 장치 간에 SSO(Single Sign-On) 기능을 활성화하는 서비스 토큰을 요청하고 관리하는 데 사용할 수 있습니다. 이러한 서비스 토큰은 인증된 프로필(SSO 프로필)을 식별하며 SSO 연결을 설정하고 유지하는 데 필수적입니다.

>[!WARNING]
>
>서비스 토큰에는 중요한 인증 정보가 포함됩니다. 응용 프로그램은 이러한 토큰을 안전하게 처리해야 하며 신뢰할 수 없는 파티에 노출해서는 안 됩니다.

서비스 토큰 API는 두 개의 기본 끝점을 제공합니다.

* **POST /api/{serviceProvider}/serviceToken** - 새로 만든 JWS 서비스 토큰을 가져옵니다.
* **GET /api/{serviceProvider}/serviceToken** - 기존 JWS 서비스 토큰 새로 고침

Adobe Pass 인증 서비스 오류로 인해 서비스 토큰 API 요청을 처리할 수 없는 경우 API 응답의 일부로 추가 오류 정보가 포함됩니다.

#### POST - serviceToken {#post-service-token}

##### 요청 {#post-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">경로</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">방법</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">경로 매개 변수</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>토큰을 요청하는 서비스 공급자 식별자입니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">인증</td>
      <td>전달자 토큰 페이로드의 생성은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">인증</a> 헤더 설명서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>
         장치 식별자 페이로드 생성은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> 헤더 문서에 설명되어 있습니다.
         <br/><br/>
         이 식별자는 X-SSO-ID가 제공되지 않을 때 기본 SSO 식별자로 사용됩니다.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-x-device-info">X-Device-Info</a> 헤더 설명서에 지정된 장치 정보입니다.
         <br/><br/>
         응용 프로그램의 장치 플랫폼에서 명시적으로 유효한 값을 제공할 수 있는 경우 <b>사용할 것을 권장</b>합니다.
         <br/><br/>
         Adobe Pass 인증 백엔드는 명시적으로 설정된 값을 암시적으로 추출된 값과 병합합니다. 제공하지 않으면 추출된 기본 값이 사용됩니다.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-링크</td>
      <td>
         이 요청을 기존 인증된 프로필과 연결하는 링크 코드입니다. 제공된 경우 응답에는 링크 코드를 생성한 프로필의 SSO를 위한 서비스 토큰이 포함됩니다.
         <br/><br/>
         일반적으로 보조 응용 프로그램 또는 장치가 기본 응용 프로그램 또는 장치에서 인증된 프로필에 연결하려고 할 때 사용됩니다.
      </td>
      <td>x-sso-id가 제공되지 않은 경우 필요합니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-ID</td>
      <td>
         애플리케이션이 SSO를 기반으로 하여 요청하는 공통 식별자입니다.
         <br/><br/>
         제공되면 이 식별자는 장치 및/또는 애플리케이션 간에 공통 SSO 프로필을 설정하는 데 사용됩니다.
      </td>
      <td>x-sso-link가 제공되지 않은 경우 필요합니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         클라이언트 애플리케이션에서 허용하는 미디어 유형입니다.
         <br/><br/>
         지정한 경우 application/json이어야 합니다.
      </td>
      <td>선택 사항</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>클라이언트 애플리케이션의 사용자 에이전트입니다.</td>
      <td>선택 사항</td>
   </tr>
</table>

##### 응답 {#post-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">코드</th>
      <th style="background-color: #EFF2F7;">텍스트</th>
      <th style="background-color: #EFF2F7;">설명</th>
   </tr>
   <tr>
      <td>201</td>
      <td>생성됨</td>
      <td>
        서비스 토큰이 생성되고 응답 본문에 반환되었습니다.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>잘못된 요청</td>
      <td>
        요청이 잘못되었습니다. 클라이언트가 요청을 수정하고 다시 시도하십시오. 응답 본문에는 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>승인되지 않음</td>
      <td>
        액세스 토큰이 잘못되었습니다. 클라이언트가 새 액세스 토큰을 얻은 후 다시 시도하십시오. 자세한 내용은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">동적 클라이언트 등록 개요</a> 설명서를 참조하십시오.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>내부 서버 오류</td>
      <td>
        서버 측에서 문제가 발생했습니다. 응답 본문에는 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
</table>

###### 성공 {#success-post-service-token}

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
      <td style="background-color: #DEEBFF;">상태</td>
      <td>HTTP 상태(예: "생성됨")</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>서비스 토큰이 포함된 Base64로 인코딩된 JSON JWS(웹 서명)입니다. 이 토큰은 인증된 프로필을 식별하고 SSO 기능을 활성화하기 위해 후속 API 호출에 사용할 수 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Epoch ms 또는 오류 시 0</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Epoch ms 또는 오류 시 0</td>
      <td><i>필수</i></td>
   </tr>
</table>

###### 오류 {#error-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">상태</td>
      <td>400, 401, 500</td>
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
      <td>응답 본문은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 추가 오류 정보를 제공할 수 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
</table>

## 샘플 {#samples-post-service-token}

### &#x200B;1. 새 서비스 토큰 요청(SSO ID 사용)

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-ID: <sso_id>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    Accept: application/json
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### &#x200B;2. 새 서비스 토큰 요청(SSO 링크 포함)

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-LINK: <link_code>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    User-Agent: <user_agent>
    Accept: application/json
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

#### GET - serviceToken {#get-service-token}

##### 요청 {#get-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">경로</td>
      <td>/api/{serviceProvider}/serviceToken</td>
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
      <td>토큰을 요청하는 서비스 공급자 식별자입니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">인증</td>
      <td>전달자 토큰 페이로드의 생성은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">인증</a> 헤더 설명서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">광고 서비스 토큰</td>
      <td>
         새로 고쳐야 하는 이전에 획득한 서비스 토큰입니다.
         <br/><br/>
         이 토큰은 유효하거나 최근 만료되었어야 새로 고칠 수 있습니다.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         클라이언트 애플리케이션에서 허용하는 미디어 유형입니다.
         <br/><br/>
         지정한 경우 application/json이어야 합니다.
      </td>
      <td>선택 사항</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>클라이언트 애플리케이션의 사용자 에이전트입니다.</td>
      <td>선택 사항</td>
   </tr>
</table>

##### 응답 {#get-service-token-response}

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
        서비스 토큰을 성공적으로 새로 고치고 응답 본문에 반환했습니다.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>잘못된 요청</td>
      <td>
        요청이 잘못되었습니다. 클라이언트가 요청을 수정하고 다시 시도하십시오. 응답 본문에는 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>승인되지 않음</td>
      <td>
        액세스 토큰 또는 서비스 토큰이 잘못되었습니다. 클라이언트가 새 액세스 토큰 또는 서비스 토큰을 얻은 후 다시 시도하십시오. 자세한 내용은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">동적 클라이언트 등록 개요</a> 설명서를 참조하십시오.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>내부 서버 오류</td>
      <td>
        서버 측에서 문제가 발생했습니다. 응답 본문에는 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
</table>

###### 성공 {#success-get-service-token}

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
      <td style="background-color: #DEEBFF;">상태</td>
      <td>HTTP 상태(예: "확인")</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>새로 고친 서비스 토큰이 포함된 Base64로 인코딩된 JSON JWS(웹 서명)입니다. 이 토큰은 인증된 프로필을 식별하고 SSO 기능을 활성화하기 위해 후속 API 호출에 사용할 수 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Epoch ms 또는 오류 시 0</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Epoch ms 또는 오류 시 0</td>
      <td><i>필수</i></td>
   </tr>
</table>

###### 오류 {#error-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">상태</td>
      <td>400, 401, 500</td>
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
      <td>응답 본문은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 추가 오류 정보를 제공할 수 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
</table>

## 샘플 {#samples-get-service-token}

### &#x200B;1. 서비스 토큰 새로 고침 요청

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### 링크 API {#link-api}

#### 설명 {#link-description}

링크 API는 여러 애플리케이션 또는 장치 간에 SSO(Single Sign-On)를 활성화할 수 있는 링크 코드(또는 QR 코드)를 요청하는 데 사용할 수 있습니다. 이 링크 코드를 사용하면 새 애플리케이션이나 장치를 기존의 인증된 프로필(SSO 프로필)에 연결하여 애플리케이션이나 장치 간에 원활한 SSO 경험을 제공할 수 있습니다.

링크 API를 사용하려면 AD-Service-Token 헤더에 올바른 서비스 토큰을 제공해야 합니다.

생성된 링크 코드는 일반적으로 기본 응용 프로그램 또는 장치의 사용자에게 표시되고 보조 응용 프로그램 또는 장치에 입력되어 SSO 연결을 설정합니다. 링크 코드는 제한된 유효 기간(일반적으로 5~30분)을 가지며 일회용입니다.

Adobe Pass 인증 서비스 오류로 인해 링크 API 요청을 처리할 수 없는 경우 추가 오류 정보가 링크 API 응답 결과의 일부로 포함됩니다.

#### 게시물 - 링크 {#post-link}

##### 요청 {#post-link-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">경로</td>
      <td>/api/{serviceProvider}/link</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">방법</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">경로 매개 변수</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>토큰을 요청하는 서비스 공급자 식별자입니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">인증</td>
      <td>전달자 토큰 페이로드의 생성은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">인증</a> 헤더 설명서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>장치 식별자 페이로드 생성은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> 헤더 문서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">광고 서비스 토큰</td>
      <td>
         서비스 토큰의 생성은 서비스 토큰 API 설명서에 설명되어 있습니다.
         <br/><br/>
         이 서비스 토큰은 링크 코드가 생성될 인증된 프로필을 식별합니다.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         클라이언트 애플리케이션에서 허용하는 미디어 유형입니다.
         <br/><br/>
         지정한 경우 application/json이어야 합니다.
      </td>
      <td>선택 사항</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>클라이언트 애플리케이션의 사용자 에이전트입니다.</td>
      <td>선택 사항</td>
   </tr>
</table>

##### 응답 {#post-link-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">코드</th>
      <th style="background-color: #EFF2F7;">텍스트</th>
      <th style="background-color: #EFF2F7;">설명</th>
   </tr>
   <tr>
      <td>201</td>
      <td>생성됨</td>
      <td>
        링크 코드가 응답 본문에 정상적으로 생성되고 반환되었습니다.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>잘못된 요청</td>
      <td>
        요청이 잘못되었습니다. 클라이언트가 요청을 수정하고 다시 시도하십시오. 응답 본문에는 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>승인되지 않음</td>
      <td>
        액세스 토큰이 잘못되었습니다. 클라이언트가 새 액세스 토큰을 얻은 후 다시 시도하십시오. 자세한 내용은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">동적 클라이언트 등록 개요</a> 설명서를 참조하십시오.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>내부 서버 오류</td>
      <td>
        서버 측에서 문제가 발생했습니다. 응답 본문에는 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
</table>

###### 성공 {#success-post-link}

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
      <td style="background-color: #DEEBFF;">상태</td>
      <td>HTTP 상태(예: "생성됨")</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">링크</td>
      <td>보조 응용 프로그램이나 장치에서 SSO 연결을 설정하는 데 사용할 수 있는 짧은 숫자 또는 영숫자 코드입니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>링크 코드가 유효해지는 타임스탬프(epoch 이후 밀리초)입니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>링크 코드가 만료되는 타임스탬프(epoch 이후 밀리초)입니다.</td>
      <td><i>필수</i></td>
   </tr>
</table>

###### 오류 {#error-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">상태</td>
      <td>400, 401, 500</td>
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
      <td>응답 본문은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 추가 오류 정보를 제공할 수 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
</table>

## 샘플 {#samples-post-link}

### &#x200B;1. 인증된 기존 프로필에 대한 링크 코드를 요청합니다

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
POST /api/{serviceProvider}/link HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{            
   "status": "CREATED",
   "code": "123456",
   "notBefore": 1623840000000,
   "notAfter": 1623842700000
}
```

>[!ENDTABS]

### API 연결 해제 {#unlink-api}

#### 설명 {#unlink-description}

링크 해제 API는 인증된 프로필(SSO 프로필)에서 장치 또는 여러 장치의 제거를 요청하는 데 사용할 수 있습니다. 이 API를 사용하면 사용자가 SSO 설정에서 장치의 연결을 끊을 수 있으므로 인증된 프로필에 액세스할 수 있는 장치를 제어할 수 있습니다.

>[!WARNING]
>
>링크 해제 API를 사용하려면 AD-Service-Token 헤더에 올바른 서비스 토큰을 제공해야 합니다.

Adobe Pass 인증 서비스 오류로 인해 연결 해제 API 요청을 처리할 수 없는 경우 추가 오류 정보가 연결 해제 API 응답 결과의 일부로 포함됩니다.

#### POST - 연결 해제 {#post-unlink}

##### 요청 {#post-unlink-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">경로</td>
      <td>/api/{serviceProvider}/링크 해제</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">방법</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">경로 매개 변수</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>서비스 공급자 식별자.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">본문 매개 변수</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">장치</td>
      <td>
         연결 해제할 장치 식별자 배열.
         <br/><br/>
         예:<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
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
      <td>전달자 토큰 페이로드의 생성은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">인증</a> 헤더 설명서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         전송 중인 리소스에 대해 허용되는 미디어 유형입니다.
         <br/><br/>
         application/json이어야 합니다.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>장치 식별자 페이로드 생성은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> 헤더 문서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">광고 서비스 토큰</td>
      <td>
         서비스 토큰의 생성은 서비스 토큰 API 설명서에 설명되어 있습니다.
         <br/><br/>
         이 서비스 토큰은 장치의 연결을 해제할 인증된 프로필을 식별합니다.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         클라이언트 애플리케이션에서 허용하는 미디어 유형입니다.
         <br/><br/>
         지정한 경우 application/json이어야 합니다.
      </td>
      <td>선택 사항</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>클라이언트 애플리케이션의 사용자 에이전트입니다.</td>
      <td>선택 사항</td>
   </tr>
</table>

##### 응답 {#post-unlink-response}

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
        요청한 장치가 SSO 설정에서 연결 해제되었습니다.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>잘못된 요청</td>
      <td>
        요청이 잘못되었습니다. 클라이언트가 요청을 수정하고 다시 시도하십시오. 응답 본문에는 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>승인되지 않음</td>
      <td>
        액세스 토큰이 잘못되었습니다. 클라이언트가 새 액세스 토큰을 얻은 후 다시 시도하십시오. 자세한 내용은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">동적 클라이언트 등록 개요</a> 설명서를 참조하십시오.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>메서드가 허용되지 않음</td>
      <td>
        HTTP 메서드가 잘못되었습니다. 클라이언트가 요청한 리소스에 대해 허용되는 HTTP 메서드를 사용하고 다시 시도하십시오.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>내부 서버 오류</td>
      <td>
        서버 측에서 문제가 발생했습니다. 응답 본문에는 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
</table>

###### 성공 {#success-post-unlink}

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
      <td style="background-color: #DEEBFF;">상태</td>
      <td>작업 결과에 대한 정보: "확인"</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">unlinkedDevices</td>
      <td>
         정상적으로 연결 해제된 장치 목록.
         <br/><br/>
         예:<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>필수</i></td>
   </tr>
</table>

###### 오류 {#error-post-unlink}

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
      <td>응답 본문은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 추가 오류 정보를 제공할 수 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
</table>

## 샘플 {#samples-post-unlink}

### &#x200B;1. SSO 프로필에서 장치 연결 해제 요청

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### &#x200B;2. 부분적인 성공이 있는 장치의 연결 해제 요청

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "unknowndevice",
    "deviceid3"
  ]
}
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### 목록 API {#list-api}

#### 설명 {#list-description}

목록 API를 사용하여 인증된 프로필(SSO 프로필)에 현재 연결된 장치 목록을 가져올 수 있습니다. 이 API를 통해 사용자와 애플리케이션은 SSO 설정의 일부인 디바이스를 볼 수 있으므로 여러 디바이스에서 인증된 환경을 위한 가시성 및 관리 기능을 제공할 수 있습니다.

>[!WARNING]
>
>목록 API를 사용하려면 AD-Service-Token 헤더에 올바른 서비스 토큰을 제공해야 합니다.

목록 API는 사용자가 장치를 인식하는 데 도움이 될 수 있는 장치 식별자 및 메타데이터를 포함하여 인증된 프로필(SSO 프로필)의 각 장치에 대한 세부 정보를 반환합니다. 이 정보는 사용자가 SSO 설정에서 유지해야 하는 장치에 대해 정보에 입각한 결정을 내리는 데 도움이 될 수 있습니다.

Adobe Pass 인증 서비스 오류로 인해 목록 API 요청을 처리할 수 없는 경우 추가 오류 정보가 목록 API 응답 결과의 일부로 포함됩니다.

#### GET - 목록 {#get-list}

##### 요청 {#get-list-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">경로</td>
      <td>/api/{serviceProvider}/list</td>
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
      <td>서비스 공급자 식별자.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">인증</td>
      <td>전달자 토큰 페이로드의 생성은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">인증</a> 헤더 설명서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>장치 식별자 페이로드 생성은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> 헤더 문서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">광고 서비스 토큰</td>
      <td>
         서비스 토큰의 생성은 서비스 토큰 API 설명서에 설명되어 있습니다.
         <br/><br/>
         이 서비스 토큰은 장치 목록을 검색할 인증된 프로필을 식별합니다.
      </td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         클라이언트 애플리케이션에서 허용하는 미디어 유형입니다.
         <br/><br/>
         지정한 경우 application/json이어야 합니다.
      </td>
      <td>선택 사항</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>클라이언트 애플리케이션의 사용자 에이전트입니다.</td>
      <td>선택 사항</td>
   </tr>
</table>

##### 응답 {#get-list-response}

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
        SSO 설정의 장치 목록을 검색하고 응답 본문에 반환했습니다.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>잘못된 요청</td>
      <td>
        요청이 잘못되었습니다. 클라이언트가 요청을 수정하고 다시 시도하십시오. 응답 본문에는 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>승인되지 않음</td>
      <td>
        액세스 토큰이 잘못되었습니다. 클라이언트가 새 액세스 토큰을 얻은 후 다시 시도하십시오. 자세한 내용은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">동적 클라이언트 등록 개요</a> 설명서를 참조하십시오.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>메서드가 허용되지 않음</td>
      <td>
        HTTP 메서드가 잘못되었습니다. 클라이언트가 요청한 리소스에 대해 허용되는 HTTP 메서드를 사용하고 다시 시도하십시오.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>내부 서버 오류</td>
      <td>
        서버 측에서 문제가 발생했습니다. 응답 본문에는 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 오류 정보가 포함될 수 있습니다.
      </td>
   </tr>
</table>

###### 성공 {#success-get-list}

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
      <td style="background-color: #DEEBFF;">장치</td>
      <td>
         키, 값 쌍의 맵이 포함된 JSON.
         <br/><br/>
         <b>키:</b> deviceId - <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> 헤더 설명서에 설명된 장치 식별자 페이로드
         <br/><br/>
         <b>값:</b> 특성 - 다음을 포함한 장치 메타데이터 특성 맵이 포함된 JSON:
         <ul>
            <li>장치 유형</li>
            <li>platform</li>
            <li>사용자 에이전트</li>
            <li>장치를 식별하는 데 도움이 되는 기타 관련 메타데이터</li>
         </ul>
         속성 값은 단순(문자열, 정수, 부울 등)일 수 있습니다.
      </td>
      <td><i>필수</i></td>
   </tr>
</table>

###### 오류 {#error-get-list}

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
      <td>응답 본문은 <a href="https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">향상된 오류 코드</a> 설명서를 준수하는 추가 오류 정보를 제공할 수 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
</table>

## 샘플 {#samples-get-list}

### &#x200B;1. SSO 프로필에 장치를 나열하도록 요청

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {
    "deviceid1": {
      "deviceType": "smartTV",
      "model": "Samsung",
      "os": "Tizen",
      "osVersion": "5.0",
      "lastSeen": 1623840000000,
      "type": "regular"
    },
    "deviceid2": {
      "deviceType": "mobile",
      "model": "iPhone",
      "os": "iOS",
      "osVersion": "14.5",
      "lastSeen": 1623830000000,
      "type": "sso"
    },
    "deviceid3": {
      "deviceType": "tablet",
      "model": "iPad",
      "os": "iPadOS",
      "osVersion": "14.5",
      "lastSeen": 1623820000000,
      "type": "sso"
    }
  }
}
```

>[!ENDTABS]

### &#x200B;2. 연결된 장치가 없는 장치 나열 요청

>[!BEGINTABS]

>[!TAB 요청]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB 응답]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {}
}
```

>[!ENDTABS]

## 오류 코드 {#error-codes}

### 오류 응답 구조 {#error-structure}

모든 오류 응답에는 다음 필드가 포함됩니다.

| 필드 | 유형 | 설명 |
|:---|:---|:---|
| 상태 | 정수 | HTTP 상태 코드(400, 401, 500 등) |
| 코드 | 문자열 | 기계가 읽을 수 있는 오류 코드 |
| 메시지 | 문자열 | 사람이 읽을 수 있는 설명 |
| 작업 | 문자열 | 클라이언트에 대한 권장 작업 |
| helpUrl | 문자열 | 설명서 링크 |
| 추적 | 문자열 | 상관 관계에 대한 고유 요청 ID(UUID) |

**오류 응답 예**

```json
{
  "status": "BAD_REQUEST",
  "error": {
    "status": 400,
    "code": "header_missing",
    "message": "Required header is missing",
    "action": "check_headers",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "trace": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
  }
}
```

**성공 응답 예**

```json
{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIs...",
  "notBefore": 1697500800000,
  "notAfter": 1697587200000
}
```

>[!NOTE]
>
>성공 응답은 오류 필드를 포함하지 않습니다. 오류 응답에 성공 필드가 포함되지 않습니다.

### 오류 코드 카탈로그 {#error-catalog}

#### 일반 오류

| 코드 | 상태 | 메시지 | 작업 |
|:---|:---|:---|:---|
| token_invalid | 400 | 입력한 토큰이 잘못되었습니다. | get_new_token |
| token_expired | 401 | 토큰이 만료되었습니다 | get_new_token |
| header_missing | 400 | 필수 헤더가 누락되었습니다. | check_headers |
| 승인되지 않음 | 401 | 승인되지 않은 액세스 | 없음 |
| internal_error | 500 | 내부 오류가 발생했습니다. | 없음 |

#### 유효성 검사 오류

| 코드 | 상태 | 메시지 | 작업 |
|:---|:---|:---|:---|
| request_null | 400 | 요청 개체는 null일 수 없음 | 없음 |

#### POST ServiceToken 오류

| 코드 | 상태 | 메시지 | 작업 |
|:---|:---|:---|:---|
| header_missing | 400 | POST 요청에 x-sso-id 또는 x-sso-link 헤더가 필요합니다. | check_headers |
| header_missing | 400 | POST 요청에 AP-Device-Identifier 헤더 필요 | check_headers |

#### GET ServiceToken 오류

| 코드 | 상태 | 메시지 | 작업 |
|:---|:---|:---|:---|
| header_missing | 400 | GET 요청에 AD-Service-Token 헤더가 필요합니다 | check_headers |
| header_invalid | 401 | AD-Service-Token의 잘못된 JWT 서명 | get_new_token |
| header_invalid | 401 | JWT 서명 유효성 검사 오류 | get_new_token |
| header_invalid | 401 | AD-Service-Token에 JWT 주체(하위)가 없거나 비어 있습니다. | get_new_token |
| header_invalid | 401 | JWT 제목을 추출하는 중 오류 | get_new_token |

#### 링크 유효성 검사 오류

| 코드 | 상태 | 메시지 | 작업 |
|:---|:---|:---|:---|
| header_missing | 401 | 링크 요청에 AD-Service-Token 헤더가 필요합니다. | check_headers |
| header_invalid | 401 | AD-Service-Token의 잘못된 JWT 서명 | get_new_token |
| header_invalid | 401 | JWT 서명 유효성 검사 오류 | get_new_token |

#### 유효성 검사 오류 연결 해제

| 코드 | 상태 | 메시지 | 작업 |
|:---|:---|:---|:---|
| header_missing | 401 | 링크 해제 요청에 AD-Service-Token 헤더가 필요합니다. | check_headers |
| header_invalid | 401 | AD-Service-Token의 잘못된 JWT 서명 | get_new_token |
| header_invalid | 401 | JWT 서명 유효성 검사 오류 | get_new_token |
| header_invalid | 401 | AD-Service-Token에 JWT 주체(하위)가 없거나 비어 있습니다. | get_new_token |
| request_invalid | 400 | 장치 목록은 null이거나 비워 둘 수 없습니다. | check_request_body |

#### 목록 유효성 검사 오류

| 코드 | 상태 | 메시지 | 작업 |
|:---|:---|:---|:---|
| header_missing | 401 | 목록 요청에 AD-Service-Token 헤더가 필요합니다. | check_headers |
| header_invalid | 401 | AD-Service-Token의 잘못된 JWT 서명 | get_new_token |
| header_invalid | 401 | JWT 서명 유효성 검사 오류 | get_new_token |
| header_invalid | 401 | AD-Service-Token에 JWT 주체(하위)가 없거나 비어 있습니다. | get_new_token |
| header_invalid | 401 | JWT 제목을 추출하는 중 오류 | get_new_token |

