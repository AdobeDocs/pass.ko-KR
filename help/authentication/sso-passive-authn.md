---
title: 수동 인증을 통한 SSO
description: 수동 인증을 통한 SSO
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# 수동 인증을 통한 SSO

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.


## 소개

이 문서의 범위는 수동 인증 흐름의 구현 및 표준 SSO(Single Sign-On) 접근 방식에서의 작동 방식을 설명하는 것입니다.

## 사용 사례

Adobe Pass 인증을 통해 앱/사이트 간 SSO(Single Sign-On)가 가능합니다. 사용자가 MVPD 자격 증명으로 로그인한 후 Adobe Pass 인증은 MVPD의 인증 세션을 나타내는 보안 토큰을 생성하고, 장치 ID를 사용하여 해당 토큰을 사용자의 장치에 바인딩합니다. Adobe Pass 인증은 서버 또는 장치에 토큰/장치 ID를 저장합니다.

토큰이 여전히 유효한 경우 사용자는 인증된 것으로 직접 표시됩니다. 이를 통해 사용자는 트랜잭션의 보안을 유지하면서 자격 증명을 덜 자주 입력할 수 있습니다.



여기에 설명된 비즈니스 사용 사례는 사용자가 방문한 모든 사이트에 대해 한 번 이상 인증되어야 한다는 매우 구체적인 요구 사항입니다. 이를 통해 MVPD는 네트워크별로 달라질 수 있는 authN 세션과 관련된 비즈니스 규칙을 적용할 수 있습니다. 사용자가 한 번만 로그인하면 Adobe Pass 인증 생태계에 속하는 모든 사이트에서 인증된다는 현재 TVE 약속과 충돌합니다.



비즈니스 규칙을 유지하면서도 양호한 사용자 경험을 유지하기 위해, MVPD는 사용자가 자격 증명 정보를 수동으로 제공할 것을 요구하지 않습니다. 이전에 설정한 세션 쿠키를 통해 수동 플로우를 사용하여 자동 재인증을 시도할 수 있습니다. 사용자 관점에서 보면 자동으로 로그인되는 것으로 표시됩니다.



이를 해결하기 위해 네트워크별 인증과 수동 인증 지원이라는 두 가지 기능을 구현했습니다. MVPD는 해당 세션이 만들어진 사이트에 관계없이 IdP에 authN 세션이 있는 경우 사용자를 다시 인증하는 SAML 수동 authN을 지원합니다.



## 네트워크별 인증

이 기능을 사용하면 MVPD가 방문한 모든 사이트에 대해 한 번씩 인증 요청을 받게 됩니다. 이 기능은 Adobe Pass 인증 토큰이 requestorID로 제한되므로 요청한 네트워크에만 유효합니다. 따라서 사용자가 사이트 &quot;A&quot;에서 인증하고 나중에는 사이트 &quot;B&quot;를 방문하면 인증해야 합니다.



MVPD IdP에서 사용자가 이미 인증되었으므로 사용자는 로그인 정보를 제공할 필요가 없습니다. 대신 브라우저가 사이트 &quot;B&quot;에서 MVPD IdP로 리디렉션된 다음 다시 리디렉션됩니다. 동일한 사용자가 돌아와도 사이트 &quot;A&quot;에서 계속 인증됩니다.



다음 흐름은 네트워크당 기본 인증 기능을 보여 줍니다.





## 수동 인증

목표는 전체 브라우저 리디렉션 및 선택기를 표시하지 않고 백그라운드에서 재인증 프로세스를 수행하는 것입니다. 따라서 사용자가 사이트 A에서 사이트 B로 이동하면 자동으로 인증됩니다.



UX 관점에서는 이 흐름과 일반 MVPD로 실행되는 흐름 간에 차이가 없을 것입니다. 사용자가 보는 것은 사이트 A를 방문한 결과 자격 증명을 입력한 후 사이트 B에서 자동으로 인증된다는 것입니다.



이 흐름을 실행 가능하게 하려면 MVPD가 더 이상 세션을 가지고 있지 않은 경우 MVPD가 로그인 페이지에서 숨겨진 iframe이 &quot;중단&quot;되지 않도록 MVPD가 수동 인증을 지원해야 합니다. 이는 표준 &quot;isPassive&quot; 속성을 통해 수행됩니다.



다음 다이어그램은 향상된 흐름과 &quot;비하인드&quot; 수동 인증을 보여 줍니다.





SAML 요청 샘플
다음은 수동 authN 흐름에 대한 SAML 요청 샘플입니다.


```xml
<saml2p:AuthnRequest xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
                     AssertionConsumerServiceURL="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                     Destination="https://mvpd_idp_url"
                     ForceAuthn="false"
                     ID="_15056686-399c-4528-b21a-4a9542cfc8ec"
                     IsPassive="true"
                     IssueInstant="2014-11-03T14:18:12.394Z"
                     ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
                     Version="2.0"
                     >
    <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth.adobe.com </saml2:Issuer>
    <saml2p:Extensions>
        <thrpty:RespondTo xmlns:thrpty="urn:oasis:names:tc:SAML:protocol:ext:third-party">https://saml.sp.auth.adobe.com</thrpty:RespondTo>
    </saml2p:Extensions>
    <saml2p:NameIDPolicy AllowCreate="true"
                         Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient"
                         SPNameQualifier="https://saml.sp.auth.adobe.com"
                         />
</saml2p:AuthnRequest>
```

## 비즈니스 규칙

MVPD에는 특정 SSO 범위 도메인 제한이 있습니다. 예를 들어, 일부 도메인만 일부 MVPD(동일한 미디어 회사의 경우 SSO)에 의해 허용될 수 있으며, 회사 간에서는 허용되지 않습니다.
일부 MVPD에는 다른 인증 규칙이 적용될 수 있습니다. 예를 들어, MVPD는 서로 다른 네트워크별로 서로 다른 인증 TTL을 가질 수 있습니다. 또한 MVPD는 일부 네트워크에 대해 홈 기반 인증을 사용할 수 있지만 다른 네트워크에 대해서는 사용할 수 없습니다(자녀 보호 사용 사례는 여기에 강력하게 표시됨).


이러한 비즈니스 요구 사항은 사용자가 MVPD에 성공적으로 로그인한 후 다시 로그인하지 않아도 되는 것이 주요 사용 사례임을 염두에 두어야 합니다.

이 작업은 수동 authN 플래그가 있는 네트워크별 인증을 사용하여 수행할 수 있습니다.



## 알려진 제한 사항

iOS - iOS 로컬 스토리지의 특성으로 인해, 다양한 공급업체에서 개발한 애플리케이션용 iOS에서 SSO 흐름이 작동하지 않습니다. iOS 8 이상의 SSO에 대한 자세한 내용은 이 기술 노트를 참조하십시오.


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
