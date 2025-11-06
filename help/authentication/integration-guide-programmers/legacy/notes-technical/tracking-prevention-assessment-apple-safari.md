---
title: 추적 방지 평가 Apple Safari
description: 추적 방지 평가 Apple Safari
exl-id: a3362020-92ff-4232-b923-e462868730d5
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '1849'
ht-degree: 0%

---

# (기존) 추적 방지 평가 - Apple Safari {#tracking-prevention-assessment-apple-safari}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 사파리 10 {#safari10}

**세부 정보**

Safari 10부터 기본 브라우저 개인 정보 설정으로 인해 SSO(Single Sign-On), SLO(Single Logout) 및 수동 인증 기능이 작동하지 않습니다. SSO(Single Sign-On) 및 수동 인증은
여러 탭이나 브라우저 창 간에 동일한 세션이 표시됩니다.

이러한 변경 사항은 Adobe Pass 인증 프로세스에 영향을 주고 영향을 줍니다
AccessEnabler JavaScript SDK v2(버전 2.x), v3(버전 3.x), v4(버전 4.x)의 경우

### 완화 {#mitigation-safari10}

이러한 제한을 완화하려면 사용자에게 Safari 10 브라우저 개인 정보 설정을 변경하고, 아래 이미지에 표시된 대로 기본 설정에서 브라우저의 개인 정보 탭에 있는 &quot;**쿠키 및 웹 사이트 데이터**&quot; 항목에 대해 &quot;**항상 허용**&quot; 옵션을 사용할 수 있습니다.

![](../../../assets/always-allow-safari10.png)


## Safar 11 {#safari11}

**세부 정보**

>[!IMPORTANT]
>
>Safari 10 섹션의 위의 모든 세부 사항은 Safari 11의 경우에도 여전히 적용됩니다.

Safari 11부터 브라우저는 사이트 간 추적을 방지하기 위해 휴리스틱스를 사용하는 기술인 [ITP(Intelligent Tracking Prevention)](https://webkit.org/blog/7675/intelligent-tracking-prevention/) 메커니즘을 도입했습니다. 이러한 휴리스틱은 네트워크 호출에서 타사 쿠키가 저장되고 재생되는 방식에 영향을 줍니다. 즉, ITP 메커니즘 활성화에 따라 Safari 브라우저는 클라이언트 - 서버 모델 통신에서 타사 쿠키를 차단합니다.

Adobe Pass Authentication Service는 **기능을 위해 인증 프로세스**&#x200B;의 일부로 쿠키를 사용하고 사용합니다. 인증 프로세스가 자동으로 발생하는 상황(예: 임시 패스) 또는 iFrame 또는 &quot;리디렉션 불가&quot; 기능을 사용하는 구현에서 Adobe의 쿠키는 기본적으로 서드파티 쿠키로 간주되어 차단됩니다. 다른 모든 경우에 대해 Safari는 모든 Adobe의 Pass Authentication 서비스 쿠키를 추적 쿠키로 플래그 지정할 수 있는 머신 러닝 알고리즘을 사용하므로 ITP의 차단이 적용됩니다.

결론적으로, Safari 11 브라우저의 사용자는 ITP(Intelligent Tracking Prevention) 메커니즘이 활성화된 후, 특히 사용자가 여러 Adobe Pass 인증이 활성화된 웹 사이트를 사용하는 경우 Adobe Pass 인증이 활성화된 웹 사이트에서 인증하지 못할 수 있습니다. 따라서 로그인 불능부터 예상 인증 기간보다 짧은 시간 등 사용자의 인증 경험이 예기치 않고 정의되지 않을 수 있습니다.

이러한 변경 사항은 AccessEnabler JavaScript SDK v2(버전 2.x), v3(버전 3.x)의 Adobe Pass 인증 프로세스에 영향을 주며 영향을 줍니다.

### 완화 {#mitigation-safari11}

AccessEnabler JavaScript SDK v3(버전 3.x)과 AccessEnabler JavaScript SDK v4(버전 4.x)의 경우 라이브러리에는 필수 쿠키가 누락되어 사용자 인증이 차단된 상황을 식별할 수 있는 메커니즘이 포함되어 있습니다. 이러한 경우 라이브러리는 특정 오류 콜백 [N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference)을(를) 트리거합니다. 이 콜백은 Adobe Pass 인증이 활성화된 웹 사이트로 다시 전달되어 사용자에게 문제를 완화할 수 있는 조치를 취하도록 지시하는 신호로 사용됩니다. 이 메커니즘을 활용하려면 웹 사이트에서 [오류 보고](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) 사양을 구현해야 합니다.

AccessEnabler JavaScript SDK v2(버전 2.x)의 경우 라이브러리가 위에서 설명한 메커니즘을 제공하지 않으므로 Adobe Pass 인증이 활성화된 웹 사이트에서 문제를 완화하기 위한 조치를 취하도록 사용자에게 지시할 때 알림을 받을 수 없습니다.

앞서 언급한 문제를 완화할 수 있는 작업 목록 **AccessEnabler JavaScript SDK의 세 가지 버전에 모두 적용**&#x200B;됩니다.

구현자의 웹 사이트에서 [N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference) 오류 콜백을 받으면 사용자는 ITP(Intelligent Tracking Prevention)를 사용하지 않도록 설정하고 다음과 같이 서드파티 쿠키를 사용하도록 설정해야 합니다.

* Mac OS X High Sierra 이상의 경우: 아래 이미지에 표시된 대로 기본 설정에서 브라우저의 개인 정보 탭에서 &quot;**웹 사이트 추적**&quot; 항목에 대한 &quot;**사이트 간 추적 방지**&quot; 옵션의 선택을 취소합니다.

  ![](../../../assets/uncheck-prvnt-cr-st-tr-safari11.png)


* Mac OS X Sierra 및 이전 버전의 경우: 아래 이미지에 표시된 대로 환경 설정에서 브라우저의 개인 정보 탭에서 &quot;**쿠키 및 웹 사이트 데이터**&quot; 항목에 대한 &quot;**항상 허용**&quot; 옵션을 선택합니다.

  ![](../../../assets/always-allow-safari11.png)

## 사파리 12 {#safari12}

**세부 정보**

>[!IMPORTANT]
>
>섹션 Safari 10 및 섹션 Safari 11의 위의 모든 세부 사항은 Safari 12의 경우에도 여전히 적용됩니다.

이 섹션에서는 Safari 12에서 **AccessEnabler JavaScript SDK 버전 4.x**&#x200B;의 호환성 문제에 대해 자세히 설명합니다.

>[!NOTE]
>
>AccessEnabler JavaScript SDK 버전 2.x와 AccessEnabler JavaScript SDK 버전 3.x의 경우 둘 다 인증 프로세스에 타사 쿠키를 사용하며, Safari 11부터 시작되는 ITP 및 타사 쿠키 정책으로 인해 로그인 불능에서 예상 인증 기간보다 짧은 시간에 이르기까지 사용자의 인증 환경이 예기치 않고 정의되지 않을 수 있다는 점을 염두에 두십시오.


### Safari 12에서 AccessEnabler JavaScript SDK v4(버전 4.x)의 인증된 기능 {#certified-functionality-of-accessenabler-javacscript=sdk-v4}

버전 4.0부터 AccessEnabler JavaScript SDK은 인증 프로세스에 더 이상 타사 쿠키를 사용하지 않으므로 사용자의 브라우저에서 타사 쿠키가 비활성화되어 있더라도 사용자 상호 작용을 사용하는 **인증** 흐름이 항상 작동합니다.

>[!NOTE]
>
>로그인 팝업을 열거나 MVPD 로그인 페이지와 상호 작용하려면 사용자가 사이트와 상호 작용해야 합니다.

사용자가 이미 인증된 경우 **Authorization/Preflight/User Metadata** 작업이 완전히 작동합니다.

### Safari 12에서 AccessEnabler JavaScript SDK v4(버전 4.x)의 알려진 문제 {#known-issues-of-accessenabler-javascript-sdk-4}

* SSO 및 SLO

   * Safari 10부터 Safari에서 localStorage가 구현되는 방식으로 인해 JS SDK은 더 이상 공통 도메인 iFrame을 통해 로그인 상태를 공유할 수 없습니다. 즉, AccessEnabler JavaScript SDK을 사용하는 모든 사이트에 로그인해야 합니다. 또한 로그아웃해도 사이트 간에 인증 토큰이 삭제되지 않으므로 사용자는 Adobe Pass 인증이 활성화된 각 웹 사이트에서 로그아웃해야 합니다.

* 임시 통과

   * 임시 패스의 경우 AccessEnabler JavaScript SDK은 개별화 메커니즘을 사용하여 인증 토큰을 특정 디바이스(브라우저 인스턴스)에 잠급니다. 추적을 방지하기 위해 설계된 Safari 12의 새로운 메커니즘으로 인해 개별화 메커니즘 **에서 계산 및 사용 중인 지문은 동일한 IP 주소를 가진 모든 사용자에 대해 동일합니다**. 우리는 개별화 목적을 위해 클라이언트 IP를 고려하지만, 그럼에도 불구하고 동일한 공개 IP 주소를 공유하는 사용자들에게 영향을 미칩니다. 이러한 사용자의 경우 동일한 개별화 ID를 계산하고 임시 패스가 여기에 연결됩니다. 즉, 이러한 사용자가 임시 패스를 사용하면 다른 사용자는 해당 패스에 액세스할 수 없습니다. \! 이는 특히 기업 사용자, 교육 기관 또는 인터넷에 액세스하기 위해 NAT 또는 공통 프록시를 사용하는 여러 사용자가 있는 기타 조직에 영향을 미칩니다.

>[!NOTE]
>
>이 문제는 구현자가 사용자 상호 작용의 결과로 임시 패스를 사용하는 경우에만 사용자에게 영향을 줍니다. 그렇지 않으면 임시 패스 인증은 아래의 **자동 흐름**&#x200B;에 속합니다.

* 자동 흐름

   * JS SDK 4.0을 사용할 때 사용자 상호 작용 없이 자동화된 모드로 인증 흐름을 시도하면 Safari 12에서 성공하지 못합니다. 예정된 JS SDK 4.1에서는 자동화된 흐름의 모든 문제를 해결합니다.

이 문제의 영향을 받는 사용 사례:

* 자동 TempPass(무료 미리보기) 인증 - 이러한 흐름에 대해 SDK에서 N130 오류가 발생합니다.

* 수동 인증(자동으로 실패) - 이 MVPD을 선택하고 자격 증명을 입력하라는 메시지가 표시됩니다.

### 완화 {#mitigation-safari12}

**SSO 및 SLO**

이 글을 쓰는 순간에 이용 가능하거나 가능한 알려진 완화는 없다. Apple은 Safari 12(`https://webkit.org/blog/8124/introducing-storage-access-api`)에 &quot;저장소 액세스 API&quot;를 도입했지만 현재 구현은 localStorage에 적용되지 않고 쿠키에만 적용됩니다. 또한 API를 사용하려면 사용자 상호 작용이 필요하며, API를 사용하면 아래 대화 상자와 유사한 권한 대화 상자가 표시됩니다.

![](../../../assets/permission-dialog-apple.png)


이 시점에서는 이러한 Safari 요구 사항/프롬프트가 UX 요구 사항과 일치하지 않으며, 다른 브라우저에서와 같이 일관된 비헤이비어가 없습니다. 여기서 SSO는 일단 공통 도메인 localStorage에 토큰을 저장하면 &quot;작동&quot;합니다.

**임시 통과**

개별화 문제를 완화하고 사용자 상호 작용을 하려면 대화형 방식으로 **[프로모션 임시 패스](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)**&#x200B;를 사용하고 사용자에 대한 하나 이상의 추가 정보(예: 이메일 주소)를 제공하는 것이 좋습니다.

## 사파리 13 {#safari13}

**세부 정보**

>[!IMPORTANT]
>
>섹션 Safari 10부터 섹션 Safari 12까지 위의 모든 세부 사항은 Safari 13의 경우에도 여전히 적용됩니다.


Safari 13부터 브라우저는 [ITP(Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/))에 대한 새로운 변경 사항을 도입하여 사이트 간 추적을 방지하기 위해 서드파티 쿠키를 추적 쿠키로 플래그를 지정하는 과정에서 메커니즘의 추론을 더욱 엄격하게 합니다.

이전 섹션에서 설명한 것처럼 Adobe Pass 인증 서비스는 구현자가 AccessEnabler JavaScript SDK v2(버전 2.x) 및 AccessEnabler JavaScript SDK v3(버전 3.x)을 사용할 때 인증 프로세스의 일부로 타사 쿠키를 사용하고 사용합니다. ITP가 사용자와 관련 당사자(프로그래머의 웹 사이트 및 Adobe) 간의 상호 작용에 대해 &quot;학습&quot;하기 위해 잠시 후에 시작된 Safari 브라우저의 이전 버전과 비교하여 Safari 13 브라우저는 클라이언트 - 서버 모델 통신에서 쿠키를 추적하는 것으로 간주되는 타사 쿠키를 처음부터 차단하고 있습니다.

결론적으로, Safari 13 브라우저의 사용자는 이전 버전의 AccessEnabler JavaScript SDK, v2(버전 2.x) 또는 v3(버전 3.x)을 사용하는 Adobe Pass 인증 지원 웹 사이트에서 새로운 인증을 시작할 수 없을 것입니다. 이는 필요한 모든 Adobe의 Primetime 인증 서비스 쿠키가 ITP에 의해 차단되므로 서비스가 인증 요청을 이행할 수 없기 때문에 발생합니다.

AccessEnabler JavaScript SDK v4(버전 4.x) 라이브러리는 인증 프로세스에 타사 쿠키를 사용하지 않으므로 Safari 13 변경 사항의 영향을 받지 않습니다.

### 완화 {#mitigation-safari13}

무엇보다도 Safari 브라우저에서 안정적이고 예측 가능한 동작을 갖도록 **AccessEnabler JavaScript SDK 버전 4.x로 마이그레이션**&#x200B;하는 것이 좋습니다.

둘째, AccessEnabler JavaScript SDK v3(버전 3.x)의 경우 라이브러리는 필수 쿠키가 누락되어 사용자 인증이 차단된 상황을 식별할 수 있는 메커니즘을 포함합니다. 이러한 경우 라이브러리는 문제를 완화할 수 있는 작업을 수행하도록 사용자에게 지시하기 위한 신호로 사용하기 위해 Adobe Pass 인증이 활성화된 웹 사이트로 다시 전달되는 특정 오류 콜백([N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference))을 트리거합니다. 이 메커니즘을 활용하려면 웹 사이트에서 [오류 보고](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) 사양을 구현해야 합니다.

AccessEnabler JavaScript SDK v2(버전 2.x)의 경우 라이브러리가 위에서 설명한 메커니즘을 제공하지 않으므로 Adobe Pass 인증이 활성화된 웹 사이트에서 문제를 완화하기 위한 조치를 취하도록 사용자에게 지시할 때 알림을 받을 수 없습니다.

구현자의 웹 사이트에서 [N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference) 오류 콜백을 받으면 사용자는 ITP(Intelligent Tracking Prevention)를 사용하지 않도록 설정하고 다음과 같이 서드파티 쿠키를 사용하도록 설정해야 합니다.

* Mac OS X High Sierra 이상의 경우: 아래 이미지에 표시된 대로 기본 설정에서 브라우저의 개인 정보 탭에서 &quot;**웹 사이트 추적**&quot; 항목에 대한 &quot;**사이트 간 추적 방지**&quot; 옵션의 선택을 취소합니다.

  ![](../../../assets/prvnt-cross-site-tr-safari13.png)

* Mac OS X Sierra 및 이전 버전의 경우: 아래 이미지에 표시된 대로 기본 설정에서 브라우저의 개인 정보 탭에 있는 &quot;</span>쿠키 및 웹 사이트 데이터&#x200B;**&quot; 항목에 대한 &quot;**&#x200B;항상 허용&#x200B;**&quot; 옵션을 선택합니다.**

  ![](../../../assets/always-allow-safari13.png)
