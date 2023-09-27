---
title: 계정 IQ 대시보드
description: 대시보드를 사용하면 다양한 구독자 데이터를 분석하여 암호 공유 인스턴스를 정확하게 파악할 수 있습니다.
exl-id: 616da2a5-c9fe-40ea-90cf-f565bc13e764
source-git-commit: ffc7caa9a857a60380d15f69abf346718de6fa4c
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 0%

---

# 대시보드 {#dashboard}

대시보드는 계정 공유의 범위와 영향에 대한 높은 수준의 개요를 제공하기 위해 설계된 그래프 및 보고서 컬렉션의 데이터를 요약하고 집계합니다. 의 주요 보고서 및 지표가 포함된 단일 페이지를 제공합니다 [!UICONTROL Account IQ].


+++프로그래머- 대시보드

![다음의 대시보드 [!UICONTROL Account IQ] 프로그래머 사용자용](assets/dashboard-programr.png)


그림: 프로그래머 사용자를 위한 대시보드

+++

+++MVPD- 대시보드

MVPD 사용자를 위한 대시보드는 프로그래머 사용자의 대시보드와 약간 다르다.

![다음의 대시보드 [!UICONTROL Account IQ] 프로그래머 사용자용](assets/dashboard-mvpd.png)

그림: MVPD 사용자를 위한 대시보드

+++

## 평균 공유 점수 - 현재 세그먼트에 대해 집계됨 {#aggregated-sharing}

Aggregated Sharing Score 패널은 계정 및 스트리밍 볼륨 측면에서 공유의 양과 영향을 요약하는 윗줄 판독을 제공합니다.

값은 구독자의 자격 증명 공유 크기를 이해하는 데 도움이 되며, 따라서 이에 대한 조치 필요성을 측정할 수 있습니다.

![](assets/aggregate-sharing-score.png)


*그림: 현재 세그먼트에 대해 집계된 평균 공유 점수 패널*

다음 세 가지 지표는 평균 공유 점수의 구성 요소입니다.

### 공유 수준 {#sharing-level}

공유 수준 측정기는 선택한 기간 동안 공유된 모든 구독자 계정(정의된 세그먼트 내)의 백분율을 보여 줍니다.

선택한 기간 동안 선택한 프로그래머 채널 중 하나에서 스트리밍된 선택한 MVPD 집합의 모든 계정에 대해 계산된 공유 확률의 평균을 기반으로 계산된 값입니다.

![](assets/sharing-level.png)


*그림: 공유 수준*

트렌드 표시기는 이전 시간대에서 의 지표 값이 백분율로 변경된 것을 보여 줍니다.

### 공유 계정의 사용 {#usage-from-shared-accounts}

이 측정은 정의된 세그먼트 및 기간 동안 공유 계정에서 모든 가입자 계정의 사용량 백분율을 나타냅니다. 이 측정기는 공유 계정의 사용 범위를 0~100% 스케일로 표시합니다. 낮음, 중간, 높음 및 비정상으로 명명된 이들 범위는 업계 평균을 기반으로 합니다.

이전 기간과 비교하여 공유 계정의 사용량 증가 또는 감소를 나타내는 트렌드 표시기도 볼 수 있습니다.

![](assets/usage-4mshared-accounts.png)


*그림: 공유 계정의 사용*

### 전체 공유 점수 {#overall-sharing-score}

전체 공유 점수는 &quot;공유 수준&quot; 및 &quot;공유 계정의 z 사용&quot;을 포함한 공유 점수로 구성됩니다.

산업과 비교할 때 공유가 미치는 상대적 영향을 반영하기 위한 가치를 제공한다. 그 목적은 상황을 하나의 숫자로 요약하여 신용평점의 목적과 유사하다. 그러나 이 경우, 수치가 높을수록 잠재적 해악이 더 커집니다.

![](assets/overall-sharing-score.png)


*그림: 전체 공유 점수*

<!--### MVPDs in segment {#mvpd-in-segment}

It is a table of risk indices and accounts totals for the top MVPDs ranked by overall usage or account sharing.

![](assets/mvpds-in-segment.png)-->

## MVPD에 대한 업계 전반의 전체 공유 점수 {#top-mvpds}

이 테이블은 세그먼트의 MVPD에 대한 서로 다른 집계된 공유 점수의 비교 보기를 제공합니다.

>[!NOTE]
>
>이 테이블은 해당 세그먼트의 MVPD로 표시되는 데이터가 아니라 업계 전체 데이터를 비교 목적으로 사용합니다.

![](assets/top-mvpds.png)


*그림: 전체 점수별 세그먼트 상위 MVPD*

## 채널 및 MVPD별 점수 공유 {#sharin-score-by-channels-and-mvpds}

이 테이블은 현재 세그먼트에서의 MVPD들에 대한 선택된 채널들의 공유 점수들의 비교 뷰를 제공한다.

![](assets/sharing-scores-by-channels-mvpds.png)


*그림: 채널 및 MVPD별 점수 공유*

## 계정 공유 가능성 {#accounts-sharing-probability}

이 차트는 매우 낮음(0-20%)부터 매우 높음(80=100%)까지 확률 분위를 공유하는 범위로 구분합니다.

>[!NOTE]
>
>막대 그래프는 로그 배율을 사용합니다.


![](assets/dashboard-ac-sharing-prob.png)


*그림: 다른 공유 확률 범위에 있는 가입자 계정의 수 및 백분율*

## 확률 수준을 공유한 계정 수 및 사용량 {#number-of-accounts-usage-sharing-probability}

이 패널에서는 공유 계정에서 각 분위의 관련 사용량을 사용하여 매우 낮음(0-20%)에서 매우 높음(80-100%)으로 공유 확률 분위의 범위로 분할된 계정을 테이블 형식으로 볼 수 있습니다.

![](assets/no-acc-usage-prob-level.png)


*그림: 다양한 확률 범위에 속하는 계정 수, 추세 및 사용량*

<!--
+++Dashboard for programmers

![dashboard of account IQ](assets/dashboard-capture.png)


*Figure: The dashboard*

>>>>>>> 7ab48cf61552febab21a5d5c05586e0aefe8ce17
## Average sharing score - aggregated for the current segment {#aggregated-sharing}

The Aggregated Sharing Score panel provides a top line readout summarizing the quantity and impact of sharing in terms of accounts and streaming volume.

The values help you understand the magnitude of credential sharing by your subscribers, hence providing a measure of the need to act upon it.

![](assets/aggregate-sharing-score.png)


*Figure: Average sharing score panel - aggregated for the current segment*

The following three metrics are components of the Average Sharing Score.

### Sharing level {#sharing-level}

The sharing level gauge shows the percentage of all your subscriber accounts (in the defined segment) that are shared, during the selected time frame.  

A value calculated based on an average of the sharing probability computed for every account for the selected MVPD(s) that has streamed from a one of the selected programmer channels during the selected time frame.

![](assets/sharing-level.png)


*Figure: Sharing level*

The Trend indicator shows the percentage change in the value of the metric in from the previous time frame.

### Usage from shared accounts {#usage-from-shared-accounts}

This gauge indicates what percent of the usage of all the subscriber accounts is from the shared accounts for the defined segment and time period. The gauge marks the ranges of usage (from shared accounts) on the scale of 0 to 100%. These ranges (named Low, Medium, High, and Abnormal) are based on the industry average.

You can also see the Trend indicator, which depicts a rise or fall in the usage from shared accounts as compared to the previous time frame.

![](assets/usage-4mshared-accounts.png)


*Figure: Usage from shared accounts*

### Overall sharing score {#overall-sharing-score}

Overall sharing score is composite of sharing scores including "Sharing level" and "Usage from shared accounts".

It provides a value meant to reflect the relative impact of sharing when compared to the industry. Its purpose is similar to that of a credit score, summarizing the situation with a single number. But in this case, the higher the number the greater the potential harm.

![](assets/overall-sharing-score.png)


*Figure: Overall sharing score*

## Industrywide overall sharing scores {#mvpd-in-segment}

+++Programmer- MVPDs in segment

This table provides a comparative view of the different Aggregated Sharing Scores for the MVPDs in the segment.

![](assets/mvpds-in-segment.png)


*Figure: Panel showing top MVPDs in a segment*


>[!NOTE]
>
>This table uses overall industry data for comparative purposes, not the data represented by those MVPDs in the segment.

+++

+++MVPD- Programmers in segment

This table provides a comparative view of the different Aggregated Sharing Scores for the programmers in the segment.

![](assets/programmers-in-segment.png)


*Figure: Panel showing top programmers in a segment*

+++


## Sharing score by channels and MVPDs {#sharin-score-by-channels-and-mvpds}

+++Programmer- MVPDs in segment

This table provides a comparative view of sharing scores of the selected channels for the MVPDs in the current segment.

![](assets/sharing-scores-by-channels-mvpds.png)


*Figure: Sharing scores by channels and MVPDs*

>[!NOTE]
>
>**Sharing score by channels and MVPDs** panel is available only for programmer login.

+++

## Accounts sharing probability distribution{#accounts-sharing-probab-dist}

This panel partitions accounts into ranges of sharing probability quintiles from very low (0-20%) to very high (80-100%).

Pie chart shows the proportions (in term of percentages) of user accounts in various sharing probability ranges. Whereas, column chart shows the absolute numbers of accounts in different probability ranges.

>[!NOTE]
>
>The column chart uses a logarithmic scale.


![](assets/dashboard-ac-sharing-prob.png)


*Figure: Percentages and number of subscriber accounts in different sharing probability ranges*

### Accounts over threshold in current segment {#acc-over-threshold-in-segment}

You can select a level of sharing probability, out of the following to view number and percentage of accounts above it:

* Over very low (0%-20%) probability

* Over low (20%-40%) probability

* Over moderate (40%-60%) probability

* Over high (60%-80%) probability

## Number of accounts and usage by sharing probability level {#number-of-accounts-usage-sharing-probability}

This panel provides tabular view of  accounts partitioned into ranges of sharing probability quintiles from very low (0-20%) to very high (80-100%) with each quintile's associated usage from shared accounts.

![](assets/no-acc-usage-prob-level.png)

*Figure: Number of accounts, trends, and usages falling in various probability ranges*

-->