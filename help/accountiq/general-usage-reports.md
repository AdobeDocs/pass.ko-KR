---
title: 일반 사용 보고서
description: 일반 사용 보고서
exl-id: 1272073a-61fe-47ec-aced-2e8055b6b11e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 0%

---

# [!UICONTROL General Usage] 보고서 {#general-usage-reports}

[!UICONTROL Account IQ] 보고서는 데이터를 드릴다운하여 분리할 수 있는 기본 분석 도구 및 보고서입니다 [집단](/help/accountiq/product-concepts.md#segmet-def), 예외 항목을 식별하고 계정 특성에 대한 이해를 구축합니다.

[!UICONTROL General Usage] 보고서 페이지는 사용 중인 계정 장치 수, 감지된 IP 및 각 우편 번호를 기반으로 하위 그룹 지표를 작성하는 도구를 제공합니다.

<!--Divide the content in cohorts.

Content filters
device filters

segment and definition replicate to cohorts. Number of people and number of account that ......
content consumption.....-->

보고서는 모두 다음을 사용하여 선택한 현재 세그먼트를 기반으로 합니다. [세그먼트 및 시간대](/help/accountiq/howto-select-segment-timeframe.md) 패널. 에서 (장치 수, IP 수 및 zip 코드 수) 임계값을 지정하여 선택 항목을 미세 조정하고 범위를 좁힐 수 있습니다. [스냅샷 개요 - 임계값 초과 계정](#snapshot-overview) 패널.

<!--To view General Usage Reports:

1. Select the desired MVPDs from the **MVPDs in Segment** option.

2. Select the desired programmer channels from the **Channels in Segment** Option.

3. Select an appropriate time frame from the **Granularity and time frame** option.

   Using the above options you have defined segments for your analysis. Based on your segment selection, following graphs and reports are displayed.

4. You can fine tune your selection and further narrow it down by specifying (number of devices, number of IPs, and number of zip codes) thresholds in [Snapshot Overview - Accounts above thresholds](#snapshot-overview) widget/panel.-->

## AuthN OK / AuthZ OK / Play Requests / Unique Subscribers {#authn-authz-playreq-uniquesubs}

여기에 있는 선 그래프는 정의된 세그먼트에 대해 선택한 시간대에서 AuthN OK, AuthZ OK, 요청 재생 및 고유 구독자 값의 시간 경과에 따른 변경 사항을 보여줍니다.

+++프로그래머 **AuthN OK / AuthZ OK / Play Requests / Unique Subscribers**

![](assets/progr-line-graph-gu.png)


*그림: AuthN OK / AuthZ OK / Play Requests / 프로그래머 사용자를 위한 고유 구독자*


+++


+++MVPD- **AuthN OK / AuthZ OK / 고유 구독자**

![](assets/mvpd-line-graph-gu.png)


*그림: AuthN OK / AuthZ OK / MVPD 사용자에 대한 고유 구독자*


+++

x축은 현재 시간대 내의 단위를 나타내고 y축은 해당 기간 동안의 기본 가입자 활동 지표를 나타냅니다. 선 그래프를 사용하면 세그먼트 선택 패널에서 선택한 MVPD 및 채널의 구독자에 대해 다음 값을 비교할 수 있습니다.

* **AuthN 확인**

  AuthN OK는 성공한 인증 수입니다. 자세한 내용 및 정의는 [제품 개념: AuthN 확인](/help/accountiq/product-concepts.md#authn-ok-def).

* **AuthZ 확인**

  AuthZ OK는 성공한 승인 수입니다. 자세한 내용 및 정의는 [제품 개념: AuthZ OK](/help/accountiq/product-concepts.md#authz-ok-def).

* **요청 재생**

  Play 요청은 재생 요청의 수입니다. 자세한 내용 및 정의는 [제품 개념: 재생 요청](/help/accountiq/product-concepts.md#play-requests-def)

  >[!NOTE]
  >
  >MVPD 사용자는 재생 요청 라인 그래프를 사용할 수 없습니다.


* **고유 구독자**

  Unique Subscribers는 성공한 고유 구독자의 수입니다. 자세한 내용 및 정의는 [제품 개념: 고유 구독자](/help/accountiq/product-concepts.md#unique-subscriber-def)

  >[!NOTE]
  >
  >프로그래머의 Adobe TempPass 사용(즉, 무료 미리보기)이 세그먼트에 속하는 경우 총 고유 구독자 수에는 고유 디바이스 수도 포함됩니다.

## 스냅샷 개요 - 임계값 초과 계정 {#snapshot-overview}

이 추가 필터를 사용하여 분석 및 보고서를 미세 조정하여 다양한 사용 임계값을 설정합니다. 원하는 MVPD 및 채널을 선택하여 분석할 세그먼트(또는 집단)를 정의하면 다음 필터를 사용하여 구독자 동작을 분석할 수도 있습니다.

* 장치 수 임계값

* IP 수 임계값

* 우편 번호 임계값 수

에서 임계값을 업데이트하는 경우 [계정 세그먼트 - 선택한 임계값 기반](#account-segments-basedon-segments) 패널, 다음에서 영향을 볼 수 있습니다.

* [계정당 주당(또는 월별) 장치](#devices-week-account)

* [계정당 주당(또는 월별) 위치](#locations-week-account)

* [계정당 주당(또는 월별) IP](#ip-week-account)

* [계정 세그먼트의 내역 보기](#account-segment-historical-view)

>[!NOTE]
>
>각 임계값의 기본값은 4입니다. 즉, [일반 사용] 페이지에는 4개(및 4개 이상)의 장치를 사용하는 구독자가 있는 MVPD와 4개(및 그 이상)의 지리적 위치 및 4개(및 그 이상)의 서로 다른 우편번호의 콘텐츠가 소비되는 분석이 표시됩니다.

### 계정 세그먼트 - 선택한 임계값 기반 {#account-segments-basedon-segments}

다음 **계정 세그먼트 - 선택한 임계값 기반** 패널은 장치 수, IP 수 및 우편번호 수에 대한 임계값(1~10 사이)을 설정하는 옵션을 제공합니다.

그래프는 다음을 보여 줍니다.

* 구독자 계정의 절대 수 및

* 해당 세그먼트의 총 구독자 계정 중 백분율,

  (의 정의된 세그먼트) MVPD에 대한 채널의 콘텐츠를 일정 기간 동안 소비하기 위해 X개의 장치, Y개의 IP 및 Z개의 우편번호를 사용하는 것입니다.

![](assets/select-thresholds.png)

## 계정당 주당(또는 월별) 장치 {#devices-week-account}

다음 **막대 그래프** 구독자가 디바이스를 사용하여 콘텐츠에 액세스하는 방법과 관련하여 사용 동작에 대한 통찰력을 제공합니다.

x축은 계정 수, y축은 장치 수를 나타냅니다. 계정당 장치 수에 대해 설정한 임계값에 따라 일주일 동안 특정 수의 장치에서 콘텐츠를 소비하는 구독자 계정의 절대 수를 표시합니다.

![](assets/bar-gr-devices-w-acc.png)

마우스로 막대(장치 수에 따라 다름)를 가리키면 일주일에 여러 장치를 사용하여 채널 콘텐츠를 스트리밍하는 구독자 계정의 수(및 세그먼트의 총 구독자 계정 중 백분율)에 대한 정보를 제공하는 레이블이 나타납니다.

이 그래프는 또한 다음을 표시합니다.

* 설정한 임계값을 표시하는 빨간색 선.

* 주(또는 월)당 구독자 계정에서 사용하는 다른 장치 수의 평균을 표시하는 녹색 줄입니다.

임계값 수준을 계정에서 사용하는 다른 장치 수의 주별 평균과 비교하여 공유 수준을 판단할 수 있습니다.

또한 이 그래프는 설정된 임계값보다 더 많은 수의 디바이스를 사용하고 있는 가입자 계정의 백분율을 보여줍니다.

도넛 차트는 설정된 임계값(일정)보다 많은 디바이스를 사용하여 채널 콘텐츠를 소비하는 구독자 계정의 크기를 한눈에 파악할 수 있도록 해 줍니다.

![](assets/donut-devices-w-acc.png)

## 계정당 주당(또는 월별) 위치 {#locations-week-account}

좋아요 [계정당 주당(또는 월별) 장치](#devices-week-account), 계정당 주당(또는 월별) 위치 지표를 사용하면 다른 위치의 구독자 계정 사용을 분석하여 암호 공유를 보다 면밀하게 식별할 수 있습니다. X축은 계정 수, Y축은 위치 수를 나타냅니다.

이 지표의 결과와 다음의 숫자 결합 [계정당 주당(또는 월별) 장치](#devices-week-account) 및 수 [계정당 주당(또는 월별) IP](#ip-week-account) 실제 사용자가 카운트되지 않도록 암호 공유 인스턴스를 보다 정확하게 판단할 수 있습니다.

![](assets/graph-loc-week-acc.png)

세그먼트를 정의하고 위치 수에 대한 임계값을 설정하면 그래프에서 다음을 식별할 수 있습니다.

* 한 주 동안 특정 위치 x개에서 콘텐츠를 소비하는 구독자의 수(및 백분율)입니다.

* 임계값보다 많은 위치에서 콘텐츠를 보고 있는 총 구독자 계정의 백분율입니다.

* 주별 평균(계정의 다른 위치 수)을 임계값과 비교합니다.

## 계정당 주당(또는 월별) IP {#ip-week-account}

과 유사 [계정당 주당(또는 월별) 장치](#devices-week-account) 및 [계정당 주당(또는 월별) 위치](#locations-week-account), **계정당 주당 IP 수** 지표를 사용하면 암호 공유를 더 정밀하고 세부적으로 분석할 수 있습니다.

x축은 계정 수를, y축은 IP 수를 나타냅니다.

![](assets/graph-ip-week-acc.png)

세그먼트를 정의하고(MVPD 및 채널 선택) IP 수에 대한 임계값을 설정하면 그래프에서 다음을 식별할 수 있습니다.

* 일주일 동안 (특정) x IP 수에서 콘텐츠를 소비하는 구독자의 수(및 백분율)입니다.

* 임계값보다 많은 IP 주소에서 콘텐츠를 보고 있는 총 구독자 계정의 백분율입니다.

* 주별 평균(계정에 대한 다른 IP 수)을 임계값과 비교합니다.

## 계정 세그먼트 - 내역 보기 {#account-segment-historical-view}

내역 보기 막대 그래프는 여러 시간대의 사용 지표를 비교하는 데 도움이 됩니다. 또한 다음과 같은 다양한 사용 지표를 전체적으로 표시합니다. [계정당 주당(또는 월별) 장치](#devices-week-account), [계정당 주당(또는 월별) 위치](#locations-week-account), 및 [계정당 주당(또는 월별) IP](#ip-week-account).

* x축은 시간대를 나타내고 y축은 가입자 계정, 디바이스, 위치 및 IP의 수를 나타냅니다.

* 주황색 막대는 다양한 시간대의 세그먼트를 나타냅니다.

* 선 그래프는에서 변경 사항을 표시합니다. [계정당 주당(또는 월별) 장치](#devices-week-account), [계정당 주당(또는 월별) 위치](#locations-week-account), 및 [계정당 주당(또는 월별) IP](#ip-week-account) 임계값을 기반으로 하는 시간 프레임 내 값입니다.

![](assets/historical-view.png)

* 파란색 막대는 한 기간 동안 업계 전체의 총 활성 구독자 수를 나타냅니다.

* 특정 범례를 선택할 수 있으며 그래프 크기를 조정할 수 있습니다.

![](assets/historical-view-total.png)

>[!MORELIKETHIS]
>
>* 다음을 사용하여 일반 사용 보고서에서 필터를 사용하여 선택한 세그먼트의 상위 1000개 구독자에 대한 보고서를 내보내는 방법을 알아봅니다. [상위 1000개 계정 내보내기](/help/accountiq/export-acc-information.md) 옵션을 선택합니다.
