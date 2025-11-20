---
product: adobe primetime
feature: Concurrency Monitoring
audience: end-user
user-guide-title: Adobe Pass 동시 모니터링
user-guide-description: 여러 애플리케이션에서의 동시 사용에 대한 제한을 정의하고 적용하는 방법에 대해 알아봅니다.
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 5%

---


# Adobe Pass 동시성 모니터링 도움말 {#cm}

- [CM 소개](cm-home.md) {#cm-intro}
- 시작 {#getting-started}
   - [동시성 모니터링 시작](getting-started/getting-started-overview.md)
   - [주요 개념](getting-started/key-concepts.md)
- 통합 안내서 {#integration-guide}
   - [용어집](cm-glossary.md)
   - API 참조 {#api-reference}
      - [개요](api/api-reference-overview.md)
      - [API 엔드포인트](api/api-endpoints.md)
      - [API 참조](api/cm-api-overview.md)
   - [동시성 모니터링 사용 API](reports/cmu-api.md)
   - [API 액세스](reports/cmu-api-access.md)
   - [동시성 모니터링 사용량 보고서 예](reports/cm-usage-reports-examples.md)
- 사용 사례 및 구현 {#use-cases-implementation}
   - [사용 사례 개요](use-cases/cm-use-cases.md)
   - [LIFO와 FIFO 전략](use-cases/lifo-fifo-strategies.md)
   - [세션 충돌 처리](use-cases/handling-409-errors.md)
   - [구현 모델](technical/implementation-models.md)
   - [단일 테넌트 정책 여러 앱](technical/single-tenant-policy-mult-app.md)
   - [여러 앱의 동시 사용 제한](technical/restrict-concurr-usage-mult-apps.md)
- [CM 사용 보고서](reports/cm-usage-reports.md) {#cm-usage-reports}
- 기술 노트 {#tech-notes}
   - [표준 메타데이터 속성](technical/standard-metadata-attributes.md)
   - [사용자 지정 메타데이터](technical/custom-metadata.md)
   - [정책 결정 지점](technical/cm-policy-decision-point.md)
   - [정책 정보 지점](technical/policy-info-pt-versionone.md)
   - [조절](technical/throttling-mechanism.md)
   - [방법: 동시 모니터링에서 VOD 및 라이브 컨텐츠 구분](technical/vod-live-dist.md)
   - [데이터 보존 정책](technical/data-retention-policy.md)
- 릴리스 {#cm-release-notes}
   - [동시 모니터링 - 3.6.2 릴리스 노트](releases/rn-cm-services-362.md)
   - [동시 모니터링 - 3.6.1 릴리스 노트](releases/rn-cm-services-361.md)
   - [동시 모니터링 - 3.6.0 릴리스 노트](releases/rn-cm-services-360.md)
   - [동시 모니터링 - 3.5.1 릴리스 노트](releases/rn-cm-services-351.md)
   - [동시 모니터링 - 3.5.0 릴리스 노트](releases/rn-cm-services-350.md)
   - [동시 모니터링 - 3.4.4 릴리스 노트](releases/rn-cm-services-344.md)
   - [동시 모니터링 - 3.4.3 릴리스 노트](releases/rn-cm-services-343.md)
   - [동시 모니터링 - 3.4.2 릴리스 노트](releases/rn-cm-services-342.md)
   - [동시 모니터링 - 3.4.0 릴리스 노트](releases/rn-cm-services-340.md)
   - [동시 모니터링 - 3.3.7 릴리스 노트](releases/rn-cm-services-337.md)
   - [동시 모니터링 - 3.3.6 릴리스 노트](releases/rn-cm-services-336.md)
   - [동시 모니터링 - 3.3.2 릴리스 노트](releases/rn-cm-services-332.md)
   - [동시성 모니터링 - 3.3.1 릴리스 정보](releases/rn-cm-services-331.md)
   - [동시성 모니터링 - 3.3.0 릴리스 노트](releases/rn-cm-services-330.md)
   - [동시 모니터링 - 3.2.3 릴리스 노트](releases/rn-cm-services-323.md)
   - [동시 모니터링 - 3.2.2 릴리스 노트](releases/rn-cm-services-322.md)
   - [동시성 모니터링 - 3.2.1 릴리스 정보](releases/rn-cm-services-321.md)
   - [동시성 모니터링 - 3.2 릴리스 정보](releases/rn-cm-services-320.md)
   - [동시성 모니터링 - 3.1 릴리스 정보](releases/rn-cm-services-31.md)
   - [동시성 모니터링 - 3.0 릴리스 노트](releases/rn-cm-services-30.md)
   - [동시 모니터링 - 2.9.6 릴리스 노트](releases/rn-cm-296.md)
   - [동시성 모니터링 - 2.9 릴리스 정보](releases/rn-cm-29.md)
   - [동시 모니터링 - 2.8.2 릴리스 노트](releases/rn-cm-282.md)
   - [동시 모니터링 - 2.7.2 릴리스 노트](releases/rn-cm-272.md)
   - [동시 모니터링 - 2.6.0 릴리스 노트](releases/rn-cm-260.md)
   - [동시 모니터링 - 2.5.0 릴리스 노트](releases/rn-cm-250.md)
   - [동시 모니터링 - 2.3.2 릴리스 노트](releases/rn-cm-232.md)
   - [동시 모니터링 - 2.2.2 릴리스 노트](releases/rn-cm-222.md)
- [지원 프로시저](support/cm-escalation-procedures.md) {#support-procedures}
