---
title: Adobe Pass Concurrency Monitoring 2.3.2 릴리스 노트
description: Adobe Pass Concurrency Monitoring 2.3.2 릴리스 노트
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---


# Adobe Pass Concurrency Monitoring 2.3.2 릴리스 노트 {#cm-232}

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

릴리스 날짜: 2015/12/11

## 새로운 기능 및 개선 사항 {#new-features}

* 사용 보고서에서 사용할 수 있는 새 분류입니다. 새 분류는 동시 모니터링과 통합된 애플리케이션이 사용자 지정 메타데이터를 전송하는 경우 사용할 수 있습니다.
   * application - 호출 URL에 보고된 애플리케이션 ID
   * mvpd - 호출 URL에 보고된 MVPD
   * 채널 - 사용자 지정 메타데이터 채널
   * platform - 사용자 지정 메타데이터 applicationPlatform
* 와 관련된 새 지표 **스트림 기간** 사용량 보고서에서 사용할 수 있습니다. 새 지표를 사용하여 스트림 기간의 히스토그램을 만들 수 있습니다. 현재 다음 간격(분)을 사용할 수 있습니다.
   * duration_0-15
   * duration_15-30
   * duration_30-60
   * duration_60-120
   * duration_over-120

## 버그 수정 {#bug-fixes}

해당 사항 없음

## 알려진 문제 {#known-issues}

해당 사항 없음
