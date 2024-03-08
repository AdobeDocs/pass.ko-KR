---
title: Adobe Pass Authentication Access Enabler 사용 시 iOS에서 SSO
description: Adobe Pass Authentication Access Enabler 사용 시 iOS에서 SSO
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '1121'
ht-degree: 0%

---

# Adobe Pass Authentication Access Enabler 사용 시 iOS에서 SSO {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>

## 개요

Adobe Pass 인증 기반 앱 간의 SSO(Single Sign-On)는 기본 운영 체제에 따라 다른 방식으로 작동합니다.

이 문서의 주소 **iOS의 SSO**, Adobe Pass 인증 사용 시 **액세스 Enabler**.

**액세스 Enabler** **1.10** 는 Adobe Pass 인증 iOS 기본 SDK의 최신 버전입니다. Adobe은 이전 버전을 유지하지 않고 이 버전으로 이동할 것을 강력히 권장합니다. 이전 버전의 Access Enabler를 사용하는 경우 최신 버전을 다운로드할 수 있습니다 [여기](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

iOS에서의 SSO는 다음 조건에 의해 결정됩니다.

- 앱은 동일한 을 사용해야 합니다. **토큰 저장소** (Access Enabler에서 만든 사용자 정의 페이스트보드 형식).
- 앱에서 동일하게 생성해야 함 **장치 ID** (Access Enabler는 OS 버전에 따라 MAC 주소 또는 IDFV를 기반으로 장치 ID를 계산합니다.)

## 비헤이비어

SSO 동작은 다음과 같습니다.

- **iOS 6 이하**: SSO는 동일한 팀이나 다른 팀에서 개발한 앱 간에 자동으로 작동합니다. 장치 ID는 MAC 주소(모든 앱에서 동일한 값이 생성됨)를 기반으로 계산되며 저장 영역은 모든 앱에 공통됩니다(사용자 지정 대지는 iOS 6 이하의 앱에서 공유할 수 있음).
   - **중요 사항:** iOS SDK 1.9.4 릴리스에는 [최소 iOS 배포 대상을 iOS 7로 늘렸습니다.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7 이상**: SSO는 다음 조건에서 작동합니다.

1. 앱은 동일한 Apple 배포 프로필 또는 동일한 팀에 속한 프로필을 사용하여 게시됩니다. 이는 앱이 iOS 7 이상에서 사용자 지정 게시판을 공유하는 유일한 방법입니다. 다른 모든 시나리오에서는 페이스트보드가 애플리케이션별로 샌드박스됩니다. 출처: [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html): \+\[`UIPasteboard pasteboardWithName:create:\`] 및 +\[`UIPasteboard pasteboardWithUniqueName`\] 이제 같은 애플리케이션 그룹에 있는 앱만 임시 보드에 액세스할 수 있도록 지정된 이름을 고유하게 지정합니다. 개발자가 이미 존재하고 동일한 앱 세트에 속해 있지 않은 이름의 임시 보드를 만들려고 하면 고유한 개인 임시 보드를 받게 됩니다. 이 기능은 시스템에서 제공한 대지보드, 일반 사항 및 찾기에 영향을 주지 않습니다.

1. 앱에는 동일한 번들 ID 접두사가 있습니다(마지막 구성 요소를 제외한 모든 구성 요소). 동일한 번들 ID 접두사를 공유하는 응용 프로그램만 동일한 IDFV를 계산합니다. 출처: [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor): IOS 7에서는 마지막 구성 요소를 제외한 번들의 모든 구성 요소가 공급업체 ID를 생성하는 데 사용됩니다. 번들 ID에 단일 구성 요소만 있는 경우 전체 번들 ID가 사용됩니다.

이제 다음 사항에 집중하겠습니다. **&#39;iOS 7 이상&#39;** 시나리오: 실제 사용자에게 가장 빈번하기 때문에:

두 조건(동일한 개발 팀의 프로필을 공유하고 공통 번들 식별자 접두사가 있음)은 모두 SSO의 필수 조건입니다.

다음은 가능한 조합과 그 결과 결과입니다.

- **동일한 팀의 프로필 및 동일한 번들 ID 접두사**: 앱은 동일한 임시 보드 스토리지를 공유하며 동일한 장치 ID(IDFV)를 갖습니다. 사용자는 (첫 번째로 사용한 앱에서) 한 번만 인증하면 되고 인증 상태는 다른 모든 앱에서 공유됩니다. 흐름 예:

1. 사용자가 앱 A(번들 ID 포함)를 엽니다. *com.x.y.AppA*) 및 가 인증되지 않음
1. 사용자가 앱 A에서 인증을 수행합니다.
1. 사용자가 앱 B(번들 ID 포함)를 엽니다. *com.x.y.AppB*)와 는에서 앱 A(2단계의 자격 데이터)의 자격 데이터를 공유하여 자동으로 인증됩니다
1. 사용자가 앱 A를 열고 아직 인증됨(2단계에서)



- **동일한 팀의 프로필이지만 번들 ID 접두사가 다릅니다.**: 앱은 동일한 임시 보드 스토리지를 공유하지만 서로 다른 장치 ID(IDFV)를 갖습니다. 사용자는 각 앱마다 한 번씩 인증해야 합니다. 흐름 예:

1. 사용자가 앱 A(번들 ID 포함)를 엽니다. *com.x.y.AppA*) 및 가 인증되지 않음
1. 사용자가 앱 A에서 인증을 수행합니다.
1. 사용자가 앱 B(번들 ID 포함)를 엽니다. *com.z.AppB*) 및 Access Enabler는 첫 번째 앱에서 가져온 토큰을 감지하지만(스토리지가 공유되기 때문에) 다른 디바이스 ID로 인해 SSO를 통해 토큰을 사용하려고 시도하지는 않습니다
1. 사용자가 앱 B에서 인증을 수행합니다.
1. 사용자가 앱 A를 열고 아직 인증됨(2단계에서)



- **다른 팀의 프로필(이 시나리오에서는 번들 ID가 관련이 없음)**: 앱에는 서로 다른 임시 보드 저장소가 있으며 앱 간에 SSO가 비활성화됩니다. 사용자는 각 앱마다 한 번씩 인증해야 하며 앱 간에 전환할 때 인증 세션은 계속 유지됩니다. 흐름 예:


1. 사용자가 앱 A를 열고 인증되지 않음
1. 사용자가 앱 A에서 인증을 수행합니다.
1. 사용자가 앱 B를 열고 인증되지 않음
1. 사용자가 앱 B에서 인증을 수행합니다.
1. 사용자가 앱 A를 열고 인증됨(2단계에서)
1. 사용자가 앱 B를 열고 인증됨(4단계에서)

**참고:** 위의 SSO 조건은 를 통해 애플리케이션을 설치할 때 적용됩니다. **Apple App Store**. 앱이 시뮬레이터에 배포되거나(앱 서명이 적용되지 않는 경우) Xcode로 설치되거나 Ad Hoc 프로필을 통해 배포되는 경우 다른 결과를 얻을 수 있습니다.

**중요 사항:** 메모(**AccessEnabler v1.8 관련 정보**): 위에서 설명한 두 번째 시나리오(동일한 팀의 프로필, 하지만 다른 번들 ID 접두사)는 의 사용자에게 매우 불쾌한 사용자 경험을 만듭니다. **AccessEnabler v1.8** 동일한 팀(미디어 회사)에서 개발한 애플리케이션 간에 동일한 미디어 회사의 앱 간에 전환하는 경우 사용자가 자동으로 로그아웃되므로, 애플리케이션 개발자는 번들 ID 및 배포 프로필을 결정할 때 주의해야 합니다. 이 경우의 정확한 시나리오는 아래에 나와 있습니다.

앱은 동일한 임시 보드 스토리지를 공유하지만 서로 다른 장치 ID(IDFV)를 갖습니다. 사용자는 각 앱에 대해 한 번씩 인증해야 합니다. **그러나 앱 간에 전환할 때 인증 세션이 지워집니다.**. 흐름 예:

1. 사용자가 앱 A(번들 ID 포함)를 엽니다. *com.x.y.AppA*) 및 가 인증되지 않음
1. 사용자가 앱 A에서 인증을 수행합니다.
1. 사용자가 앱 B(번들 ID 포함)를 엽니다. *com.z.AppB*) 및 앱 A에서 만든 자격 데이터는 Access Enabler(앱 B에서 현재 계산된 장치 ID와 앱 A에서 만든 자격 토큰에 저장된 장치 ID 간의 충돌을 감지하는 보안 메커니즘)에 의해 자동으로 지워집니다
1. 사용자가 앱 B에서 인증을 수행합니다.
1. 사용자가 앱 A를 열면 앱 B에서 만든 자격 데이터는 Access Enabler(앱 A에서 현재 계산된 장치 ID와 앱 B에서 만든 자격 토큰에 저장된 장치 ID 간의 충돌을 감지하는 보안 메커니즘)에 의해 자동으로 지워집니다
