---
title: 프로그래머 킥스타트 안내서
description: 프로그래머 킥스타트 안내서
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '971'
ht-degree: 0%

---

# 프로그래머 킥스타트 안내서 {#programmer-kickstart-guide}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 소개 {#prog-kickstart-guide-intro}

TV Everywhere용 Adobe Pass 인증에 오신 것을 환영합니다. 귀사와 함께 일할 수 있기를 기대합니다.

>[!NOTE]
>
>프로그래머(콘텐츠 공급자)를 위한 킥스타트 안내서입니다. MVPD(다채널 비디오 프로그래밍 디스트리뷰터)를 사용하는 경우 다음을 참조하십시오. [MVPD 킥스타트 안내서](/help/authentication/mvpd-kickstart-guide.md).


Adobe Pass 인증 연락처:

* 지원 - 모든 질문, 장애 또는 기능 요청에 대해 `tve-support@adobe.com`
* 지원 담당자는 프로젝트 개시 전까지 프로젝트에 할당됩니다.

다음은 견고하고 효율적인 출발을 위한 몇 가지 중요한 첫 번째 단계를 간략히 설명합니다. 목표는 파트너와 협력하여 통합을 달성하는 방법에 대한 설명과 기대를 제공하는 것입니다. 각 항목에 대해 아래의 &quot;제공 예정&quot; / &quot;Adobe 제공 예정&quot; 섹션을 참조하십시오. 이 목록들은 체크리스트로 나열되어 있으며, 프로젝트를 진행할 때 참조할 수 있습니다.

이 문서에서는 프로그래머가 선택한 MVPD 파트너와 함께 작동하도록 등록되었다고 가정합니다.

## 릴리스 일정 {#release-schedule}

Adobe 스프린트 개발 주기가 계획되어 있으므로 릴리스가 언제 예약되고 개발 시스템을 통해 각 릴리스가 어떻게 홍보되는지 볼 수 있습니다.

각 스프린트가 완료되면 QE를 사용하거나 UAT 서버에서 새로운 구현을 확인할 수 있습니다.

약 6주마다 Adobe Pre-Qualification Server에 릴리스됩니다. (최종 QE를 수행하는 동안 제안된 다음 릴리스를 수행하는 서버입니다.) 이 빌드에는 마지막 드롭 이후 스프링에서 완료된 모든 작업이 포함됩니다. 이 릴리스를 테스트할 수 있도록 2주 간의 QE 기간을 사용할 수 있습니다.

이전 2주 테스트 기간 동안 중요한 문제가 발생하지 않았다고 가정할 경우 릴리스는 라이브 프로덕션으로 승격됩니다. 즉, Adobe 릴리스 환경에서 통합을 사용할 수 있지만 파트너가 릴리스를 공개할 시기를 선택합니다.

<!--For the latest release schedule information, see the Release Calendar.-->

## 지원 설명서 {#supp-doc}

Adobe은 다음을 제공합니다.

* 배포 안내서: **`https://tve.zendesk.com/entries/498741-tve-deployment-guide`**
* Zendesk 고객 지원 시스템에 액세스 또한 여기에서 일부 프로세스에 대한 샘플, 정보 및 비디오 튜토리얼을 찾을 수 있습니다. Zendesk에 게시된 다른 문서와 함께 이 문서에 액세스하려면 다음을 등록하고 계정을 만들어야 합니다. `https://tve.zendesk.com/home`. 등록할 수 있는 사용자의 수에는 제한이 없습니다.  당신은 어떤 파일된 티켓에 대한 의견을 보고 공유할 수 있습니다. 모든 지원 질문은 다음 주소로 전달되어야 합니다. `tve-support@adobe.com`.
* [프로그래머 통합 안내서](/help/authentication/programmer-integration-guide-overview.md)
* 미디어 토큰 검증기 라이브러리: `https://tve.zendesk.com/entries/471323-media-token-validator-library`.

## 환경 설정 테스트 {#test-env-setup}

Adobe은 먼저 Adobe 테스트 사이트를 설정하며, 여기서 Adobe은 테스트 목적으로 MVPD 역할을 합니다. 그런 다음 Adobe API를 호출하는 테스트 웹 사이트를 설정할 수 있습니다. 기본 MVPD 선택기를 사용하고 &quot;Adobe&quot;를 idP로 선택합니다.

다음을 제공합니다.

1. 요청자 ID. Adobe Pass 인증에 요청하는 웹 사이트 또는 애플리케이션의 브랜드를 고유하게 식별하는 문자열입니다. 문자열 자체는 임의적이지만 Adobe과 프로그래머 간에 동의해야 합니다
1. 채널 정보. 요청자 ID에서 요청하는 콘텐츠 채널을 식별하는 문자열 세트입니다. 대부분의 경우 채널과 요청자 ID는 동일합니다. 그러나 동일한 ID로 요청할 수 있는 콘텐츠 채널이 여러 개 있을 수 있습니다. 채널 이름 문자열은 케이블 TV 채널에 해당해야 합니다. 일부 MVPD는 AuthN 및/또는 AuthZ 프로토콜을 통해 이 값을 확인합니다.
1. 도메인 이름(해당 요청자 ID에 대해 허용). 이는 요청자 ID를 수락하기 위해 Adobe 별로 나열되는 실제 도메인 이름 목록입니다. 이렇게 하면 승인된 도메인만 메타데이터로 Adobe Pass 인증에 액세스할 수 있습니다. 참고: 프로덕션에 유효한 도메인 이름은 테스트/스테이징에 따라 다를 수 있으며 두 이름을 모두 제공하고 식별해야 합니다.

Adobe은 계정을 설정하고 Adobe은 다음을 제공합니다.

* 테스트 사이트에 액세스하기 위한 로그인 및 암호

## MVPD로 설정 {#setup-mvpd}

이 섹션에서는 Adobe 테스트 사이트에서 MVPD로 작업할 때 필요한 사항을 설명합니다.

(MVPD를 통해) 다음을 제공합니다.

* **두 세트의 자격 증명**:
   * AuthN + AuthZ : 인증되고 승인된 사용자에 대한 로그인/암호
   * AuthN + Non-AuthZ : 인증되었지만 승인되지 않은 사용자에 대한 로그인/암호
* **리소스 ID**. AuthZ 프로토콜을 통해 MVPD로 확인할 특정 콘텐츠 식별자입니다. 채널, 쇼, 에피소드 또는 에셋 수준일 수 있으며 MVPD와 동의해야 합니다.

Adobe Pass 인증은 리소스 ID가 필요한 만큼 특정할 수 있으며, 특정 MVPD에 고유할 수 있는 식별자를 포함할 수 있는 MRSS 기반 메타데이터 스키마를 지원합니다.

**새로운 MVPD 통합**: 선택한 MVPD가 통합을 완료하는 데 중요한 역할을 하고 있음을 기억해야 합니다. Adobe은 사양에 따라 각 MVPD에 대한 코드를 작성해야 합니다. 이 단계를 완료할 때까지 대화 상자에서 해당 MVPD를 선택하거나 제품 테스트를 완료할 수 없습니다. Adobe은 사용 가능한 다음 스프린트에 맞게 이 작업을 미리 예약해야 합니다. (현재 일정 정보는 릴리스 달력을 참조하십시오.)

**기존 MVPD 통합**: 선택한 MVPD가 이미 Adobe으로 설정되어 있는 경우 연결 단계가 훨씬 간단해야 (더 빨라야) 하며, 구성 변경을 통해 연결을 수행할 수 있는 경우가 많습니다.

>[!NOTE]
>
>MVPD는 여전히 프로그래머를 활성화해야 하며 관련된 모든 비즈니스 거래에 동의해야 합니다.

**MVPD가 포함된 QE**: 모든 통합에는 공동 QE가 포함되며, 최종 사용자는 궁극적으로 MVPD의 고객이므로 많은 사용자가 &quot;라이브&quot;를 푸시하기 전에 테스트 주기를 설정했습니다. 이것은 MVPD 자원들의 스케줄링을 포함하기 때문에, 이것은 지연을 위한 잠재적인 영역이다.

<!--
>[RELATEDINFORMATION]
>[MVPD Kickstart Guide](help\authentication\mvpd-kickstart-guide.md)
-->
