---
title: 용어집
description: 용어집
exl-id: e64a94f6-7460-4aa8-8d6b-e0553ba1e4ec
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# 용어집 {#glossary}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 액세스 활성 {#accessEnabler}

Adobe Pass 인증의 클라이언트 구성 요소입니다. Adobe Pass 인증은 지원되는 각 플랫폼에 대한 AccessEnabler 라이브러리를 제공합니다.

## AuthN {#authn}

&quot;AuthN 토큰&quot; 또는 &quot;AuthN 흐름&quot;에서와 같이 &quot;인증&quot;의 축약으로 사용됩니다.


## 인증 토큰{#authn-token}

사용자가 MVPD로 인증된 후 Adobe Pass 인증에서 생성된 인증 토큰입니다. 프로그래머의 통합 플랫폼에 따라 토큰은 사용자의 장치 또는 Adobe Pass 인증 서버에 저장됩니다.

## 인증Z {#authz}

&quot;AuthZ Token&quot; 또는 &quot;AuthZ Flow&quot;에서와 같이 &quot;authorization&quot;의 축약으로 사용됩니다.

## AuthZ 토큰 {#authz-token}

사용자가 보호된 콘텐츠를 볼 수 있는 권한이 부여된 후 Adobe Pass 인증에서 생성된 인증 토큰. AuthZ 토큰은 Adobe Pass 인증 서버에 저장되고 [단기 미디어 토큰](#short-lived-token).

## 채널 ID(더 이상 사용되지 않음) {#channel_id}

리소스 ID에 대한 이전 용어입니다.

## 클라이언트 없는 API {#clientless-api}

AccessEnabler 클라이언트 구성 요소 대신 웹 서비스를 사용하는 Adobe Pass 인증 통합 솔루션입니다.

## 장치 ID {#device-id}

장치(예: 휴대폰, 태블릿 등)를 고유하게 식별합니다. Adobe Pass 인증 내에서 사용할 수 있습니다. 이 ID는 프로그래머 애플리케이션에서 획득/제공합니다.


## 권한 흐름{#entitlement_flow}

Adobe Pass 인증 설명서에서 사용되는 용어는 Adobe Pass 인증을 통해 장치/사용자를 등록하고, MVPD를 통해 사용자를 인증하고, 사용자를 위한 리소스를 인증하고, 사용자를 로그아웃하는 전체 프로세스를 말합니다.


## GUID {#guid}

다음을 참조하십시오 [사용자 ID](#user-id).

## IdP {#idp}

공급자 식별. Adobe Pass 인증 통합에서 MVPD의 역할과 관련하여 MVPD와 동의어입니다. (고객은 유료 TV 공급자의 로그인 페이지를 통해 ID를 확인해야 합니다.)

## 미디어 토큰 검증기 {#media-token-verifier}

자격 흐름이 성공적으로 완료된 후 Adobe Pass 인증으로 생성된 단기 미디어 토큰을 확인하기 위해 프로그래머가 사용하는 Adobe 제공 라이브러리입니다.

## MVPD {#mvpd}

&quot;유료 TV 공급자&quot;의 동의어인 멀티채널 비디오 프로그래밍 디스트리뷰터.

## MVPD ID {#mvpd-id}

다음을 참조하십시오 [사용자 ID](#user-id).

## 파트너 ID {#partner-id}

Adobe Pass 인증을 요청하는 사용자를 대신하여 식별하는 데 Adobe이 사용하는 MVPD에 전달하는 식별자입니다. 때때로 특정 프로그래머에 대한 UI를 구성하는 데 사용되며, 때로는 모든 프로그래머에서 동일하며, MVPD의 요구 사항에 따라 다릅니다.

## 유료 TV 공급자 {#pay-tv-provider}

의 동의어 [MVPD](#mvpd).

## 프로그래머 {#programmer}

&quot;콘텐츠 공급자&quot;, &quot;계정&quot;, &quot;채널&quot;, &quot;서비스 공급자&quot;, &quot;브랜드&quot; 등의 동의어입니다.

## 프록시 MVPD {#proxy-mvpd}

다른 MVPD에 대해 ID 서비스를 제공하는 MVPD로, Adobe Pass 인증과 직접 통합됩니다.

## 프록시화된 MVPD {#proxied-mvpd}

Adobe SP와 직접 통합되지 않지만 프록시 MVPD를 통해 통합된 MVPD입니다.

## 요청자 ID {#requestor-id}

를 고유하게 식별합니다. [프로그래머](#programmer) (계정, 브랜드, 채널 또는 속성) Adobe Pass 인증 내에서 사용할 수 있습니다. 이 ID는 계정의 초기 설정 중에 프로그래머와 Adobe 간에 결정됩니다. 웹에서 요청자 ID는 화이트리스트에 추가한 도메인 세트와 관련되어 있습니다. 외부 도메인의 ID를 사용하는 모든 호출은 거부됩니다. 프로그래머는 Analytics에 요청자 ID도 사용합니다. 일반적으로 프로그래머당 하나의 요청자 ID만 있습니다. 요청자 ID와 관련된 한 가지 추가 기능은 setRequestor API 호출이 Adobe Pass 인증 시스템에서 프로그래머를 인증하는 데 사용되는 암호화된 데이터가 전송될 것으로 예상하므로 프로그래머가 Adobe에게 공개 인증서를 제공해야 한다는 것입니다.

## 리소스 ID {#resource-id}

다음을 식별하는 문자열 또는 mRSS 리소스 [프로그래머](#programmer) MVPD에 대한 프로그래머와 MVPD 간에 동의합니다. Adobe Pass 인증은 그대로 리소스 ID를 통과하므로 모든 MVPD에 대해 동일해야 합니다. 프로그래머는 MVPD가 각 ID가 나타내는 내용을 알고 있는 한 여러 리소스 ID를 사용할 수 있습니다.

## 세션 GUID {#sessionGUID}

다음을 참조하십시오 [사용자 ID](#user-id).

## 단기 미디어 토큰 {#short-lived-token}

이 토큰은 특정 사용자에 대한 권한 부여 프로세스가 완료되면 Adobe Pass 인증에 의해 생성됩니다. 토큰은 프로그래머에게 전달되며, 프로그래머는 단기 미디어 토큰에서 Adobe Pass 인증 토큰 검증기를 사용하여 자격 부여 프로세스의 보안을 확인합니다.

## 스마트 장치 {#smart-device}

Adobe Pass 인증 설명서 전체에서 셋톱 박스, 게임 콘솔 및 스마트 TV를 지칭하는 데 사용되는 용어입니다. 네트워킹 기능은 있지만 웹 페이지를 렌더링할 수는 없는 장치입니다.

## SP{#sp}

서비스 공급자. 일반적으로 *역할* SP의 경우, Adobe Pass 인증에서 재생되며 [MVPD](#mvpd).

## 임시 통과 {#temp-pass}

프로그래머가 유료 콘텐츠에 대한 임시 무료 액세스를 제공할 수 있는 기능입니다. 프로그래머가 지정한 기간에 대한 액세스는 요청자별로 다릅니다.

## TL {#ttl}

살 시간이에요 토큰이 유효한 지정된 기간입니다.

## TVE {#tve}

어디에나 TV가 있습니다.

## 사용자 ID {#user-id}

프로그래머 앱의 사용자를 고유하게 식별하지만 MVPD에서 시작됩니다. 다양한 사용 사례에 대해 다양한 양식으로 사용할 수 있습니다. 다음을 참조하십시오 [프로그래머 개요에서 사용자 ID 이해](/help/authentication/programmer-overview.md#user-ids).

## 허용 목록 {#whitelist}

Adobe Pass 인증과 통신하기 위해 적법한 것으로 지정된 도메인 목록입니다.

## XSTS 토큰 {#xsts-token}

Xbox 콘솔 앱 개발을 위해 Microsoft에서 발급한 보안 토큰으로, Xbox/Adobe Pass 인증 통합에 사용됩니다.
