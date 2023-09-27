---
title: Adobe 동시 모니터링 2.9 릴리스 노트
description: Adobe 동시 모니터링 2.9 릴리스 노트
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---


# Concurrency Monitoring 2.9 릴리스 정보 {#rn-cm29}

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

## 릴리스 날짜 {#release-date}

03/14/2019


## 릴리스 개요 {#release-overview}

* 이 버전부터 동시 사용 사용을 이해하기 위한 새 보고서 도입: 동시 사용 수준에 대한 히스토그램으로, 다음을 표시합니다.

* 각 세부 기간 간격 동안 각 동시 실행 수준에 도달한 사용자 수(즉, 2개의 동시 스트림, 3개의 동시 스트림 등을 보유한 사용자 수)
* 각 동시성 수준에 대한 총 기간(분)(이 값을 위의 수로 나누어 평균 값을 계산할 수 있음)
* 영향을 받는 사용자 및 집계된 사용자 경험의 관점에서 주어진 규칙의 영향을 추정하기 위해 사용자가 각 동시성 수준에 접한 총 횟수입니다. 자세한 내용은 [사용 보고서](/help/concurrency-monitoring/cm-usage-reports.md) 페이지를 가리키도록 업데이트하는 중입니다.

또한 SQL 삽입 보호를 개선하고 몇 가지 버그 수정을 추가했습니다.

## 알려진 문제 {#known-issues}

없음.
