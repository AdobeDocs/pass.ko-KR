---
title: 공유 계정 보고서
description: 공유 계정 보고서
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---

# 공유 계정 보고서 {#shared-accounts-reports}

공유 계정 보고서는 현재 세그먼트에 대한 공유 동작과 소비를 반영하는 다른 그래프 및 차트 그룹을 제공합니다. 예를 들어, **[!UICONTROL Over Moderate Probability]** 및 **[!UICONTROL Over Low Probability]** (현재 세그먼트)

## 계정 공유 가능성 {#accounts-sharing-probability}

이 도넛 및 막대 차트는 특정 공유 확률 범위에 속하는 구독자 계정의 백분율(및 절대수)을 보여 줍니다. 이러한 범위는 다음과 같이 정의됩니다.

* 매우 높음(80%-100%)
* 높음(60%-80%)
* 중재(40%-60%)
* 낮음(20%-40%)
* 매우 낮음(0%-20%)

빨간색 선은 아래에서 선택한 임계값 범위를 표시합니다. [현재 세그먼트의 임계값 초과 계정](#threshold-selector) 패널 및 밝은 빨간색 영역에는 해당 임계값을 초과하는 모든 계정의 합계가 포함됩니다.

![](assets/accounts-sharing-probability-pie.png)

막대 차트는 각 범위(x축에 표시)에 대해 y축의 각 범위에 속하는 계정 수를 표시합니다.

![](assets/accounts-sharing-probability-bar.png)

여기서 다시 빨간색 선은 현재 임계값을 표시하며 연한 빨간색 영역은 해당 임계값 위의 모든 계정의 합계를 포함합니다.

>[!NOTE]
>
> 막대 차트의 y축은 로그입니다.

### 현재 세그먼트의 임계값 초과 계정{#threshold-selector}

이 패널을 통해 위의 도넛 및 막대 그래프에 대한 임계값 범위를 선택할 수 있습니다. 네 가지 옵션은 다음과 같습니다.

* 계정 **매우 낮음** 공유 **가능성**

* 계정 **매우 낮음** 공유 **가능성**

* 계정 **지나치게 중재** 공유 **가능성**

* 계정 **지나치게 높아** 공유 **가능성**

![](assets/threshold-selector-shared-accounts.png)

임계값을 선택하면 패널에 선택한 세그먼트의 모든 가입자 계정 중 계정의 백분율(및 개수)이 표시됩니다.

## 총 세그먼트 재생 요청 수 중 {#play-request-out-total}

도넛 차트에는 세그먼트에 있는 구독자에 의한 재생 요청의 비율(및 수)이 표시되어 정의된 세그먼트에 없는 구독자에 의한 재생 요청을 비교할 수 있습니다.

![](assets/play-req-outof-total.png)

도넛 차트 위로 커서를 이동하면 다양한 확률 범위의 가입자 백분율과 숫자도 표시됩니다.

<!--![](assets/play-request-total.gif)-->

## 계정당 세그먼트 평균 장치 수{#avg-devices-account}

막대 차트는 현재 세그먼트의 구독자가 현재 사용 중인 각 유형의 평균 디바이스 수와 현재 세그먼트에 없는 디바이스의 평균 디바이스 수를 보여 줍니다.

![](assets/avg-devices-per-acc.png)

## 계정당 기간당 세그먼트-우편 번호 {#zip-codes-period-account}

이 그래프는 지정된 시간 간격 동안 다른 위치(우편 번호로 측정)에서 콘텐츠를 소비하는 현재 세그먼트의 구독자 수에 대해 알려 줍니다.

![](assets/zip-period-account.png)

>[!NOTE]
>
>두 개 이상의 우편 번호 세트를 나타내는 막대를 확대/축소할 수 있습니다. **+** (더하기) 기호를 두 번 클릭하여 서명(예: 10+)합니다.


## 계정당 기간당 세그먼트-지리적 범위 {#geo-span-period-account}

이 막대 그래프는 마일 단위의 다양한 지리적 범위에 있는 위치에서 콘텐츠를 소비하는 구독자 계정의 수를 나타냅니다. 범위는 시간 간격 동안 가입자가 스트리밍한 위치 간의 최대 거리를 기반으로 합니다.

![](assets/geogr-span-account.png)

>[!NOTE]
>
> 두 개 이상의 지리적 거리 세트를 나타내는 막대를 확대/축소할 수 있습니다. **+** (더하기) 기호를 두 번 클릭하여 서명합니다(예: 1000+).

>[!MORELIKETHIS]
>
>* 다음을 사용하여 공유 계정 보고서에서 필터를 사용하여 선택한 세그먼트의 상위 1000개 구독자에 대한 보고서를 내보내는 방법에 대해 알아봅니다. [상위 1000개 계정 내보내기](/help/accountiq/export-acc-information.md) 옵션을 선택합니다.
