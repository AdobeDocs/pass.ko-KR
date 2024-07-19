---
title: 제한 사항
description: Account IQ의 프로그래머를 위한 제한 사항 및 격리 모드 MVPD에 대해 알아봅니다.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# 제한 사항 {#limitations}

Account IQ의 D2C 및 TV Everywhere 버전은 스트리밍 공급자에게 사용 및 구독 공유 분석을 제공합니다. 그러나 현재 버전 1.3에는 몇 가지 제한 사항이 있으며 이는 향후 릴리스에서 해결될 예정입니다.

* [전체 공유 점수](/help/accountiq/data-panels.md#overall-sharing-score)에는 현재 [공유 수준](/help/accountiq/data-panels.md#sharing-level) 및 [공유 계정의 사용량](/help/accountiq/data-panels.md#usage-from-shared-accounts)이 포함되어 있습니다. 향후 릴리스에는 더 많은 지표가 포함됩니다.

* 대시보드의 세그먼트나 사용 패턴을 정의할 때 [세그먼트의 비디오 범주](/help/accountiq/data-panels.md#video-categories-segment), [채널 및 MVPD별 점수 공유](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds), [비디오 범주에 대한 사용 패턴 분포](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) 보고서에는 최대 20개의 [비디오 범주](product-concepts.md#video-category-def)에 대한 데이터만 표시됩니다. 20개 이상의 비디오 카테고리를 포함하는 세그먼트에는 이러한 보고서에 데이터가 표시되지 않습니다.

* 현재, 계정 통계를 내보내는 옵션은 1000개의 계정을 내보내는 것으로 제한됩니다.

* 작업을 정의할 때 [세그먼트 유형](/help/accountiq/operations.md#segment)을(를) 선택하는 옵션은 **계정 수가 고정됨**(으)로 제한됩니다. **계정의 변수 개수** 옵션은 향후 릴리스에서 사용할 수 있습니다.

* 왼쪽 탐색의 **벤치마킹**, **검색 모델**, **작업** 및 **설정** 섹션이 현재 비활성화되어 있으며 향후 릴리스에서 사용할 수 있습니다.

* 작업을 만들 때는 동시성 모니터링 규칙과 외부 작업, 두 종류의 [작업](/help/accountiq/operations.md#action)만 적용할 수 있습니다.

* 현재 [만들기](/help/accountiq/operations.md#create-new-operation) 및 [일정](/help/accountiq/operations.md#schedule) 작업만 수행할 수 있습니다. 향후 릴리스를 통해 일시 중단, 재개 및 전체 관리를 수행할 수 있습니다.

* 세부기간 및 시간 간격을 선택할 때는 한 번에 단일 주 또는 달의 데이터만 분석할 수 있습니다.

* 격리 모드 MVPD를 다른 MVPD와 함께 세그먼트 정의에 추가할 수 없습니다. 일부 MVPD는 여러 프로그래머 채널에서 가입자를 고유하게 식별하지 못합니다. 따라서 이러한 MVPD는 TV Everywhere 프로그래머에 대해 개별적으로 처리됩니다.



