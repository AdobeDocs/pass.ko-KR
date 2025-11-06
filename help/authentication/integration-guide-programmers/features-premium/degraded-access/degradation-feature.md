---
title: 성능 저하 기능
description: 성능 저하 기능
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 49a6a75944549dbfb062b1be8a053e6c99c90dc9
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# 성능 저하 기능 {#degradation-feature}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

라이브 스포츠 및 주요 이벤트의 동적 세계에서 원활한 뷰어 경험을 보장하는 것은 필수적입니다. 이러한 이벤트 동안 트래픽이 많으면 다채널 비디오 프로그래밍 디스트리뷰터(MVPD) 인증 및 권한 부여 종단점을 압도하여 지연 또는 중단으로 이어질 수 있습니다.

Adobe Pass 인증은 특정 MVPD 인증 및 권한 부여 끝점을 일시적으로 우회할 수 있도록 하는 솔루션인 **성능 저하 기능**&#x200B;을 통해 이러한 문제를 해결합니다. 이 기능은 특히 MVPD 시스템의 과부하로 인해 응답 시간이 저하될 수 있는 최대 트래픽 이벤트 동안 중요합니다.

**성능 저하 기능**&#x200B;은(는) 프로그래머를 위한 중요한 보호 기능이 될 수 있으므로 서비스의 연속성을 보장합니다. 기본 대상에는 라이브 스포츠 및 뉴스 채널이 포함되어 있지만 이 유틸리티는 MVPD 종단점으로 인해 발생하는 중단 위험을 완화하려는 모든 프로그래머에게 확장됩니다.

>[!IMPORTANT]
>
> 저하 API는 프리미엄 기능이며, Adobe의 현재 라이선스가 필요합니다.

성능 저하 규칙을 적용하여 프로그래머는 일시적으로 자동 인증 또는 자동 인증을 활성화하여 성능 저하가 적용되는 기간 동안 콘텐츠에 중단 없이 액세스할 수 있습니다. 저하 동작은 항상 MVPD와의 사전 약속에 따라 프로그래머가 시작합니다. 현재 Adobe이 직접 성능 저하를 트리거하지는 않지만 SLA(서비스 수준 계약)가 설정된 경우 향후 기능에 사전 예방적 관리가 포함될 수 있습니다.

이 기능은 [사용 모니터링 API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)와 함께 사용하도록 설계되었으며 MVPD와의 사전 계약에 따라 Adobe Pass 인증은 중요한 순간 동안 사용자 경험, 안정성 및 운영 제어의 균형을 맞출 수 있는 강력한 도구를 제공합니다.

>[!IMPORTANT]
>
> 이 기능은 Adobe Pass 인증 서비스 자체를 우회하는 것을 허용하지 않습니다. Adobe Pass 인증을 사용할 수 없는 경우 이 서비스에는 사용자 액세스를 용이하게 하는 기본 제공 메커니즘이 없습니다. 이러한 경우, 사이트 또는 애플리케이션은 콘텐츠 전달을 유지하기 위해 자체 대체 라우팅 솔루션을 구현하도록 선택할 수 있습니다.

## API 액세스 저하 {#degradation-api-access}

[Degradation API](#degradation-api)에 액세스하려면 DCR(Dynamic Client Registration) 프로세스의 필수 단계를 완료해야 합니다. 이 필수 프로세스를 사용하면 성능 저하 API와 상호 작용하는 데 필요한 액세스 토큰을 보유할 수 있습니다.

포괄적인 지침은 [동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) 설명서를 참조하십시오.

## 저하 API {#degradation-api}

Degradation API는 프로그래머가 특정 MVPD에 대한 저하 규칙을 관리할 수 있는 RESTful API입니다. API는 활성화, 제거 및 활성 상태의 저하 규칙 상태를 검색할 수 있는 수단을 제공합니다.

Degradation API에 대한 자세한 내용은 다음 Zendesk 문서 [Adobe Pass 인증을 참조하십시오 | API v3](https://tve.zendesk.com/hc/en-us/articles/33912526308372-Adobe-Pass-Authentication-Degradation-API-v3)을(를) 저하하고 다운로드할 PDF 파일을 찾습니다.

## REST API V2 {#rest-api-v2}

저하 기능을 활용하려면 TVE(TV Everywhere) 응용 프로그램이 Adobe Pass 인증 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)과(와) 상호 작용하는 방법을 수정하기 위해 코드 업데이트를 구현해야 합니다.

이러한 업데이트 및 관련 워크플로에 대한 포괄적인 안내서는 [액세스 흐름 저하](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md) 설명서를 참조하십시오.
