---
title: 추적 방지 평가 Google Chrome
description: 추적 방지 평가 Google Chrome
exl-id: f3d552da-2fd7-4ac8-9f82-876625af5d47
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '673'
ht-degree: 0%

---

# (기존) 추적 방지 평가 - Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 개요

이 문서는 유용한 리소스를 집계하고, 타사 쿠키를 단계적으로 제거하는 이니셔티브의 일부로 Google Chrome에서 계획하는 향후 변경 사항을 평가합니다.

평가는 Google Chrome 브라우저에서 실행 중이며 Adobe Pass Access Enabler JavaScript SDK v4를 사용하여 Adobe Pass 인증 백엔드 서비스와 통합하는 TVE(TV Everywhere) 애플리케이션에 대해 수행됩니다.

## 공개 리소스

Google의 개발자 웹 사이트와 공식 블로그를 통해 집계된 아래 리소스 목록을 확인하여 고객에게 문의해 주시기 바랍니다.

* [Chrome에서 타사 쿠키를 단계적으로 제거하는 다음 단계](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [개인 정보 샌드박스에 대한 개발자 설명서](https://developers.google.com/privacy-sandbox)
* [타사 쿠키 제한 준비](https://developers.google.com/privacy-sandbox/3pcd)
* [타사 쿠키 페이스아웃 준비](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [타사 쿠키의 종료를 준비하는 중](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Chrome 사용자의 1%에 대해 기본적으로 타사 쿠키가 제한됨](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## 타임라인

간략한 요약으로 Google Chrome은 모든 타사 쿠키에 영향을 주는 사이트 간 추적을 제한하는 새로운 기능인 [추적 보호](https://privacysandbox.com/) 테스트를 시작했습니다.

처음에는 사용자의 약 1%에 영향을 미치는 2024년 초에 시작되었으며 (임시) 계획은 2024년 3분기부터 사용자의 최대 100%까지 연장할 예정입니다.

## 평가

Google은 https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout 링크에서 타사 쿠키 단계를 준비하기 위해 권장되는 플레이북을 집계하는 문서를 게시했습니다.

Adobe Pass Access Enabler Google Chrome 브라우저에서 실행되고 Enabler JavaScript SDK v4를 사용하여 Adobe Pass 인증 백엔드 서비스와 통합하는 TVE(TV Everywhere) 애플리케이션의 평가를 위해 이 플레이북을 준수했습니다.

### 결론

Google Chrome의 예정된 업데이트를 시뮬레이션하는 테스트를 기반으로 기본 TVE 비즈니스 흐름 **이(가) 예상대로 계속 작동합니다**.

그러나 서드파티 쿠키를 중단하는 것뿐만 아니라 서드파티 스토리지를 파티션하는 Google의 광범위한 전략을 인식하는 것이 중요합니다.

따라서 Chrome 사용자는 SSO(Single Sign-On), SLO(Single Logout) 및 수동 인증 기능이 중단되어 사용하는 각 TVE 애플리케이션에 대해 별도의 로그인/로그아웃 작업이 필요합니다(Safari의 현재 경험과 일치).

## 자체 평가 요청

고객이 사전에 유사한 평가를 실시하여 잠재적인 문제를 사전에 잘 파악하고 수정된 Google Chrome 사용자 경험을 숙지할 것을 촉구합니다.

이 평가에는 자사 서비스와 타사 서비스가 모두 포함되어야 하며, 특히 Adobe Pass Access Enabler JavaScript SDK v4 통합에 대해 더욱 그렇습니다.

인증, 사전 인증, 권한 부여, 사용자 메타데이터 또는 로그아웃을 포함하여 TVE 비즈니스 흐름과 관련된 어려움이 발생할 경우 고객 지원 팀에서 Zendesk 티켓을 통해 보고서를 제출해 주십시오.

자체 평가 계획을 수립하는 데 도움이 필요하면 아래 섹션을 참조하십시오.

### 쿠키 사용 감사

Chrome 118부터 [DevTools 문제](https://developer.chrome.com/docs/devtools/issues/) 탭에서는 영향을 받을 수 있는 쿠키를 다음과 같은 메시지와 함께 강조 표시합니다. `Cookie sent in cross-site context will be blocked in future Chrome versions`.

서드파티 사용으로 표시된 쿠키는 `SameSite=None` 특성 값으로 식별할 수 있습니다.

자세한 내용은 https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies 링크를 참조하십시오.

### 파손 검사

`--test-third-party-cookie-phaseout` 명령줄 플래그를 사용하여 Chrome을 시작하거나 Chrome 118에서 `chrome://flags/`의 `#test-third-party-cookie-phaseout`을(를) 활성화하십시오.

이렇게 하면 Google Chrome이 타사 쿠키를 차단하도록 설정되며, 종료 후 상태를 가장 잘 시뮬레이션할 수 있도록 향후 기능이 활성화되어 있는지 확인합니다.

다음 Chrome 플래그에 대한 기술 사양을 자세히 살펴볼 가치가 있습니다.

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

자세한 내용은 https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage 링크를 참조하십시오.

## 기타 브라우저

### Firefox

Firefox에서는 몇 년 전에 &#39;`Enhanced Tracking Protection`&#39;이라는 메커니즘을 롤아웃했습니다.

Firefox의 몇 가지 유용한 리소스를 아래에서 참조하십시오.

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari는 몇 년 전에 &#39;`Intelligent Tracking Prevention`&#39;이라는 메커니즘을 롤아웃했습니다.

Safari의 몇 가지 유용한 리소스를 아래에서 참조하십시오.

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Adobe Pass의 유용한 리소스 몇 가지를 아래를 참조하십시오.

* [추적 방지 평가 - Apple Safari](tracking-prevention-assessment-apple-safari.md)
