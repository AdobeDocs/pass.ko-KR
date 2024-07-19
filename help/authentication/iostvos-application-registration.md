---
title: iOS/tvOS 애플리케이션 등록
description: iOS/tvOS 애플리케이션 등록
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 0%

---

# iOS/tvOS 애플리케이션 등록 {#iostvos-application-registration}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 소개 {#Intro}

iOS/tvOS AccessEnabler SDK 버전 3.0부터 Adobe 서버의 인증 메커니즘을 변경하고 있습니다. 공개 키 및 비밀 시스템을 사용하여 requestorID에 서명하는 대신 SDK가 서버에 대해 수행하는 모든 호출에 나중에 사용되는 액세스 토큰을 얻는 데 사용할 수 있는 소프트웨어 문 문자열 개념을 도입합니다. 소프트웨어 명령문 외에 응용 프로그램에 대한 사용자 지정 URL 체계도 필요합니다.

자세한 내용은 [동적 클라이언트 등록](/help/authentication/dynamic-client-registration.md)을 참조하십시오.

## 소프트웨어 명령문이란? {#Soft_state}

소프트웨어 명령문은 애플리케이션에 대한 정보가 포함된 JWT 토큰입니다. 모든 애플리케이션에는 Adobe 시스템에서 애플리케이션을 식별하기 위해 서버에서 사용하는 고유한 소프트웨어 명령문이 있어야 합니다. AccessEnabler SDK를 초기화할 때 Software 문을 전달해야 하며 이 문은 Adobe에 응용 프로그램을 등록하는 데 사용됩니다. 등록하면 SDK는 액세스 토큰을 가져오는 데 사용할 클라이언트 ID와 클라이언트 암호를 수신합니다. SDK가 서버에 대해 수행하는 모든 호출에는 유효한 액세스 토큰이 필요합니다. SDK는 애플리케이션 등록, 액세스 토큰 획득 및 새로 고침을 담당합니다.

**참고:** Software 문은 앱에 따라 다르며 둘 이상의 응용 프로그램에서 동일한 소프트웨어 문을 사용할 수 없습니다. 프로그래머 수준 소프트웨어 명령문도 동일하게 따릅니다. 즉, 단일 채널이든 다중 채널이든 단일 애플리케이션에 대해서만 사용할 수 있습니다. 이 제한은 사용자 지정 구성표에도 적용됩니다.

## 소프트웨어 명령문을 얻는 방법 {#obtain}

### Adobe의 TVE 대시보드에 액세스할 수 있는 경우:

- 브라우저를 열고 <https://console.auth.adobe.com>(으)로 이동
- `Channels` 섹션으로 이동하여 채널을 선택하십시오.
- `Registered Applications` 탭으로 이동합니다.
- `Add new application`을(를) 클릭합니다.
- 응용 프로그램의 이름과 버전을 입력하고   플랫폼을 사용할 수 있습니다. 이 예제에서는 iOS/tvOS입니다.
- 변경 사항을 서버에 푸시한 다음 채널의 등록된 애플리케이션 탭으로 다시 이동합니다.
- 등록된 모든 지원서가 있는 목록이 표시됩니다. 다음을 클릭합니다.   방금 만든 응용 프로그램의 `Download` 단추입니다. 소프트웨어 명령문을 다운로드할 준비가 되기 전에 몇 분 정도 기다려야 할 수 있습니다.
- 텍스트 파일이 다운로드됩니다. 내용을 소프트웨어 선언으로 사용하십시오.

자세한 내용은 [동적 클라이언트 등록 관리](/help/authentication/dynamic-client-registration-management.md)를 참조하십시오.

### Adobe의 TVE 대시보드에 대한 액세스 권한이 없는 경우:

<tve-support@adobe.com>(으)로 티켓을 제출합니다. 채널, 애플리케이션 이름, 버전 및 플랫폼 등 필요한 모든 정보를 포함하십시오. 지원 팀에서 소프트웨어 설명을 만들어 드릴 것입니다.

## Software Statement 사용 방법 {#use}

소프트웨어 명령문을 얻은 후에는 Access Enabler 생성자에 매개 변수로 전달해야 합니다. Software Statement를 원격 위치에 호스팅하는 것이 좋습니다. 이렇게 하면 응용 프로그램의 새 버전을 릴리스하지 않고도 소프트웨어 문을 쉽게 취소하고 변경할 수 있습니다.

## 애플리케이션에 대한 사용자 정의 URL 체계 생성 {#generating}

### Adobe의 TVE 대시보드에 액세스할 수 있는 경우:

- 브라우저를 열고 <https://console.auth.adobe.com>(으)로 이동
- `Channels` 섹션으로 이동하여 채널을 선택하십시오.
- `Custom Schemes` 탭으로 이동합니다.
- `Generate a new custom scheme`을(를) 클릭합니다.
- 애플리케이션에 대한 새 사용자 정의 체계가 생성됩니다. 예: `adbe.1JqxQsYhQOCIrwPjaooY8w://`
- 변경 사항을 서버에 푸시합니다.

### Adobe의 TVE 대시보드에 대한 액세스 권한이 없는 경우:

<tve-support@adobe.com>(으)로 티켓을 제출합니다. 채널 ID를 포함하십시오. 그러면 지원 팀에서 사용자를 위한 사용자 지정 체계를 만들 수 있습니다.

## 사용자 지정 체계를 사용하는 방법 {#use_custom}

응용 프로그램의 `info.plist` 파일에 다음 코드를 추가합니다.

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>adbe.u-XFXJeTSDuJiIQs0HVRAg</string> // replace this with your custom scheme
            </array>
        </dict>
    </array>
```
