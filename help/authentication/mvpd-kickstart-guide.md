---
title: MVPD 직접 통합 계획
description: MVPD 직접 통합 계획
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# MVPD 킥스타트 안내서: MVPD 직접 통합 계획 {#mvpd-dir-int-plan}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 소개 {#mvpd-kickstart-intro}

TV Everywhere용 Adobe Pass 인증에 오신 것을 환영합니다.  귀사와 함께 일할 수 있기를 기대합니다.

>[!NOTE]
>
>멀티채널 비디오 프로그래밍 디스트리뷰터(MVPD)를 위한 킥스타트 안내서입니다. 프로그래머(콘텐츠 공급자)인 경우 [프로그래머 킥스타트 안내서](/help/authentication/programmer-kickstart-guide.md).

Zendesk의 Adobe Pass 인증 티켓 시스템을 통해 상시 지원이 가능합니다. 또한 여기에서 프로세스에 대한 샘플, 설명서 및 비디오 튜토리얼을 찾을 수 있습니다. 를 사용하려면 [젠데스크](https://adobeprimetime.zendesk.com/), https://tve.zendesk.com/home에 등록하고 계정을 만들어야 합니다. 등록할 수 있는 사용자 수와 등록된 티켓에서 보거나 의견을 게시할 수 있는 사용자의 수에는 제한이 없습니다. 모든 지원 질문은 adobe.com에서 tve-support로 문의해야 합니다.

**팀 연락처**:

**지원** - 모든 질문, 인시던트 또는 기능 요청 **tve-support@adobe.com**.

## 1. 킥오프 미팅 {#kickoff-meetings}

이러한 미팅의 범위는 Adobe과 MVPD 간의 기술적 논의의 시작이다. 이 프로세스의 시점에서 설명서는 양쪽에서 공유되어야 합니다. 후속 조치로서 Adobe은 티켓 시스템(https://tve.zendesk.com/)에서 티켓을 열어 통합 상태를 추적해야 합니다.

## 2. 기능 {#features}

Adobe은 통합의 전체 일정, 단계, 타임라인 및 구현 세부 사항을 논의하고 추적하는 주간 상태 호출을 설정합니다. 이 단계에서 Adobe은 MVPD의 사양을 검토합니다. 그 결과는 MVPD에 필요한 모든 기능을 자세히 설명하는 사양 페이지여야 합니다. MVPD는 Adobe에게 MVPD의 인증/권한 부여를 자세히 설명하는 사양 문서를 보냅니다.

명확히 할 항목, 참조 [MVPD 통합 기능](/help/authentication/mvpd-integr-features.md).

이 시점에서 자세히 설명해야 하는 몇 가지 설정이 있습니다.

* **MVPD의 로고 URL** - 다음 크기의 파일입니다. 112 x 33픽셀. 사용자가 &quot;로그인&quot; 버튼을 클릭하여 유료 TV 공급자를 선택하면 프로그래머가 사이트에 로고를 표시합니다.
* **TTL(time-to-live) 값** - TTL은 일반적으로 인증/권한 부여 프로세스 동안 MVPD에 의해 설정됩니다. 그러나 Adobe은 이러한 TTL 값을 재정의하고 프로그래머와 MVPD 양자가 동의한 것에 따라 다른 값을 제공할 수 있습니다.
* **표시 이름** - 사용자가 &quot;로그인&quot; 버튼을 클릭하여 유료 TV 공급자를 선택하면 프로그래머가 사이트에 표시합니다.
* **자격 증명 테스트** - 두 프로필(스테이징 및 프로덕션)에 테스트 자격 증명 목록이 있어야 합니다.

>[!IMPORTANT]
>
>사용자가 권한 흐름을 시작할 때마다 단일 불투명 고유 사용자 ID와 연결됩니다.  사용자 ID는 프로그래머 앱의 사용자를 식별하는 데 사용되지만 MVPD에서 가져옵니다.

>[!CAUTION]
>
>각 MVPD에 대한 중요 공지: 사용자 ID에는 사용자를 식별, 연락 또는 찾기 위해 단독으로 또는 다른 정보와 함께 사용할 수 있는 PII(개인 식별 정보)가 포함되어서는 안 됩니다.

## 2. 메타데이터 교환 {#metadata-ex}

양측은 관련된 모든 환경(프로덕션, 스테이징 등)에 대한 메타데이터를 교환해야 합니다.

* **Adobe**
   * 스테이징 환경 Adobe의 SP 메타데이터는에서 검색할 수 있습니다. [인증 스테이징 sp 메타데이터](https://sp.auth-staging.adobe.com/sp/metadata)
   * 프로덕션 환경의 경우 Adobe의 SP 메타데이터는에서 검색할 수 있습니다. [인증 프로덕션 sp 메타데이터](https://sp.auth.adobe.com/sp/metadata)

* **MVPD**
   * 메타데이터(스테이징/프로덕션)를 추가합니다.

## 4. IP 목록 허용 {#allow-ip-list}

다음 IP는 MVPD의 방화벽에 허용 목록에 추가해야 합니다. IP 목록은 Adobe에 문의하십시오.

* Adobe Pass 인증을 사용하려면 제한된 리소스에 액세스할 수 있도록 포트 80 및 443에서 방화벽을 열어야 합니다.

* MVPD는 인증 및 권한 부여 서버에 대한 IP 주소 목록을 추가해야 합니다(이 경우).

## 5. 개발 {#deve}

개발 단계의 기간은 세부 항목을 검토하고 양측이 체결하는 항목을 고려하여 결정될 것입니다. Adobe은 인증 부분에 대한 사용자 지정 코드를 작성해야 합니다.

>[!NOTE]
>
>인증은 리소스별로 수행됩니다. 인증 트랜잭션은 일반적으로 프로그래머 사이트에서 전달된 ID 문자열로 수행되며, 사용자가 인증을 요청하는 채널을 나타냅니다. 이 리소스 ID는 프로그래머와 MVPD 간에 설정되며 필요에 따라 세부적으로 지정할 수 있습니다.

## 6. Adobe 환경 {#adobe-env}

Adobe은 개발 프로세스의 각 단계에 대해 서로 다른 환경을 제공합니다.

* **사전 자격 조건** (PRE-QUAL): PRE-QUAL 환경에는 다음 릴리스 후보가 포함됩니다. Adobe은 통합을 릴리스 환경으로 업그레이드하기 전에 처음에 이 환경에서 새 파트너를 통합합니다. 파트너는 2주 동안 PRE-QUAL 환경에서 테스트할 수 있으며 PRE-QUAL 구성에 대한 변경 사항을 명시적으로 요청해야 합니다(변경 요청 프로세스에 대한 자세한 내용은 Adobe 담당자에게 문의). 버그 수정은 이 환경에서 새 배포를 트리거합니다.
* **릴리스** (릴리스): Adobe의 현재 프로덕션 빌드가 여기에서 라이브 환경에 배포됩니다.

Adobe 환경을 사용하는 방법에 대한 자세한 내용은 [Adobe 환경 이해](/help/authentication/understanding-the-adobe-environments.md)

## 7. 스테이징 배포 {#stag-env}

Adobe은 MVPD에서 받은 메타데이터를 기반으로 Adobe Pass 인증 시스템에서 새 MVPD를 만들고 구성합니다. 이 은(는) Adobe의 사전 준비 환경에 배포되며 테스트 프로그래머(TestDistributors)로 구성됩니다.

MVPD는 QA/스테이징/테스트 환경에서 동일한 배포를 수행해야 합니다.

## 8. 테스트 및 문제 해결 {#tes-troubleshoot}

이 단계에서는 Adobe 및 MVPD를 테스트하고 통합 문제를 해결합니다. 통합 테스트를 지원하기 위해 Adobe Pass 인증 팀은 Adobe의 API 테스트 사이트를 사용할 수 있습니다. Adobe의 API 테스트 사이트 사용에 대한 자세한 내용은 을 참조하십시오. [Adobe API 테스트 사이트를 사용하여 인증 및 권한 부여 흐름 테스트](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md).

테스트 및 문제 해결이 완료되면 Adobe의 릴리스 스테이징 환경에서 통합이 활성화됩니다. 이 시점에서, Adobe은 MVPD를 실제 프로그래머와 통합할 수 있다.

## 9. 프로덕션 배포 {#prod-dep}

* 연결을 테스트하려면 프로덕션 프로필에 MVPD를 먼저 배포해야 합니다.

* Adobe이 이전 프로덕션에 배포됩니다.

* 이제 모든 파티가 프로덕션 프로필에서 테스트할 수 있습니다.

* 이 시점에서 모든 것이 괜찮으면 Adobe은 모든 사용자가 사용할 수 있는 릴리스 프로덕션 환경(&quot;라이브&quot;)으로 통합을 이동할 수 있습니다.

## 10. 에스컬레이션 절차 {#esc-proc}

통합이 프로덕션에 실행되면 최상의 고객 경험을 제공하는 것이 중요합니다. 서버 작동 중지 문제의 경우 최상의 응답을 보장하기 위해 MVPD는 Adobe의 주의를 끌 수 있는 문제에 대한 에스컬레이션 절차 문서를 제공해야 합니다.

차례로 Adobe은 MVPD에 최신 Adobe Pass 인증 에스컬레이션 프로세스를 제공합니다.


<!--- [!RELATEDINFORMATION]
>
>* [Programmer Kickstart Guide](/help/authentication/programmer-kickstart-guide.md)
>* [MVPD Integration Guide](/help/authentication/mvpd-integr-features.md)
-->
