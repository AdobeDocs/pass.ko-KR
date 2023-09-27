---
title: Adobe Pass 인증 지표에서 Clientless deviceType 매개 변수를 사용할 때의 이점
description: Adobe Pass 인증 지표에서 Clientless deviceType 매개 변수를 사용할 때의 이점
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 0%

---

# Adobe Pass 인증 지표에서 Clientless deviceType 매개 변수를 사용할 때의 이점 {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>

## 컨텍스트

선택 사항이지만 매개 변수 `deviceType` 클라이언트 없음 API에서 (있는 경우)는 를 통해 노출되는 Adobe Pass 인증 지표에 사용됩니다. [권한 부여 서비스 모니터링](/help/authentication/entitlement-service-monitoring-overview.md).

를 고려할 때 `deviceType` 매개 변수 및 해당 **이점** 처음에 명시되지 않은 Adobe Pass 인증 지표에 대해 이 기술 노트의 범위는 더 많은 정보를 추가하는 것입니다.

## 설명

다음 `deviceType` 매개 변수는 첫 번째 버전 이후 Clientless API에 표시되었지만 Adobe Pass 인증 지표에 미치는 영향은 최신 릴리스에 추가되었습니다.



>[!IMPORTANT]
>
>매개 변수인 경우 `deviceType` 가 올바르게 설정되고 다음이 포함됩니다 **이익** 권한 부여 서비스 모니터링에서: 다음과 같은 지표를 제공합니다. [장치 유형별 분류](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) 예: Roku, AppleTV, Xbox 등에 대해 다양한 유형의 분석을 수행할 수 있도록 Clientless를 사용할 때


권한 부여 서비스 모니터링 API에 대한 자세한 내용은 다음을 참조하십시오. [드릴다운 트리,](/help/authentication/entitlement-service-monitoring-api.md#drill-down_tree) 를 보여 줍니다. [치수](/help/authentication/entitlement-service-monitoring-overview.md#esm_dimensions) (리소스) ESM 2.0에서 사용 가능.

>[!NOTE]
>
>이 기술 노트의 콘텐츠도 [클라이언트 없는 API](#clientless_device_type).




## 구현

Adobe Pass 인증 지표의 이점을 최대한 활용하려면 다음 두 가지 유형을 사용하십시오. [클라이언트 없는 API](#web_srvs_summary) 현재 사용 중이고 올바른 항목이 있어야 합니다. `deviceType` 설정:

1. 가 있는 API `regcode` 를 필수 매개 변수로 사용하고 `deviceType` 매개 변수: `regcode`, 다음 API 호출 사용:
   - [\&lt;reggie _fqdn=&quot;&quot;>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. 가 있는 API `deviceType` 선택적 매개 변수로:
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/authorize](#init_authz)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/media](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/mediatoken](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/사전 승인](#PreAuthZ_Resources)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/logout](#init_logout)

권장 사항은 `deviceType` 매개 변수를 지정하고 모든 API에 대해 올바른 클라이언트 없는 장치 유형을 전달합니다.
