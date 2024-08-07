---
title: Adobe Pass Concurrency Monitoring 2.5.0 릴리스 노트
description: Adobe Pass Concurrency Monitoring 2.5.0 릴리스 노트
exl-id: da392b18-a2aa-4f51-a75f-2c5b65b2b073
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# Adobe Pass Concurrency Monitoring 2.5.0 릴리스 노트 {#cm-250}

이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.

릴리스 날짜: 2016/04/21

## 새로운 기능 {#new-features}

이제 동시 모니터링 API v2.0을 프로덕션에서 사용할 수 있습니다.

V2 버전은 하트비트 및 쿼리 호출을 통합하고 이러한 API를 동시에 사용할 때 구현을 크게 간소화합니다.



### 세션 라이프사이클 {#session-lifecycle}

* 세션 생성, Keep-Alive 및 종료를 위한 쓰기 전용 API(즉, 더 이상의 쿼리가 없는 경우 세션 초기화 및 하트비트 호출 시 결정이 반환됩니다.)
* 이제 하트비트 간격은 세션 초기화 및 keep-alive 호출 모두에 설정된 만료 헤더를 통해 서버에 의해 제어됩니다. 이렇게 하면 앱에서 하트비트 간격과 하나의 덜 코딩된 값을 서버측에서 구성할 수 있습니다.
* 행복 경로(사용자와 앱 모두의 준수 동작)는 2XX 상태 코드와 함께 &quot;포장&quot;됩니다

### 오류 처리 {#error-handling}

* 거부 결정, 메타데이터 누락 또는 애플리케이션 오동작은 4XX 응답(충돌, 잘못된 요청, 승인되지 않음, 제거됨 등)으로 플래그가 지정됩니다.

* 의미가 있을 때마다 응답은 다음을 포함합니다.

   * 연관된 권고 사항 - 사용자에게 프롬프트가 표시되는 실패에 대한 자세한 설명입니다.

   * 의무 — 애플리케이션이 수행해야 하는 필수 작업(예: 메타데이터 새로 고침, Adobe Pass에서 로그아웃).

### 메타데이터 {#metadata}

* 사용자 지정 메타데이터 대신 표준 - API를 사용하면 잘못된 메타데이터가 전송되는지 또는 규칙에 필요한 메타데이터가 누락되었는지 식별할 수 있습니다.
* 각 애플리케이션에 대한 필수 속성은 이제 API 호출에서 제공되며 클라이언트가 캐시해야 합니다.
메모

>[!NOTE]
>
>V1 버전은 계속 사용할 수 있습니다. 고객이 하트비트 호출 외부의 쿼리를 사용해야 하는 경우 V1 버전을 사용할 수 있습니다.




### 버그 수정 {#bug-fixes}

해당 사항 없음

### 알려진 문제 {#known-issues}

해당 사항 없음
