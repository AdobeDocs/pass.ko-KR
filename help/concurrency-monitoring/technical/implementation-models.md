---
title: 구현 모델
description: 구현 모델
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---

# 구현 모델 {#imp-models}

## 서버측 정책 {#ss-policies}

이 모델은 CM을 정책 결정 지점으로 사용하여 서비스에 대한 액세스 결정을 위임합니다.

클라이언트는 적용된 정책에 대해 가정하지 않아야 하므로, 구현은 하트비트 응답에서 재생하는 동안 세션 초기화와 정기적으로 결정 사항을 확인해야 합니다.
