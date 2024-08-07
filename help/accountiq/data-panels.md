---
title: 대시보드의 데이터 패널
description: 대시보드는 다양한 구독자 데이터를 분석하여 암호 공유 인스턴스를 정확하게 파악할 수 있습니다.
exl-id: 12abba05-7422-4bcc-8b11-76aca4911c0b
source-git-commit: 2bb570ab14a3295d46ee6dc0d38485697d63809c
workflow-type: tm+mt
source-wordcount: '889'
ht-degree: 0%

---

# 대시보드의 데이터 패널 {#data-panels}

세그먼트 및 시간 간격을 선택하면 대시보드에는 선택한 세그먼트 내의 공유 활동에 대한 높은 수준의 보기를 반영하는 다양한 데이터 패널, 테이블 및 그래프가 표시됩니다.

아래 표는 Account IQ의 다른 [버전](/help/accountiq/versions-aiq.md)에 있는 데이터 패널의 사용 가능 여부 및 차이점을 간략하게 설명합니다.

| 데이터 패널 | D2C 서비스 | 프로그래머 | TVE MVPD |
|---|---|---|---|
| [현재 세그먼트에 대해 집계된 평균 공유 점수](#aggregated-sharing) | 사용 가능 및 정합성 보장 | 사용 가능 및 정합성 보장 | 사용 가능 및 정합성 보장 |
| [세그먼트의 비디오 범주](#video-categories-segment) | 약간의 변형이 있음 | 약간의 변형이 있음 | 약간의 변형이 있음 |
| [채널 및 MVPD별로 점수 공유](#sharin-score-by-channels-and-mvpds) | 사용할 수 없음 | 사용 가능 | 사용할 수 없음 |
| [계정 공유 가능성](#accounts-sharing-probability) | 사용 가능 및 정합성 보장 | 사용 가능 및 정합성 보장 | 사용 가능 및 정합성 보장 |
| [가능성 수준을 공유한 계정 수 및 사용량](#number-of-accounts-usage-sharing-probability) | 사용 가능 및 정합성 보장 | 사용 가능 및 정합성 보장 | 사용 가능 및 정합성 보장 |


## 현재 세그먼트에 대해 집계된 평균 공유 점수 {#aggregated-sharing}

평균 공유 점수 패널에서는 계정 및 스트리밍 볼륨 측면에서 공유의 양과 영향을 요약하는 윗줄 판독을 제공합니다.

지표를 통해 계정 및 소비량 측면에서 측정된 구독자의 자격 증명 공유의 크기(낮음, 중간, 높음 내지 비정상)를 파악할 수 있습니다.

![](assets/aggregate-sharing-score.png)


*현재 세그먼트에 대해 패널 집계된 평균 공유 점수*

>[!NOTE]
>
> 현재 세그먼트에 대해 집계된 **평균 공유 점수**&#x200B;의 파란색 표시기가 TV Everywhere와 비교하여 D2C 서비스에 대해 다른 용도로 사용됩니다. D2C 서비스의 경우 이전 이미지와 같이 **서비스 평균 인덱스**&#x200B;을(를) 나타냅니다. 프로그래머 또는 MVPD로 로그인하면 이 레이블이 **업계 평균 색인**(으)로 변경됩니다.

다음 지표는 평균 공유 점수 패널의 구성 요소입니다.

### 공유 수준 {#sharing-level}

공유 수준 측정기는 선택한 시간 간격 동안 정의된 세그먼트 내에 있는 모든 공유 가입자 계정의 백분율을 보여 줍니다.

백분율은 세그먼트의 모든 계정에 대해 계산된 공유 확률의 평균을 기반으로 계산됩니다. 이 계산에는 선택한 시간 간격 동안 최소 한 번 이상 스트리밍된 계정이 포함됩니다.

트렌드 표시기는 이전 시간 간격에서 지표 값의 백분율 변화를 보여 줍니다.

![](assets/sharing-level.png){width="350" align="left"}


*공유 수준*

### 공유 계정의 사용 {#usage-from-shared-accounts}

측정은 정의된 세그먼트 및 기간 동안 모든 가입자 계정 중 공유 계정의 사용률을 나타냅니다. Low, Medium, High 및 Abnormal로 명명된 이 범위는 업계 평균을 기반으로 합니다.

이전 시간 간격과 비교하여 공유 계정의 사용량 증가 또는 감소를 보여 주는 트렌드 표시기입니다.

![](assets/usage-4mshared-accounts.png){width="350" align="left"}


*공유 계정의 사용*

### 전체 공유 점수 {#overall-sharing-score}

전체 공유 점수는 &quot;공유 수준&quot; 및 &quot;공유 계정의 사용&quot;을 포함한 공유 점수의 조합입니다.

이는 공유의 전반적인 영향을 반영하는 점수를 제공한다. 그 목적은 하나의 숫자로 공유 수준을 요약하여 신용점수와 유사하다. 그러나 이 경우 점수가 높을수록 공유 수준이 높다는 것을 의미합니다.

![](assets/overall-sharing-score.png){width="350" align="left"}


*전체 공유 점수*

## 세그먼트의 비디오 카테고리 {#video-categories-segment}

열 머리글을 선택하여 모든 버전의 Account IQ에서 데이터를 정렬할 수 있습니다.

+++D2C 서비스: 세그먼트 내 지역

D2C 서비스로 로그인하면 **세그먼트의 지역** 테이블에서 현재 세그먼트의 [비디오 범주](/help/accountiq/product-concepts.md#video-category-def)에 대해 집계된 서로 다른 공유 점수의 비교 보기를 제공합니다.

![](assets/sharing-scores-by-regions-in-segment.png)

*세그먼트에서 지역별로 점수 공유*

>[!NOTE]
>
> 세그먼트에 있는 **지역**&#x200B;과 같이 이전 이미지에 표시된 [비디오 범주](product-concepts.md#video-category-def)이 예입니다. Account IQ에 로그인하면 이 패널에 회사의 특정 비디오 카테고리가 표시됩니다.

.csv 파일로 데이터를 다운로드하려면 **내보내기**&#x200B;를 선택하십시오. [데이터 패널 보고서를 내보내는 방법](/help/accountiq/export-reports.md)을 알아보세요.

+++

+++프로그래머: 세그먼트의 MVPD

프로그래머로 로그인하면 **세그먼트의** MVPD가 현재 세그먼트의 MVPD에 대해 집계된 서로 다른 공유 점수의 비교 보기를 제공합니다.

![](assets/sharing-scores-by-mvpds-in-segment.png)

.csv 파일로 데이터를 다운로드하려면 **내보내기**&#x200B;를 선택하십시오. [데이터 패널 보고서를 내보내는 방법](/help/accountiq/export-reports.md)을 알아보세요.

+++

+++MVPDs: 세그먼트의 프로그래머

MVPD로 로그인하면 **세그먼트의 프로그래머** 테이블에서 현재 세그먼트의 프로그래머에 대해 집계된 여러 공유 점수의 비교 보기를 제공합니다.

데이터를 정렬할 열 머리글을 선택합니다.

![](assets/sharing-scores-by-programmers-in-segment.png)

*세그먼트에서 프로그래머가 점수 공유*

.csv 파일로 데이터를 다운로드하려면 **내보내기**&#x200B;를 선택하십시오. [데이터 패널 보고서를 내보내는 방법](/help/accountiq/export-reports.md)을 알아보세요.

+++

## 채널 및 MVPD별 점수 공유  {#sharin-score-by-channels-and-mvpds}

프로그래머로 로그인하면 이 테이블은 현재 세그먼트의 MVPD에 대해 선택한 채널의 점수를 공유하는 비교 보기를 제공합니다.

데이터를 정렬할 열 머리글을 선택합니다.

![](assets/sharing-scores-by-channels-mvpds.png)


*채널 및 MVPD별로 점수 공유*

## 계정 공유 가능성 {#accounts-sharing-probability}

이 차트는 매우 낮음(0-20%)부터 매우 높음(80-100%)의 범위에 이르기까지 확률 분위를 공유하는 범위로 구분합니다. [계정 공유 확률](#accounts-sharing-probability)의 범위에 대해 자세히 알아보세요.

>[!NOTE]
>
>막대 그래프는 로그 배율을 사용합니다.


![](assets/dashboard-ac-sharing-prob.png)


*다른 공유 확률 범위에 있는 구독자 계정의 수 및 백분율*


## 확률 수준을 공유한 계정 수 및 사용량 {#number-of-accounts-usage-sharing-probability}

이 패널에서는 공유 계정에서 각 분위의 관련 사용을 사용하여 매우 낮음(0-20%)부터 매우 높음(80-100%)의 범위의 공유 확률 분위 범위로 분할된 계정을 테이블 형식으로 볼 수 있습니다. [계정 공유 확률](#accounts-sharing-probability)의 범위에 대해 자세히 알아보세요.

![](assets/no-acc-usage-prob-level.png)

*다양한 확률 범위에 속하는 계정, 트렌드 및 사용의 수*

.csv 파일로 데이터를 다운로드하려면 **내보내기**&#x200B;를 선택하십시오. [데이터 패널 보고서를 내보내는 방법](/help/accountiq/export-reports.md)을 알아보세요.
