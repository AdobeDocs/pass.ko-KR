---
title: 계정 IQ의 작업
description: 계정 IQ의 작업에는 구독자 계정에 대한 자동화 및 대량 작업을 수행하고 그 효과를 추적하는 작업이 포함됩니다.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# 작업 {#operations-tab-next-steps}

구독자의 사용 패턴을 분석하고 다음을 사용하여 선택한 세그먼트에 대한 암호 공유 인스턴스를 식별한 후 [!DNL Account IQ] analytics에서 작업이라는 집중 절차를 통해 타깃팅된 작업을 수행할 수 있습니다. [!DNL Account IQ].

**작업** 을(를) 사용하면 계정 그룹에 대한 자격 증명 공유를 효과적으로 추적 및 관리하여 암호 공유를 완화하고 중요한 구독자의 경험을 향상시킬 수 있습니다.

정의된 항목에 작업을 적용할 수 있습니다 [세그먼트](/help/accountiq/product-concepts.md#segment-def) 특정 암호 내에서 암호 공유 문제 해결 [시간 간격](/help/accountiq/product-concepts.md#time-interval-def) 미래의 날짜에 실행되도록 작업을 예약합니다. 이러한 작업에는 암호 공유를 최소화하기 위한 제한 또는 공유하지 않는 계정에 대한 제한 완화가 포함됩니다.

작업을 사용하여 작업과 해당 범위를 지정할 뿐만 아니라 결과를 측정합니다.

결과를 평가하여 대출자를 전환하든, 자격 증명 공유를 완화하든 또는 이탈을 감소하든 간에 효과를 최적화하기 위한 전략을 구체화할 수 있습니다.

다음과 같은 작업을 통해 다양한 기능을 수행할 수 있습니다.

* [작업 보고서 보기](#operation-reports)
* [새 작업 만들기](#create-new-operation)
* [작업 중지](#stop-operation)

## 작업 보고서 보기 {#operation-reports}

작업 보고서를 통해 작업의 효과를 검토할 수 있습니다. 작업 보고서를 보려면 다음을 선택합니다. **작업** 아래의 탭 **작업** 계정 IQ 애플리케이션의 왼쪽 패널에서. 시스템에서 사용할 수 있는 작업 목록이 표시됩니다. 테이블 형식으로 각 작업에 대한 주요 세부 정보에 액세스할 수 있습니다. 세부 사항은 다음과 같습니다.

* 작업 이름
* 현재 상태(예: 예약됨, 실행 중, 종료됨, 오류 또는 중지됨)
* 진행 완료율
* 작업이 적용되는 타겟 대상자 또는 세그먼트
* 작업에 대해 선택한 작업 유형
* 작업 시작일
* 작업 종료일
* 작업이 생성된 날짜
* 작업의 마지막 수정 날짜

![](assets/operations-page.png)

*계정 IQ의 기존 작업 목록 및 세부 정보*

원하는 을 선택합니다 **작업 이름** 작업 목록에서 참조할 수 있습니다. 다음 보고서가 표시됩니다.

### 작업 성능 {#operation-performance}

작업 성능은 영향을 받는 계정 수, 작업 진행 상황 및 작업 중 세그먼트에 있는 계정의 전체 공유 점수를 요약하는 맨 위 라인 판독을 제공합니다 [평가 기간](/help/accountiq/product-concepts.md#evaluation-period-def).

![작업 성과 보고서](assets/operation-performance.png)

*작업 성과 보고서*

**A.** 영향을 받는 계정 **B.** 작업 진행률 **C.** 전체 공유 점수

#### 영향을 받는 계정 {#impacted-accounts}

이 숫자는 작업의 평가 기간 동안 수행한 작업의 영향을 받는 구독자 계정 수를 표시합니다.

#### 작업 진행률 {#operation-progress}

이 측정은 계획된 일정에서 완료된 작업의 일수와 백분율을 보여 줍니다.

#### 전체 공유 점수 {#overall-sharing-score}

이 선 그래프는 [전체 공유 점수](/help/accountiq/data-panels.md#overall-sharing-score)를 반환합니다. 여기에는 작업의 평가 기간 동안 매주 공유 계정의 공유 수준 및 사용량이 포함됩니다.

### 작업 영향: 세그먼트의 계정 {#impact-accounts}

이 보고서는 시간에 따른 작업의 영향을 보여 주는 누적 열 그래프로 표시됩니다.

![작업이 세그먼트 그래프의 계정에 미치는 영향](assets/accounts-in-segment.png)

*작업이 세그먼트 그래프의 계정에 미치는 영향*

x축은 작업의 [평가 기간](/help/accountiq/product-concepts.md#evaluation-period-def), y축은 작업 세그먼트에 있는 계정의 상태를 나타냅니다. 그래프의 각 막대는 세 가지 색상으로 구분됩니다.

* Pink는 이 작업에 사용된 세그먼트의 조건을 충족하는 계정 수를 나타냅니다.

* 파란색은 원래 세그먼트에 있었지만 작업 중 각 주 또는 월 동안 세그먼트의 조건을 충족하지 않은 활성 계정 수를 나타냅니다. [평가 기간](/help/accountiq/product-concepts.md#evaluation-period-def).

* 회색은 평가 기간 동안 비활성 상태였던 계정을 나타냅니다.

>[!NOTE]
>
>첫 번째 분홍색 막대는 평가 기간이 시작될 때 운영 세그먼트의 조건을 충족하는 계정 수를 나타냅니다.

시간이 지남에 따라 그래프는 원래 기준에 상대적인 계정 동작의 변화를 보여 줍니다(예를 들어, 90개 이상의 공유 확률과 5개 이상의 디바이스 사용이 비활성화됨).

### 작업에 미치는 영향: 공유 계정 지표 {#impact-shared-accounts}

공유 계정 지표는 작업 동안 작업의 세그먼트에서 가입자 계정의 공유 수준 및 재생 요청에 대한 개요를 제공합니다. [평가 기간](/help/accountiq/product-concepts.md#evaluation-period-def).

#### 공유 수준 {#share-level}

이 선 그래프는 [공유 수준](/help/accountiq/data-panels.md#sharing-level) 매주 작업 평가 기간 동안.

![공유 수준 선 그래프](assets/share-level.png){width="550" align="left"}

*공유 수준 선 그래프*

#### 재생 요청 수 {#play-requests}

이 선 그래프는 [재생 요청](/help/accountiq/general-usage-reports.md#playreq-uniquesubs) 작업의 평가 기간 내 매주.

![라인 그래프의 재생 요청 수](assets/number-play-requests.png){width="550" align="left"}

*라인 그래프의 재생 요청 수*

### 작업에 미치는 영향: 일반 사용 지표 {#impact-general-usage}

일반 사용 지표는 작업 동안 작업의 세그먼트에 있는 평균 장치, IP 수 및 위치에 대한 개요를 제공합니다. [평가 기간](/help/accountiq/product-concepts.md#evaluation-period-def).

#### 장치 수 {#devices}

이 선 그래프는 평균을 나타냅니다. [장치 수](/help/accountiq/general-usage-reports.md#devices-week-account) 작업의 평가 기간 내 매주.

![장치 선 그래프 수](assets/number-devices.png){width="550" align="left"}

*장치 선 그래프 수*

#### IP 및 위치 수 {#IPs-locations}

이 선 그래프는 평균을 나타냅니다. [ip 수](/help/accountiq/general-usage-reports.md#ip-week-account) 및 [위치](/help/accountiq/general-usage-reports.md#locations-week-account) 작업의 평가 기간 내 매주.

![IP 수 및 위치 선 그래프](assets/number-ips-locations.png){width="550" align="left"}

*IP 수 및 위치 선 그래프*

보고서를 닫고 메인으로 돌아가려면 **작업** 페이지, 선택 **작업** 아래의 탭 **작업** 왼쪽 패널에서

## 새 작업 만들기 {#create-new-operation}

로 이동하면 **작업** 아래의 탭 **작업** 왼쪽 패널에서 을 선택합니다 **새 작업 만들기** 의 맨 위에 **작업** 페이지를 가리키도록 업데이트하는 중입니다.

새 작업을 만들려면 다음 섹션의 지침을 따릅니다.

* [작업 세부 정보](#operation-details)
* [세그먼트](#segment)
* [작업](#action)
* [예약](#schedule)

### 작업 세부 정보 {#operation-details}

이 섹션에서 작업 이름을 입력합니다. **작업 이름**.

>[!TIP]
>
>의 작업 목적 또는 작업 특성 설명 **작업 이름** 식별을 빠르게 하기 위해. 다음에 대한 옵션: **설명 및 태그 추가** 은 향후 릴리스에서 사용할 수 있습니다.

![작업 세부 정보에 작업 이름 추가](assets/operation-details.png)

*작업 이름 추가*

### 세그먼트 {#segment}

이 섹션에서 다음을 클릭합니다. **세그먼트 선택** 이 작업을 사용할 세그먼트를 선택합니다. 학습 [세그먼트 선택 방법](/help/accountiq/segments-timeinterval.md#segment-selection).

세그먼트를 선택한 후에는 <img alt= "세그먼트 요약 확장" src="./assets/expand-segment-summary.svg" width="25"> 세부 세그먼트 요약을 보는 아이콘. 자세한 내용 [세그먼트 요약](segments-timeinterval.md#segment-summary).

![세그먼트 및 시간 간격 선택](assets/select-segment-timeinterval.png)

*세그먼트 및 시간 간격 선택*

>[!NOTE]
>
>다음 [비디오 카테고리](product-concepts.md#video-category-def) 이전 이미지에 표시된 항목: **MVPD**, **프로그래머**, 및 **채널** 계정 IQ의 TV Everywhere 버전에 사용된 레이블을 나타냅니다. D2C 서비스로 로그인하는 경우 이러한 레이블에 회사의 특정 비디오 카테고리가 표시됩니다.

필요한 경우 다음을 사용합니다. <img alt= "세그먼트 편집" src="./assets/edit-segment.svg" width="25"> 선택한 세그먼트를 편집하는 아이콘 또는  <img alt= "새 세그먼트 만들기" src="./assets/create-new-segment.svg" width="25"> 새 세그먼트를 만드는 아이콘입니다. 자세한 내용은 다음 지침을 참조하십시오. [새 세그먼트 만들기](work-with-segments.md#create-new-segment) 또는 [세그먼트 편집](work-with-segments.md#edit-segment).

>[!IMPORTANT]
>
>**세그먼트 유형** 명명된 **[!UICONTROL Fixed number of accounts]** 는 현재 기본적으로 선택되어 있습니다. 선택할 옵션 **[!UICONTROL Variable number of accounts]** 향후 릴리스에서 제공될 예정입니다.

선택 **세부기간 및 시간 간격** 특정 기간 동안의 작업을 모니터링합니다. 에 대해 자세히 알아보기 [세부기간 및 시간 간격을 선택하는 방법](/help/accountiq/segments-timeinterval.md#granularity-timeinterval).

### 작업 {#action}

이 섹션에서 **작업** 드롭다운 메뉴에서 선택한 세그먼트에 대해 수행할 작업을 선택합니다.

![작업 유형 선택](assets/apply-actions.png)

*작업 유형 선택*

다음 두 가지 옵션을 사용할 수 있습니다.

* 선택 **CM 정책** 계정 IQ와 통합된 동시성 모니터링 시스템용입니다.

* 선택 **외부 작업** 계정 IQ 시스템과 통합되지 않고 계정 IQ 외부의 워크플로우를 만들고 처리합니다.

>[!NOTE]
>
>외부 작업이 항상 암호 공유와 직접 관련되지는 않을 수 있지만, 새 시즌 시작과 같이 여전히 영향을 줄 수 있습니다.

### 예약 {#schedule}

이 섹션에서 **시작일** 및 **종료일** 을 클릭하여 작업에 대한 활성화를 설정할 수 있습니다.

>[!IMPORTANT]
>
>현재, 기본 활성화 **시작일** 및 **종료일** 이(가) (으)로 설정됩니다. **날짜**. 선택할 옵션 **조건이 충족되는 경우** 및 **수동** 향후 릴리스에서 제공될 예정입니다.

>[!NOTE]
>
>시작 날짜와 종료 날짜가 평가용으로 선택한 세부 기간과 일치하는지 확인합니다. **4단계**.

* 주별로 집계된 세부 기간을 선택한 경우 주 단위 시작 및 종료 날짜를 선택합니다(예: 10주).
* 월별로 집계된 세부 기간을 선택한 경우 시작 날짜와 종료 날짜를 월 단위로 선택합니다.

![날짜 선택기에서 시작 날짜 및 종료 날짜 선택](assets/add-schedule.png)

*날짜 선택기에서 시작 날짜 및 종료 날짜 선택*

**A.** 시작 날짜 선택기 **B.** 종료일 선택

>[!NOTE]
>
>다음 **시작일** 은(는) 평가 기간과 현재 날짜보다 이후여야 하지만 **종료일** 이후 기간에 작업을 예약하고 실행하려면 시작 날짜 및 현재 날짜보다 이후여야 합니다.

선택 **저장 작업** 의 맨 위에 **작업** 새 작업을 처리하는 페이지입니다.

## 작업 중지 {#stop-operation}

현재 진행 중인 작업만 중지할 수 있습니다. **실행 중** 상태. 기존 작업을 중지하려면 다음 단계를 수행합니다.

1. 다음 위치로 이동 **작업** 아래의 탭 **작업** 계정 IQ 애플리케이션의 왼쪽 탐색.
1. 선택 **옵션** 중지할 작업의 메뉴.

   ![작업을 중지하려면 옵션 메뉴 선택](assets/stop-operation.png)

   *옵션 메뉴를 선택하여 작업을 중지합니다.*

1. 선택 **중지**.



