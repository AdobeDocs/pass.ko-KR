---
title: 세그먼트 및 시간 간격
description: 계정 IQ의 그래픽 도구 및 보고서를 사용할 채널 뷰어의 계정 공유 가능성 및 패턴을 측정하려면 집단을 정의하거나 구독자 세그먼트를 선택하십시오.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# 세그먼트 및 시간 간격 {#segment-timeinterval}

계정 IQ에 로그인하면 대시보드 위에 있는 세그먼트 및 시간 간격 패널을 통해 가입자를 정의할 수 있습니다 [세그먼트](product-concepts.md#segmet-def). 이 패널은 결과를 필터링하고 구독자 공유 동작 및 패턴에 대한 보고서를 표시하는 데 도움이 됩니다. 이름이 인 세그먼트 **속성의 모든 계정** 는 현재 다음 옵션을 볼 수 있는 기본적으로 선택되어 있습니다.

![](assets/new-segment-selector-collapsed.png){align="left"}

*축소된 세그먼트 요약이 있는 세그먼트 및 시간 간격 패널*

**A.** 현재 선택된 세그먼트 이름 **B.** 세그먼트 목록 열기 **C.** 세그먼트 편집 **D.** 새 세그먼트 만들기 **E.** 세부 기간 및 시간 간격 선택기 **F** 세그먼트 요약을 확장하는 아이콘 **G.** 축소된 세그먼트 요약 **H.** 선택한 간격 동안 세그먼트의 계정 수

>[!NOTE]
>
> 축소된 세그먼트 요약에는 [비디오 카테고리](product-concepts.md#video-category-def) 계정 IQ의 TV Everywhere 버전에 사용됩니다. D2C 서비스로 로그인하는 경우 이러한 레이블에 회사의 특정 비디오 카테고리가 표시됩니다.

자세한 내용 [만드는 방법](work-with-segments.md#create-new-segment) 및 [세그먼트 관리](work-with-segments.md#manage-segment) 다음에서 **세그먼트** 왼쪽 패널의 탭입니다.

## 세그먼트 선택 {#segment-selection}

특정 세그먼트를 선택하려면 다음 단계를 따르십시오.

1. 다음 위치로 이동 **[!UICONTROL Open segment]** [세그먼트] 및 [시간 간격] 패널 내의 옵션입니다.
1. 다음 항목 선택 **세그먼트 이름** 계정 공유 보고서를 보려는 경우 사용할 수 있습니다.

   ![](assets/open-segment.png){align="left"}

   *세그먼트 이름 선택*

   >[!NOTE]
   >
   > 이전 이미지에 표시된 비디오 범주, 예: **MVPD**, **프로그래머**, 및 **채널** 계정 IQ의 TV Everywhere 버전에 사용된 레이블을 나타냅니다. D2C 서비스로 로그인하는 경우 이러한 레이블에 회사의 특정 비디오 카테고리가 표시됩니다.

1. 선택 **[!UICONTROL Open segment]**.


## 세부 기간 및 시간 간격 선택 {#granularity-timeinterval}

다음 **세부기간 및 시간 간격** 선택기를 사용하면 구독자 공유 동작을 관찰하기 위해 주별/월별 기준으로 집계된 날짜 및 기간을 지정할 수 있습니다. 기본 선택 항목은 현재 주입니다.

![세부기간 및 시간 간격](assets/granularity-timeinterval-weekwise.png){align="left"}

*세부 기간 및 시간 간격 대화 상자*

**A.** 세부 기간 및 시간 간격 선택기 **B.** 다음 달/주로 이동하는 오른쪽 화살표 **C.** 주/월별로 세부기간을 선택하는 옵션 **D.** 현재 선택된 시간 간격 **E.** 이전 달/주로 이동하려면 왼쪽 화살표

다음 단계를 사용하여 기간을 수정할 수 있습니다.

1. 다음 항목 선택 **[!UICONTROL Granularity and Time Interval]** 날짜 선택기에서 가져옵니다.

1. 다음 중 하나를 선택합니다. **[!UICONTROL Week]** 또는 **[!UICONTROL Month]** 출처: **[!UICONTROL Aggregate By]** 평가를 위한 세부기간을 설정하는 옵션입니다.

1. 세부기간을 선택하면 앞 또는 뒤 화살표를 사용하여 시간 범위를 탐색할 수 있습니다.

1. 평가할 특정 기간을 선택하십시오.

1. 선택 **[!UICONTROL Apply]** 선택 사항이 적용되도록 합니다.

이를 통해 문제 진술서를 &quot;12월 마지막 주 동안 채널 X, Y, Z를 시청한 MVPD A 구독자&quot;로 정의할 수 있습니다.

## 세그먼트 요약 {#segment-summary}

세그먼트 요약 은 D2C 서비스 및 TV Everywhere와 유사합니다. 비디오 카테고리는 계정 IQ의 각 버전에 대해 달라집니다.

선택 <img alt= "세그먼트 요약 확장" src="./assets/expand-segment-summary.svg" width="25"> 세부 세그먼트 요약을 보는 아이콘. 또한 선택한 기간 내의 구독자 계정 및 해당 재생 요청 수에 대한 정보를 제공합니다.

+++ D2C 서비스

![](assets/segment-panel-d2c.png){align="left"}

*D2C 서비스에 대한 세그먼트 요약*

>[!NOTE]
>
>다음 [비디오 카테고리](product-concepts.md#video-category-def) 이전 이미지에 표시된 항목: **지역** 및 **콘텐츠 유형** 세그먼트에는 예시가 있을 뿐입니다. 계정 IQ에 로그인하면 이러한 레이블에 회사의 특정 비디오 범주가 표시됩니다.

다음 **세그먼트 요약** 에는 세그먼트를 정의하는 다음 조건이 포함됩니다.

**[지역 및 콘텐츠 유형](product-concepts.md#video-category-def) 세그먼트 내** 계정 공유 보고서에 표시된 공유 계정이 시청한 비디오 스트림과 연결된 메타데이터 레이블을 참조하십시오.

**[지표](product-concepts.md#metric) 세그먼트 내** 계정 공유 보고서에서 구독자가 식별되도록 이행해야 하는 속성 또는 기준을 참조하십시오.

+++

+++ TV Everywhere

![](assets/segment-panel-programmers-mvpd.png){align="left"}

*프로그래머/MVPD에 대한 세그먼트 요약*

다음 **세그먼트 요약** 에는 세그먼트를 정의하는 다음 조건이 포함됩니다.

**[프로그래머](product-concepts.md#programmer-def) 세그먼트 내**  계정 공유 보고서에 표시된 공유 계정이 비디오 스트림을 시청한 콘텐츠 공급자를 참조하십시오.

**[채널](product-concepts.md#channel-def) 세그먼트 내** 계정 공유 보고서에 표시된 공유 계정이 비디오 스트림을 시청한 채널을 참조하십시오.

**[MVPD](product-concepts.md#mvpd-def) 세그먼트 내** 계정 공유 보고서에서 식별하려면 구독자가 연결된 다중 비디오 프로그래밍 배포자를 참조하십시오.

**[지표](product-concepts.md#metric) 세그먼트 내** 계정 공유 보고서에서 구독자가 식별되도록 이행해야 하는 속성 또는 기준을 참조하십시오.

+++
