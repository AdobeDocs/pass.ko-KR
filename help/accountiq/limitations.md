---
title: 제한 사항 및 알려진 문제
description: 제품의 알려진 문제.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 2ced89dc1f77d2c090b599c40e778f3054f1a8dd
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# 알려진 문제 및 제한 사항 {#known-issues}

Adobe은 오퍼링을 통해 강력한 기능과 원활한 사용자 경험을 제공하기 위해 노력하고 있습니다. 계정 IQ의 현재 버전(버전 1.0)에서는 높은 신뢰도로 스트리밍 공급자에게 사용 및 구독 공유 분석을 제공합니다. 그러나 다음 제한 사항은 향후 릴리스 버전에서 해결될 예정입니다.

* 대시보드 또는 보고서 페이지에서 집단을 정의할 때 현재 다음과 같은 지표를 추가할 옵션이 없습니다. **장치 수** 세그먼트를 세분화합니다. 이 기능은 향후 버전에서 사용할 수 있습니다.

* 계정 IQ는 개별 계정에 대한 공유 점수를 추정할 때 기업이 상당한 자신감을 가지고 공유 작업을 수행할 수 있도록 보수적인 접근 방식을 취합니다. 그러나 이 접근 방식은 여러 계정에서 집계할 때 총 공유 양을 과소 평가하는 경향이 있습니다.

* 다음 [전체 공유 점수](/help/accountiq/dashboard.md#overall-sharing-score) 현재에 있는 요소만 해당 [공유 수준](/help/accountiq/dashboard.md#sharing-level) 및 [공유 계정의 사용](/help/accountiq/dashboard.md#usage-from-shared-accounts). 이후 버전은 추가 지표를 반영합니다.

* 대시보드 또는 보고서 페이지에서 집단을 정의할 때 MVPD 및 채널에 대한 선택기에는 지금으로서는 검색 메커니즘이 없습니다.

* 대시보드 또는 보고서 페이지에서 집단을 정의할 때 최대 10개의 MVPD 및 프로그래머(또는 개별 채널)만 선택한다는 제한이 있습니다.

* 계정 통계를 내보내는 옵션은 현재 1000개의 계정을 내보내는 것으로 제한됩니다.

* 선택할 옵션 [세그먼트 유형](#segment-type) 작업을 정의할 때 다음으로 제한 **고정 계정 수**. 다음 **가변 계정 수** 옵션은 향후 버전에서 사용할 수 있습니다.

* 왼쪽 탐색의 벤치마킹, 탐지 모델, 세그먼트, 스냅샷 및 규칙 섹션이 현재 비활성화되어 있으며 향후 버전에서 사용할 수 있습니다.

* 생성 시 [작업](/help/accountiq/operation-affecting-user-segment.md), 다음의 두 종류만 식별할 수 있습니다 [작업](/help/accountiq/operation-affecting-user-segment.md) 현재 — 동시 모니터링 규칙 및 외부 작업

* 현재 작업은 및 [예약됨](/help/accountiq/operation-affecting-user-segment.md#action). 이후 버전에서는 일시 중지, 재개 및 전체 관리를 수행할 수 있습니다.

* 사용되는 데이터 세트가 더 제한적이기 때문에 격리 모드는 실제로 공유 양을 반영하지 않습니다. 따라서 격리 모드의 MVPD는 다른 MVPD와 비교할 수 없습니다. <!--do we need to separate out this limitation, which is from a different persona i.e. only for Programmer persona?-->

* 새 항목을 정의하는 경우 [세그먼트](/help/accountiq/segments-timeframe.md) 작업에 대해 지표를 추가할 수 있습니다. 그러나 저장된 세그먼트를 선택한 경우 더 많은 지표를 추가하여 세그먼트를 세분화할 수 없습니다.

* 세부 기간 및 기간 선택기는 1주 또는 1개월로 제한됩니다. 즉, 데이터를 단일 주 또는 단일 달에만 평가할 수 있습니다.

* 사전 정의된 간격은 현재 세부 기간 및 시간대 선택기에서 비활성화되며 이후 버전에서 사용할 수 있습니다.
