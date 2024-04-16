---
title: Adobe 환경 이해
description: Adobe 환경 이해
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: c6afb9b080ffe36344d7a3d658450e9be767be61
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Adobe 환경 이해 {#understanding-the-adobe-environments}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

Adobe 환경을 설명하는 공식 설명서를 사용할 수 있습니다 [이전 단계에서 환경 설정 및 테스트](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md):

Adobe 환경은 다음과 같이 요약됩니다.

Adobe에는 두 가지 환경이 있습니다. **사전 검증** 및 **릴리스**.

* 사전 검증 환경에서는 릴리스할 새 빌드를 준비하고 있습니다.

* 현재 릴리스 빌드는 릴리스 환경에 있습니다.

각 환경에는 두 개의 프로필 이 있습니다. **스테이징** 및 **production**.

* 스테이징 프로필이 MVPD 스테이징 서버에 연결됩니다
* 프로덕션 프로필은 MVPD 프로덕션 프로필에 연결합니다.

두 개의 프로필이 있는 이유는 스테이징 프로필에서 새로운 파트너가 라이브로 진행될 수 있도록 준비하고 있으며 예정된 빌드(사전 검증) 또는 릴리스 1개(보다 안정적인)로 시스템을 테스트하려고 하기 때문입니다.

파트너가 새 버전을 테스트하려는 경우 수행해야 하는 몇 가지 추가 단계가 있습니다. 다음을 참조하십시오. [이전 단계에서 환경 설정 및 테스트](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

위의 단계에 따라 예정된 릴리스가 사전 검증 환경에서 테스트될 것임을 보장합니다.
