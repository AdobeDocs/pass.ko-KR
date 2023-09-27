---
title: 콘솔 앱 로그를 사용하여 AccessEnabler iOS/tvOS SDK 디버깅
description: 콘솔 앱 로그를 사용하여 AccessEnabler iOS/tvOS SDK 디버깅
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 0%

---

# 콘솔 앱 로그를 사용하여 AccessEnabler iOS/tvOS SDK 디버깅 {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.


## 개요

이 문서의 범위는 콘솔 앱 로그를 사용하여 AccessEnabler 프레임워크를 디버깅하는 데 유용한 몇 가지 세부 사항과 함께 AccessEnabler의 iOS/tvOS SDK 로깅 메커니즘의 발전 과정을 캡처하고 제시하는 것입니다.

## 로깅 메커니즘 상태

AccessEnabler iOS/tvOS 로깅 메커니즘 목적은 AccessEnabler 프레임워크를 사용하는 응용 프로그램에서 발생할 수 있는 문제를 해결하는 데 유용한 메시지를 내보내는 것입니다.

### AccessEnabler iOS/tvOS 3.5.0 이상

AccessEnabler iOS/tvOS 3.5.0 버전부터 로깅 메커니즘은 다음과 같은 변경 사항이 도입되었습니다.

* AccessEnabler 프레임워크는 Apple의 권장 사항을 사용합니다 [OSLog](https://developer.apple.com/documentation/os/oslog) 구현.

* AccessEnabler 프레임워크는 하위 시스템을 기반으로 콘솔 앱 로그를 필터링하는 기능을 도입했습니다. **com.adobe.pass.AccessEnabler**. SDK에서 내보내는 모든 메시지는 com.adobe.pass.AccessEnabler의 일부입니다.

* AccessEnabler 프레임워크에서는 Any(접두사)에 따라 콘솔 앱 로그를 필터링할 수 있습니다. **[액세스 활성]**. SDK에서 내보내는 모든 메시지 앞에는 가 붙습니다. [액세스 활성].

* AccessEnabler 프레임워크는 범주를 기준으로 콘솔 앱 로그를 필터링하는 기능을 도입했습니다. **디버그**, **오류** 위의 두 가지 기준 중 하나(하위 시스템 또는 임의(접두어))와 함께 사용할 수 있습니다.

## 콘솔 앱 로그를 사용하여 디버깅

조사된 문제에 따라 AccessEnabler 프레임워크에서 제공하는 로깅 메시지를 포함하거나 제외할 수 있으므로 조사 도중 및 콘솔 앱 로그를 사용할 때 도움이 되는 몇 가지 유용한 세부 정보를 아래에서 확인할 수 있습니다.


### AccessEnabler iOS/tvOS 3.5.0 이상

#### 포함 {#including}

먼저 AccessEnabler 프레임워크에서 제공하는 로깅 메시지를 확인할 수 있습니다. **필수** 아래 이미지에 표시된 대로 콘솔 앱의 작업 섹션에서 &quot;정보 메시지 포함&quot; 및 &quot;디버그 메시지 포함&quot;을 선택합니다.

![](assets/include-info-debug-msg.png)


AccessEnabler iOS/tvOS SDK 및 **참조** AccessEnabler 프레임워크 로그를 사용하여 다음과 같은 작업을 수행할 수 있습니다.

* 다음을 사용하여 콘솔 앱에서 검색 **하위 시스템** 옵션은 아래 이미지와 같이 com.adobe.pass.AccessEnabler 값과 같습니다.

![](assets/subsys-console-app.png)

* 다음을 사용하여 콘솔 앱에서 검색 **임의** 다음을 포함하는 옵션
  [액세스 활성] 아래 이미지에서와 같은 값입니다.

![](assets/any-optn-console-app.png)

위의 두 가지 기준과 함께 **범주** 과 함께 사용되는 옵션 **하위 시스템** 또는 **Any (접두사)** 을 명시적으로 검색하려면 **디버그** 또는 **오류** AccessEnabler iOS/tvOS SDK에서 보내는 레벨 메시지입니다.

#### 제외

다른 구성 요소 및 의 기능을 더 잘 디버깅하려면 **제외** AccessEnabler 프레임워크 로그를 사용하여 다음과 같은 작업을 수행할 수 있습니다.

* 다음을 사용하여 콘솔 앱에서 검색 **하위 시스템** com.adobe.pass.AccessEnabler 값과 같지 않은 옵션입니다.
* 다음을 사용하여 콘솔 앱에서 검색 **임의** 다음을 포함하지 않는 옵션 [액세스 활성] 값.

## 문제 보고

Adobe Pass 인증에 문제를 보고할 때 다음 제안을 고려하십시오.

* 재생 단계를 제공하십시오.
* 문제가 발생하는 OS 버전 및 장치 모델을 제공하십시오.
* 문제가 발생하는 AccessEnabler iOS/tvOS SDK 버전을 제공하십시오.
* 에 제시된 두 옵션 중 하나를 사용하여 모든 AccessEnabler iOS/tvOS SDK 로깅 메시지를 캡처하고 첨부하십시오. [포함](#including) 섹션.
