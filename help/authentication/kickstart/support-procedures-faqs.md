---
title: 지원 절차 FAQ
description: 지원 절차 FAQ
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 0ab1fc212752dd4a4d6e12a4ab1287ef74e4a282
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# 지원 절차 FAQ {#support-procedures-faqs}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 문서에서는 Adobe Pass 인증 및 해당 파트너에 영향을 주는 주요 문제(심각도 1 수준)에 대한 지원 절차에 대한 FAQ에 대해 간략히 설명합니다.

## FAQ {#faqs}

### SEVERITY 1 수준 인시던트란 무엇입니까? {#support-procedures-faqs-1}

SEVERITY 1 수준 인시던트는 하나의 채널 및 하나의 MVPD에 대한 인증 또는 권한 부여 플로우의 완료를 차단하는 프로덕션 환경의 라이브 상황으로, 많은 구독자에게 영향을 줍니다.

심각도 1 인시던트의 예

* 인증 프로세스 중에 사용자가 지원되는 브라우저에서 MVPD을 선택한 후에는 사용자가 로그인 페이지로 리디렉션되지 않습니다.

* 인증 프로세스 중에 사용자가 인증 흐름을 다시 시작하지 못하는 상태로 Adobe 오류 페이지에서 멈춥니다.

* 파트너는 사용자가 특정 MVPD을 인증하거나 승인할 수 없는 다양한 보고서를 수신합니다.

* https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js에서 호스팅하는 프로덕션 Access Enabler를 사용할 수 없습니다.

### 심각도가 아닌 1 수준 인시던트란 무엇입니까?

Adobe은 이러한 문제에 대한 조사를 지원하지만 심각도 1 수준 인시던트로 간주되지 않습니다.

* 한 명 또는 일부 구독자가 인증할 수 없으며 MVPD 로그인 페이지에 남아 있습니다.

* 한 명 또는 몇 명의 구독자가 인증되었지만 비디오를 재생할 수 없습니다.

* 일부 또는 모든 구독자에게 프로그래머 사이트에서 JavaScript 오류가 발생합니다.

### SEVERITY 1 레벨 인시던트는 어떻게 처리됩니까?

SEVERITY 1 수준 인시던트는 Adobe 또는 Adobe Pass 인증 파트너에 의해 시작될 수 있습니다. 각 단계에 대한 단계는 아래에 요약되어 있습니다.

**파트너가 시작한 흐름**

1. 파트너는 Adobe의 즉각적인 주의가 필요한 심각도 1 수준 인시던트를 확인합니다.

1. 파트너가 **tve-support@adobe.com**(으)로 제목 줄의 **긴급 - 문제** 및 다음 정보를 추가하는 전자 메일을 보냅니다.
   * 제목
   * 설명 및 재현 단계
   * OS / 브라우저
   * SDK 및 버전
   * 영향을 받는 장치
   * 영향을 받는 사용자 %
   * 문제를 보여주는 HTTP 추적 또는 장치 로그
   * (선택 사항) 문제를 보여 주는 사용 가능한 스크린샷 또는 비디오 캡처입니다

1. Adobe이 일정 기간 내에 티켓에 응답하지 않으면 파트너는 다음 번호로 전화를 걸 수 있습니다. **1-657-312-4623**.

>[!IMPORTANT]
>
> 티켓 제목에 &quot;URGENT-INCIDENT&quot;를 포함하지 않으면 알림 시스템에서 선택하지 않습니다.

**Adobe 시작 흐름**

Adobe Pass 인증 문제의 경우:

1. Adobe은 내부 문제를 식별하고 추적 시스템에서 티켓을 엽니다.

1. Adobe은 티켓 번호와 예상 문제 영향을 지정하여 파트너의 프로그램 관리자 및 기술 담당자에게 알립니다.

1. Adobe은 문제 해결을 위해 노력하며 영향을 받는 모든 파트너에게 계속 정보를 제공합니다.

파트너 문제(프로그래머/MVPD)의 경우:

1. Adobe은 MVPD 또는 프로그래머 사이트 중 하나와의 통합과 관련된 문제를 식별합니다.

1. Adobe은 해당 파트너와의 지원 절차에 따라 영향을 받는 파트너에게 통지하고 파트너의 지원 조직과 티켓을 엽니다.

1. 영향 분석 중에 Adobe이 문제가 사고 시나리오에 대해 미리 합의된 결정 중 하나에 해당한다고 식별하면 파트너의 입력을 기다리지 않고 적절하게 작동합니다.

1. Adobe은 서비스가 복원되면 파트너의 업데이트와 알림을 기다립니다.

### 사고 시나리오에 대한 사전 합의된 결정은 무엇입니까?

시나리오가 발생할 경우 수행되는 기본 작업이 있는 특정 상황:

|    | 시나리오 | 설명 | 작업 |
|----|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S1 | Adobe은 일반 프로덕션 작업 중 MVPD 통합의 문제를 식별합니다. | 정상적인 프로덕션 작업 중에 Adobe은 인증/권한 부여 흐름을 수행할 수 없는 MVPD 중 하나의 문제를 확인합니다(예: 만료된 인증서, 만료된 SAML 응답, 닫힌 포트, 변경된 매개 변수 등). | Adobe은 영향을 받는 MVPD 및 프로그래머에게 알립니다.  </br></br> Adobe은 영향을 받는 모든 프로그래머에 대해 이 MVPD을 비활성화합니다. </br></br> Adobe은 해당 MVPD과의 합의된 지원 절차에 따라 MVPD으로 티켓을 엽니다. |
| S2 | Adobe은 프로그래머를 위한 새 MVPD을 활성화하고 프로그래머는 시작 날짜 이전에 MVPD을 허용합니다. | Adobe이 프로그래머 사이트에 대해 새 MVPD을 활성화하고 있지 않았더라도 사이트에 선택기에 새 MVPD이 이미 표시됩니다. | Adobe은 예약된 날짜 이전에 선택기에 표시되는 새 MVPD에 대해 프로그래머에게 알립니다. </br></br> 프로그래머는 필요한 경우 선택기에서 제거하기 위한 작업을 수행합니다. |
| S3 | Adobe은 MVPD이 프로덕션에 들어갈 준비가 되지 않았더라도 프로그래머를 위해 새 MVPD을 활성화합니다 | Adobe이 프로그래머를 위한 새 MVPD을 활성화하고 있지만 MVPD이 아직 통합 지원을 배포하지 않았기 때문에 인증/권한 부여 흐름을 수행할 수 없습니다 | Adobe은 프로그래머 </br></br>이(가) 요청한 경우에만 배포를 수행합니다. 모든 테스트가 수행되면 프로그래머는 MVPD의 허가를 확인해야 합니다. |
