---
title: 동시 모니터링에서 VOD 및 라이브 컨텐츠를 구분하는 방법
description: 동시 모니터링에서 VOD 및 라이브 컨텐츠를 구분하는 방법
exl-id: 51ba686a-7c1f-4403-9e8e-cd247bf9e345
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# 방법: 동시 모니터링에서 VOD 및 라이브 컨텐츠 구분 {#dist-vod-live}

**Q:** 동시성 모니터링 서비스에서 재생 중인 콘텐츠 유형(라이브 콘텐츠와 Video on demand)을 구분할 수 있습니까?



**A:** 동시성 모니터링에서 라이브 콘텐츠와 주문형 비디오(VOD)를 직접 구분할 수 없습니다. 비디오 플레이어가 재생 중인 콘텐츠 형식을 알고 있어야 하며 [세션 초기화 호출](/help/concurrency-monitoring/cm-api-overview.md#session-initial) 중에 이 정보를 보내야 합니다(동시 모니터링에 필요). 일반 워크플로우는 다음과 같습니다.

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
