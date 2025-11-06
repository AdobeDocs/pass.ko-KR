---
title: MVPD 킥스타트 안내서
description: MVPD 킥스타트 안내서
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# MVPD 킥스타트 안내서 {#mvpd-kickstart-guide}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 킥스타트 안내서는 Adobe® 패스 인증과 통합할 예정인 다채널 MVPD(비디오 프로그래밍 디스트리뷰터)를 위한 것입니다.

이 문서에서는 통합 프로세스를 원활하고 효율적으로 시작하기 위한 주요 초기 단계를 간략하게 설명합니다. 이는 성공적인 통합을 위해 파트너와 협력할 방법에 대한 지침을 제공하고 기대치를 명확히 하는 것을 목표로 합니다.

Adobe은 Adobe Pass 인증과 통합하는 데 도움이 되는 다양한 리소스를 제공합니다. **을(를) 참조하세요.&quot;**&#x200B;을(를) 제공하고 **&quot;Adobe은 아래 각 섹션에서 &quot;**&quot;을(를) 제공합니다.

>[!CAUTION]
>
> 사용자가 권한 흐름을 시작할 때마다 불투명하고 고유한 단일 사용자 ID가 할당됩니다. MVPD에서 가져온 이 ID는 프로그래머 앱 내에서 사용자를 식별하는 데 사용됩니다.
>
> <br/>
>
> 사용자 ID에는 사용자를 식별, 연락 또는 찾기 위해 단독으로 또는 다른 세부 정보와 함께 사용할 수 있는 PII(개인 식별 정보) 또는 데이터가 포함되어서는 안 됩니다.

## 설정 프로세스 {#setup-process}

설정 프로세스에는 다음 단계가 포함됩니다.

![Adobe® 인증 통합 프로세스 통과](../assets/mvpd-int-lifecycle.png)

*Adobe® 인증 통합 프로세스 통과*

### 개시 {#kickoff}

**킥오프 단계 동안**&#x200B;을(를) 제공합니다.

* **표시 이름**

  사용자가 유료 TV 공급자를 선택하라는 메시지를 표시할 때 프로그래머 웹 사이트 또는 애플리케이션에 표시되는 문자열입니다.

* **로고 URL**

  이 파일은 112 x 33 픽셀로, 사용자에게 유료 TV 공급자를 선택하라는 메시지가 표시될 때 프로그래머 웹 사이트 또는 애플리케이션에 표시되는 로고가 들어 있습니다.

* **TTL(Time-to-Live)**

  TTL은 일반적으로 인증 또는 권한 부여 프로세스의 일부로 MVPD에 의해 설정되는 값입니다. 그러나 Adobe은 이러한 TTL 값을 재정의하고 프로그래머와 MVPD 모두에서 동의한 것에 따라 다른 값을 제공할 수 있습니다.

* **자격 증명 집합**

  MVPD을 사용하여 사용자를 인증 및 승인하거나 단독으로 인증하는 데 사용되는 자격 증명입니다. 일반적으로 이러한 자격 증명은 사용자 이름과 암호로 구성되며, 두 프로필(스테이징 및 프로덕션) 모두에 대해 제공해야 합니다.

### 메타데이터 교환(SAML) {#metadata-exchange-saml}

**Adobe에서 메타데이터 교환 단계 중**&#x200B;을(를) 제공합니다.

* **스테이징 환경 메타데이터**

  Adobe의 SP 메타데이터는 https://sp.auth-staging.adobe.com/sp/metadata에서 검색할 수 있습니다.

* **프로덕션 환경 메타데이터**

  Adobe의 SP 메타데이터는 https://sp.auth.adobe.com/sp/metadata에서 검색할 수 있습니다.

**메타데이터 교환 단계에서**&#x200B;을(를) 제공합니다.

* **스테이징 메타데이터**

  스테이징 환경에 대한 MVPD의 메타데이터입니다.

* **프로덕션 메타데이터**

  프로덕션 환경에 대한 MVPD의 메타데이터입니다.

### 연결 {#connectivity}

**Adobe Pass 인증에서는 인증 및 권한 부여 프로세스 동안 제한된 리소스에 대한 액세스를 허용하기 위해 포트 80 및 443을 통한 트래픽을 허용하는 방화벽이 필요하므로 Adobe에서 IP를 허용 목록에 추가하다하는 방법을 제공합니다**.

**연결을 테스트하기 위해 스테이징 프로필에 배포를 제공**&#x200B;합니다.

### 개발 {#development}

**Adobe은 기술 통합이 올바르게 설정되었는지 확인하기 위해 MVPD과 긴밀하게 작업할 수 있는 엔지니어링 시간을**&#x200B;제공합니다. 이 프로세스에는 MVPD의 특정 요구 사항에 맞게 사용자 지정 코드를 개발하는 작업이 포함됩니다.

### 스테이징에서 배포 {#deployment-staging}

**Adobe은 QUAL 이전 스테이징 환경에서 먼저 배포되는 필수 코드 업데이트가 포함된 빌드를 제공**&#x200B;합니다. 이 단계에서는 테스트 목적으로 MVPD을 `TestDistributors` 서비스 공급자와 통합하기 위해 필요한 구성 변경도 구현됩니다.

**귀하와 Adobe은 통합 이전 스테이징 환경에서 성공적으로 테스트될 수 있도록** 품질 보증(QA) 시간을 제공합니다. 이 단계 후에는 실제 프로그래머와의 추가 테스트를 위해 MVPD이 릴리스 스테이징 환경으로 이동됩니다.

### 프로덕션 배포 {#deployment-production}

**연결을 테스트하기 위해 프로덕션 프로필에 배포를 제공**&#x200B;합니다.

**Adobe은 PRE-QUAL 프로덕션 환경에 배포되는 필수 코드 업데이트가 포함된 빌드를 제공**&#x200B;합니다.

**귀하와 Adobe은 프로덕션 프로필을 사용하여 통합을 성공적으로 테스트할 수 있도록** 품질 보증(QA) 시간을 제공합니다. 이 시점에서 모든 것이 괜찮다면 Adobe은 모든 사용자가 사용할 수 있는 릴리스 프로덕션 환경(&quot;라이브&quot;)으로 통합을 이동할 수 있습니다.

>[!IMPORTANT]
>
> 릴리스 프로덕션 환경에서 통합이 실행되면 최적의 고객 경험을 유지하는 것이 무엇보다 중요합니다. 서버 다운 시나리오를 효과적으로 해결하려면 MVPD는 이러한 문제를 관리하기 위해 Adobe에 자세한 에스컬레이션 절차 문서를 제공해야 합니다.
>
> 그 대신 Adobe은 MVPD가 최신 버전의 Adobe Pass 인증 에스컬레이션 프로세스를 수신하여 문제를 능률적으로 해결하도록 합니다.

## 환경에 대한 액세스 {#access-environments}

**Adobe은(는) 개발 프로세스의 여러 단계를 위해 환경에 대한** 액세스를 제공합니다.

* **사전 자격(PRE-QUAL)**

  PRE-QUAL 환경은 다음 릴리스 후보를 호스팅하며 새로운 파트너를 위한 초기 통합 플랫폼 역할을 합니다. 릴리스 환경으로 이전하기 전에 파트너에게는 PRE-QUAL에서 통합을 테스트할 시간이 제공됩니다.

* **릴리스(릴리스)**

  RELEASE 환경은 안정적인 현재 프로덕션 빌드를 호스팅합니다.

이러한 환경을 사용하는 방법에 대한 자세한 내용은 [Adobe 환경 이해](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md) 설명서를 참조하십시오.

>[!IMPORTANT]
> 
> 이러한 환경에 대한 구성 변경은 설정된 변경 요청 프로세스에 따라 Adobe 담당자를 통해 명시적으로 요청해야 합니다.

## 고객 지원 액세스 {#access-customer-support}

**Adobe은** Zendesk[을(를) 통해 고객 지원 시스템에 ](https://tve.zendesk.com/home) 액세스를 제공합니다. Zendesk에 액세스하려면 https://tve.zendesk.com/home에서 계정을 등록 및 생성해야 합니다.

Adobe Pass 인증 팀은 통합 프로세스 중에 발생할 수 있는 모든 질문이나 기술 문제를 처리할 수 있습니다. [tve-support@adobe.com](mailto:tve-support@adobe.com)(으)로 문의하십시오.

## 설명서 액세스 {#access-documentation}

**Adobe은** Adobe Experience League[를 통해 ](https://experienceleague.adobe.com/en/docs/pass/authentication/home) 공개 설명서에 대한 액세스 권한을 제공합니다.

Adobe Pass 인증 팀은 [MVPD에 대한 통합 안내서](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md) 섹션에서 사용 가능한 기능 및 워크플로에 대한 포괄적인 설명서를 제공합니다. 각 주제에 대한 자세한 정보에 대한 링크는 이 섹션의 목차를 참조하십시오.

## 테스트 도구에 액세스 {#access-testing-tool}

**Adobe은** Adobe Developer[ 웹 사이트를 통해 API 탐색 도구에 대한 ](https://developer.adobe.com/adobe-pass/) 액세스를 제공합니다.
