---
title: Adobe Pass 인증 지표에서 Clientless deviceType 매개 변수를 사용할 때의 이점
description: Adobe Pass 인증 지표에서 Clientless deviceType 매개 변수를 사용할 때의 이점
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 0%

---

# (기존) Adobe Pass 인증 지표에서 Clientless deviceType 매개 변수 사용의 이점 {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

</br>

## 컨텍스트

선택 사항이지만 Clientless API의 매개 변수 `deviceType`이(가) 존재하는 경우 [자격 서비스 모니터링](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)을 통해 노출되는 Adobe Pass 인증 지표에 사용됩니다.

Adobe Pass 인증 지표에 있는 `deviceType` 매개 변수와 해당 **이점** 간의 연결이 처음에 언급되지 않았음을 고려하면 이 기술 노트의 범위는 이러한 매개 변수에 대한 정보를 더 추가하는 것입니다.

## 설명

`deviceType` 매개 변수는 첫 번째 버전 이후 Clientless API에 존재했지만 Adobe Pass 인증 지표에 미치는 영향이 최신 릴리스에 추가되었습니다.



>[!IMPORTANT]
>
>매개 변수 `deviceType`이(가) 올바르게 설정된 경우 권한 부여 서비스 모니터링에 다음 **혜택**&#x200B;이(가) 있습니다. Clientless를 사용할 때 [장치 유형별로 분류된](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) 지표를 제공하므로 Roku, AppleTV, Xbox 등과 같은 다양한 유형의 분석을 수행할 수 있습니다.


권한 부여 서비스 모니터링 API에 대한 자세한 내용은 ESM 2.0에서 사용할 수 있는 [차원](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md#drill-down_tree)(리소스)을 보여 주는 [드릴다운 트리,](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#esm_dimensions)를 참조하십시오.

>[!NOTE]
>
>이 기술 노트의 콘텐츠가 [클라이언트 없는 API](#clientless_device_type)에도 추가되었습니다.




## 구현

Adobe Pass 인증 지표를 최대한 활용하려면 현재 사용 중이고 올바른 [을(를) 설정해야 하는 두 가지 유형의 &#x200B;](#web_srvs_summary)클라이언트 없는 API`deviceType`가 있습니다.

1. `regcode`이(가) 필수 매개 변수로 사용되고 다음 API 호출과 함께 `deviceType`을(를) 만들 때 설정된 `regcode` 매개 변수를 사용하는 API:
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/](#reg_serv)

1. `deviceType`을(를) 선택적 매개 변수로 사용하는 API:
   - [\&lt;SP\_FQDN\>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/권한 부여](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/토큰/인증](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/토큰/미디어](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/사전 권한 부여](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/로그아웃](#init_logout)

`deviceType` 매개 변수를 사용하고 모든 API에 대해 올바른 Clientless 장치 유형을 전달하는 것이 좋습니다.
