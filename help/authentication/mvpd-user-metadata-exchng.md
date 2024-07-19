---
title: MVPD 사용자 메타데이터 교환
description: MVPD 사용자 메타데이터 교환
exl-id: 8bce6acc-cd33-476c-af5e-27eb2239cad1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# MVPD 사용자 메타데이터 교환

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 소개 {#intro-user-metadata-exchange}

MVPD는 경우에 따라 프로그래머와 공유되는 고객에 대한 사용자별 메타데이터를 유지 관리합니다. Adobe Pass 인증의 목적은 이 &quot;사용자 메타데이터&quot;의 교환을 중개하는 것이지만 교환과 관련된 모든 종류의 규칙을 적용하는 것은 아닙니다. 교환 규칙은 MVPD가 프로그래머 파트너와 협력하는 것입니다.

Exchange에서 사용할 수 있는 사용자 메타데이터 유형은 현재 다음과 같습니다.

* 우편 번호
* 최대 등급(VChip 또는 MPAA)
* 사용자 ID
* 세대 ID
* 채널 ID

이 기능을 사용하여 MVPD와 프로그래머는 자녀 보호와 같은 특수 사용 사례를 구현할 수 있습니다. 예를 들어 MVPD는 자녀 보호 등급 데이터를 프로그래머에게 전달하고 프로그래머는 해당 데이터를 사용하여 사용자에 대한 사용 가능한 보기 선택 사항을 필터링할 수 있습니다.

사용자 메타데이터 핵심 사항:

* MVPD는 인증 및 권한 부여 흐름 동안 프로그래머의 애플리케이션에 사용자 메타데이터를 전달합니다
* Adobe Pass 인증은 AuthN 및 AuthZ 토큰에 메타데이터 값을 저장합니다
* Adobe Pass 인증은 다양한 형식으로 사용자 메타데이터를 제공하는 MVPD의 값을 정규화할 수 있습니다
* 일부 매개 변수는 프로그래머 키를 사용하여 암호화할 수 있습니다
* 특정 값은 구성 변경을 통해 Adobe에서 사용할 수 있습니다

>[!NOTE]
>
>사용자 메타데이터는 이전에 Adobe Pass 인증에서 사용할 수 있었던 정적 메타데이터(인증 토큰 TTL, 인증 토큰 TTL 및 장치 ID)의 확장입니다.

## 예시 {#example-mvpd-user-metadata-exch}

### 자녀 보호 {#example-parental-control}

이 예제는 다음 내용을 교환하는 방법을 보여 줍니다.

* [프로그래머에서 MVPD 메타데이터 교환으로](#progr-mvpd-metadata-exch)

* [MVPD에서 프로그래머 메타데이터로의 교환 흐름](#mvpd-progr-exchange-flow)

### 프로그래머에서 MVPD 메타데이터 교환으로 {#progr-mvpd-metadata-exch}

현재 프로그래머 API, Adobe Pass 인증 및 MVPD 권한 부여자는 모두 채널 수준 권한 부여만 지원합니다. 채널은 Programmer의 getAuthorization() API 호출에서 일반 텍스트 문자열로 지정됩니다. 이 문자열은 MVPD의 인증 백엔드로 전파됩니다.

프로그래머 앱 또는 사이트에서 사용자는 XACML 가능 MVPD(이 예에서는 &quot;TNT&quot;)를 선택합니다. XACML에 대한 자세한 내용은 [Xtensible 액세스 제어 마크업 언어](https://en.wikipedia.org/wiki/XACML){target=_blank}를 참조하십시오.
프로그래머 앱은 리소스와 해당 메타데이터를 포함하는 AuthZ 요청을 형성한다.  이 예에는 &quot;pg&quot;의 MPAA 등급이 채널 요소의 미디어 속성에 포함됩니다.

```XML
var resource = '<rss version="2.0" xmlns:media="http://video.search.yahoo.com/mrss/">
                    <channel> 
                        <title>TNT</title> 
                        <media:rating scheme="urn:mpaa">pg</media:rating>
                    </channel>
                </rss>';
getAuthorization(resource);
```

Adobe Pass 인증은 MVPD와 프로그래머 모두에서 지원하는 경우 자산 수준까지 보다 세분화된 인증을 실제로 지원합니다. 리소스 및 해당 메타데이터는 Adobe에 불투명합니다. 목적은 리소스 ID 및 메타데이터를 표준화된 방식으로 지정하기 위한 표준 포맷을 설정하여 리소스 ID를 다른 MVPD로 전송하는 것입니다.

>[!NOTE]
>
>사용자가 채널 전용 가능 MVPD를 선택하면 Adobe Pass 인증이 채널 제목(위 예에서 &quot;TNT&quot;)만 추출하고 제목만 MVPD에 전달합니다.

### MVPD에서 프로그래머 메타데이터로의 교환 흐름 {#mvpd-progr-exchange-flow}

Adobe Pass 인증은 다음과 같은 가정을 합니다.

* MVPD가 SAML 응답의 일부로 최대 등급을 보냅니다.
* 이 정보는 인증 토큰의 일부로 저장됩니다.
* 프로그래머가 이 정보를 검색할 수 있도록 Adobe Pass 인증에서 API를 제공합니다
* 프로그래머는 사이트 또는 앱에서 이 기능을 구현합니다(예를 들어, 사용자의 최대 등급 이상인 비디오를 숨기기 위해)

```XML
<saml:Assertion ID="pfxec5f92e0-8589-3fc3-c708-f4fb8e2fad59"
                 IssueInstant="2010-07-20T10:05:41Z" Version="2.0"
                 xmlns:xs="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:AttributeStatement>
        <saml:Attribute
                Name="MaxTVRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">tv-ma</saml:AttributeValue>
        </saml:Attribute>
        <saml:Attribute
                Name="MaxMovieRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">nc-17</saml:AttributeValue>
        </saml:Attribute>
    </saml:AttributeStatement>
</saml:Assertion>
```

### 메모 {#notes-mvpd-progr-metadata-exch-flow}

**리소스 정규화 및 유효성 검사입니다.**&#x200B;개의 리소스 ID를 일반 문자열 또는 MRSS 문자열로 전달할 수 있습니다. 프로그래머는 일반 문자열 형식 또는 MRSS를 사용할 수 있지만 MVPD가 해당 리소스를 처리하는 방법을 알 수 있도록 MVPD와의 사전 합의가 필요합니다.

**리소스 ID 및 메타데이터 사양.** Adobe Pass 인증은 미디어 RSS 확장과 함께 RSS 표준을 사용하여 리소스와 해당 메타데이터를 지정합니다. Adobe Pass 인증은 Media RSS 확장과 함께 자녀 보호(`<media:rating>`을 통해) 또는 지리적 위치(`<media:location>`)와 같은 다양한 메타데이터를 지원합니다.

Adobe Pass 인증은 또한 레거시 채널 문자열에서 RSS가 필요한 MVPD에 대한 해당 RSS 리소스로의 투명한 전환을 지원할 수 있습니다. 다른 방향으로, Adobe Pass 인증은 채널 전용 MVPD에 대해 RSS+MRSS에서 일반 채널 제목으로의 전환을 지원합니다.

**Adobe Pass 인증은 기존 통합과 완벽하게 이전 버전과의 호환성을 보장합니다.** 즉, 채널 수준 인증을 사용하는 프로그래머의 경우 Adobe Pass 인증은 채널 ID를 해당 형식을 이해하는 MVPD로 보내기 전에 필요한 형식으로 패키징합니다. 역도 적용됩니다. 프로그래머가 모든 리소스를 새 형식으로 지정하는 경우 Adobe Pass 인증은 채널 수준 인증만 수행하는 MVPD에 대해 인증할 경우 새 형식을 간단한 채널 문자열로 변환합니다.

## 사용자 메타데이터 사용 사례 {#user-metadata-use-cases}

더 많은 MVPD가 법적 절차를 마련하고 기능을 추가함에 따라 사용 사례가 지속적으로 변경되고 확장되고 있습니다. 다음은 사용할 수 있는 사용자 메타데이터의 예입니다.

* [MVPD 사용자 ID](#mvpd-user-id)
* [세대 ID](#household-user-id)
* [우편번호](#zip-code)
* [최대 등급(자녀 보호)](#max-rating-parental-control)
* [채널 라인업](#channel-line-up)

### MVPD 사용자 ID {#mvpd-user-id}

* MVPD에서 제공한 대로
* MVPD에 의해 해시되므로 사용자의 실제 로그인 정보가 아님
* 또는 특정 사용자에 대한 문제를 표시하는 데 사용할 수 있습니다.
* 암호화됨
* MVPD 지원: 모든 MVPD

### 세대 사용자 ID {#household-user-id}

* 유용한 지표 정보 허용
* 암호화됨
* MVPD 지원: 일부 MVPD

### 우편번호 {#zip-code}

* 사용자의 청구 우편 번호
* 주로 스포츠 이벤트 고정 기간 규칙을 적용하는 데 사용됩니다.
* 빠른 업데이트를 위한 AuthZ 응답을 제공할 수 있습니다.
* MVPD 지원: 일부 MVPD

### 최대 등급(자녀 보호) {#max-rating-parental-control}

* 처음에는 AuthN에 AuthZ 새로 고침 추가
* UI에서 콘텐츠 필터링
* MPAA 또는 VChip 등급
* MVPD 지원: 일부 MVPD

### 채널 라인업 {#channel-line-up}

* MVPD는 사용자가 볼 수 있는 채널 목록을 제공할 수 있습니다
* 빠른 UI 페인팅 허용
* OLCA 사양은 AuthN 응답에서 이것을 속성 문으로 허용합니다
* MVPD 지원: 일부 MVPD

<!--
>[!RELATEDINFORMATION]
>
>* [Proxy MVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Content Metadata Exhange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [OLCA AuthN / AuthZ Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank}
>* [User Metadata (Programmer Integration Guide)](/help/authentication/user-metadata-feature.md)
-->
