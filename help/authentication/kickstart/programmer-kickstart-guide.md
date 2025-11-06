---
title: 프로그래머 킥스타트 안내서
description: 프로그래머 킥스타트 안내서
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# 프로그래머 킥스타트 안내서 {#programmer-kickstart-guide}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 킥스타트 안내서는 Adobe® 인증 전달을 웹 사이트 또는 애플리케이션에 통합하려는 콘텐츠 공급자(프로그래머)를 대상으로 합니다.

이 문서에서는 통합 프로세스를 원활하고 효율적으로 시작하기 위한 주요 초기 단계를 간략하게 설명합니다. 이는 성공적인 통합을 위해 파트너와 협력할 방법에 대한 지침을 제공하고 기대치를 명확히 하는 것을 목표로 합니다.

Adobe은 Adobe Pass 인증을 웹 사이트 또는 애플리케이션에 통합하는 데 도움이 되는 다양한 리소스를 제공합니다. **을(를) 참조하세요.&quot;**&#x200B;을(를) 제공하고 **&quot;Adobe은 아래 각 섹션에서 &quot;**&quot;을(를) 제공합니다.

## 설정 프로세스 {#setup-process}

설정 프로세스에는 다음 단계가 포함됩니다.

![Adobe® 인증 통합 프로세스 통과](../assets/progr-flow-int-lifecycle.png)

*Adobe® 인증 통합 프로세스 통과*

**킥오프 단계 동안**&#x200B;을(를) 제공합니다.

* **서비스 공급자(요청자 식별자)**

  Adobe Pass 인증에 요청하는 웹 사이트 또는 애플리케이션의 브랜드를 고유하게 식별하는 문자열입니다. 문자열 자체는 임의적이지만 Adobe과 프로그래머 간에 동의해야 합니다

* **채널 정보**

  서비스 공급자가 요청한 콘텐츠 채널을 식별하는 데 사용되는 문자열 집합입니다. 많은 경우 채널과 서비스 제공자는 동일합니다. 그러나 단일 식별자는 여러 콘텐츠 채널을 나타낼 수 있습니다. 이러한 채널 이름 문자열은 해당 케이블 TV 채널과 일치해야 합니다. 일부 MVPD는 인증 및/또는 권한 부여 프로세스 중에 이 값을 검증할 수 있습니다.

* **도메인 이름**

  이 목록에는 서비스 공급자를 나타내는 Adobe에 나열된 실제 도메인 이름이 포함됩니다. 이렇게 하면 인증된 도메인만 메타데이터를 사용하여 Adobe Pass 인증에 액세스할 수 있습니다. 프로덕션 환경과 스테이징(테스트) 환경 모두에 대한 도메인 이름은 다를 수 있으므로 제공하고 명확하게 식별해야 합니다.

**MVPD을 통해**&#x200B;을(를) 제공합니다.

* **자격 증명 집합**

  MVPD을 사용하여 사용자를 인증 및 승인하거나 단독으로 인증하는 데 사용되는 자격 증명입니다. 일반적으로 이러한 자격 증명은 MVPD이 두 프로필(스테이징 및 프로덕션) 모두에 대해 제공할 사용자 이름과 암호로 구성됩니다.

* **리소스 식별자**

  서비스 공급자가 보호하려는 콘텐츠 채널, 쇼, 에피소드 또는 에셋에 대한 고유 식별자입니다. 이러한 식별자는 권한 부여 결정을 요청하는 데 사용되며 MVPD과 동의해야 합니다.

>[!IMPORTANT]
>
> 프로그래머는 필요한 비즈니스 계약을 마무리하기 위해 MVPD과 협력할 책임이 있습니다. 한편, Adobe Pass 인증은 기술 통합이 올바르게 설정되도록 MVPD과 공동 작업합니다.
>
> * **새 MVPD**
>
>     MVPD이 Adobe과 통합되지 않은 경우 MVPD 관련 요구 사항을 기반으로 사용자 지정 코드를 개발해야 합니다. 이 개발이 완료될 때까지 MVPD을 사용할 수 없으며 해당 MVPD과의 제품 테스트를 진행할 수 없습니다.
>
> * **기존 MVPD**
>
>     MVPD이 이미 Adobe과 통합된 경우 연결 프로세스가 상당히 간소화됩니다. 대부분의 경우 광범위한 개발이 아닌 구성 조정을 통해 신속하게 연결을 설정할 수 있습니다.
>
> 최종 사용자는 결국 MVPD의 고객이므로 모든 통합에는 MVPD의 테스트를 포함하여 공동 품질 보증(QA) 노력이 필요합니다. 테스트 주기를 조정하는 것은 종종 MVPD의 리소스 가용성에 따라 다르며, 이로 인해 잠재적 지연이 발생할 수 있습니다.

## 고객 지원 액세스 {#access-customer-support}

**Adobe은** Zendesk[을(를) 통해 고객 지원 시스템에 ](https://tve.zendesk.com/home) 액세스를 제공합니다. Zendesk에 액세스하려면 https://tve.zendesk.com/home에서 계정을 등록 및 생성해야 합니다. 등록할 수 있는 사용자 수에는 제한이 없습니다. 등록되면 제출된 티켓에 대한 의견을 보고 공유할 수 있습니다.

통합 프로세스 중에 발생할 수 있는 질문이나 기술 문제에 대해 Adobe Pass 인증 팀이 도움을 줄 수 있습니다. [tve-support@adobe.com](mailto:tve-support@adobe.com)(으)로 문의하십시오.

## 설명서 액세스 {#access-documentation}

**Adobe은** Adobe Experience League[를 통해 ](https://experienceleague.adobe.com/en/docs/pass/authentication/home) 공개 설명서에 대한 액세스 권한을 제공합니다.

Adobe Pass 인증 팀은 [프로그래머를 위한 통합 안내서](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md) 섹션에서 사용 가능한 기능 및 API에 대한 포괄적인 설명서를 제공합니다. 각 주제에 대한 자세한 정보에 대한 링크는 이 섹션의 목차를 참조하십시오.

## 테스트 도구에 액세스 {#access-testing-tool}

**Adobe은** Adobe Developer[ 웹 사이트를 통해 API 탐색 도구에 대한 ](https://developer.adobe.com/adobe-pass/) 액세스를 제공합니다.

## 구성 관리 도구에 액세스 {#access-configuration-management-tool}

**Adobe은** Adobe Pass TVE 대시보드[를 통해 구성 및 데이터를 관리하기 위한 셀프서비스 도구에 대한 액세스 권한을 ](https://experience.adobe.com/pass/authentication)제공합니다.

Adobe Pass 인증 팀은 [TVE 대시보드의 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) 섹션 아래에 TVE 대시보드 사용에 대한 포괄적인 설명서를 제공합니다. 각 주제에 대한 자세한 정보에 대한 링크는 이 섹션의 목차를 참조하십시오.
