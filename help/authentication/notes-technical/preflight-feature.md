---
title: Preflight 기능, 문제를 활성화, 문제 해결 또는 확인하는 방법
description: Preflight 기능, 문제를 활성화, 문제 해결 또는 확인하는 방법
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# Preflight 기능: 문제를 활성화, 문제 해결 또는 확인하는 방법 {#preflight-feature}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

Adobe Pass 인증에서 preAuthorizeResources를 계산하는 방식이 변경되었습니다. PreAuthorization API에는 새로운 구현이 있습니다. 이 구현은 여러 인증 호출만 수행하는 기존의 솔루션을 대체합니다.
PreAuthorization API에 대한 외부 인터페이스는 변경되지 않으므로 프로그래머 애플리케이션에서 업데이트가 필요하지 않습니다.

Preflight 리소스를 계산하는 방법에는 세 가지가 있습니다.

* **MVPD에 대한 메서드 포크 및 조인**: 이 작업에는 Adobe이 MVPD에 대해 여러 권한 부여 호출을 수행하는 작업이 포함됩니다(단, 클라이언트는 여전히 한 번의 프리플라이트 호출을 수행해야 함).
* **채널 라인업**: MVPD는 SAML 인증 응답에서 로그인한 사용자에 대한 채널 라인업을 노출하고 Adobe은 이에 따라 승인된 리소스를 반환합니다. SAML 추적기의 SAML authN 응답이 해당 목록을 표시해야 합니다.
* **다중 채널 인증**: 클라이언트 및 Adobe 인증은 모두 리소스 집합에 대해 MVPD를 한 번 호출합니다.

MVPD에 관계없이 클라이언트 애플리케이션은 Preflight 엔드포인트(checkPreauthorizedResources API)에 대한 단일 호출을 수행하여 리소스 ID 세트를 전달합니다. MVPD에서 지원하는 위의 방법 중 하나에 따라 Adobe은 사전 승인된 resourceID를 반환합니다.

Preflight가 포크 및 조인 방법을 기반으로 하는 경우 Adobe Pass 인증 백엔드는 해당 구성에서 &#39;최대 사전 권한 부여 호출&#39;에 대해 설정된 값을 확인합니다. Adobe으로 구성됩니다.

&#39;최대 사전 인증 호출 수&#39; 구성의 기본값은 &#39;5&#39;입니다. 즉, Preflight에서 포크 및 조인 MVPD에 대해 최대 5개의 리소스만 전송할 수 있습니다. 5개가 넘는 리소스를 전달하면 예외가 발생하고 null 목록이 반환됩니다. 이는 예상되는 비헤이비어입니다. MVPD가 채널 라인업이나 다중 채널 인증을 지원하지 않는 경우 이 값을 어떤 값으로든 구성할 수 있지만, 여러 포크 및 조인 인증 호출이 해당 로드 시간을 증가시키므로 이를 컨설팅한 후에만 가능합니다.

따라서 MVPD에 대한 Preflight를 활성화/해결할 때 알아 두어야 할 사항은 다음과 같습니다.

* MVPD가 지원하는 메서드(포크 및 결합, 채널 라인업 또는 다중 채널).
* fork &amp; join만 지원되는 경우 프로그래머는 Preflight 호출에서 전송할 리소스 ID의 수를 요청해야 합니다.
* MVPD는 상담을 받아야 하며, 포크 및 가입 승인 호출의 &#39;n&#39; 수를 만드는 영향을 알고 있어야 합니다. 그런 다음 값이 5보다 큰 경우 config에 값을 구성해야 합니다.

**제한**

AT&amp;T 및 TWC와 같은 일부 MVPD에 대한 Preflight 호출에서 resourceID를 다시 가져오지 않습니다. 단, Preflight 호출에서 보낸 resourceID 목록에 잘못된 ID 또는 인식할 수 없는 ID가 있는 resourceID가 있는 경우에는 해당 목록에 유효하고 승인된 리소스가 있습니다.
