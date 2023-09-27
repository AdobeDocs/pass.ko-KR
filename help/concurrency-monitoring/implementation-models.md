---
title: 구현 모델
description: 구현 모델
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---


# 구현 모델 {#imp-models}

## 서버측 정책 {#ss-policies}

이 모델은 CM을 정책 결정 지점으로 사용하여 서비스에 대한 액세스 결정을 위임합니다.

클라이언트는 적용된 정책에 대해 가정하지 않아야 하므로, 구현은 하트비트 응답에서 재생하는 동안 세션 초기화와 정기적으로 결정 사항을 확인해야 합니다.
