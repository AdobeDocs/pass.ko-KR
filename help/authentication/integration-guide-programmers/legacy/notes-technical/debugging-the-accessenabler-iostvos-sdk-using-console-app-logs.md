---
title: 콘솔 앱 로그를 사용하여 AccessEnabler iOS/tvOS SDK 디버깅
description: 콘솔 앱 로그를 사용하여 AccessEnabler iOS/tvOS SDK 디버깅
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---

# (레거시) 콘솔 앱 로그를 사용하여 AccessEnabler iOS/tvOS SDK 디버깅 {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 개요

이 문서의 범위는 콘솔 앱 로그를 사용하여 AccessEnabler 프레임워크를 디버깅하는 데 유용한 몇 가지 세부 사항과 함께 AccessEnabler의 iOS/tvOS SDK 로깅 메커니즘의 발전 과정을 캡처하고 제시하는 것입니다.

## 로깅 메커니즘 상태

AccessEnabler iOS/tvOS 로깅 메커니즘 목적은 AccessEnabler 프레임워크를 사용하는 응용 프로그램에서 발생할 수 있는 문제를 해결하는 데 유용한 메시지를 내보내는 것입니다.

### AccessEnabler iOS/tvOS 3.5.0 이상

AccessEnabler iOS/tvOS 3.5.0 버전부터 로깅 메커니즘은 다음과 같은 변경 사항이 도입되었습니다.

* AccessEnabler 프레임워크는 Apple에서 권장하는 [OSLog](https://developer.apple.com/documentation/os/oslog) 구현을 사용합니다.

* AccessEnabler 프레임워크는 하위 시스템을 기반으로 콘솔 앱 로그를 필터링하는 기능을 도입했습니다. **com.adobe.pass.AccessEnabler**. SDK에서 내보내는 모든 메시지는 com.adobe.pass.AccessEnabler의 일부입니다.

* AccessEnabler 프레임워크는 Any(접두사)를 기반으로 콘솔 앱 로그를 필터링하는 기능을 도입했습니다. **[AccessEnabler]**. SDK에서 내보내는 모든 메시지 앞에는 [AccessEnabler]이(가) 붙습니다.

* AccessEnabler 프레임워크는 위의 두 가지 기준인 하위 시스템 또는 모두(접두사)와 함께 범주: **debug**, **error**&#x200B;을(를) 기반으로 콘솔 앱 로그를 필터링하는 기능을 도입했습니다.

## 콘솔 앱 로그를 사용하여 디버깅

조사된 문제에 따라 AccessEnabler 프레임워크에서 제공하는 로깅 메시지를 포함하거나 제외할 수 있으므로 조사 도중 및 콘솔 앱 로그를 사용할 때 도움이 되는 몇 가지 유용한 세부 정보를 아래에서 확인할 수 있습니다.


### AccessEnabler iOS/tvOS 3.5.0 이상

#### 포함 {#including}

먼저 **해야**&#x200B;하는 AccessEnabler 프레임워크에서 보내는 로깅 메시지를 보려면 아래 이미지에 표시된 대로 콘솔 앱의 작업 섹션에서 &quot;정보 메시지 포함&quot; 및 &quot;디버그 메시지 포함&quot;을 선택하십시오.

![](../../../assets/include-info-debug-msg.png)


AccessEnabler iOS/tvOS SDK의 기능을 디버깅하고 AccessEnabler 프레임워크 로그를 **참조**&#x200B;하려면 다음을 수행할 수 있습니다.

* 아래 이미지와 같은 com.adobe.pass.AccessEnabler 값과 같은 **하위 시스템** 옵션을 사용하여 콘솔 앱에서 검색합니다.

![](../../../assets/subsys-console-app.png)

* 다음을 포함하는 **모두** 옵션을 사용하여 콘솔 앱에서 검색
  아래 이미지와 같은 [AccessEnabler] 값입니다.

![](../../../assets/any-optn-console-app.png)

위의 두 가지 기준과 함께 **하위 시스템** 또는 **모두(접두사)**&#x200B;와 함께 **범주** 옵션을 사용하여 AccessEnabler iOS/tvOS SDK에서 내보내는 **디버그** 또는 **오류** 수준 메시지를 명시적으로 검색할 수도 있습니다.

#### 제외

다른 구성 요소의 기능을 보다 잘 디버깅하고 AccessEnabler 프레임워크 로그를 **제외**&#x200B;하려면 다음을 수행할 수 있습니다.

* com.adobe.pass.AccessEnabler 값과 같지 않은 **하위 시스템** 옵션을 사용하여 콘솔 앱에서 검색합니다.
* **AccessEnabler** 값을 포함하지 않는 [Any] 옵션을 사용하여 콘솔 앱에서 검색합니다.

## 문제 보고

Adobe Pass 인증에 문제를 보고할 때 다음 제안을 고려하십시오.

* 재생 단계를 제공하십시오.
* 문제가 발생하는 OS 버전 및 장치 모델을 제공하십시오.
* 문제가 발생하는 AccessEnabler iOS/tvOS SDK 버전을 제공하십시오.
* [포함](#including) 섹션에 있는 두 옵션 중 하나를 사용하여 모든 AccessEnabler iOS/tvOS SDK 로깅 메시지를 캡처하고 첨부해 보십시오.
