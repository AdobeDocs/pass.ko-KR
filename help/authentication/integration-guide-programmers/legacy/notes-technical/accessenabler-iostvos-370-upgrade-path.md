---
title: AccessEnabler iOS/tvOS 3.7.0 업그레이드 경로
description: AccessEnabler iOS/tvOS 3.7.0 업그레이드 경로
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# (기존) AccessEnabler iOS/tvOS 3.7.0 업그레이드 경로 {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

</br>

[새 AccessEnabler 버전 3.7.0](/help/authentication/notes-releases/authn-rn-ios-tvos-370.md)의 키체인 스토리지 변경 사항이 AccessEnabler 버전 3.7.0보다 낮은 키체인 스토리지 구현과 호환되지 않습니다.

새로운 AccessEnabler 버전 3.7.0을 채택한 하나의 애플리케이션에 대한 업그레이드 경로는 이전 버전의 키체인 스토리지에서 모든 토큰을 마이그레이션하게 됩니다. 따라서 최종 사용자 **은(는) AccessEnabler 프레임워크 업데이트 프로세스 중에 인증/권한 부여 세션**&#x200B;이 손실되지 않아야 합니다.

## 알려진 제한 사항

아래 설명된 몇 가지 제한 사항이 구현자에 의해 발생할 수 있습니다.


1. AccessEnabler 버전 3.7.0을 사용하는 애플리케이션과 AccessEnabler 버전 3.7.0보다 낮은 버전을 사용하는 애플리케이션 간에는 동일한 공급업체에서 개발한 애플리케이션에서도 일반(Adobe) SSO가 작동하지 않습니다.

   >[!IMPORTANT]
   >
   >* 시스템 수준(Apple) SSO는 영향을 받지 않습니다!
   >
   >* 일반(Adobe) SSO는 동일한 공급업체에서 두 애플리케이션을 개발하고 AccessEnabler 버전이 3.7.0보다 낮은 경우 계속 작동합니다.
   >
   >* 일반(Adobe) SSO는 동일한 공급업체에서 두 애플리케이션을 개발하고 AccessEnabler 버전 3.7.0을 사용하는 경우에 작동합니다.


1. AccessEnabler 버전 3.7.0을 사용하는 애플리케이션 하나를 더 낮은 버전의 AccessEnabler로 다운그레이드하는 경우에는 새로 생성된 토큰이 마이그레이션되지 않습니다. 따라서 최종 사용자는 기다리지 않고 인증/인증 세션이 손실될 수 있습니다.

   >[!IMPORTANT]
   >
   >* 시스템 수준(Apple) SSO를 통해 인증된 최종 사용자는 영향을 받지 않습니다!
   >* AccessEnabler 버전 3.7.0을 사용하여 새 애플리케이션으로 업데이트하기 전에 이미 인증된 최종 사용자는 영향을 받지 않습니다.

1. AccessEnabler 버전 3.7.0을 사용하여 한 애플리케이션을 더 낮은 버전의 AccessEnabler로 다운그레이드하는 경우 삭제된 토큰은 인식되지 않습니다. 따라서 최종 사용자는 필요 없이 인증/권한 부여 세션이 있는 것을 경험할 수 있습니다.
