---
title: Amazon FireOS 애플리케이션 등록
description: Amazon FireOS 애플리케이션 등록
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 0%

---

# Amazon FireOS 애플리케이션 등록 {#amazon-fireos-application-registration}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>

## 소개 {#intro}

FireOS AccessEnabler SDK 버전 3.0부터 Adobe 서버의 인증 메커니즘을 변경하고 있습니다. 공개 키 및 비밀 시스템을 사용하여 requestorID에 서명하는 대신 SDK가 서버에 대해 수행하는 모든 호출에 나중에 사용되는 액세스 토큰을 얻는 데 사용할 수 있는 소프트웨어 문 문자열 개념을 도입합니다. 소프트웨어 명령문 외에도 응용 프로그램에 대한 딥링크를 만들어야 합니다.

자세한 내용은 [동적 클라이언트 등록](/help/authentication/dynamic-client-registration.md)

## 소프트웨어 명령문이란? {#what}

소프트웨어 명령문은 애플리케이션에 대한 정보가 포함된 JWT 토큰입니다. 모든 응용 프로그램에는 Adobe 시스템에서 응용 프로그램을 식별하기 위해 서버에서 사용하는 고유한 소프트웨어 문이 있어야 합니다. AccessEnabler SDK를 초기화할 때 Software 문을 전달해야 하며 이 문은 Adobe에 응용 프로그램을 등록하는 데 사용됩니다. 등록하면 SDK는 액세스 토큰을 가져오는 데 사용할 클라이언트 ID와 클라이언트 암호를 수신합니다. SDK가 서버에 대해 수행하는 모든 호출에는 유효한 액세스 토큰이 필요합니다. SDK는 애플리케이션 등록, 액세스 토큰 획득 및 새로 고침을 담당합니다.

**참고:** 소프트웨어 명령문은 앱에 따라 다르며 개별 소프트웨어 명령문은 둘 이상의 애플리케이션에 사용할 수 없습니다. 이는 여러 채널에 대한 액세스를 제공하는 애플리케이션에도 적용됩니다.

## 소프트웨어 명령문을 얻는 방법 {#how-to}

### Adobe의 TVE 대시보드에 액세스할 수 있는 경우:

- 브라우저를 열고 다음으로 이동합니다. <https://console.auth.adobe.com>
- 다음으로 이동 `Channels` 을(를) 섹션에서 선택하고 채널을 선택합니다.
- 다음으로 이동 `Registered Applications` 탭.
- 클릭 `Add new application`.
- 애플리케이션의 이름과 버전을 입력하고 이를 사용할 수 있는 플랫폼(예: Android의 경우)을 선택합니다.
- 프로그래머용으로 이미 구성된 도메인 목록에서 선택하여 도메인 이름을 제공하십시오.
- 변경 사항을 서버에 푸시하고 채널의 등록된 애플리케이션 탭으로 다시 이동합니다.
- 등록된 모든 응용 프로그램이 포함된 목록이 표시됩니다. 다음을 클릭합니다. `Download` 방금 만든 응용 프로그램의 단추입니다. 참고: 소프트웨어 명령문을 다운로드할 준비가 될 때까지 몇 분 정도 기다려야 할 수 있습니다.
- 텍스트 파일이 다운로드됩니다. 내용을 소프트웨어 선언으로 사용하십시오.

자세한 내용은 [동적 클라이언트 등록 관리](/help/authentication/dynamic-client-registration-management.md)

### Adobe의 TVE 대시보드에 대한 액세스 권한이 없는 경우:

다음 대상에게 티켓 제출 <tve-support@adobe.com>. 채널, 애플리케이션 이름, 버전 및 플랫폼을 포함하여 필요한 모든 정보를 포함하십시오. 그러면 지원팀에서 소프트웨어 설명을 만들어 드릴 것입니다.

## Software Statement 사용 방법 {#use}

소프트웨어 명령문을 얻은 후에는 Access Enabler 생성자에 매개 변수로 전달해야 합니다. Software Statement를 원격 위치에 호스팅하는 것이 좋습니다. 이렇게 하면 응용 프로그램의 새 버전을 릴리스하지 않고도 소프트웨어 문을 쉽게 취소하고 변경할 수 있습니다.

## 소프트웨어 명령문 사용 방법 {#use-both}

응용 프로그램의 리소스 파일에서 `strings.xml` 다음 코드를 추가합니다.

```XML
<string name="software_statement">softwarestatement value</string>
```
