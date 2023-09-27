---
title: 공유 점수가 높은 계정의 정보 내보내기
description: 공유 점수가 높은 계정의 정보를 내보냅니다.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 1%

---

# 공유 점수가 높은 계정의 정보 내보내기 {#export-account-info-high-score}

[!UICONTROL Account IQ] 을(를) 기반으로 상위 1000개 구독자 계정에 대한 계정 공유 세부 사항을 내보내는 옵션을 제공합니다. [확률 공유](/help/accountiq/product-concepts.md#account-sharing-probability-def). 내보낸 CSV 파일의 데이터는 다음 위치에서 선택한 MVPD의 구독자 계정 공유 가능성이 감소하는 순서로 정렬됩니다. [세그먼트](/help/accountiq/product-concepts.md#segment-def)의 경우 [지정된 시간대](/help/accountiq/product-concepts.md#time-frame-def).

계정 공유 정보를 내보내는 옵션은에서 사용할 수 있습니다. [일반 사용 보고서](/help/accountiq/general-usage-reports.md) 및 [공유 계정 보고서](/help/accountiq/shared-acc-reports.md) 페이지.

>[!NOTE]
>
>다운로드한 CSV 파일의 번호는 일반 사용 및 공유 계정 보고서 페이지에 대해 다릅니다. 이것은 [일반 사용 보고서] 페이지에는 프로그래머가 장치, IP 및 우편번호에 대해 임계값을 선택할 수 있는 추가 필터가 있기 때문입니다. 따라서 일반 사용 보고서에서 내보낸 데이터는 적용된 추가 임계값 필터를 기반으로 합니다.

![일반 사용의 내보내기 옵션](assets/export.png)

구독자의 계정 공유 정보를 내보내려면 다음을 수행합니다.

1. 의 단계에 따라 원하는 세그먼트를 정의합니다 [세그먼트를 정의하고 시간대를 선택하는 방법](/help/accountiq/howto-select-segment-timeframe.md) 평가용 [세그먼트 및 일정](/help/accountiq/segments-timeframe.md) 패널.

1. 다음 항목 선택 **[!UICONTROL Export top 1000 accounts]** 공유 확률이 가장 높은 1000명의 가입자에 대한 계정 정보를 내보내는 옵션입니다.

내보내기 옵션을 사용하면 공유 가능성이 가장 높은 1000개 계정에 대한 통계(정의된 기간 동안)가 로컬 컴퓨터의 다운로드 폴더에 다운로드됩니다.

>[!NOTE]
>
>다운로드한 CSV 파일은 CSV 파일을 읽는 모든 애플리케이션(예: Microsoft Excel)을 사용하여 열 수 있습니다.

![csv 형식으로 내보낸 데이터](assets/exported-csv.png)

*그림: CSV 형식으로 내보낸 공유 계정 데이터*

## 내보낸 보고서의 열 {#columns-in-export}

**주/월**

에서 선택한 주 또는 월 **[!UICONTROL Granularity and Time Frame]** 공유 통계를 찾는 세그먼트 선택기의 옵션입니다.

**MVPD**

프로그래머 사용자인 경우 열에는 구독자 계정이 속한 MVPD가 표시됩니다.

**구독자 Id**

연속해서 이야기하는 특정 계정입니다.

**최소 장치 수**

실제 장치 수(해당 스트림 콘텐츠)는 특정 계정에 대해 지정된 최소 장치 수보다 거의 확실히 큽니다.

>[!NOTE]
>
>실제 장치 수(해당 스트림 콘텐츠)는 특정 계정에 대해 지정된 최소 장치 수보다 확실히 큽니다.

**최소 인원 수**

해당 장치를 사용하여 콘텐츠를 스트리밍하는 활성 사용자의 절대 최소 수입니다.

>[!NOTE]
>
>실제 사용자 수(해당 스트림 콘텐츠)는 특정 계정에 대해 지정된 최소 사용자 수보다 거의 확실히 훨씬 큽니다.

**[!UICONTROL # IPs]**

콘텐츠가 스트리밍되는 IP 주소 수입니다.

**[!UICONTROL # Locations]**

콘텐츠가 스트리밍되는 위치 수(우편 번호 기준).

**[!UICONTROL # Cities]**

스트리밍이 발생한 도시 수.

**[!UICONTROL # States]**

스트리밍이 발생한 상태 수입니다.

**[!UICONTROL # Clusters]**

고유 개수 [클러스터](/help/accountiq/product-concepts.md#cluster-def) 스트리밍이 발생한 위치.

**[!UICONTROL Geographic span (miles)]**

계정과 연결된 스트리밍 위치 간의 최대 거리입니다.

**[!UICONTROL # AuthN OK]**

해당 계정을 사용하여 해당 기간 동안 사용자가 로그인한 횟수입니다.

**[!UICONTROL # AuthZ OK]**

MVPD가 해당 계정에 대해 스트림을 승인하거나 액세스 권한(콘텐츠)을 부여한 횟수입니다.

>[!NOTE]
>
>다음 **[!UICONTROL # AuthZ OK]** 은(는) 과(와) 관련이 있습니다. **[!UICONTROL # Play Requests]**; 보다 작습니다. **[!UICONTROL # Play Requests]** Adobe은 MVPD에 대해 일반적으로 24시간 동안 제공되는 권한을 캐시하기 때문입니다.

**[!UICONTROL # Play Requests]**

해당 기간 동안의 실제 스트림 수입니다.

**[!UICONTROL # Channels]**

해당 기간 동안 계정이 시청한 총 다른 채널 수입니다.

>[!NOTE]
>
>**[!UICONTROL # Channels]** 로그인된 프로그래머에 속하지 않을 수 있는 채널을 포함합니다.
>
>이 계정 번호는 계정이 채널을 시청했지만 해당 기간 동안 다른 채널에도 액세스했기 때문에 표시됩니다.

**사용 패턴**

이 열의 숫자는 모든 사용자 계정을 로 식별하는 14가지 패턴 중 하나에 매핑되는 식별자입니다.

*표: 사용 패턴이 있는 내보낸 CSV 매핑의 사용 패턴 식별자*

| ID | 1 | 2 | 3 | 4 | 5 및 8 | 6 | 7 | 9 | 10 및 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 사용 패턴 | 일반 사용자 | 여행자 또는 통근자 | 대가족 | 가까운 가족 및 친구 | 소셜 그룹 공유 | 친구 그룹 | 동시 스트리밍 | 커뮤니티 공유 | 불확실한 행동 | 작은 가족 | 세컨드 홈 | 비정상적인 사용 |

{style="table-layout:auto"}

**공유 확률**

공유 확률은 특정 계정이 자격 증명을 공유하고 있을 확률입니다.

>[!NOTE]
>
> (선택한 세그먼트에 있는) 모든 계정의 공유 확률의 평균을 사용하여 [공유 수준](/help/accountiq/dashboard.md#sharing-level) / [집계된 공유 점수](/help/accountiq/dashboard.md#aggregated-sharing).
