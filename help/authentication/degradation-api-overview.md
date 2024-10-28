---
title: 저하 API 개요
description: 저하 API 개요
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 95c3b1cbce4a591ce387ae3b242721e50ba2ddb1
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# 저하 API 개요 {#degradation-api-overview}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> Degradation API를 사용하기 전에 다음 전제 조건이 충족되는지 확인하십시오.
>
> * [클라이언트 자격 증명 검색](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API 설명서에 설명된 대로 클라이언트 자격 증명을 가져옵니다.
> * [액세스 토큰 검색](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API 설명서에 설명된 대로 액세스 토큰을 가져옵니다.
>
> 등록된 응용 프로그램을 만들고 소프트웨어 문을 다운로드하는 방법에 대한 자세한 내용은 [동적 클라이언트 등록 개요](./dcr-api/dynamic-client-registration-overview.md) 설명서를 참조하십시오.

## 일반 정보 {#general_info}

>[!NOTE]
>
>이 API는 일반적으로 사용할 수 없습니다. 가용성 업데이트는 Adobe 담당자에게 문의하십시오.

이 기능은 특정 MVPD 인증 및 권한 부여 끝점을 일시적으로 우회하는 기능이 있는 통합의 세 가지 주요 당사자(프로그래머, MVPD 및 Adobe)를 제공합니다. 일반적으로 이러한 작업을 시작하는 것은 프로그래머이지만 누가 열화 이벤트를 트리거하는지에 관계없이, 작업은 영향을 받는 MVPD와의 이전에 합의된 배열에 따라 달라집니다.

이 기능의 주요 사용 사례는 라이브 스포츠 또는 큰 이벤트 동안 발생합니다. 이러한 높은 트래픽 시나리오에서 특정 MVPD 엔드포인트의 로드가 너무 높아져 사용자의 응답 시간이 매우 길어질 수 있습니다. 이러한 시나리오 동안 양호한 사용자 경험을 유지하기 위해, 프로그래머는 사용자를 일시적으로 자동 인증/자동 승인할 수 있는 저하 규칙을 트리거하거나, 가용 MVPD 목록에서 제거하여 MVPD를 비활성화하기로 결정할 수 있다.

성능 저하 규칙은 고정된 기간 동안만 적용됩니다. 이 기능의 주요 고객은 스포츠 채널과 라이브 뉴스 채널이지만, MVPDs 서비스가 수시로 감소하므로 모든 프로그래머가 이 기능에 액세스할 수 있습니다.

성능 저하 참고 사항:

- 이 기능은 사용 모니터링 API와 함께 사용하도록 설계되었습니다. 이 API는 MVPD당 인증 및 권한 부여 수, 평균 인증 지연 시간 및 전체 서비스 개요에 필요한 기타 지표에 대한 실시간 정보를 제공합니다.
- 이 기능을 사용하면 Adobe Pass 인증 서비스를 우회할 수 없습니다. Adobe Pass 인증이 다운된 경우 서비스 내에서 사용자가 콘텐츠를 볼 수 있도록 하는 데 사용할 수 있는 메커니즘이 없습니다. 그러나 사이트 또는 앱은 Adobe Pass 인증 주변을 스스로 라우팅할 수 있습니다.
- Adobe이 현재 바로 성능 저하를 트리거하지 않습니다. 결정은 항상 MVPD와 이러한 조건에 동의한 특정 프로그래머와 함께 있어야 합니다. 향후 Adobe Pass 인증은 MVPD와 계약(SLA 보호)에 도달할 수 있는 경우 성능 저하 규칙을 트리거할 때 사전 예방적일 수 있습니다.

<!--
## Related Information {#related}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
