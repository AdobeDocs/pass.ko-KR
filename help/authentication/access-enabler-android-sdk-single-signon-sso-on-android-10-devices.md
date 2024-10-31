---
title: Android 10 앱에서 Enabler Android SDK SSO(Single Sign-On) 액세스
description: Android 10 앱에서 Enabler Android SDK SSO(Single Sign-On) 액세스
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 0%

---

# Android 10 앱에서 Enabler Android SDK SSO(Single Sign-On) 액세스 {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요

Adobe Pass 인증 기반 앱 간의 SSO(Single Sign-On)는 Access Enabler Android SDK를 통해 Android OS를 사용하는 디바이스에서 사용할 수 있습니다. Android 장치에서 SSO(Single Sign-On)를 제공하기 위해 Access Enabler Android SDK 버전 3.2.1(최신) 및 이전 버전은 Android 스토리지 구현에 저장된 공유 데이터베이스 파일을 사용하며 모든 Adobe Pass 인증 기반 앱에서 액세스할 수 있습니다.

그러나 최신 Android 10 릴리스의 Google에서는 &quot;사용자에게 파일에 대한 더 많은 제어 권한을 부여하고 파일이 복잡하지 않도록 제한하기 위해&quot; 일부 변경 사항이 생성되었습니다. Android 10(API 레벨 29) 이상을 타깃팅하는 앱에는 기본적으로 외부 저장 장치 또는 범위가 지정된 저장소에 대한 액세스 범위가 제공됩니다. 이러한 앱은 앱별 디렉터리 `\[...\]`만 볼 수 있습니다. 이러한 Android 10 저장소 변경 사항과 관련된 자세한 내용은 [Android의 데이터 및 파일 저장소 설명서](https://developer.android.com/training/data-storage/files/external-scoped)에 나와 있습니다.

이러한 변경 사항으로 인해 Access Enabler Android 버전 **3.2.1 SDK(최신)** 및 이전 버전에서 제공하는 SSO(Single Sign-On)가 다음 섹션에 설명된 대로 Android 10 디바이스에서 영향을 받을 수 있습니다.

## 비헤이비어

앱의 **[!UICONTROL target SDK level]** 또는 **android:requestLegacyExternalStorage** 매니페스트 특성 사용에 따라 Access Enabler Android 버전 3.2.1 SDK(최신)와 이전 버전에서 제공하는 SSO(Single Sign-On)가 현재 다음과 같이 작동합니다.

- 앱 대상 **Android 9(API 수준 28)** 또는 하위 **-\>** SSO(Single Sign-On) **이(가) 작동합니다**
- 앱 대상은 **Android 10** **(API 수준 29)**&#x200B;이며 앱의 매니페스트 파일 **-\>**&#x200B;에서 **requestLegacyExternalStorage의 값을 true**&#x200B;로 **설정**&#x200B;합니다. SSO(Single Sign-On) **이(가) 작동합니다**
- 앱 대상 **Android 10** **(API 수준 29)** 및 앱의 매니페스트 파일 **-\>** SSO(Single Sign-On) **에서** requestLegacyExternalStorage의 값을 true **로**&#x200B;설정하지&#x200B;**않습니다**

>[!TIP]
>
> Adobe Pass Authentication Access Enabler Android SDK가 범위 지정 저장소와 완전히 호환되기 전에, 공용 [Android 설명서](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage)에 설명된 대로 앱의 대상 SDK 수준 또는 requestLegacyExternalStorage 매니페스트 특성을 기반으로 일시적으로 옵트아웃할 수 있습니다.
