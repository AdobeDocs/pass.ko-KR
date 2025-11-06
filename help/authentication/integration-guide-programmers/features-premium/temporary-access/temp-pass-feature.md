---
title: TempPass 기능
description: TempPass 기능
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '2203'
ht-degree: 0%

---

# TempPass 기능 {#temp-pass-feature}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

TempPass는 프로그래머가 유효한 MVPD 계정 자격 증명이 없는 사용자를 위해 보호된 콘텐츠에 일시적으로 액세스할 수 있도록 해주는 다양한 기능입니다. 기본 액세스 시나리오나 타깃팅된 프로모션 캠페인을 통해 시청자의 참여를 유도하는 효과적인 도구 역할을 합니다.

TempPass는 프로그래머가 다음과 같은 작업을 수행할 수 있는 강력한 솔루션입니다.

* **뷰어 참여:** 새로운 구독자를 유치하기 위한 프리미엄 콘텐츠 기능을 제공합니다.
* **프로모션 추진:** 타깃팅된 캠페인을 실행하여 콘텐츠 노출을 늘리고 브랜드 충성도를 구축하십시오.
* **제어 유지:** 액세스 기간을 관리하고, 제한을 적용하고, 비즈니스 목표에 맞게 필요한 액세스를 재설정합니다.

TempPass 기능은 참여하는 프로그래머와의 통합으로 Adobe Pass 인증 서버 구성 내에 의사 MVPD(추가 &quot;Temp Pass&quot;라고 함)를 도입하여 제공됩니다. TempPass 기능은 다음 두 가지 구성으로 사용할 수 있습니다.

* 시간 기반 액세스를 위한 [기본 TempPass](#basic-temp-pass).
* 유연한 캠페인 기반 액세스를 위한 [Promotional TempPass](#promotional-temp-pass).

>[!IMPORTANT]
>
> TempPass 기능은 프리미엄 기능이며 Adobe의 현재 라이선스가 필요합니다.

다음 표에서는 기본 및 프로모션 TempPass 기능에 대한 간략한 비교를 제공합니다.

| **기능** | **기본 TempPass** | **프로모션 TempPass** |
|-------------------------------|------------------------------|-------------------------------------------------------------------------------------------------|
| **콘텐츠에 액세스** | <ul><li>시간 기반</li></ul> | <ul><li>시간 기반</li><li>최대 리소스 수로 제한</li></ul> |
| **액세스 보안 기준** | <ul><li>장치 ID</li></ul> | <ul><li>장치 ID</li><li>제공된 사용자 식별자 정보(예: 이메일)의 해시</li></ul> |
| **향상된 오류 코드** | 사용 가능 | 사용 가능 |
| **TempPass 재설정 기능** | 사용 가능 | 사용 가능 |

>[!IMPORTANT]
> 
> Adobe Pass 인증에는 할당된 시간(X분)이 경과하면 진행 중인 스트림을 자동으로 중지하는 내장 메커니즘이 포함되어 있지 않습니다. 지속적인 스트림 중에 TempPass가 만료되면 액세스 제한을 적용하는 것은 프로그래머의 책임입니다.

콘텐츠 라이브러리를 살짝 엿보거나 선택 윤곽 이벤트를 홍보하는 경우 TempPass는 액세스를 제어하는 동시에 대상을 확장하는 도구를 제공합니다.

## 기본 TempPass {#basic-temp-pass}

기본 TempPass 기능을 통해 프로그래머는 다양한 시나리오에 따라 제한된 콘텐츠 액세스를 제공할 수 있습니다.

* **짧은 미리 보기:** 일일 액세스 기간(예: 10분)과 같은 간단한 미리 보기를 제공하여 잠재적 구독자를 유도합니다.
* **이벤트 기반 액세스:** 4시간 세션과 같은 주요 이벤트에 대해 더 이상 액세스할 수 없습니다.
* **조합 액세스:** 초기 확장된 보기 기간과 며칠 동안의 더 짧은 일일 미리 보기 등 여러 기간을 혼합하고 일치시킵니다.

특정 이벤트들은 콘텐츠에 대한 단계적인 자유 액세스를 요구할 수 있는데, 예를 들어, 초기 연장된 자유 액세스 기간(예를 들어, 4시간), 후속하여 더 짧은 일일 자유 액세스 간격(예를 들어, 매일 10분)이 뒤따른다. 이 시나리오를 구현하려면 프로그래머는 Adobe 담당자와 조정하여 요구 사항에 맞게 두 개의 TempPass MVPD를 구성해야 합니다.

예를 들어 초기 4시간 무료 세션에 이어 매일 10분 무료 세션을 제공하기 위해 Adobe은 프로그래머를 위해 를 구성할 수 있습니다.

* **TempPass1**: 초기 무료 액세스 기간을 처리하기 위해 TTL(Time-To-Live)이 4시간으로 구성되었습니다.
* **TempPass2**: 이후의 일별 무료 액세스 간격에 대해 TTL(Time-To-Live)이 10분으로 구성되었습니다.

매일 액세스하는 데 적절한 기능을 유지하려면 매일 00:00시간에 모든 장치에 대해 TempPass2를 다시 설정해야 합니다.

### 기능 세부 사항 {#basic-temp-pass-feature-details}

**구성 매개 변수:**

* **TTL(Time-To-Live):** 프로그래머가 액세스 기간을 지정할 수 있습니다. 이 시계 기반 TTL은 실제 시청 시간에 관계없이 만료됩니다.

**사용자 식별:**

기본 TempPass 기능은 장치 식별자를 사용자 식별 매개 변수로 사용합니다.

다음 표는 사용자 식별 매개 변수가 사용자 체험판 경험에 미치는 영향을 이해하는 데 도움이 됩니다.

| 장치 식별자 | 결과 |
|-------------------|----------------|
| 신규 | 새 평가판 |
| 기존 항목 | 기존 평가판 |

**시간 계산 보기:**

TTL은 콘텐츠를 보는 데 걸린 실제 시간과 관계없이 초기 권한 부여 요청 시간에서 만료 시간까지의 기간을 나타냅니다. 이후 각 요청은 저장된 만료 시간과 현재 서버 시간을 확인하여 액세스 권한을 부여합니다.

**인증:**

기본 TempPass에는 인증이 필요하지 않으므로 권한 부여 단계로 바로 진행할 수 있습니다.

**인증:**

실제 MVPD과 상호 작용하지 않으므로 기본 &quot;임시 패스&quot; MVPD은 임시 패스가 유효한 경우 모든 리소스를 승인합니다. 인증에 성공한 경우 미디어 토큰 검증기 라이브러리는 콘텐츠 재생을 시작하기 전에 미디어 토큰을 확인하고 리소스 유효성 검사를 확인하는 데 적용 가능한 상태로 유지됩니다.

인증 결정은 사용자 식별 매개 변수 및 구성된 TTL을 기반으로 합니다. 리소스에 대한 성공적인 인증을 받으려면 유효한 요청으로 다음 조건을 충족해야 합니다.

* **사용하지 않은 기간:** 구성된 TTL에 초기 인증 요청 시간(데이터베이스에 저장됨)을 추가하여 만료 시간을 계산합니다. 현재 서버 시간과 이 만료 시간을 비교하여 TempPass가 여전히 유효한지 확인합니다.

구성된 TTL을 초과하는 사용자의 경우 TempPass가 다시 설정되지 않으면 더 이상 동일한 장치에서 콘텐츠를 볼 수 없습니다.

**사전 인증:**

기본 &quot;Temp Pass&quot; MVPD에 대해 사전 권한 부여 요청이 수행된 경우 이 응답은 요청의 전체 리소스 목록을 성공적으로 사전 권한 부여됨으로 반환합니다. 인증 조건이 특정 리소스가 아닌 시간 제한을 기반으로 한다는 점을 감안할 때 이 동작은 인증 논리를 반영합니다. 시간 제한이 유효한 한 요청된 리소스가 승인됩니다.

**로그아웃:**

기본 TempPass에는 로그아웃이 필요하지 않으므로 실제 사용자 MVPD을 사용하여 인증 단계로 직접 전환할 수 있습니다.

**추적 데이터 및 분석:**

기본 TempPass 흐름 동안 추적 데이터는 MVPD 식별자가 &quot;Temp Pass&quot;로 설정된 장치 식별자의 해시된 버전을 사용합니다. 프로그래머는 분석 구현에서 TempPass 지표를 표준 MVPD 지표와 구별해야 합니다.

## 프로모션 TempPass {#promotional-temp-pass}

프로모션 TempPass 기능은 프로모션 캠페인을 실행하기 위해 특별히 설계된 기본 TempPass의 기능을 확장합니다. 이 기능을 사용하면 프로그래머가 이메일 주소와 같은 유효한 사용자 ID를 수집한 후 지정된 기간 동안 사전 정의된 수의 VOD 제목에 대한 액세스를 허용하여 사용자를 참여시킬 수 있습니다.

프로모션 TempPass에는 다음과 같은 유연성을 갖춘 기본 TempPass의 모든 기능이 포함되어 있습니다.

* 프로모션 기간 동안 액세스할 수 있는 최대 VOD 제목 수를 정의합니다.
* 프로모션 액세스가 유효한 기간을 설정합니다.

사용자가 사전 정의된 액세스 제한(VOD 제목 또는 기간 수)을 초과하면 TempPass가 재설정되지 않는 한 더 이상 동일한 장치에서 또는 동일한 사용자 식별자로 콘텐츠를 볼 수 없습니다.

### 기능 세부 사항 {#promotional-temp-pass-feature-details}

**구성 매개 변수:**

* **사용자 정보 키:** 사용자가 제공한 식별자를 전달하는 데 사용되는 키(예: 이메일 주소)입니다.
* **리소스 수:** 사용자가 액세스할 수 있는 VOD 제목 수를 정의합니다.
* **TTL(Time-To-Live):** 사용자가 허용된 리소스를 사용할 수 있는 기간입니다.

**사용자 식별:**

프로모션 TempPass 기능은 디바이스 식별자 상단에 있는 사용자 제공 식별자의 해시를 사용자 식별 매개 변수로 사용합니다.

>[!IMPORTANT]
>
> 사용자 제공 식별자의 유효성 검사 및 해싱은 Adobe이 아닌 프로그래머가 관리합니다. Adobe은 PII(개인 식별 정보)를 저장하지 않습니다. 따라서 프로그래머는 Adobe Pass 인증 API와 상호 작용할 때 고유 사용자 제공 식별자의 해시를 생성하고 제출할 책임이 있습니다.

Adobe에서는 데이터를 Adobe으로 보내기 전에 데이터에 대해 **SHA-2** 계열 또는 특정 **SHA-256**, **SHA-512** 함수를 사용하는 것이 좋습니다. 예를 들어 **&quot;user@domain.com&quot;**&#x200B;에 대한 **SHA-256**&#x200B;은(는) **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**&#x200B;입니다.

다음 표는 사용자 식별 매개 변수가 사용자 체험판 경험에 미치는 영향을 이해하는 데 도움이 됩니다.

| 사용자 제공 식별자 해시 | 장치 식별자 | 결과 |
|-------------------------------|-------------------|---------------------------------------------------------|
| 신규 | 신규 | 새 평가판 |
| 기존 항목 | 신규 | 기존 평가판(사용자 제공 식별자 해시 기반) |
| 신규 | 기존 항목 | 기존 체험판(장치 식별자 기반) |
| 기존 항목 | 기존 항목 | 기존 평가판 |

**시간 계산 보기:**

TTL은 콘텐츠를 보는 데 걸린 실제 시간과 관계없이 초기 권한 부여 요청 시간에서 만료 시간까지의 기간을 나타냅니다. 이후 각 요청은 저장된 만료 시간과 현재 서버 시간을 확인하여 액세스 권한을 부여합니다.

**인증:**

프로모션 TempPass에는 인증이 필요하지 않으므로 승인 단계로 바로 진행할 수 있습니다.

프로그래머 애플리케이션의 구현을 지원하기 위해, 프로모션 TempPass는 해당 키를 통해 액세스할 수 있는 다음 사용자 메타데이터 정보를 노출합니다.

* **`remaining_resources`**: 사용자가 사용할 수 있는 리소스 수를 나타냅니다.
* **`used_assets`**: 사용자가 이미 사용한 리소스 목록을 제공합니다.
* **`expiration_date`**: 사용자의 프로모션 임시 패스의 만료 날짜를 표시합니다.

**인증:**

실제 MVPD과 상호 작용하지 않으므로 프로모션 &quot;임시 패스&quot; MVPD은 임시 패스가 유효한 경우 모든 리소스를 승인합니다. 인증에 성공한 경우 미디어 토큰 검증기 라이브러리는 콘텐츠 재생을 시작하기 전에 미디어 토큰을 확인하고 리소스 유효성 검사를 확인하는 데 적용 가능한 상태로 유지됩니다.

인증 결정은 사용자 식별 매개 변수 및 구성된 리소스 수와 TTL을 기반으로 합니다. 리소스에 대한 성공적인 인증을 받으려면 유효한 요청으로 다음 조건을 충족해야 합니다.

* **사용하지 않은 기간:** 구성된 TTL에 초기 인증 요청 시간(데이터베이스에 저장됨)을 추가하여 만료 시간을 계산합니다. 현재 서버 시간과 이 만료 시간을 비교하여 TempPass가 여전히 유효한지 확인합니다.
* **사용되지 않은 리소스:** 사용된 리소스 수가 추적됩니다(데이터베이스에 저장됨). 사용된 리소스 수와 구성된 리소스 수를 비교하여 TempPass가 여전히 유효한지 확인합니다.

구성된 TTL 또는 리소스 수를 초과하는 사용자의 경우 TempPass가 다시 설정되지 않으면 동일한 장치에서 또는 동일한 사용자 제공 식별자로 콘텐츠를 더 이상 볼 수 없습니다.

**사전 인증:**

프로모션 &quot;Temp Pass&quot; MVPD에 대해 사전 승인 요청이 수행된 경우 응답은 요청의 전체 리소스 목록을 성공적으로 사전 승인된 것으로 반환합니다. 권한 부여 조건이 특정 리소스가 아닌, 시간 제한 및 액세스된 총 리소스 수를 기반으로 한다는 점을 감안할 때 이 동작은 권한 부여 논리를 반영합니다. 시간 제한이 유효하고 리소스 제한이 초과되지 않은 한 요청된 리소스가 승인됩니다.

**로그아웃:**

프로모션 TempPass에는 로그아웃이 필요하지 않으므로 실제 사용자 MVPD을 사용하여 인증 단계로 직접 전환할 수 있습니다.

**추적 데이터 및 분석:**

프로모션 TempPass 흐름 동안 추적 데이터는 MVPD 식별자가 &quot;Temp Pass&quot;로 설정된 장치 식별자의 해시된 버전을 사용합니다. 프로그래머는 분석 구현에서 TempPass 지표를 표준 MVPD 지표와 구별해야 합니다.

## TempPass API 액세스 재설정 {#reset-tempass-api-access}

Reset TempPass API에 액세스하려면 먼저 DCR(Dynamic Client Registration) 프로세스에서 필요한 단계를 완료해야 합니다. 이 필수 프로세스를 사용하면 Reset TempPass API와 상호 작용하는 데 필요한 액세스 토큰을 보유할 수 있습니다.

포괄적인 지침은 [동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) 설명서를 참조하십시오.

## TempPass API 재설정 - DELETE /reset-tempass/v3/reset {#reset-tempass-v3-reset}

장치 또는 모든 장치에 대한 특정 TempPass를 재설정하기 위해 Adobe Pass 인증에서는 프로그래머에게 기본 및 프로모션 TempPass에 모두 작동하는 API를 제공합니다.

### 요청 {#reset-tempass-v3-reset-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">호스트</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">경로</td>
      <td>/reset-tempass/v3/reset</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">방법</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">쿼리 매개변수</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>온보딩 프로세스 중 서비스 공급자와 연결된 내부 고유 식별자입니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>온보딩 프로세스 중 TempPass와 연결된 내부 고유 식별자입니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">device_id</td>
      <td>
            이 재설정 작업이 유효한 장치 식별자입니다.
            <br/><br/>
            값이 제공되지 않으면 모든 장치에 재설정 작업이 적용됩니다.
      </td>
      <td>선택 사항</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">인증</td>
      <td>전달자 토큰 페이로드의 생성은 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">액세스 토큰 검색</a> 문서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
</table>

### 응답 {#reset-tempass-v3-reset-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">코드</th>
      <th style="background-color: #EFF2F7;">텍스트</th>
      <th style="background-color: #EFF2F7;">설명</th>
   </tr>
   <tr>
      <td>204</td>
      <td>콘텐츠 없음</td>
      <td>
        재설정되었습니다.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>잘못된 요청</td>
      <td>
        요청이 잘못되었습니다. 클라이언트가 요청을 수정하고 다시 시도하십시오.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>승인되지 않음</td>
      <td>
        액세스 토큰이 잘못되었습니다. 클라이언트가 새 액세스 토큰을 얻은 후 다시 시도하십시오. 자세한 내용은 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">동적 클라이언트 등록 개요</a> 설명서를 참조하십시오.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>금지됨</td>
      <td>
        액세스 토큰이 잘못되었습니다. 클라이언트는 새 클라이언트 자격 증명과 새 액세스 토큰을 가져와서 다시 시도하십시오. 자세한 내용은 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">동적 클라이언트 등록 개요</a> 설명서를 참조하십시오.
      </td>
   </tr>
</table>

### 샘플 {#reset-tempass-v3-reset-samples}

#### 특정 장치에 대한 TempPass 재설정 {#reset-tempass-v3-reset-specific-device}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=ba23d141-d715-561c-94f4-e9e4c966b1eb"
```

#### 모든 장치에 대한 TempPass 재설정 {#reset-tempass-v3-reset-all-devices}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=all"
```

## TempPass API 재설정 - DELETE /reset-tempass/v3/reset/generic {#reset-tempass-v3-reset-generic}

일반 키(사용자가 제공한 식별자 해시) 또는 모든 키에 대한 특정 TempPass를 재설정하기 위해 Adobe Pass 인증에서는 프로그래머에게 프로모션 TempPass에 작동하는 API를 제공합니다.

### 요청 {#reset-tempass-v3-reset-generic-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">호스트</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">경로</td>
      <td>/reset-tempass/v3/reset/generic</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">방법</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">쿼리 매개변수</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>온보딩 프로세스 중 서비스 공급자와 연결된 내부 고유 식별자입니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>온보딩 프로세스 중 TempPass와 연결된 내부 고유 식별자입니다.</td>
      <td><i>필수</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">key</td>
      <td>
            이 재설정 작업이 유효한 사용자 제공 식별자 해시.
            <br/><br/>
            값이 제공되지 않으면 모든 사용자에게 재설정 작업이 적용됩니다.
      </td>
      <td>선택 사항</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">헤더</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">인증</td>
      <td>전달자 토큰 페이로드의 생성은 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">액세스 토큰 검색</a> 문서에 설명되어 있습니다.</td>
      <td><i>필수</i></td>
   </tr>
</table>

### 응답 {#reset-tempass-v3-reset-generic-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">코드</th>
      <th style="background-color: #EFF2F7;">텍스트</th>
      <th style="background-color: #EFF2F7;">설명</th>
   </tr>
   <tr>
      <td>204</td>
      <td>콘텐츠 없음</td>
      <td>
        재설정되었습니다.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>잘못된 요청</td>
      <td>
        요청이 잘못되었습니다. 클라이언트가 요청을 수정하고 다시 시도하십시오.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>승인되지 않음</td>
      <td>
        액세스 토큰이 잘못되었습니다. 클라이언트가 새 액세스 토큰을 얻은 후 다시 시도하십시오. 자세한 내용은 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">동적 클라이언트 등록 개요</a> 설명서를 참조하십시오.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>금지됨</td>
      <td>
        액세스 토큰이 잘못되었습니다. 클라이언트는 새 클라이언트 자격 증명과 새 액세스 토큰을 가져와서 다시 시도하십시오. 자세한 내용은 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">동적 클라이언트 등록 개요</a> 설명서를 참조하십시오.
      </td>
   </tr>
</table>

### 샘플 {#reset-tempass-v3-reset-generic-samples}

#### 특정 키에 대한 TempPass 재설정 {#reset-tempass-v3-reset-specific-key}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7"
```

#### 모든 키에 대한 TempPass 재설정 {#reset-tempass-v3-reset-all-keys}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=all"
```

## REST API V2 {#rest-api-v2}

TempPass 기능을 활용하려면 TVE(TV Everywhere) 응용 프로그램이 Adobe Pass 인증 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)과(와) 상호 작용하는 방법을 수정하기 위해 코드 업데이트를 구현해야 합니다.

이러한 업데이트 및 관련 워크플로에 대한 포괄적인 안내서는 [임시 액세스 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) 설명서를 참조하십시오.
