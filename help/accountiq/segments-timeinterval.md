---
title: 세그먼트 및 시간 간격
description: 집단을 정의하거나 구독자 세그먼트를 선택하여 Account IQ에서 그래픽 도구 및 보고서를 사용할 수 있는 채널 뷰어의 계정 공유 가능성 및 패턴을 측정합니다.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# 세그먼트 및 시간 간격 {#segment-timeinterval}

Account IQ에 로그인하면 대시보드 위에 있는 세그먼트 및 시간 간격 패널을 통해 구독자 [세그먼트](product-concepts.md#segmet-def)를 정의할 수 있습니다. 이 패널은 결과를 필터링하고 구독자 공유 동작 및 패턴에 대한 보고서를 표시하는 데 도움이 됩니다. 다음 옵션을 볼 수 있는 **속성의 모든 계정**&#x200B;이라는 세그먼트가 현재 기본적으로 선택되어 있습니다.

![](assets/new-segment-selector-collapsed.png){align="left"}

*축소된 세그먼트 요약이 있는 세그먼트 및 시간 간격 패널*

**A.** 현재 선택한 세그먼트 이름 **B.** 세그먼트 목록 열기 **C.** 세그먼트 편집 **D.** 새 세그먼트 만들기 **E.** 세부기간 및 시간 간격 선택기 **F.** 아이콘을 확장하여 세그먼트 요약 **G.** 축소된 세그먼트 요약 **H.** 선택한 간격에 대한 세그먼트의 계정 수

>[!NOTE]
>
> 축소된 세그먼트 요약에는 Account IQ의 TV Everywhere 버전에서 사용되는 [비디오 범주](product-concepts.md#video-category-def)가 표시됩니다. D2C 서비스로 로그인하는 경우 이러한 레이블에 회사의 특정 비디오 카테고리가 표시됩니다.

왼쪽 패널의 **세그먼트** 탭에서 [만드는 방법](work-with-segments.md#create-new-segment) 및 [세그먼트 관리](work-with-segments.md#manage-segment)에 대해 자세히 알아보세요.

## 세그먼트 선택 {#segment-selection}

특정 세그먼트를 선택하려면 다음 단계를 따르십시오.

1. 세그먼트 및 시간 간격 패널 내의 **[!UICONTROL Open segment]** 옵션으로 이동합니다.
1. 계정 공유 보고서를 보려는 **세그먼트 이름**&#x200B;을(를) 선택하십시오.

   ![](assets/open-segment.png){align="left"}

   *세그먼트 이름 선택*

   >[!NOTE]
   >
   > **MVPD**, **프로그래머**, **채널**&#x200B;과 같이 이전 이미지에 표시된 비디오 범주는 Account IQ의 TV Everywhere 버전에서 사용되는 레이블을 나타냅니다. D2C 서비스로 로그인하는 경우 이러한 레이블에 회사의 특정 비디오 카테고리가 표시됩니다.

1. **[!UICONTROL Open segment]**&#x200B;을(를) 선택합니다.


## 세부 기간 및 시간 간격 선택 {#granularity-timeinterval}

**세부 기간 및 시간 간격** 선택기를 사용하면 구독자 공유 동작을 관찰하기 위해 주별/월별 기준으로 집계된 날짜와 기간을 지정할 수 있습니다. 기본 선택 항목은 현재 주입니다.

![세부 기간 및 시간 간격](assets/granularity-timeinterval-weekwise.png){align="left"}

*세부 기간 및 시간 간격 대화 상자*

**A.** 세부 기간 및 시간 간격 선택기 **B.** 다음 달/주로 이동하는 오른쪽 화살표 **C.** 주/월별로 세부 기간을 선택하는 옵션 **D.** 현재 선택한 시간 간격 **E.** 이전 달/주로 이동하는 왼쪽 화살표

다음 단계를 사용하여 기간을 수정할 수 있습니다.

1. 날짜 선택기에서 **[!UICONTROL Granularity and Time Interval]**&#x200B;을(를) 선택합니다.

1. **[!UICONTROL Aggregate By]** 옵션에서 **[!UICONTROL Week]** 또는 **[!UICONTROL Month]**&#x200B;을(를) 선택하여 평가에 대한 세부 기간을 설정하십시오.

1. 세부기간을 선택하면 앞 또는 뒤 화살표를 사용하여 시간 범위를 탐색할 수 있습니다.

1. 평가할 특정 기간을 선택하십시오.

1. 선택 내용이 적용되도록 하려면 **[!UICONTROL Apply]**&#x200B;을(를) 선택하십시오.

이를 통해 문제 진술서를 &quot;12월 마지막 주 동안 채널 X, Y, Z를 시청한 MVPD A 구독자&quot;로 정의할 수 있습니다.

## 세그먼트 요약 {#segment-summary}

세그먼트 요약 은 D2C 서비스 및 TV Everywhere와 유사합니다. 비디오 카테고리는 Account IQ의 각 버전에 따라 달라집니다.

선택 자세한 세그먼트 요약을 보려면 <img alt= "세그먼트 요약 확장" src="./assets/expand-segment-summary.svg" width="25"> 아이콘을 클릭하세요. 또한 선택한 기간 내의 구독자 계정 및 해당 재생 요청 수에 대한 정보를 제공합니다.

+++ D2C 서비스

![](assets/segment-panel-d2c.png){align="left"}

*D2C 서비스에 대한 세그먼트 요약*

>[!NOTE]
>
>세그먼트에 있는 **지역** 및 **콘텐츠 형식**&#x200B;과 같이 이전 이미지에 표시된 [비디오 범주](product-concepts.md#video-category-def)은(는) 예제입니다. Account IQ에 로그인하면 이러한 레이블에 회사의 특정 비디오 범주가 표시됩니다.

**세그먼트 요약**&#x200B;에는 세그먼트를 정의하는 다음 조건이 포함되어 있습니다.

**세그먼트의**&#x200B;[&#x200B;지역 및 콘텐츠 형식](product-concepts.md#video-category-def)은(는) 계정 공유 보고서에 표시된 공유 계정이 시청하는 비디오 스트림과 연결된 메타데이터 레이블을 참조합니다.

세그먼트&#x200B;**의**&#x200B;[&#x200B;지표](product-concepts.md#metric)은(는) 구독자가 계정 공유 보고서에서 식별하도록 이행해야 하는 특성 또는 조건을 참조합니다.

+++

+++ TV Everywhere

![](assets/segment-panel-programmers-mvpd.png){align="left"}

프로그래머/MVPD에 대한 *세그먼트 요약*

**세그먼트 요약**&#x200B;에는 세그먼트를 정의하는 다음 조건이 포함되어 있습니다.

세그먼트&#x200B;**의**&#x200B;[&#x200B;프로그래머](product-concepts.md#programmer-def)는 계정 공유 보고서에 표시된 공유 계정이 비디오 스트림을 시청한 콘텐츠 공급자를 가리킵니다.

세그먼트&#x200B;**의**&#x200B;[&#x200B;채널](product-concepts.md#channel-def)은(는) 계정 공유 보고서에 표시된 공유 계정에서 비디오 스트림을 시청한 채널을 나타냅니다.

세그먼트&#x200B;**의**&#x200B;[ MVPD](product-concepts.md#mvpd-def)은(는) 계정 공유 보고서에서 식별되도록 구독자가 연결된 다중 비디오 프로그래밍 배포자를 참조합니다.

세그먼트&#x200B;**의**&#x200B;[&#x200B;지표](product-concepts.md#metric)은(는) 구독자가 계정 공유 보고서에서 식별하도록 이행해야 하는 특성 또는 조건을 참조합니다.

+++
