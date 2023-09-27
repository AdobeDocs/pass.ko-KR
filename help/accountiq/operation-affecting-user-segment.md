---
title: 사용자 세그먼트에 대한 작업 만들기 및 효과 추적
description: 정의된 사용자 세그먼트에 미치는 영향을 효과를 효과를 효과를 효과를 효과를 효과를 분석하고 추적하는 작업을 만드는 방법입니다.
exl-id: ab74f857-e178-4120-8f9c-655ec921d096
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '970'
ht-degree: 0%

---

# 사용자 세그먼트에 대한 작업 만들기 {#operation-to-track-segment}

계정 IQ의 각 보고서 페이지에는 **새 작업 만들기** 옵션은 구독자 계정에 대한 다양한(일괄) 작업을 자동화(및 간소화)하는 워크플로우를 만드는 데 도움이 됩니다. 규칙을 정의하여 샘플을 지정하고, 작업을 정의하고, 해당 작업의 효과를 기록 및 분석합니다. 작업을 생성할 페이지에서 작업을 수행할 사용자 그룹 샘플을 정의하고 미래 날짜에 작업이 실행되도록 예약할 수 있습니다.

작업을 생성하려면 다음을 수행합니다.

1. 다음 단계를 사용하여 보고서 또는 대시보드 페이지에서 분석할 세그먼트(집단)를 정의합니다. [세그먼트 및 일정 정의](/help/accountiq/howto-select-segment-timeframe.md).

1. 선택 **[!UICONTROL Create new operation]** 옵션은 보고서 또는 대시보드 페이지에서 사용할 수 있습니다.

   다음 **[!UICONTROL Create new operation]** 페이지가 표시됩니다.

   ![새 작업을 만들 페이지](assets/create-new-operations.png)
   *그림: 새 작업을 만드는 페이지*

1. 다음에서 **[!UICONTROL Create new operation]** 페이지에서 다음에 대한 양식 필드에 세부 사항을 입력합니다.

   * [작업 이름](#operation-details) 작업 세부 정보
   * 다음에서 작업을 실행할 세그먼트 [대상 세그먼트](#segment) 및 을 사용하여 세그먼트 세분화 [추가 세분화](#additional-segmentation)
   * [세그먼트 유형](#segment-type) 아래에 [대상 세그먼트](#segment)
   * [작업](#action)
   * [활성화 예약](#schedule)

1. [작업 저장](#save-operation).

## 작업 세부 정보 {#operation-details}

+++프로그래머- 작업 세부 정보

새 작업 이름을 로 지정합니다. **[!UICONTROL Operation name]** 작업 세부 사항 아래의 필드. 예, &quot;*20개 이상의 장치에서 MVPD X의 구독자에 대한 다단계 인증의 효과를 테스트하거나 &quot;동시 모니터링의 스트림 수를 제한하거나 MVPD D의 구독자에 대해 채널 &#39;N&#39;을 볼 수 있도록 제한하십시오.&quot;*&quot;.

+++

+++MVPD- 작업 세부 정보

새 작업 이름을 로 지정합니다. **[!UICONTROL Operation name]** 작업 세부 사항 아래의 필드. 예, &quot;*채널 N의 뷰어에 대한 다단계 인증의 효과를 테스트합니다.&quot; 또는 &quot;동시 모니터링에서 스트림 수 제한&quot; 또는 &quot;20개 이상의 장치에서 채널 &#39;N&#39;을 보는 구독자를 제한합니다.&quot;*&quot;.

+++

## 대상 세그먼트 {#segment}

+++프로그래머 - Target 세그먼트

다음 **[!UICONTROL Segment]** 여기에서는 이 작업으로 작업을 수행할 사용자 또는 작업에 대한 샘플 그룹을 정의합니다. 기본 세그먼트는 입니다 **[!UICONTROL segment]** 다음을 사용하여 선택함 [세그먼트 및 일정 패널](/help/accountiq/howto-select-segment-timeframe.md) 위의 1단계에서 기본 보고서 또는 대시보드 페이지에서 다음을 수행합니다.

<!--* The first segment entry in the **Segment** section, by default, shows the **segment** you selected in the step 1.

* The **segment evaluation period** is the time period of analysis you selected in step 1 from **Granularity and Timeframe** option.
![](assets/operations-segment-selection.png)
*Figure: Segment and timeframe selection on the main page*-->

이 세그먼트는 만들어지는 작업의 영향을 받을 구독자를 정의합니다. 예를 들어 선택한 세그먼트는 *채널 &#39;N 스포츠&#39;를 보는 MVPD &#39;C&#39;라는 이름의 모든 가입자 계정*.

+++

+++MVPD- 대상 세그먼트

다음 **세그먼트** 여기에서는 이 작업으로 작업을 수행할 사용자 또는 작업에 대한 샘플 그룹을 정의합니다. 기본 세그먼트는 입니다 **세그먼트** 다음을 사용하여 선택함 [세그먼트 및 일정 패널](/help/accountiq/howto-select-segment-timeframe.md) 위의 1단계에서 기본 보고서 또는 대시보드 페이지에서 다음을 수행합니다.

<!--* The first segment entry in the **Segment** section, by default, shows the **segment** you selected in the step 1.

* The **segment evaluation period** is the time period of analysis you selected in step 1 from **Granularity and Timeframe** option.
![](assets/operations-segment-selection.png)
*Figure: Segment and timeframe selection on the main page*-->

이 세그먼트는 만들어지는 작업의 영향을 받을 구독자(특정 채널의 시청자)를 정의합니다. 예를 들어 (기본값) 세그먼트에는 다음이 포함됩니다 *&#39;N 스포츠&#39; 채널을 보는 모든 구독자 계정*.
+++

### 추가 세분화 {#additional-segmentation}

또한 지표를 더 추가하여 대상 세그먼트를 세분화할 수 있습니다. 예를 들어 다음을 추가할 수 있습니다 [!UICONTROL Sharing Probability] 다른 지표로 90%보다 큽니다. 그래서, 이제 문제 진술은 *&quot;공유 확률이 90% 이상인 채널 &#39;N 스포츠&#39;를 보고 있는 MVPD 가입자 계정 &#39;C&#39;에 대한 작업을 만듭니다.&quot;*.

![](assets/additional-segment.gif)

*그림: 추가 세그멘테이션*

또한 디바이스 수에 대해 다른 지표를 추가하여 작업을 구체화하면 업데이트된 문제 문이 읽힙니다 *&#39;C&#39;라는 이름의 MVPD 가입자 계정 중 공유 점수가 90점 이상인 채널 &#39;N스포츠&#39;를 보고 있고 평가 기간 동안 콘텐츠를 보기 위해 5개 이상의 장치를 사용하고 있는 계정에 대한 작업을 만듭니다.*.

![](assets/refined-segment.png)

*그림: 전체 공유 점수 및 장치 수 지표가 있는 세분화된 예제 세그먼트*

이렇게 함으로써, 사용자 그룹이 더 세련되게 된다. 따라서 더 많은 지표와 조건을 추가하여 세그먼트를에 인증하여 운영할 계정을 정의할 수 있습니다.

### [!UICONTROL Segment Type] {#segment-type}

[!UICONTROL Segment Type] 는 작업의 평가 기간 동안 세그먼트를 처리하는 방법입니다.

![](assets/segment-type.png)

*그림: 세그먼트 유형을 사용하여 작업할 세그먼트 수 세분화*

<!--The segment type option allows you to further refine your segment based on the evaluation period (or time).

**Fixed number of accounts** 

When you select **Fixed number of accounts** segment type, then you need to specify an evaluation period as well.

By doing so, you are fixing the sample size for evaluation in terms of numbers. You are making Account IQ identify a specific set of users (that meet the criteria of defined evaluation period and segment metrics) to operate on. The analysis and graphs will be generated for this specific set of users only (identified initially) throughout the operation.

**Variable number of accounts**

When you select **Variable number of accounts** segment type, you do not limit the number of accounts in segment. The accounts which fall under the defined segment metrics are the part of the segment, and the number of accounts will change continuously during the course of operation.-->

>[!IMPORTANT]
>
>만 사용할 수 있습니다. **[!UICONTROL Fixed number of accounts]** 옵션, 지금. 선택할 옵션 **[!UICONTROL Variable number of accounts]** 향후 릴리스에서 제공될 예정입니다.

<!--

you tell Account IQ in the beginning of the operation which number of accounts to operate on.

Account IQ system only has a segment definition, and during the operation it looks into all the accounts that fit that segments.

the number of accounts in segment is not limited, the accounts that fall under defined segment metrics will be part of the segment, and the no of accounts will change continuously, as there are no specific limitations - like an evaluation period in the past.When the segment is defined (which in this example is, subscriber accounts of MVPD 'C' who are viewing the channel 'N Sports' that have a sharing score above 80 and are using 10 different IPs) and we also identified a time period to evaluate a segment. This identifies X number of accounts as sample (for example 5000). How many devices they are using?
It identifies x-number of accounts (5000)...a very specific set of users that meet this criteria.
for every period that we schedule (within that operation) during that operation) we will look at those 5K users that are originally identified and we will present graph about them. How are the sharing scores coming up?u We identified a period. Are their sharing scores going up? Are there fewer of them who are meeting this definition?
Fixed versus variable is the way the treated in fixed or variable way.

1. we identified a fixed set of accounts.
2. we evaluate those specific accounts on criteria throughout the operation.

General idea independent of graph is that we will evaluate a set of accounts identified initially, for no of periods during operation and generate graphs against that.
Those are the 5000 users for which I will create graphs for for every period of the operation.

**Variable number of accounts**
We do not identify any initial set of accounts, we just have a segment definition.
Each period during the operation, we go and look into all the accounts that fit that segments.
If it is not a fixed segment, I won't initially evaluate it. I won't have an initial set of 5000. Instead at every period during the evaluation I will evaluate the segment then, and then I will produce graph about the next 3000 users.
the......will vary from period to period.

if not fixed segment, then I won't initially evaluate or have initial set of 5000, instead at every period during an operation and the.-->

## [!UICONTROL Action] {#action}

다음 **[!UICONTROL Action]** 정의된 세그먼트에서 수행할 작업을 정의합니다.

다음과 같은 두 가지 유형의 작업을 수행할 수 있습니다.

* 계정 IQ와 통합된 시스템을 사용하는 작업: **[!UICONTROL Concurrency Monitoring]** <!--[Concurrency Monitoring](https://tve.helpdocsonline.com/concurrency-monitoring-introduction), or Adobe Target-->.

* 계정 IQ 외부에 있고 계정 IQ 시스템과 통합되지 않은 워크플로우를 만들고 처리하는 작업입니다. 예를 들어 채널 프로그래머 &#39;N&#39;이 MVPD &#39;C&#39;의 모든 구독자에게 벌크 이메일을 보내는 작업입니다.

>[!NOTE]
>
>작업을 만들면 작업을 지정하고 범위를 정의할 뿐만 아니라 이러한 작업의 효과를 기록합니다.

## 예약{#schedule}

시작 및 종료 날짜를 설정하여 작업에 대한 활성화를 예약할 수 있습니다.

>[!NOTE]
>
>시작 날짜 및 종료 날짜의 세부기간은 을 사용하여 세그먼트를 정의할 때 평가에 대해 선택한 세부기간과 동일합니다 **[!UICONTROL segment and timeframe panel]** 1단계에서 사용할 수 있습니다.
>
>
>따라서 세부 기간을 주로 선택하면 시작 날짜와 종료 날짜가 주 단위로 표시됩니다(예: 주 14). 세부 기간을 월로 선택하면 시작 날짜와 종료 날짜가 월 단위로 표시됩니다.


>[!IMPORTANT]
>
>시작 일자는 평가 기간 이후이면서 현재 일자보다 이후여야 합니다. 마찬가지로 종료 날짜도 시작 날짜 및 현재 날짜보다 이후여야 합니다.

### 작업 저장 {#save-operation}

작업을 저장할 때 이 작업에서 정의한 세그먼트가 나중에 저장되었음을 알려주는 메시지 화면이 표시됩니다. 그러나 이 세그먼트의 이름을 지정해야 합니다.

![](assets/save-operation.png)

*그림: 저장 작업 및 세그먼트 이름 지정*

>[!NOTE]
>
>수행할 세그먼트와 함께 수행할 작업을 기반으로 작업 이름을 지정하는 것이 좋습니다.

<!--In future you can select this saved segment when defining a segment for your analysis on the main reports page. Moreover, the saved segment is also listed when you create an operation the next time.

![](assets/saved-segment-operations-page.png)

*Figure: Saved segments in segment selector on Create new operations page* 

>[!IMPORTANT]
>
>When creating an operation, if you select a segment that was previously created then you cannot add new metrics to it and refine it.
>
>Adding new metrics creates a new segment, but you cannot modify an existing segment.-->

작업을 생성하면 시작 날짜부터 지정한 종료 날짜까지 작업이 실행됩니다.

저장된 작업의 세부 정보는 메인에 표시됩니다. [작업](/help/accountiq/operations.md) 페이지를 가리키도록 업데이트하는 중입니다.

![](assets/new-operation-created.png)

*그림: 새로 생성된 작업이 기본 작업 페이지에 나열됩니다.*
