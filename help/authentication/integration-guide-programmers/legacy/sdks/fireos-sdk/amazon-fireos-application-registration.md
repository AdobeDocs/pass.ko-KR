---
title: Amazon FireOS 애플리케이션 등록
description: Amazon FireOS 애플리케이션 등록
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# (기존) Amazon FireOS 애플리케이션 등록 {#amazon-fireos-application-registration}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

</br>

## 소개 {#intro}

FireOS AccessEnabler SDK 버전 3.0부터 Adobe 서버의 인증 메커니즘을 변경하고 있습니다. 공개 키 및 암호 시스템을 사용하여 requestorID에 서명하는 대신 SDK에서 서버에 대해 수행하는 모든 호출에 나중에 사용되는 액세스 토큰을 얻는 데 사용할 수 있는 소프트웨어 문 문자열 개념을 도입합니다. 소프트웨어 명령문 외에도 응용 프로그램에 대한 딥링크를 만들어야 합니다.

자세한 내용은 [동적 클라이언트 등록 개요](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)를 참조하십시오.

## 소프트웨어 명령문이란? {#what}

소프트웨어 명령문은 애플리케이션에 대한 정보가 포함된 JWT 토큰입니다. 모든 응용 프로그램에는 Adobe 시스템에서 응용 프로그램을 식별하기 위해 서버에서 사용하는 고유한 소프트웨어 문이 있어야 합니다. AccessEnabler SDK을 초기화할 때 Software 문을 전달해야 하며 이 문은 Adobe에 응용 프로그램을 등록하는 데 사용됩니다. 등록하면 SDK은 액세스 토큰을 얻는 데 사용될 클라이언트 ID와 클라이언트 암호를 수신하게 됩니다. SDK이 서버에 대해 수행하는 모든 호출에는 유효한 액세스 토큰이 필요합니다. SDK은 애플리케이션 등록, 액세스 토큰 획득 및 새로 고침을 담당합니다.

**참고:** Software 문은 앱에 따라 다르며 개별 Software 문은 둘 이상의 응용 프로그램에 사용할 수 없습니다. 이는 여러 채널에 대한 액세스를 제공하는 애플리케이션에도 적용됩니다.

## 소프트웨어 명령문을 얻는 방법 {#how-to}

### Adobe의 TVE 대시보드에 액세스할 수 있는 경우:

1. 브라우저를 열고 `https://experience.adobe.com/#/pass/authentication`(으)로 이동합니다.

1. **[!UICONTROL Channels]** 섹션으로 이동한 다음 채널을 선택합니다.

1. **[!UICONTROL Registered Applications]** 탭으로 이동합니다.

1. **[!UICONTROL Add new application]**&#x200B;을(를) 클릭합니다.

1. 애플리케이션의 이름과 버전을 입력하고 이를 사용할 수 있는 플랫폼(예: Android)을 선택합니다.

1. 프로그래머용으로 이미 구성된 도메인 목록에서 선택하여 **[!UICONTROL Domain Name]**&#x200B;을(를) 제공하십시오.

1. 변경 내용을 서버에 푸시한 다음 채널의 **[!UICONTROL Registered Applications]** 탭으로 다시 이동합니다.

   등록된 모든 지원서가 있는 목록이 표시됩니다.

1. 방금 만든 응용 프로그램에서 **[!UICONTROL Download]**&#x200B;을(를) 클릭합니다.

   소프트웨어 명령문을 다운로드할 준비가 되기 전에 몇 분 정도 기다려야 할 수 있습니다.

   텍스트 파일을 다운로드합니다. 해당 내용을 소프트웨어 선언으로 사용합니다.

자세한 내용은 [Dynamic Client Registration Management](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management)를 참조하십시오.

### Adobe의 TVE 대시보드에 대한 액세스 권한이 없는 경우:

[tve-support@adobe.com](mailto:tve-support@adobe.com)(으)로 티켓을 제출합니다. 채널, 애플리케이션 이름, 버전 및 플랫폼을 포함하여 필요한 모든 정보를 포함하십시오. 지원팀에서 소프트웨어 설명을 만들어 드릴 것입니다.

## 소프트웨어 명령문 사용 방법 {#use}

소프트웨어 명령문을 가져온 후에는 Access Enabler 생성자에서 매개 변수로 전달해야 합니다. Adobe은 원격 위치에서 소프트웨어 문을 호스팅할 것을 권장합니다. 이렇게 하면 응용 프로그램의 새 버전을 릴리스하지 않고도 소프트웨어 문을 쉽게 취소하고 변경할 수 있습니다.

## 소프트웨어 명령문 사용 방법 {#use-both}

응용 프로그램의 리소스 파일 `strings.xml`에 다음 코드를 추가합니다.

```XML
<string name="software_statement">softwarestatement value</string>
```
