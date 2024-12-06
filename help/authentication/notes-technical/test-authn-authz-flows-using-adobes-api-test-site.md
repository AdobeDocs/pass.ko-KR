---
title: Adobe의 API 테스트 사이트를 사용하여 인증 및 권한 부여 흐름을 테스트하는 방법
description: Adobe의 API 테스트 사이트를 사용하여 인증 및 권한 부여 흐름을 테스트하는 방법
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# Adobe의 API 테스트 사이트를 사용하여 인증 및 권한 부여 흐름을 테스트하는 방법 {#How-to-test-auth-flows}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

AuthN 및 AuthZ 흐름을 테스트하기 위해 사용자가 원하는 **API 테스트 사이트**&#x200B;를 준비했습니다. 우리의 지원 팀은 기꺼이 자격 증명을 제공할 것입니다. **support@tve.zendesk.com**(으)로 연락하세요.


## 1부 {#part-I}

RELEASE 환경에 대한 테스트는 Part II로 바로 건너뜁니다.  사전 자격 환경에서 테스트하려면 [환경 설정 및 사전 자격 조건 테스트](/help/authentication/notes-technical/setting-up-your-environment-and-testing-in-prequal.md)를 참조하십시오.

## 제2편

Part I를 완료한 후 다음 단계를 수행합니다.


1. 웹 페이지를 엽니다. [스테이징 API 테스트](https://sp.auth-staging.adobe.com/apitest/api.html).
1. 다음 URL에서 액세스 Enabler를 로드합니다.
   * [스테이징을 위해 Enabler Javascript 액세스](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * 또는
   * [프로덕션용 Enabler Javascript 액세스](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * 그런 다음 &quot;**액세스 Enabler 로드**&quot; 단추를 클릭합니다.
1. 이제 요청자 ID 값을 &quot;**requestorID**&quot;(으)로 설정하고 &quot;setRequestor&quot; 단추를 클릭하십시오.
1. 그런 다음 &quot;getAuthentication&quot; 버튼을 누르고 디스플레이 선택기가 나타날 때까지 기다립니다.
1. 선택기에서 &quot;**MVPD**&quot; 선택
1. &quot;**MVPD**&quot; 로그인 페이지에 자격 증명을 입력하십시오.
1. 다시 리디렉션된 후 1~3단계를 다시 실행합니다.
1. setAuthenticationStatus에서 3단계를 다시 수행하면 값 &quot;1&quot;이 표시됩니다. 인증이 작동하지 않으면 MVPD 대화 상자가 표시됩니다.
1. 인증을 테스트하려면 리소스 입력 필드에 &quot;**requestorID**&quot;을(를) 입력하고 &quot;getAuthorization&quot; 단추를 클릭하십시오.
1. 따라서 &quot;setToken&quot;-\>&quot;resource id&quot; 텍스트 상자에는 리소스가 표시되고, &quot;setToken&quot;-\>&quot;token&quot; 텍스트 상자에는 authZ가 성공했음을 의미하는 shortAuthorizationToken이 표시됩니다.
1. 이제 &quot;로그아웃&quot; 버튼을 클릭하여 토큰을 삭제할 수 있습니다.
