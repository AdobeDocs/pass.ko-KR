---
title: 쿠키 업데이트 - SameSite 및 보안 플래그
description: 쿠키 업데이트 - SameSite 및 보안 플래그
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# (기존) 쿠키 업데이트 - SameSite 및 Secure 플래그 {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>


## 업데이트 {#Updates}

이 섹션에서는 타사 쿠키를 처리하기 위해 Chrome 브라우저 및 Adobe Pass 인증에서 도입된 변경 사항을 중점적으로 다룹니다.



### Chrome 80 업데이트 {#Chrome}

Chrome 버전 80(버전 82 제외)부터 *SameSite* 특성을 지정하지 않는 쿠키는 *SameSite=Lax*&#x200B;인 것처럼 처리됩니다. 따라서 사이트 간 컨텍스트에서 전달해야 하는 쿠키는 *SameSite=None*&#x200B;을 명시적으로 지정해야 하며 *Secure* 특성으로 표시되고 *HTTPS*&#x200B;을(를) 통해 전달되어야 합니다. 이러한 업데이트에 대한 자세한 내용은 공식 Chromium 페이지(<https://www.chromium.org/updates/same-site> 및 <https://web.dev/samesite-cookies-explained/>)에서 읽을 수 있습니다.


### Adobe Pass 인증 업데이트 {#Pass-Updates}

Adobe Pass 인증 서비스는 현재 Adobe Pass 인증 SDK의 일부 플랫폼 및 버전과 함께 작동하기 위해 Chrome을 포함하여 브라우저의 관점에서 타사 쿠키로 간주되는 두 개의 쿠키를 사용하고 있습니다. 따라서 예정된 변경 사항을 준수하고 이러한 이전 SDK의 사이트 간 컨텍스트에서 이러한 쿠키를 계속 제공하기 위해 Adobe Pass 인증 서비스는 *adobe-pass-2.55.1* 버전에서 필요한 변경 사항을 구현합니다.

*adobe-pass-2.55.1* 버전의 이러한 변경 사항에는 버전 80 이상으로 시작하는 Chrome 브라우저를 사용할 때(버전 82 제외) 모든 Adobe Pass 인증 SDK에 다시 전달된 모든 해당 쿠키에 대한 *Secure* 및 *SameSite=None* 특성을 추가하는 작업이 포함됩니다.

다음 섹션에서는 한 명의 사용자가 Adobe Pass 브라우저 80 이상을 사용하는 경우(버전 82 제외) 플랫폼 및 Chrome 인증 SDK 버전 목록에 대해 몇 가지 잠재적인 문제를 설명합니다.

## 문제 해결 {#Troubleshooting}

이 섹션을 탐색하는 동안 모든 Adobe Pass 인증 서비스 쿠키에는 모든 브라우저에 대해 *adobe-pass-2.55.1* 버전으로 설정된 *Secure* 특성이 있어야 하지만 *SameSite=None* 특성은 Chrome 브라우저 버전 80 이상(버전 82 제외)에 대해서만 설정해야 합니다.


### 일반 문제 해결 {#General}

1. 일부 사용자 에이전트는 *SameSite=None* 특성과 호환되지 않는 것으로 알려져 있습니다.

   - Chrome 51부터 Chrome 66까지의 Chrome 버전(양쪽 끝 포함). 이 Chrome 버전은 *SameSite=None*&#x200B;이 포함된 쿠키를 거부합니다. 이 기능은 이전 버전의 Chromium에서 파생된 브라우저와 Android WebView에도 영향을 줍니다. 이 동작은 그 당시 쿠키 사양의 버전에 따라 정확했지만 사양에 새 &quot;없음&quot; 값을 추가하면 이 동작이 Chrome 67 이상에서 업데이트되었습니다. (Chrome 51 이전에는 SameSite 속성이 완전히 무시되었으며 모든 쿠키가 *SameSite=None*&#x200B;인 것처럼 처리되었습니다.)
   - 12.13.2 이전 버전의 Android에서 UC 브라우저. 이전 버전은 *SameSite=None*&#x200B;이 포함된 쿠키를 거부합니다. 이 동작은 그 당시 쿠키 사양의 버전에 따라 정확했지만 사양에 새 &quot;없음&quot; 값을 추가하면 이 동작이 최신 버전의 UC 브라우저에서 업데이트되었습니다.
   - macOS 10.14의 Safari 및 포함된 브라우저와 iOS 12의 모든 브라우저 버전. 이 버전에서는 *SameSite=None*&#x200B;으로 표시된 쿠키를 *SameSite=Strict*&#x200B;으로 표시된 것처럼 잘못 처리합니다. 이 버그는 최신 버전의 iOS 및 MacOS에서 수정되었습니다.


1. *Secure* 특성이 있는 쿠키는 *HTTPS*&#x200B;을(를) 통해 전송되어야 합니다. 그렇지 않으면 쿠키가 Adobe Pass 인증 서비스에 도달하지 않습니다.

   - AccessEnabler JavaScript SDK:
      - *sp.auth.adobe.com*&#x200B;과의 통신에서 *2.35* 및 *3.5.0* 버전에 대해 *HTTPS*&#x200B;을(를) 사용한 후에 Dynamic Client Registration을 도입해야 합니다.
   - AccessEnabler iOS/tvOS SDK:
      - *sp.auth.adobe.com*&#x200B;과의 통신에 Dynamic Client Registration을 도입하기 전에 *3.0.0* 이전 버전의 경우 *HTTPS*&#x200B;을(를) 사용해야 합니다.
   - AccessEnabler Android SDK:
      - *sp.auth.adobe.com*&#x200B;과의 통신에 Dynamic Client Registration을 도입하기 전에 *3.0.0* 이전 버전의 경우 *HTTPS*&#x200B;을(를) 사용해야 합니다.
   - AccessEnabler Fireos SDK:
      - *sp.auth.adobe.com*&#x200B;과의 통신에서 버전 *2.0.4*&#x200B;에 대해 *HTTPS*&#x200B;을(를) 사용하는 것은 필수입니다.

</br>

### AccessEnabler JavaScript SDK 버전 2.35 문제 해결 {#235-Troubleshooting}

사용자의 인증 흐름은 Chrome 80 이상에서 영향을 받을 수 있습니다(버전 82 제외). 위의 업데이트로 인해 사용자를 인증하는 데 문제가 없는지 확인하기 위해 다음을 수행할 수 있습니다.

- *JSESSIONID* 쿠키가 브라우저에 설정되어 있고 *SameSite=None* 및 *Secure* 특성이 설정되어 있는지 확인하십시오.
- *https://sp.auth.adobe.com/authenticate/saml* 네트워크 요청의 *JSESSIONID* 쿠키가 *https://sp.auth.adobe.com/session* 네트워크 요청의 *JSESSIONID* 쿠키와 일치하는지 확인하십시오.


### AccessEnabler JavaScript SDK 버전 3.5.0 문제 해결 {#350-Troubleshooting}

사용자의 인증 흐름은 Chrome 80 이상에서 영향을 받을 수 있습니다(버전 82 제외). 위의 업데이트로 인해 사용자를 인증하는 데 문제가 없는지 확인하기 위해 다음을 수행할 수 있습니다.

- *JSESSIONID* 쿠키가 브라우저에 설정되어 있고 *SameSite=None* 및 *Secure* 특성이 설정되어 있는지 확인하십시오.
- *https://sp.auth.adobe.com/authenticate/saml* 네트워크 요청의 *JSESSIONID* 쿠키가 *https://sp.auth.adobe.com/session* 네트워크 요청의 *JSESSIONID* 쿠키와 일치하는지 확인하십시오.
- *pass\_sfp* 쿠키가 브라우저에 설정되어 있고 *SameSite=None* 및 *Secure* 특성이 설정되어 있는지 확인하십시오.
- *pass\_sfp* 쿠키가 *https://sp.auth.adobe.com/session* 네트워크 요청에 설정되어 있는지 확인하십시오.


사용자의 인증 흐름은 Chrome 80 이상에서 영향을 받을 수 있습니다(버전 82 제외). 위의 업데이트로 인해 사용자가 인증된 후 보호된 리소스를 보는 데 문제가 없는지 확인하기 위해 다음을 수행할 수 있습니다.

- *pass\_sfp* 쿠키가 브라우저에 설정되어 있고 *SameSite=None* 및 *Secure* 특성이 설정되어 있는지 확인하십시오.
- *pass\_sfp* 쿠키가 *https://sp.auth.adobe.com/adobe-services/authorize* 네트워크 요청에 설정되어 있는지 확인하십시오.
- *pass\_sfp* 쿠키가 *https://sp.auth.adobe.com/adobe-services/shortAuthorize* 네트워크 요청에 설정되어 있는지 확인하십시오.
