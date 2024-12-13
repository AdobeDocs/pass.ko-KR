---
title: Amazon fireTV SSO - 프로그래머 시작 안내서
description: Amazon fireTV SSO - 프로그래머 시작 안내서
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 0%

---

# (레거시) Amazon fireTV SSO - 프로그래머 시작 안내서 {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

</br>

## 소개 {#intro}

이 문서에서는 fireTV 응용 프로그램에서 새 **Adobe Pass 인증의 fireTV SDK**&#x200B;을(를) 통합하는 데 필요한 정보를 설명합니다. 이 새로운 SDK은 Amazon의 fireTV 플랫폼에서 OS 수준 통합을 활용하므로 **단일 사인온** 지원을 제공합니다. SSO(Single Sign On)를 활용하려면 Clientless API에서 새로운 fireTV SDK으로 애플리케이션을 마이그레이션하기 위해 옆에서 약간의 노력이 필요합니다. 인증 흐름에는 아래에 자세히 설명되어 있는 몇 가지 변경 사항이 있습니다.

## 높은 수준의 아키텍처 및 OS 수준의 통합 {#high}

Amazon fireTV 플랫폼에서 TV Everywhere 응용 프로그램 간 단일 사인온을 달성하고 이 플랫폼에서 전반적인 환경을 개선하기 위해 fireTV OS 수준에서 핵심 SDK을 통합하기로 결정했습니다. 프로그래머는 Adobe에서 제공하는 스텁 라이브러리에 대해 컴파일해야 할 것이다. 실제 기능은 Amazon의 fireTV OS에 있는 Adobe 라이브러리에서 제공됩니다.

Amazon이 OS 레벨에서 라이브러리를 통합하는 fireTV 시뮬레이터를 제공할 때까지 실제 fireTV 디바이스를 사용해야 개발이 가능합니다.

## 이점 {#bene}

* 모든 통합 MVPD가 있는 Amazon fireTV 플랫폼의 모든 Adobe 지원 TV Everywhere 응용 프로그램 간 단일 사인온
* 지원되는 MVPD와 함께 HBA를 활용할 수 있습니다.
* 새 SDK 버전이 릴리스될 때마다 애플리케이션을 업데이트할 필요 없이 최신 fireTV SDK을 사용할 수 있습니다.
* 모든 TVE 앱은 AccessEnabler 라이브러리의 로컬 복사본을 필요 없이 공유 시스템 라이브러리를 사용할 수 있습니다. 또한 모든 애플리케이션이 동일한 SDK 버전을 사용하고 있는지 확인합니다.
* 단일 화면 인증 - 등록 코드 및 보조 화면 워크플로가 필요 없음.

## Clientless API 기반 앱에서 fireTV SDK 기반 앱으로 마이그레이션 {#migra1}

Clientless API에서 fireTV SDK으로 마이그레이션하려면 Clientless API와 관련된 코드베이스를 제거하고 새 fireTV SDK을 통합해야 합니다.

Clientless API 기반 앱과 비교하여 새로운 fireTV SDK을 사용하면 인증이 첫 번째 화면으로 이동하므로 더 이상 두 번째 화면 인증이 필요하지 않습니다.

이를 위해 프로그래머는 사용자가 fireTV 장치에서 바로 TV 공급자를 선택할 수 있도록 앱에 MVPD 선택기를 추가해야 합니다. MVPD을 선택하면 fireTV 장치에서 MVPD 로그인 페이지가 표시됩니다.

fireTV의 일반, HBA 및 SSO 시나리오를 설명하는 사용자 흐름의 와이어프레임은 [Amazon Fire TV - MVVPD 로그인 사용자 흐름](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/)에서 찾을 수 있습니다.

## Android SDK 기반 앱에서 fireTV SDK 기반 앱으로 마이그레이션 {#migra2}

이 새로운 fireTV SDK은 기존의 Android SDK과 매우 유사하며, **Android SDK 통합**&#x200B;에 대한 현재 설명서와 유사합니다. <!--http://tve.helpdocsonline.com/android-technical-overview-->fireTV SDK 문서를 준비할 때까지 사용할 수 있습니다. Android SDK을 사용하는 Android 애플리케이션이 이미 있다면 fireTV 애플리케이션에 fireTV SDK을 통합하는 것은 간단해야 합니다.

기존 Android SDK과 비교하여 fireTV SDK에서는 MVPD 로그인 페이지를 관리/제시하고 AuthN 토큰을 검색하는 작업이 AccessEnabler 라이브러리에서 내부적으로 수행되므로 인증 프로세스를 보다 간편하게 개발할 수 있습니다.

## FAQ {#faq}

1. **SSO**&#x200B;이(가) 어떻게 작동합니까?

   * SSO는 동일한 Amazon fireTV 장치에서 새로운 fireTV SDK을 사용하는 Adobe Pass 인증에서 제공하는 모든 프로그래머 애플리케이션에서 작동합니다
   * Clientless REST API에 구현된 프로그래머 앱과 fireTV SDK **에 구현된 앱 간의 SSO는 지원되지 않습니다**

1. FireTV SSO의 MVPD 적용 범위는 무엇입니까?

   * Adobe Pass 인증에 의해 통합된 **모든 MVPD**&#x200B;은(는) fireTV SDK에서 기술적으로 SSO가 지원됩니다.

1. 새로운 SDK을 사용하는 것 외에, 프로그래머가 알아야 할 **워크플로 변경 사항**&#x200B;은 무엇입니까?

   * 프로그래머는 fireTV 플랫폼용 MVPD 선택기를 구현해야 합니다.

1. 인증 **TTL**&#x200B;에 변경 사항이 있습니까?

   * 인증 TTL과 관련된 비헤이비어는 변경되지 않습니다.
   * 첫 번째 유효한 인증 토큰은 SSO 수행에 사용되며, 이 경우 SSO를 통해 인증될 다른 모든 애플리케이션은 만료될 때까지 동일한 TTL을 사용합니다. 따라서 한 애플리케이션에서 다른 애플리케이션으로 이동할 때 두 번째 애플리케이션은 인증하는 첫 번째 애플리케이션의 TTL을 공유합니다.

1. **저하 API**&#x200B;는 어떻게 작동합니까?

   * 성능 저하 API에 필요한 변경 사항이 없습니다. 사용자 경험은 Android 장치와 동일합니다.

1. **TempPass** 흐름은 어떻게 영향을 받습니까?

   * TempPass 흐름은 단일 화면이며 다른 모든 기본 디바이스에서와 같이 동작합니다.

1. 다른 Adobe 기능이 이전처럼 작동합니까?

   * 모든 Adobe Pass 인증 기능은 Android 장치에서처럼 fireTV에서 작동합니다.
