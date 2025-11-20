---
title: API 엔드포인트
description: 동시성 모니터링 API의 전체 목록
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '54'
ht-degree: 3%

---


# API 엔드포인트

## 핵심 세션 관리

| 엔드포인트 | 방법 | 설명 |
|---------------------------------------|--------|---------------------------------------|
| `/sessions/{idp}/{subject}` | POST | 새 스트리밍 세션 만들기 |
| `/sessions/{idp}/{subject}/{session}` | POST | 세션을 유지하기 위해 하트비트 전송 |
| `/sessions/{idp}/{subject}/{session}` | DELETE | 세션 종료 |
| `/runningStreams/{idp}/{subject}` | GET | 주제에 대한 모든 활성 세션 가져오기 |

## 메타데이터 관리

| 엔드포인트 | 방법 | 설명 |
|-------------|--------|----------------------------------------------|
| `/metadata` | GET | 애플리케이션에 필요한 메타데이터 필드 가져오기 |
