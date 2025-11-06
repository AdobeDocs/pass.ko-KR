---
title: Android 애플리케이션 등록
description: Android 애플리케이션 등록
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 0%

---

# (기존) Android 애플리케이션 등록 {#android-application-registration}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 소개 {#intro}

Android AccessEnabler SDK 버전 3.0부터 Adobe 서버의 인증 메커니즘을 변경하고 있습니다. 공개 키 및 암호 시스템을 사용하여 requestorID에 서명하는 대신 SDK에서 서버에 대해 수행하는 모든 호출에 나중에 사용되는 액세스 토큰을 얻는 데 사용할 수 있는 소프트웨어 문 문자열 개념을 도입합니다. 소프트웨어 선언문 외에도 응용 프로그램에 대한 딥링크를 만들어야 합니다.

자세한 내용은 [동적 클라이언트 등록 개요](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)를 참조하십시오.

## 소프트웨어 명령문이란? {#what}

소프트웨어 명령문은 애플리케이션에 대한 정보가 포함된 JWT 토큰입니다. 모든 애플리케이션에는 Adobe 시스템에서 애플리케이션을 식별하기 위해 서버에서 사용하는 고유한 소프트웨어 명령문이 있어야 합니다.

`AccessEnabler` SDK을 초기화할 때 Software 문을 전달해야 합니다. Adobe에 애플리케이션을 등록하는 데 사용됩니다. 등록 시 SDK은 액세스 토큰을 얻는 데 사용되는 클라이언트 ID 및 클라이언트 암호를 수신합니다. SDK이 Adobe 서버를 호출하려면 유효한 액세스 토큰이 필요합니다. SDK은 애플리케이션 등록, 액세스 토큰 획득 및 새로 고침을 담당합니다.

>[!NOTE]
>
>소프트웨어 명령문은 앱에 따라 다르며 개별 소프트웨어 명령문은 둘 이상의 애플리케이션에 사용할 수 없습니다. 프로그래머 수준 소프트웨어 문은 제약 조건이 동일하며 단일 채널이든 다중 채널이든 단일 응용 프로그램에만 사용할 수 있습니다.

## 소프트웨어 명령문을 얻는 방법 {#how-to-get-ss}

소프트웨어 명령문을 얻을 수 있는 방법은 다음과 같습니다.

### Adobe의 TVE 대시보드에 액세스할 수 있는 경우

1. 브라우저를 열고 [Adobe Pass TVE 대시보드](https://experience.adobe.com/#/pass/authentication)로 이동합니다.

1. **[!UICONTROL Channels]** 섹션으로 이동한 다음 채널을 선택하십시오.

1. **[!UICONTROL Registered Applications]** 탭으로 이동합니다.

1. **[!UICONTROL Add new application]**&#x200B;을(를) 클릭합니다.

1. 애플리케이션 이름을 지정하고 버전을 지정합니다.

1. 애플리케이션을 사용할 수 있는 플랫폼을 선택합니다(이 경우 Android).

1. 프로그래머용으로 이미 구성된 도메인 목록에서 선택하여 **[!UICONTROL Domain Name]**&#x200B;을(를) 제공하십시오.

1. 변경 내용을 서버에 푸시한 다음 채널의 **[!UICONTROL Registered Applications]** 탭으로 다시 이동합니다.

   등록된 모든 지원서가 있는 목록이 표시됩니다. 만든 응용 프로그램에서 **[!UICONTROL Download]**&#x200B;을(를) 선택합니다. 소프트웨어 명령문을 다운로드할 준비가 되기 전에 몇 분 정도 기다려야 할 수 있습니다.

   텍스트 파일을 다운로드합니다. 해당 내용을 소프트웨어 선언으로 사용합니다.

자세한 내용은 [Dynamic Client Registration Management](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management)를 참조하십시오.

### Adobe의 TVE 대시보드에 액세스할 수 없는 경우

`tve-support@adobe.com`(으)로 티켓을 제출합니다. 채널, 애플리케이션 이름, 버전 및 플랫폼 등 필요한 정보를 포함합니다. 지원 팀의 누군가가 귀하를 위해 소프트웨어 설명을 만들 것입니다.

## 소프트웨어 명령문 사용 방법 {#how-to-use-ss}

소프트웨어 명령문을 가져온 후에는 Access Enabler 생성자에서 매개 변수로 전달해야 합니다. Software Statement를 원격 위치에 호스팅하는 것이 좋습니다. 이렇게 하면 응용 프로그램의 새 버전을 릴리스하지 않고도 소프트웨어 문을 쉽게 취소하고 변경할 수 있습니다.

## 응용 프로그램에 대한 딥링크 만들기 및 사용 {#create}

Android에서 소프트웨어 명령문을 만들 때 선택한 도메인 이름의 역방향을 딥링크 값으로 사용합니다

생성된 딥링크는 Android 장치에서 고유한 값을 가져야 합니다. 여러 애플리케이션이 동일한 딥링크 값을 사용하는 경우 인증 및 로그아웃 흐름이 방해가 됩니다.

## Software Statement 및 딥링크를 사용하는 방법 {#use-both}

응용 프로그램의 리소스 파일 `strings.xml`에 다음 코드를 추가합니다.

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
