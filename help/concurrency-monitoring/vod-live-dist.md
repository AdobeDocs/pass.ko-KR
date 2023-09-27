---
title: 동시 모니터링에서 VOD와 라이브 콘텐츠를 구분하는 방법
description: 동시 모니터링에서 VOD와 라이브 콘텐츠를 구분하는 방법
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---


# 방법: 동시 모니터링에서 VOD와 라이브 컨텐츠 구분 {#dist-vod-live}

**Q:** 동시성 모니터링 서비스는 재생 중인 콘텐츠 유형(라이브 콘텐츠와 주문형 비디오)을 구분할 수 있습니까?



**A:** 동시성 모니터링은 라이브 콘텐츠와 VOD(Video on demand)를 직접 구별할 수 없습니다. 비디오 플레이어는 재생 중인 콘텐츠 유형을 알고 다음 작업 중에 이 정보를 전송해야 합니다. [세션 초기화 호출](/help/concurrency-monitoring/cm-api-overview.md#session-initial) (동시성 모니터링에 필요). 일반 워크플로우는 다음과 같습니다.

1. 동시성 모니터링 고객은 규칙을 구현할 메타데이터 집합을 정의합니다(예: content-type=live|vod, device-type=mobile|console|desktop).
1. 동시성 모니터링 팀은 원하는 정책을 구현합니다. 예:
   1. content-type=live, max streams=3, latest wins
   1. content-type=vod, max streams=1, latest wins

1. 최종 사용자가 콘텐츠를 재생할 때 비디오 플레이어는 정책의 일부로 설정된 메타데이터 필드에 대한 값을 보내야 합니다.

1. 정의된 정책 및 받은 값을 기반으로 동시성 모니터링 서비스가 결정(재생/중지)을 발행합니다.

1. 시스템이 작동하려면 비디오 플레이어에 의해 그 결정에 따라야 합니다.



## 관련 정보 {#related-info-vod-live-dist}

* [동시 모니터링 표준 메타데이터 속성](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [동시성 모니터링 API 개요](/help/concurrency-monitoring/cm-api-overview.md)
