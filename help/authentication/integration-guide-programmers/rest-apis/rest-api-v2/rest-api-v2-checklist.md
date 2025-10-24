---
title: REST API V2 검사 목록
description: REST API V2 검사 목록
exl-id: 9095d1dd-a90c-4431-9c58-9a900bfba1cf
source-git-commit: 63dc9636f74f8eee1af6205c4d31a01df4503050
workflow-type: tm+mt
source-wordcount: '2563'
ht-degree: 0%

---

# REST API V2 검사 목록 {#rest-api-v2-checklist}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 문서는 Adobe Pass 인증 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)을(를) 사용하는 클라이언트 응용 프로그램을 구현하는 프로그래머에 대한 필수 요구 사항 및 권장 사례를 한 곳에서 집계합니다.

이 문서에 따라 REST API V2를 구현할 때는 허용 기준의 일부로 간주해야 하며 성공적인 통합을 위해 필요한 모든 단계를 수행했는지 확인하는 검사 목록으로 사용해야 합니다.

>[!TIP]
>
> AI 지원 개발의 경우 이러한 요구 사항을 AI 코딩 도우미에 대한 구조화된 규칙으로 변환하는 [AI 규칙](rest-api-v2-ai-rules.md)을 참조하십시오.

## 필수 요구 사항 {#mandatory-requirements}

### &#x200B;1. 등록 단계 {#mandatory-requirements-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">요구 사항</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>등록된 응용 프로그램 범위 지정</i></td>
      <td>REST API v2 범위에 등록된 애플리케이션을 사용합니다.</td>
      <td>HTTP 401 "승인되지 않은" 오류 응답을 트리거하여 시스템 리소스를 오버로드하고 지연을 증가시킬 수 있습니다.</td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;"><i>클라이언트 자격 증명 캐싱</i></td>
      <td>클라이언트 자격 증명을 영구 저장소에 저장하고 모든 액세스 토큰 요청에 재사용합니다.</td>
      <td>클라이언트 자격 증명이 다시 생성될 때 인증 손실이 발생할 위험이 있습니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>액세스 토큰 캐싱</i></td>
      <td>액세스 토큰을 영구 저장소에 저장하고 만료될 때까지 재사용합니다.<br/><br/>모든 REST API v2 호출에 대해 새 토큰을 요청하지 말고 액세스 토큰이 만료될 때만 새로 고치십시오.</td>
      <td>시스템 리소스를 오버로드하고, 지연을 늘리며, HTTP 429 "요청이 너무 많음" 오류 응답을 트리거할 수 있습니다.</td>
   </tr>
</table>

### &#x200B;2. 구성 단계 {#mandatory-requirements-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">요구 사항</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>구성 검색</i></td>
      <td>인증 단계 전에 사용자에게 MVPD(TV 공급자)를 선택하라는 메시지를 표시해야 하는 경우에만 구성 응답을 검색합니다.<br/><br/>다음 경우에 구성 응답을 검색할 필요가 없습니다.<ul><li>사용자가 이미 인증되었습니다.</li><li>사용자에게 임시 액세스 권한이 제공됩니다.</li><li>사용자 인증이 만료되었지만 아직 이전에 선택한 MVPD의 구독자인지 확인하는 메시지가 표시될 수 있습니다.</li></ul></td>
      <td>시스템 리소스를 과부하하고 지연 시간을 늘릴 위험이 있습니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>TV 공급자 선택 캐싱</i></td>
      <td>사용자의 유료 TV 공급자(MVPD) 선택을 영구 저장소에 저장하여 이후의 모든 단계에서 사용합니다.<ul><li>구성 응답 저장소에서 사용자가 MVPD "id"를 선택했습니다.</li><li>구성 응답 저장소에서 사용자가 MVPD "displayName"을(를) 선택했습니다.</li><li>구성 응답 저장소에서 사용자가 MVPD "logoUrl"을 선택했습니다.</li></ul></td>
      <td>시스템 리소스를 과부하하고 지연 시간을 늘릴 위험이 있습니다.</td>
   </tr>
</table>

### &#x200B;3. 인증 단계 {#mandatory-requirements-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">요구 사항</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>폴링 메커니즘 시작</i></td>
      <td>다음 조건에서 폴링 메커니즘을 시작하십시오.<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">기본(화면) 응용 프로그램 내에서 수행되는 인증</a></b><ul><li>브라우저 구성 요소가 세션 끝점 요청에서 "redirectUrl" 매개 변수에 대해 지정된 URL을 로드한 후 사용자가 최종 대상 페이지에 도달하면 기본(스트리밍) 응용 프로그램에서 폴링을 시작해야 합니다.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">보조(화면) 응용 프로그램 내에서 수행된 인증</a></b><ul><li>기본(스트리밍) 애플리케이션은 사용자가 인증 프로세스를 시작하는 즉시( 세션 끝점 응답을 받고 인증 코드를 사용자에게 표시한 후) 폴링을 시작해야 합니다.</li></ul></td>
      <td>시스템 리소스를 오버로드하고, 지연을 늘리며, HTTP 429 "요청이 너무 많음" 오류 응답을 트리거할 수 있습니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>폴링 메커니즘 중지</i></td>
      <td>다음 조건에서 폴링 메커니즘을 중지합니다.<br/><br/><b>인증 성공</b><ul><li>사용자의 프로필 정보가 성공적으로 검색되어 인증 상태가 확인되므로 더 이상 폴링이 필요하지 않습니다.</li></ul><br/><b>인증 세션 및 코드 만료</b><ul><li>인증 세션과 코드가 만료되고 사용자는 인증 프로세스를 다시 시작해야 하며 이전 인증 코드를 사용한 폴링을 즉시 중지해야 합니다.</li></ul><br/><b>새 인증 코드가 생성됨</b><ul><li>사용자가 새 인증 코드를 요청하면 기존 세션이 무효화되고 이전 인증 코드를 사용한 폴링은 즉시 중지되어야 합니다.</li></ul></td>
      <td>시스템 리소스를 오버로드하고, 지연을 늘리며, HTTP 429 "요청이 너무 많음" 오류 응답을 트리거할 수 있습니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>폴링 메커니즘 구성</i></td>
      <td>다음 조건에서 폴링 메커니즘 빈도를 구성합니다.<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">기본(화면) 응용 프로그램 내에서 수행되는 인증</a></b><ul><li>기본(스트리밍) 애플리케이션은 3~5초 이상 간격으로 폴링해야 합니다.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">보조(화면) 응용 프로그램 내에서 수행된 인증</a></b><ul><li>기본(스트리밍) 애플리케이션은 3~5초마다 폴링해야 합니다.</li></ul></td>
      <td>시스템 리소스를 오버로드하고, 지연을 늘리며, HTTP 429 "요청이 너무 많음" 오류 응답을 트리거할 수 있습니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>프로필 캐싱</i></td>
      <td>성능을 개선하고 불필요한 REST API v2 호출을 최소화하기 위해 사용자 프로필 정보의 일부를 영구 저장소에 저장합니다.<br/><br/>캐싱은 다음 프로필 응답 필드에 중점을 두어야 합니다.<br/><br/><b>mvpd</b><ul><li>클라이언트 애플리케이션은 이를 사용하여 사용자가 선택한 TV 공급자를 추적하고 사전 승인 또는 승인 단계 동안 계속 사용할 수 있습니다.</li><li>현재 사용자 프로필이 만료되면 클라이언트 애플리케이션은 기억된 MVPD 선택을 사용하여 사용자에게 확인을 요청할 수 있습니다.</li></ul><br/><b>특성</b><ul><li>다양한 사용자 메타데이터 키(예: zip, maxRating 등)를 기반으로 사용자 경험을 개인화하는 데 사용됩니다.</li><li>사용자 메타데이터는 인증 흐름이 완료된 후에 사용할 수 있으므로 클라이언트 애플리케이션은 프로필 정보에 이미 포함되어 있으므로 사용자 메타데이터 정보를 검색하기 위해 별도의 엔드포인트를 쿼리할 필요가 없습니다.</li><li>특정 메타데이터 속성은 MVPD(예: Charter) 및 특정 메타데이터 속성(예: householdID)에 따라 인증 단계 중에 업데이트될 수 있습니다. 그 결과, 클라이언트 애플리케이션은 최신 사용자 메타데이터를 검색하도록 권한 부여 후 프로필 API를 다시 쿼리해야 할 수 있습니다.</li></ul></td>
      <td>시스템 리소스를 오버로드하고, 지연을 늘리며, HTTP 429 "요청이 너무 많음" 오류 응답을 트리거할 수 있습니다.</td>
   </tr>
</table>

### &#x200B;4. (선택 사항) 사전 인증 단계 {#mandatory-requirements-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">요구 사항</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>사전 인증 결정 검색</i></td>
      <td>콘텐츠 필터링에 사전 인증 결정을 사용하고 재생 결정에 대해서는 사용하지 마십시오.</td>
      <td>프로그래머, MVPD 및 Adobe 간의 계약 계약을 위반할 위험이 있습니다.<br/><br/>모니터링 및 경고 시스템을 무시하는 위험.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>사전 인증 결정 검색 다시 시도</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">향상된 오류 코드</a>을(를) 적절하게 처리하고 작업 필드를 사용하여 필요한 수정 단계를 결정합니다.<br/><br/>제한된 수의 향상된 오류 코드만 다시 시도할 수 있으며, 대부분의 경우 작업 필드에 지정된 대체 해결 방법이 필요합니다.<br/><br/>사전 권한 부여 결정을 검색하기 위해 구현된 다시 시도 메커니즘이 무한 루프가 되지 않도록 하고 다시 시도를 적절한 수(예: 2-3)로 제한하는지 확인하십시오.</td>
      <td>시스템 리소스를 오버로드하고, 지연을 늘리며, HTTP 429 "요청이 너무 많음" 오류 응답을 트리거할 수 있습니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>사전 인증 결정 캐싱</i></td>
      <td>애플리케이션에서 실행 중인 구독 업데이트가 드물게 발생하므로 메모리에 성공적인 허용 결정을 캐시하여 성능을 개선하고 불필요한 REST API v2 호출을 최소화합니다.</td>
      <td>시스템 리소스를 오버로드하고, 지연을 늘리며, HTTP 429 "요청이 너무 많음" 오류 응답을 트리거할 수 있습니다.</td>
   </tr>
</table>

### &#x200B;5. 인증 단계 {#mandatory-requirements-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">요구 사항</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>인증 결정 검색</i></td>
      <td>재생 전에 권한 부여 결정 받기 — 사전 권한 부여 결정이 있는지 여부에 관계없이.<br/><br/>재생 중에 미디어 토큰이 만료되더라도 스트림이 중단되지 않고 계속 진행될 수 있도록 허용하고, 사용자가 다음 재생 요청을 할 때 (새로운) 미디어 토큰이 포함된 새 승인 결정을 요청하십시오(동일한 리소스에 대한 것이든 다른 리소스에 대한 것이든).<br/><br/>장기간 실행 중인 라이브 스트림은 MRSS가 변경될 때 콘텐츠 일시 중지, 상업용 중단 시작 또는 에셋 수준 구성 수정과 같은 비디오 작업 후 새 권한 부여 결정을 요청할 수 있습니다.</td>
      <td>프로그래머, MVPD 및 Adobe 간의 계약 계약을 위반할 위험이 있습니다.<br/><br/>모니터링 및 경고 시스템을 무시하는 위험.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>인증 결정 검색 다시 시도</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">향상된 오류 코드</a>을(를) 적절하게 처리하고 작업 필드를 사용하여 필요한 수정 단계를 결정합니다.<br/><br/>제한된 수의 향상된 오류 코드만 다시 시도할 수 있으며, 대부분의 경우 작업 필드에 지정된 대체 해결 방법이 필요합니다.<br/><br/>권한 부여 결정을 검색하기 위해 구현된 다시 시도 메커니즘이 무한 루프가 되지 않고 다시 시도를 적절한 수(예: 2-3)로 제한하는지 확인하십시오.</td>
      <td>시스템 리소스를 오버로드하고, 지연을 늘리며, HTTP 429 "요청이 너무 많음" 오류 응답을 트리거할 수 있습니다.</td>
   </tr>
</table>

### &#x200B;6. 로그아웃 단계 {#mandatory-requirements-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">요구 사항</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>로그아웃 지원</i></td>
      <td>사용자가 수동으로 로그아웃하고 인증된 프로필을 종료하며 제거된 각 프로필에 대해 지정된 REST API v2 작업 이름을 따를 수 있도록 로그아웃 API를 구현합니다.<ul><li>로그아웃 끝점을 지원하는 MVPD의 경우 클라이언트 애플리케이션은 사용자 에이전트에서 제공된 "url"로 이동해야 합니다.</li><li>"appleSSO" 유형 프로필의 경우 클라이언트 애플리케이션은 파트너 수준(Apple의 시스템 설정)에서도 로그아웃하도록 사용자를 안내해야 합니다.</li></ul></td>
      <td>클라이언트 애플리케이션 측의 지원 누락으로 인해 클라이언트 애플리케이션 오작동이 발생할 수 있습니다.</td>
   </tr>
</table>

### &#x200B;7. 매개변수 및 헤더 {#mandatory-requirements-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">요구 사항</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>인증 헤더 전송</i></td>
      <td>모든 REST API v2 요청에 대해 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a> 헤더를 보냅니다.</td>
      <td>HTTP 401 "승인되지 않은" 오류 응답을 트리거하여 시스템 리소스를 오버로드하고 지연을 증가시킬 수 있습니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>AP-Device-Identifier 헤더 보내기</i></td>
      <td>모든 REST API v2 요청에 대해 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> 헤더를 보냅니다.<br/><br/>요청이 장치를 대신하여 서버에서 시작된 경우에도 AP-Device-Identifier 헤더 값은 실제 스트리밍 장치 식별자를 반영해야 합니다.</td>
      <td>HTTP 400 "잘못된 요청" 오류 응답을 트리거하여 시스템 리소스를 오버로드하고 지연을 증가시킬 수 있습니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>X-Device-Info 헤더 보내기</i></td>
      <td>모든 REST API v2 요청에 대해 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> 헤더를 보냅니다.<br/><br/>요청을 장치 대신 서버에서 보내는 경우에도 X-Device-Info 헤더 값은 실제 스트리밍 장치 정보를 반영해야 합니다.</td>
      <td>알 수 없는 플랫폼에서 시작된 것으로 분류되고 안전하지 않은 것으로 처리되어 더 짧은 인증 TTL과 같은 더 제한적인 규칙이 적용되는 위험이 있습니다.<br/><br/>또한 스트리밍 장치 connectionIp 및 connectionPort와 같은 일부 필드는 Spectrum의 Home Base Authentication과 같은 기능에 필수입니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>안정적인 장치 식별자</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> 헤더에 대한 업데이트 또는 다시 부팅되지 않는 안정적인 장치 식별자를 계산하고 저장합니다.<br/><br/>하드웨어 식별자가 없는 플랫폼의 경우 응용 프로그램 특성에서 고유 식별자를 생성하여 유지합니다.</td>
      <td>장치 식별자가 변경되면 인증 손실이 발생합니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>API 참조 팔로우</i></td>
      <td>REST API v2 예상 매개 변수와 헤더만 전송하는지 확인합니다.</td>
      <td>HTTP 400 "잘못된 요청" 오류 응답을 트리거하여 시스템 리소스를 오버로드하고 지연을 증가시킬 수 있습니다.</td>
   </tr>
</table>

### &#x200B;8. 오류 처리 {#mandatory-requirements-error-handling}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">요구 사항</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>향상된 오류 코드 처리 지원</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">향상된 오류 코드</a>을(를) 적절하게 처리하고 작업 필드를 사용하여 필요한 수정 단계를 결정합니다.<br/><br/>제한된 수의 향상된 오류 코드만 다시 시도할 수 있으며, 대부분의 경우 작업 필드에 지정된 대체 해결 방법이 필요합니다.<br/><br/>응용 프로그램을 시작하기 전에 개발 단계에서 올바르게 처리하면 <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2">향상된 오류 코드 - REST API V2</a> 설명서에 나열된 가장 향상된 오류 코드를 완전히 방지할 수 있습니다.</td>
      <td>시스템 리소스를 오버로드하고, 지연을 늘리며, HTTP 429 "요청이 너무 많음" 오류 응답을 트리거할 수 있습니다.<br/><br/>향상된 오류 코드 처리가 누락되어 클라이언트 응용 프로그램이 제대로 작동하지 않을 수 있습니다. 이로 인해 명확하지 않은 오류 메시지, 잘못된 사용자 지침 또는 잘못된 대체 동작이 발생합니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>HTTP 오류 처리 지원</i></td>
      <td>위에서 설명한 대로 HTTP 오류 응답(예: 400, 401, 403, 404, 405, 500) 처리와 향상된 오류 코드 페이로드를 포함하는 성공 응답(예: 200, 201)을 구별합니다.<br/><br/>제한된 수의 HTTP 오류 코드만 다시 시도할 수 있지만 대부분의 경우 대체 해결 방법이 필요합니다.<br/><br/>대부분의 HTTP 오류 응답은 응용 프로그램을 시작하기 전에 개발 단계에서 올바르게 처리되면 완전히 방지할 수 있습니다.</td>
      <td>시스템 리소스를 오버로드하고, 지연을 늘리며, HTTP 429 "요청이 너무 많음" 오류 응답을 트리거할 수 있습니다.<br/><br/>향상된 오류 코드 처리가 누락되어 클라이언트 응용 프로그램이 제대로 작동하지 않을 수 있습니다. 이로 인해 명확하지 않은 오류 메시지, 잘못된 사용자 지침 또는 잘못된 대체 동작이 발생합니다.</td>
   </tr>
</table>

### &#x200B;9. 테스트 {#mandatory-requirements-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">요구 사항</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>라이프사이클 테스트</i></td>
      <td>공식 Adobe Pass 인증 비프로덕션 환경을 사용하여 애플리케이션을 개발하고 테스트합니다.<ul><li>프리퀄프로덕션</li><li>릴리스 스테이징</li></ul><br/>릴리스 프로덕션으로 시작하기 전에 이러한 환경에서 철저한 품질 보증(QA)을 수행합니다.<br/><br/>클라이언트 응용 프로그램은 비프로덕션 환경에서 전체 유효성 검사를 먼저 완료하지 않고 릴리스 프로덕션으로 진행할 수 없습니다.</td>
      <td>중대한 결함과 함께 위험 발생<br/><br/>짧고 효율적인 디버깅 경로가 부족하면 Adobe 지원 및 엔지니어링이 빠르게 개입하는 데 방해가 될 수 있습니다.</td>
   </tr>
</table>

## 권장 사례 {#recommended-practices}

### &#x200B;1. 등록 단계 {#recommended-practices-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">사례</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>액세스 토큰 유효성 검사</i></td>
      <td>액세스 토큰 유효성을 미리 확인하여 만료되면 새로 고칩니다.<br/><br/>HTTP 401 "승인되지 않음" 오류를 처리하기 위한 다시 시도 메커니즘이 원래 요청을 다시 시도하기 전에 먼저 액세스 토큰을 새로 고치는지 확인하십시오.</td>
      <td>HTTP 401 "승인되지 않은" 오류 응답을 트리거하여 시스템 리소스를 오버로드하고 지연을 증가시킬 수 있습니다.</td>
   </tr>
</table>

### &#x200B;2. 구성 단계 {#recommended-practices-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">사례</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>구성 캐싱</i></td>
      <td>구성 응답을 짧은 시간(예: 3~5분) 동안 메모리나 영구 저장소에 저장하여 성능을 개선하고 불필요한 REST API v2 호출을 최소화합니다.</td>
      <td>시스템 리소스를 과부하하고 지연 시간을 늘릴 위험이 있습니다.</td>
   </tr>
</table>

### &#x200B;3. 인증 단계 {#recommended-practices-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">사례</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>인증 코드 유효성 검사(두 번째 화면 인증)</i></td>
      <td>다음 조건에서 /api/v2/authenticate API를 호출하기 전에 보조(두 번째) 응용 프로그램(화면)에서 사용자 입력을 통해 제출된 인증 코드의 유효성을 검사합니다.<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-with-preselected-mvpd">미리 선택된 mvpd를 사용하여 보조(화면) 응용 프로그램 내에서 수행되는 인증</a></b><ul><li><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md">인증 세션 다시 시작</a> - POST /api/v2/{serviceProvider}/세션/{code}을 사용합니다.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-without-preselected-mvpd">미리 선택된 mvpd 없이 보조(화면) 응용 프로그램 내에서 인증 수행</a></b><ul><li><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md">인증 세션 검색</a> - GET /api/v2/{serviceProvider}/sessions/{code} 사용</li></ul><br/>제공된 인증 코드가 잘못 입력되었거나 인증 세션이 만료된 경우 클라이언트 응용 프로그램에 오류가 표시됩니다.</td>
      <td>인증 중에 다양한 오류 응답 및 작업 흐름 문제가 발생할 수 있습니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>여러 프로필 지원</i></td>
      <td>클라이언트 애플리케이션이 사용자에게 프로필을 선택하라는 메시지를 표시하거나 유효 기간이 가장 긴 프로필을 자동으로 선택하는 등의 사용자 지정 논리를 적용하여 여러 프로필을 처리할 수 있는지 확인합니다.</td>
      <td>클라이언트 애플리케이션 측의 지원 누락으로 인해 클라이언트 애플리케이션 오작동이 발생할 수 있습니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>(선택 사항) 비기본 플로우 지원</i></td>
      <td>클라이언트 애플리케이션 비즈니스에 다음이 필요한 경우 기본이 아닌 플로우 지원:<ul><li>액세스 흐름 저하(프리미엄 기능)</li><li>임시 액세스 흐름(프리미엄 기능)</li><li>SSO(Single Sign-On) 액세스 흐름(표준 기능)</li></ul></td>
      <td>이상적인 사용자 경험보다 적은 사용자 경험을 만들 위험이 있습니다.</td>
   </tr>
</table>

### &#x200B;4. (선택 사항) 사전 인증 단계 {#recommended-practices-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">사례</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>사용자 경험</i></td>
      <td>향상된 오류 코드를 통해 MVPD 또는 Adobe에서 제공하는 메시징을 사용하여 사전 권한 부여 결정이 거부되면 사용자 피드백을 명확하게 표시합니다.</td>
      <td>이상적인 사용자 경험보다 적은 사용자 경험을 만들 위험이 있습니다.</td>
   </tr>
</table>

### &#x200B;5. 인증 단계 {#recommended-practices-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">사례</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>미디어 토큰 유효성 검사</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier">미디어 토큰 검증기</a> 라이브러리를 사용하여 미디어 토큰의 유효성을 검사합니다.</td>
      <td>하천 갈취 같은 사기 수법의 위험이 있다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>사용자 경험</i></td>
      <td>승인 결정이 거부된 경우 향상된 오류 코드를 통해 MVPD 또는 Adobe에서 제공하는 메시징을 사용하여 명확한 사용자 피드백을 표시합니다.</td>
      <td>이상적인 사용자 경험보다 적은 사용자 경험을 만들 위험이 있습니다.</td>
   </tr>
</table>

### &#x200B;6. 로그아웃 단계 {#recommended-practices-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">사례</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>사용자 경험</i></td>
      <td>로그아웃 API는 직접 사용자 요청에 응답해서만 호출해야 하므로 사전 권한 부여 거부 또는 권한 부여와 같은 시나리오에서 로그아웃 API를 자동으로(프로그래밍 방식으로) 호출하지 마십시오.</td>
      <td>인증에 실패하여 사용자를 혼동시킬 위험이 있습니다.</td>
   </tr>
</table>

### &#x200B;7. 매개변수 및 헤더 {#recommended-practices-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">사례</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>코드 재사용</i></td>
      <td>약간의 조정으로 장치 식별자 및 장치 정보를 계산하기 위해 REST API v1의 코드를 다시 사용하지만, REST API v2 예상 매개 변수 및 헤더만 전송하는지 확인합니다.<br/><br/>액세스 토큰을 검색하기 위해 DCR API를 호출하기 위해 REST API v1의 코드를 다시 사용합니다.</td>
      <td>-</td>
   </tr>
</table>

### &#x200B;8. 테스트 {#recommended-practices-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">사례</th>
      <th style="background-color: #EFF2F7;">위험</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>테스트 범위</i></td>
      <td>장치 및 플랫폼 간에 다음 기본 흐름을 테스트하십시오.<br/><br/><b>인증 흐름</b><ul><li>기본 애플리케이션(화면) 인증 시나리오</li><li>보조 애플리케이션(화면) 인증 시나리오</li></ul><br/><b>(선택 사항) 사전 인증 흐름</b><ul><li>허용 결정 시나리오 테스트</li><li>테스트 거부 결정 시나리오</li></ul><br/><b>인증 흐름</b><ul><li>허용 결정 시나리오 테스트</li><li>테스트 거부 결정 시나리오</li></ul><br/><b>로그아웃 흐름</b><br/><br/>또한 해당되는 경우 다른 액세스 흐름을 테스트하십시오.<br/><br/><ul><li>액세스 흐름 저하(프리미엄 기능)</li><li>임시 액세스 흐름(프리미엄 기능)</li><li>SSO(Single Sign-On) 액세스 흐름(표준 기능)</li></ul><br/>주요 MVPD 통합(가장 널리 사용되는 공급자)을 다룹니다.</td>
      <td>특히 덜 자주 테스트되는 플랫폼 또는 부정적 시나리오와 같은 드문 흐름에서 예기치 않은 프로덕션 실패가 발생할 수 있습니다.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>테스트 도구</i></td>
      <td><a href="https://developer.adobe.com/adobe-pass/">Adobe Developer</a> 웹 사이트를 사용합니다.</td>
      <td>-</td>
   </tr>
</table>

## 요약 {#summary}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">단계</th>
      <th style="background-color: #EFF2F7;">필수</th>
      <th style="background-color: #EFF2F7;">(강력함) 권장</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>등록</i></td>
      <td>캐시 클라이언트 자격 증명<br/><br/>캐시 액세스 토큰</td>
      <td>액세스 토큰 유효성 검사 및 새로 고침</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>구성</i></td>
      <td>구성 응답 검색 최소화</td>
      <td>캐시 구성 응답</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>인증</i></td>
      <td>폴링 메커니즘 미세 조정<br/><br/>프로필 일부를 캐시합니다.</td>
      <td>여러 프로필 지원<br/><br/>성능 저하 기능 지원(비즈니스 요구 사항)<br/><br/>TempPass 기능 지원(비즈니스 요구 사항)<br/><br/>Single Sign-On 기능 지원(비즈니스 요구 사항)</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>사전 인증</i></td>
      <td>캐시 허용 사전 권한 부여 결정<br/><br/>메커니즘 미세 조정 다시 시도</td>
      <td>거부된 사전 권한 부여 결정에 대한 오류 코드를 사용하여 사용자 경험 개선</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>인증</i></td>
      <td>사용자가 재생을 요청할 때 권한 부여 결정을 검색합니다<br/><br/>메커니즘 미세 조정 다시 시도</td>
      <td>거부된 권한 부여 결정에 대한 오류 코드를 사용하여 사용자 경험을 개선합니다<br/><br/>미디어 토큰 유효성 검사</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>로그아웃</i></td>
      <td>사용자가 수동으로 로그아웃할 수 있도록 로그아웃 API 구현</td>
      <td>자동으로 로그아웃 API를 호출하지 않음</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">필수</th>
      <th style="background-color: #EFF2F7;">(강력함) 권장</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>매개 변수 및 헤더</i></td>
      <td>필수 헤더 사양 준수</td>
      <td>REST API v1의 코드 재사용</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>오류 처리</i></td>
      <td>향상된 오류 처리 구현<br/><br/>HTTP 오류 처리 구현</td>
      <td>-</td>
   </tr>
</table>
