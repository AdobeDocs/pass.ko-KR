---
title: 사전 영업 시 환경 설정 및 테스트
description: 사전 영업 시 환경 설정 및 테스트
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: 3a6a5633c728398a3847ee3e341e82aba915f0d9
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Pre-Qual에서 환경 설정 및 테스트{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>이 페이지의 내용은 정보 제공의 목적으로만 제공됩니다. 이 API를 사용하려면 Adobe의 최신 라이선스가 필요합니다. 무단 사용은 허용되지 않습니다.

이 TechNote의 목적은 파트너가 환경을 설정하고 Adobe 사전 검증 환경에 배포된 새 빌드를 테스트할 수 있도록 지원하는 것입니다.

프로덕션&#x200B;***과***&#x200B;스테이징&#x200B;***이라는 두 가지 빌드 버전이***&#x200B;있으므로 이 문서에서는 스테이징에 대한 모든 단계가 동일하고 URL만 다르다는 언급과 함께 프로덕션 설정에 중점을 둘 것입니다.

1단계와 2단계는 시험기 중 하나에서 테스트 환경을 설정하고, 3단계는 기본 흐름을 검증하며, 4단계와 5단계는 몇 가지 테스트 지침을 제시합니다.

>[!IMPORTANT]
>
> 테스트 환경을 변경할 때마다 1단계와 2단계를 실행하는 것이 매우 중요합니다(스테이징에서 프로덕션 프로필로 또는 그 반대로 전환)


## 1단계. IP에 도메인 전달 확인 {#resolving-pass-domain-to-an-ip}

* 스푸핑에 사용할 수 있는 로드 밸런서 IP를 찾으려면 다음 명령을 실행합니다.

* **Windows에서**

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

* **리눅스/맥**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

>[!NOTE]
>
>도메인은 관련성이 없고 사용자마다 다를 수 있으므로 답변에서 제외됩니다.

>[!IMPORTANT]
>
> 이러한 IP 주소는 향후 변경될 수 있으며 다른 지리적 지역의 사용자에게는 동일하지 않을 수 있습니다.


## 2 단계.  프로덕션이 될 사전 검증 환경 스푸핑 {#spoofing-the-prequalification-environment}

* *c:\\windows\\System32\\drivers\\etc\\hosts* 파일(Windows의 경우) 또는 */etc/hosts* 파일(Macintosh/Linux/Android의 경우)을 편집하고 다음을 추가합니다.

* 스푸핑 프로덕션 프로필
   * 52.13.71.11 entitlement.auth.adobe.com sp.auth.adobe.com api.auth.adobe.com

**Android에서 스푸핑:** Android에서 스푸핑하려면 Android 에뮬레이터를 사용해야 합니다.

* 스푸핑이 적용되면 프로덕션 및 스테이징 프로필에 대한 일반 URL(즉, `http://sp.auth-staging.adobe.com` 및 `http://entitlement.auth-staging.adobe.com`)만 사용할 수 있으며 실제로 새 빌드의 *사전 검증 환경/ 프로덕션*&#x200B;에 도달하게 됩니다.


## 3단계.  올바른 환경을 가리키는지 확인합니다. {#Verify-you-are-pointing-to-the-right-environment}

**이것은 쉬운 단계입니다.**

* 로드 [인타이틀먼트 prequal environment](https://entitlement-prequal.auth.adobe.com/environment.html) and [entitlement](https://entitlement.auth.adobe.com/environment.html). 동일한 응답을 반환해야 합니다.


## 4 단계.  프로그래머의 웹 사이트를 사용하여 간단한 인증/권한 부여 흐름 수행 {#peform-a-simple-auth-flow}

* 이 단계에는 프로그래머의 웹 사이트 주소와 유효한 MVPD 자격 증명(인증되고 권한이 부여된 사용자)이 필요합니다.

## 5 단계.  프로그래머의 웹 사이트를 사용하여 시나리오 테스트 수행 {#perform-scenario-testing-using-programmer-website}

* 환경 설정을 완료하고 기본 인증-권한 부여 흐름이 작동하는지 확인한 후 더 복잡한 시나리오의 테스트를 진행할 수 있습니다.


## 6단계.  API 테스트 사이트를 사용하여 테스트 수행 {#perform-testing-using-api-testing-site}

* Adobe Pass 인증 테스트에 더 자세히 알아보려면 [API 테스트 사이트](http://entitlement-prequal.auth.adobe.com/apitest/api.html)를 사용하는 것이 좋습니다.

API 테스트 사이트에 대한 자세한 내용은 [Adobe의 API 테스트 사이트를 사용하여 인증 및 권한 부여 흐름을 테스트하는 방법](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md)을 참조하십시오.
