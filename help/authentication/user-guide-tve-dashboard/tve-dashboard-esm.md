---
title: ESM 대시보드
description: ESM 대시보드를 사용하여 MVPD 파트너 전체의 자격 및 이벤트 데이터를 모니터링하는 방법에 대해 알아봅니다.
source-git-commit: 53ebbd82fc160f68fccdddb18cf98e249ad6ecce
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 1%

---


# ESM 대시보드 {#esm-dashboard}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

ESM 대시보드는 성과를 모니터링하고, 예외 항목을 식별하고, MVPD 파트너 전반에 걸친 사용자 액세스 패턴을 이해하는 데 도움이 되는 자격 및 이벤트 데이터에 대한 통합 보기를 제공합니다. 이 안내서에서는 구성 가능한 시간 간격으로 대시보드의 필터를 사용하고, 보고서를 해석하고, 주요 지표를 이해하는 방법에 대해 설명합니다.

![ESM 대시보드 보기](../assets/tve-dashboard/new-tve-dashboard/esm/esm-full-page.png)

## 사용 사례 {#use-cases}

- 플랫폼 또는 MVPD별 트렌드 시각화
- MVPD 성능 비교
- 애플리케이션별 고객 사용 이해

ESM 데이터 및 이벤트에 대한 자세한 내용은 [자격 서비스 모니터링 개요](https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview)에서 확인할 수 있습니다.

## 보고서 {#reports}

다음 보고서를 사용할 수 있습니다.

### 요청 재생 {#play-requests}

선택한 시간 간격에 대한 재생 요청 수를 표시합니다.

**그래프 스타일** - 줄입니다.

### 인증 전환 {#authentication-conversion}

성공한 인증 이벤트와 총 인증 시도 횟수 간의 비율을 표시합니다.

**그래프 스타일** - 가로 막대

### 인증 전환 {#authorization-conversion}

성공한 인증 이벤트와 총 인증 시도 횟수 간의 비율을 표시합니다.

**그래프 스타일** - 가로 막대

### 인증 지연 {#authorization-latency}

MVPD 응답의 평균 대기 시간(밀리초)을 보여 줍니다.

**그래프 스타일** - 줄입니다.

### MVPD 사용 {#mvpd-usage}

MVPD당 고유 사용자 수를 비교하여 표시합니다.

**그래프 스타일** - 스택 영역입니다.

### 성공한 인증 {#successful-authentications}

선택한 시간 간격에 대해 성공한 총 인증 수를 표시합니다.

**그래프 스타일** - 줄입니다.

### 성공한 승인 {#successful-authorizations}

선택한 시간 간격 동안 성공한 총 승인 수(MVPD의 &#39;허용&#39; 응답)를 표시합니다.

**그래프 스타일** - 줄입니다.

## 액션 {#actions}

### 보기 기준 {#view-by}

각 그래프에 대해 표시할 데이터를 정확하게 선택할 수 있는 &quot;보기 기준&quot; 드롭다운이 있습니다.

- **집계** - 전체 데이터를 표시합니다.
- **MVPD/채널/플랫폼** - 선택한 특정 필터에 대한 개요를 제공합니다.

### 다운로드 {#download}

원시 데이터를 다운로드할 수 있습니다.

- **차트 데이터를 CSV로 다운로드** - 특정 차트에 대한 데이터를 다운로드합니다.
- **모든 데이터를 CSV로 다운로드** - 모든 그래프에서 모든 ESM 데이터를 다운로드합니다.

## 필터 {#filters}

필터를 사용하여 데이터 세트의 범위를 좁히고 분석에 초점을 맞춥니다. 다음 필터를 사용할 수 있습니다.

- **채널**: 사용 가능한 모든 채널(브랜드)을 포함합니다.
- **MVPD**: 하나 이상의 공급자에 주력
- **플랫폼**: 웹, 모바일, 연결된 TV 또는 장치 제품군

새 필터를 추가하려면 &quot;필터 추가&quot; 버튼을 선택합니다.

&quot;데이터 세트 필터&quot; 페이지에서 필요한 필터를 끌어서 놓을 수 있습니다.

![ESM 대시보드 필터](../assets/tve-dashboard/new-tve-dashboard/esm/filters-modal.png)

각 섹션에 대해 필터를 개별적으로 제거하거나 전체 선택 항목을 지울 수 있습니다.

### 필터 팁 {#filter-tips}

- 여러 필터를 결합하여 집단을 분리합니다(예: 모바일 플랫폼에서 채널을 위한 하나의 MVPD).
- 드릴인하기 전에 축소하고 기준선을 설정하는 필터를 추가하지 마십시오.

## 시간 간격 {#time-intervals}

분석 창 및 세부기간을 제어합니다.

![ESM 대시보드 시간 간격](../assets/tve-dashboard/new-tve-dashboard/esm/date-picker.png)

### 날짜 범위 {#date-range}

**사전 설정**: 오늘, 현재 주, 지난 7일, 현재 월, 지난 30일, 지난 3개월, 지난 6개월, 지난 12개월

**사용자 지정**: 원하는 시간 간격을 선택하십시오.

### 세부기간 {#granularity}

일별 / 월별
