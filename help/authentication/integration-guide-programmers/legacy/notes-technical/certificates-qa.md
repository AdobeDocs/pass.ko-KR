---
title: 인증서 Q&A
description: 인증서 Q&A
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# (기존) 인증서 Q&amp;A {#certificates-q}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

</br>

**Q1:** iOS 및 Android에서 인증서를 등록할 수 있습니까?

**A:** iOS 및 Android에 대한 인증서는 현재 구성에서 동일합니다. 기본 인증서는 두 플랫폼 모두에 사용됩니다.

</br>

**Q2:** 프로덕션 및 스테이징 환경에서 동일한 iOS 인증서를 기본 및 백업 인증서로 사용할 수 있습니까? 만약 추천하지 않는다면 설명을 해주실 수 있나요?

**A:** 기본 및 백업 인증서와 동일한 인증서를 구성할 필요가 없습니다. 기본 인증서의 개념이 기본 및 백업 인증서이므로 기본 인증서가 만료되거나 해지될 경우 프로그래머를 위해 여러 인증서를 구성할 수 있습니다. 백업 인증서가 있으면 프로그래머가 릴리스 환경에 영향을 주지 않고 기본 인증서를 변경할 수 있습니다. 하지만 운영 프로필과 스테이징 프로필 모두에 동일한 기본 및 백업 인증서 세트를 사용할 수 있습니다.

</br>

**Q3:** 새로운 유연한 TempPass를 사용하는 웹 페이지에 필요한 새 인증서입니까?

**A:** 인증서(및 모든 인증서)가 미디어 회사 및 프로그래머 수준에서 구성되었습니다. FlexibleTempPass는 MVPD이므로 이에 대한 인증서를 구성할 필요가 없습니다. 따라서 기존 Programmer를 Flexible TempPass와 통합하는 경우 Programmer/Media Company 수준에서 이미 구성된 인증서가 사용됩니다.
