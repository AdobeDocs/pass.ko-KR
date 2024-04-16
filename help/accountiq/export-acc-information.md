---
title: 공유 점수가 높은 계정의 정보 내보내기
description: 공유 점수가 높은 계정의 정보를 내보냅니다.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# 공유 점수가 높은 계정의 정보 내보내기 {#export-account-info-high-score}

[!UICONTROL Account IQ] 을(를) 통해 상위 1000개 가입자 계정의 계정 공유 세부 사항을 내보낼 수 있습니다. [확률 공유](/help/accountiq/product-concepts.md#account-sharing-probability-def). 현재 항목에 대한 계정 공유 정보를 내보낼 수 있습니다 [세그먼트](/help/accountiq/product-concepts.md#segment-def) 및 [지정된 시간 간격](/help/accountiq/product-concepts.md#time-interval-def) 다음에 있음 [공유 계정 보고서](/help/accountiq/shared-acc-reports.md) 페이지를 가리키도록 업데이트하는 중입니다.

단계에 따라 특정 세그먼트에 대한 가입자 계정의 계정 공유 정보를 내보냅니다.

1. 자격 증명으로 로그인합니다.
1. 다음 위치로 이동 **공유 계정** 아래의 탭 **보고서** 섹션.
1. 세그먼트 및 시간 간격 패널에서 필요한 세그먼트 및 시간 간격을 선택합니다. 학습 [세그먼트 및 시간 간격을 선택하는 방법](segments-timeinterval.md).

   필요한 경우 다음 지침을 참조하십시오. [세그먼트 만들기](work-with-segments.md#create-new-segment) 또는 [세그먼트 편집](work-with-segments.md#edit-segment).

1. 선택 **[!UICONTROL Export top 1000 accounts]** 세그먼트 및 시간 간격 패널의 오른쪽 위 모서리에 있습니다.

   ![상위 1000개 계정 내보내기](assets/export-top-1000-accounts.png)

   *상위 1000개 계정 내보내기 옵션을 선택합니다.*

파일은 로컬 컴퓨터에 .csv 로 자동으로 다운로드됩니다.

이 파일에는 현재 세그먼트의 구독자 계정의 공유 확률을 기반으로 한 상위 1000개 계정에 대한 데이터가 감소 순서로 포함되어 있습니다.

다음은 내보낸 .csv 파일의 예입니다.

![.csv 파일로 내보낸 데이터](assets/exported-csv.png)

*.csv 파일로 내보낸 데이터*

## 내보낸 보고서의 열 {#columns-in-export}

**주/월**

다음 기간 내에 선택한 주 또는 월 **[!UICONTROL Granularity and Time Interval]** 선택 사항을 추가합니다.

**MVPD**

프로그래머인 경우 열에 계정이 구독되는 배포자가 표시됩니다.

>[!NOTE]
>
> 다음 **MVPD** 열은 TV Everywhere 버전에만 사용할 수 있습니다.

**구독자 ID**

특정 계정에 대한 고유 식별자.

**최소 장치 수**

사용자가 능동적으로 콘텐츠를 스트리밍하는 최소 장치 수입니다.

>[!NOTE]
>
>콘텐츠를 스트리밍하는 장치의 실제 수는 특정 계정에 대해 지정된 최소 장치 수보다 큽니다.

**최소 인원 수**

해당 디바이스를 사용하여 컨텐츠를 능동적으로 스트리밍한 개인의 최소 수입니다.

>[!NOTE]
>
>콘텐츠를 스트리밍하는 개인의 실제 숫자는 특정 계정에 할당된 최소 인원 수보다 큽니다.

**[!UICONTROL # IPs]**

콘텐츠가 스트리밍되는 IP 주소 수입니다.

**[!UICONTROL # Locations]**

콘텐츠가 스트리밍되는 위치 수(우편 번호 기준)입니다.

**[!UICONTROL # Cities]**

스트리밍 활동이 발생한 도시의 수입니다.

**[!UICONTROL # States]**

스트리밍 활동이 발생한 상태의 수입니다.

**[!UICONTROL # Clusters]**

고유 개수 [클러스터](/help/accountiq/product-concepts.md#cluster-def) 스트리밍이 발생한 위치.

**[!UICONTROL Geographic span (miles)]**

계정과 연결된 스트리밍 위치 간의 최대 거리입니다.

**[!UICONTROL # AuthN OK]**

사용자가 해당 계정을 사용하여 지정된 기간 동안 수행한 로그인 횟수입니다.

>[!NOTE]
>
> 일부 D2C 서비스에는 표시되지 않을 수 있습니다. **[!UICONTROL # AuthN OK]** 회사 데이터에 포함되지 않을 수 있는 데이터입니다.

**[!UICONTROL # AuthZ OK]**

MVPD가 해당 계정에 대한 스트림을 승인하거나 콘텐츠에 대한 액세스 권한을 부여한 횟수입니다.

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]** 는 D2C 서비스에 사용할 수 없습니다.

>[!NOTE]
>
>TV는 어디에서나 **[!UICONTROL # AuthZ OK]** 은(는) 의 수와 상관 관계가 있습니다. **[재생 요청 수](/help/accountiq/product-concepts.md##play-requests-def)**. 항상 다음보다 작음 **[!UICONTROL # Play Requests]** Adobe은 일반적으로 약 24시간 동안 MVPD의 권한을 캐시하기 때문입니다.


**[!UICONTROL # Play Requests]**

지정된 기간 동안 발생한 실제 스트림 수입니다.

>[!NOTE]
>
>다음 [재생 요청 수](/help/accountiq/product-concepts.md##play-requests-def) 열은 TV Everywhere MVPD 버전에서 사용할 수 없습니다.

**[!UICONTROL # Channels]**

계정이 지정된 기간 동안 시청한 전체 채널 수입니다.

>[!NOTE]
>
> D2C 서비스용 **[!UICONTROL # Channels]** 은(는) 의 수에 해당합니다. **[!UICONTROL # Video categories]**.

>[!NOTE]
>
>TV Everywhere의 경우 로그인한 프로그래머에 속하지 않을 수 있는 채널을 포함합니다. 이 계정 번호에는 지정된 기간 동안 액세스한 채널 및 기타 채널이 포함됩니다.


**사용 패턴**

이 열 내의 값은 모든 사용자 계정을 분류하는 데 사용하는 14가지 패턴 중 하나에 해당하는 식별자 역할을 합니다.

<table>
    <tbody>
      <tr>
        <th style="width:10%">ID</th>
        <th style="width:30%">사용 패턴</th>
      </tr>
      <tr>
        <td>1</td>
        <td>일반 사용자</td>
      </tr>
      <tr>
        <td>2</td>
        <td>여행자 또는 통근자</td>
      </tr>
      <tr>
        <td>3</td>
        <td>대가족</td>
      </tr>
      <tr>
        <td>4</td>
        <td>가까운 가족 및 친구</td>
      </tr>
      </tr>
         <td>5 및 8</td>
         <td>소셜 그룹 공유</td>
      </tr>
      </tr>
         <td>6</td>
         <td>친구 그룹</td>
      </tr>
      </tr>
         <td>7</td>
         <td>동시 스트리밍</td>
      </tr>
      </tr>
         <td>9</td>
         <td>커뮤니티 공유</td>
      </tr>
      </tr>
         <td>10 및 11</td>
         <td>불확실한 행동</td>
      </tr>
      </tr>
         <td>12</td>
         <td>작은 가족</td>
      </tr>
      </tr>
         <td>13</td>
         <td>세컨드 홈 </td>
      </tr>
      </tr>
         <td>14</td>
         <td>비정상적인 사용</td>
      </tr>
    </tbody>
  </table>

*사용 패턴을 포함하는 내보낸 .csv 매핑의 사용 패턴 식별자*

**공유 확률**

특정 계정이 자격 증명을 공유하고 있을 가능성.

>[!NOTE]
>
> 선택한 세그먼트의 모든 계정에 대한 공유 확률의 평균을 사용하여 [공유 수준](/help/accountiq/data-panels.md#sharing-level) / [평균 공유 점수](/help/accountiq/data-panels.md#aggregated-sharing).
