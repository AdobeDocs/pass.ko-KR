---
title: Adobe Pass Concurrency Monitoring 2.6.0 릴리스 노트
description: Adobe Pass Concurrency Monitoring 2.6.0 릴리스 노트
exl-id: f24980e3-ffe8-4b5e-8adc-ae443baed40f
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---

# Adobe Pass Concurrency Monitoring 2.6.0 릴리스 노트 {#cm-260}


이 페이지에서는 이 릴리스의 새로운 기능, 변경 사항 및 알려진 문제에 대해 설명합니다.



## 릴리스 날짜: 2016/11/10



## 새로운 기능

이 릴리스에는 기존 스트림을 종료하여 현재 스트림이 시작될 수 있도록 하는 기능이 추가됩니다(킬 스트림이라고도 함).



**원격 종료**

* 409 충돌 응답에서 조언의 &quot;충돌&quot; 필드 내에 나열된 각 세션은 terminationCode 특성을 전달합니다.
* 충돌 세션 목록을 입력하라는 메시지가 표시되고 종료할 세션을 선택할 수 있습니다
* 세션 초기화 시도 내에 X-Terminate 요청 헤더(선택한 종료 코드를 값으로 사용)를 전달해야만 원격 세션을 종료할 수 있습니다.
* 현재 세션을 종료한 세션을 표시하기 위해 410 Gone 응답에 대해 새로운 유형의 &quot;권고&quot;가 정의되었습니다.


자세한 내용은 업데이트된 설명서 를 참조하십시오.



>[!NOTE]
>
>활성 세션을 나열하는 데 사용된 세션 정의가 애플리케이션 이름 및 디바이스 이름(애플리케이션 ID 대신)을 포함하도록 업데이트되었습니다.




## 버그 수정 {#bug-fixes}

서버 응답에서 중복 헤더를 제거했습니다(이 수정 사항에는 CORS 헤더와 날짜 헤더가 모두 포함됨).




## 알려진 문제 {#known-issues}

해당 사항 없음
