---
title: 세그먼트 및 시간대 정의
description: 세그먼트 및 시간대 정의
exl-id: 86fe010d-3202-4ce2-b803-ff44f5538d7e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---

# 세그먼트 및 시간대 정의 {#define-segment}

의 모든 분석 또는 보고서 보기 [!UICONTROL Account IQ] 세그먼트를 정의하고 평가할 기간을 선택하는 것부터 시작하십시오. [세그먼트](/help/accountiq/product-concepts.md#segmet-def) 은 평가 기준(MVPD 구독 및 특정 채널 보기)을 충족하는 모든 구독자 또는 뷰어를 의미합니다.

![](assets/segment-panel.png)

*그림: 세그먼트 및 시간대 선택*

의 모든 보고서 페이지 맨 위에 [!UICONTROL Account IQ], MVPD, 채널 프로그래머, 세부기간 및 시간대를 선택하여 세그먼트를 정의하는 패널이 있습니다.

## 세그먼트 선택 {#select-segment}

### 세그먼트에서 MVPD 선택 {#select-segment-mvpds}

MVPD 선택 **[!UICONTROL MVPDs in segment]** 옵션:

1. 을(를) 클릭하거나 탭합니다 **[!UICONTROL MVPDs in segment]** 드롭다운 옵션입니다.

   >[!NOTE]
   >
   >**모두** 업계 MVPD는 기본적으로 선택됩니다. 여기에서 다음 중 하나를 선택할 수 있습니다. **점수를 공유하여 상위 10개 MVPD**, **사용량별 상위 10개 MVPD**, **계정별 상위 10개 MVPD**&#x200B;또는 개별 MVPD입니다. 그러나 개별 MVPD를 선택하려면 선택을 해제해야 합니다 **모두**.

1. 원하는 MVPD를 클릭하거나 탭합니다.

   선택을 취소하여 선택 항목에서 MVPD를 제거할 수 있습니다.

1. 클릭 또는 탭 **[!UICONTROL Apply selection]** 을 참조하십시오. 그렇지 않으면 선택한 항목이 해제됩니다.

   >[!NOTE]
   >
   >격리 모드를 선택하면 다른 MVPD를 선택할 수 없습니다.

### 세그먼트에서 채널 선택 {#select-segment-channels}

에서 원하는 프로그래머 채널을 선택하려면 **[!UICONTROL Channels in segment]** 옵션:

1. 을(를) 클릭하거나 탭합니다 **[!UICONTROL Channels in segment]** 드롭다운 옵션입니다.

   >[!NOTE]
   >
   >**모두** 회사에 대한 프로그래머 채널은 기본적으로 선택됩니다. 개별 채널이나 프로그래머를 선택하려면 먼저 선택을 해제해야 합니다 **모두**.

1. 원하는 채널 또는 프로그래머를 클릭하거나 탭합니다.

   의 최상위 목록 항목 **[!UICONTROL Channels in segment]** 은(는) [프로그래머](/help/accountiq/product-concepts.md#programmer-def) 회사와 프로그래머 이름의 목록 항목이 [채널](/help/accountiq/product-concepts.md#channel-def). 프로그래머에서 개별 채널을 선택하거나 프로그래머를 선택하고 해당 프로그래머에 속한 채널의 모든 활동이 보고서 및 그래프 결과에 포함됩니다.

   ![](assets/programmer-channels.png)


   *그림: 채널 선택기에 나열된 프로그래머 및 채널*

   >[!IMPORTANT]
   >
   >프로그래머 아래에서 개별 채널을 선택한 결과는 프로그래머를 선택한 결과와 동일하지 않다.
   >
   >
   >개별 채널을 선택하면 일부 보고서에서 해당 채널의 활동이 개별적으로 분류됩니다. 그러나 이러한 모든 채널의 상위 프로그래머를 선택하면 이러한 채널의 모든 활동이 포함되지만 보고서에서 개별적으로 분류되지는 않습니다.

1. 클릭 또는 탭 **[!UICONTROL Apply selection]** 을 참조하십시오.

>[!NOTE]
>
>MVPD 또는 프로그래머 풀다운 메뉴에서는 10개 이상의 항목을 선택할 수 없습니다.

### MVPD 및 채널 선택 해제 {#deselect-segment-mvpds-channels}

에서 선택 사항을 변경하는 것 외에도 **[!UICONTROL MVPDs in segment]** 및 **[!UICONTROL Channels in segment]** 세그먼트 선택기를 사용하면 이전에 선택한 MVPD 및 채널을 다음과 같이 선택 해제할 수 있습니다.

* 선택 **[!UICONTROL Remove]** 아이콘(![제거 아이콘](assets/remove-icon.png))을 클릭하여 세그먼트 선택기 아래에 표시된 선택한 MVPD 및 채널의 이름을 표시합니다.

* 다음을 사용할 수도 있습니다. **[!UICONTROL Clear Selection]** 이전에 선택한 모든 MVPD 또는 채널을 제거합니다.

![](assets/segment-panel-selection.png)

*그림: 세그먼트 및 일정 패널에서 선택한 MVPD 및 채널*

## 세부 기간 및 시간대 선택 {#granularity-timeframe}

평가 기간을 선택하려면 다음을 수행합니다.

1. 다음 항목 선택 **[!UICONTROL Granularity and time frame]** 날짜 선택기.

1. 다음 중 하나를 선택합니다. **[!UICONTROL Week]** 또는 **[!UICONTROL Month]** 출처: **[!UICONTROL Aggregate By]** 평가를 위한 세부기간을 설정하는 옵션입니다.

   ![](assets/granularity-timeframe-weekwise.png)


   *그림: 세부기간 및 시간대를 선택하는 날짜 선택기*

1. 세부기간을 선택하면 이전 또는 이전 화살표를 사용하여 시간을 기준으로 이전 또는 뒤로 이동할 수 있습니다.

1. 평가를 위해 과거 기간(선택한 세부 기간을 기반으로 월 또는 주 단위)을 지정합니다.

1. 선택 **[!UICONTROL Apply Selection]** 선택 사항이 적용되도록 합니다.
