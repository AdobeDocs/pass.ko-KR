---
title: 헤더 - AP-Device-Identifier
description: REST API V2 - 헤더 - AP-Device-Identifier
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# 헤더 - AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

<b>AP-Device-Identifier</b> 요청 헤더에 클라이언트 응용 프로그램에서 만든 스트리밍 장치 식별자가 포함되어 있습니다.

## 구문 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>: &lt;type&gt; &lt;identifier&gt;</td>
   </tr>
   <tr>
      <td>헤더 유형</td>
      <td>요청 헤더</td>
   </tr>
   <tr>
      <td>표준</td>
      <td>아니요</td>
   </tr>
</table>

## 지시문 {#directives}

<b>&lt;type></b>

장치 식별자 유형.

아래에 제시된 대로 지원되는 유형은 한 가지뿐입니다.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">유형</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>지문</td>
      <td>
            장치 식별자는 클라이언트 애플리케이션에 의해 생성되고 관리되는 안정적이고 고유한 식별자로 구성된다.
            <br/>
            클라이언트 응용 프로그램은 응용 프로그램 제거, 다시 설치 또는 업그레이드와 같은 사용자 작업으로 인한 값 변경을 방지해야 합니다.
      </td>
   </tr>
</table>


<b>&lt;식별자></b>

장치 식별자의 `Base64-encoded` 값입니다.

## 예 {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```

## 요리책 {#cookbooks}

>[!IMPORTANT]
>
> 설명서 리소스는 참조 목적으로 제공됩니다.
>
> 설명서 리소스는 완벽하지 않으며, 프로젝트에서 작업하려면 추가 수정이 필요할 수 있습니다.
> 
> 실제 구현과 관계없이 `AP-Device-Identifier` 헤더에는 [지시문](#directives) 섹션에 설명된 대로 형식이 지정된 값이 있어야 합니다.

### 브라우저 {#browsers}

브라우저에서 실행 중인 장치에 대한 `AP-Device-Identifier` 헤더를 작성하려면 클라이언트 응용 프로그램에서 브라우저, 장치 또는 사용자별 데이터와 같은 사용 가능한 데이터를 기반으로 안정적이고 고유한 식별자를 계산해야 합니다.

_(*) 브라우저 또는 장치 지문 인식 메커니즘을 제공하는 라이브러리 또는 서비스를 통합하는 것이 좋습니다._

### 모바일 장치 {#mobile-devices}

#### iOS 및 iPadOS {#ios-ipados}

[iOS 또는 iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes)를 실행하는 장치의 `AP-Device-Identifier` 헤더를 작성하려면 다음 문서를 참조하십시오.

* [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)용 Apple 개발자 설명서입니다.

_(*) OS에서 제공한 값에 SHA-256 해시 함수를 적용하는 것이 좋습니다._

#### Android {#android}

[Android](https://developer.android.com/about/versions)을(를) 실행하는 장치의 `AP-Device-Identifier` 헤더를 빌드하려면 다음 문서를 참조할 수 있습니다.

* [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)용 Android 개발자 설명서입니다.

_(*) OS에서 제공한 값에 SHA-256 해시 함수를 적용하는 것이 좋습니다._

### TV 연결 장치 {#tv-connected-devices}

#### tvOS {#tvos}

[tvOS](https://developer.apple.com/documentation/tvos-release-notes)를 실행하는 장치의 `AP-Device-Identifier` 헤더를 빌드하려면 다음 문서를 참조하십시오.

* [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)용 Apple 개발자 설명서입니다.

_(*) OS에서 제공한 값에 SHA-256 해시 함수를 적용하는 것이 좋습니다._

#### Fire OS {#fireos}

[Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html)를 실행하는 장치의 `AP-Device-Identifier` 헤더를 빌드하려면 다음 문서를 참조할 수 있습니다.

* [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)용 Android 개발자 설명서입니다.

_(*) OS에서 제공한 값에 SHA-256 해시 함수를 적용하는 것이 좋습니다._

#### Roku 운영 체제 {#rokuos}

[Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md)를 실행하는 장치의 `AP-Device-Identifier` 헤더를 만들려면 다음 문서를 참조할 수 있습니다.

* [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string)에 대한 Roku 개발자 설명서.

_(*) OS에서 제공한 값에 SHA-256 해시 함수를 적용하는 것이 좋습니다._
