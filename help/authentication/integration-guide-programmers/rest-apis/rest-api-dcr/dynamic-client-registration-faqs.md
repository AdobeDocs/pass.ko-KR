---
title: DCR(Dynamic Client Registration) FAQ
description: DCR(Dynamic Client Registration) FAQ
exl-id: 12268163-632e-4884-b35d-a29cc8ef45bf
source-git-commit: 747c3d9b6de537be5e7e0a0244b2b301603d9b18
workflow-type: tm+mt
source-wordcount: '1135'
ht-degree: 0%

---

# DCR(Dynamic Client Registration) FAQ {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 문서에서는 Adobe Pass 인증 DCR(Dynamic Client Registration) 채택에 대한 FAQ에 대한 개요 답변을 제공합니다.

DCR(동적 클라이언트 등록) 전체에 대한 자세한 내용은 [동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) 설명서를 참조하십시오.

## 일반 FAQ {#general-faqs}

새 응용 프로그램이든 이전 메커니즘 중 하나에서 마이그레이션되는 기존 응용 프로그램이든 관계없이 DCR(Dynamic Client Registration)을 통합해야 하는 응용 프로그램에서 작업 중인 경우 이 섹션으로 시작합니다.

>[!MORELIKETHIS]
>
> * [REST API v2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#general-faqs)

### REST API V2 액세스 FAQ {#rest-api-v2-access-faqs}

+++REST API V2 액세스 FAQ

#### &#x200B;1. 등록 단계의 목적은 무엇입니까? {#rest-api-v2-access-faq1}

등록 단계의 목적은 [DCR(Dynamic Client Registration)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) 프로세스를 통해 Adobe Pass 인증에 대해 클라이언트 응용 프로그램을 등록하는 것입니다.

DCR(Dynamic Client Registration) 프로세스를 진행하려면 클라이언트 애플리케이션이 클라이언트 자격 증명 쌍을 가져오고 등록 단계의 최종 목표로 액세스 토큰을 검색해야 합니다.

자세한 내용은 [동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) 설명서를 참조하십시오.

#### &#x200B;2. 등록 단계가 필수입니까? {#rest-api-v2-access-faq2}

등록 단계는 필수이지만, 클라이언트 응용 프로그램은 여전히 유효한 클라이언트 자격 증명 및 액세스 토큰의 캐시된 쌍이 있는 경우 이 단계를 건너뛸 수 있습니다.

#### &#x200B;3. 소프트웨어 설명서는 무엇이며 유효기간은 얼마나 됩니까? {#rest-api-v2-access-faq3}

소프트웨어 문은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement) 설명서에 정의된 용어입니다.

소프트웨어 문은 조직 관리자 중 한 사람이나 사용자를 대신하여 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)에서 생성 및 다운로드할 수 있는 JSON 웹 토큰(JWT)으로 구성됩니다.

소프트웨어 문은 무제한 유효하지만 언제든지 Adobe Pass 인증 담당자에게 취소를 요청하도록 선택할 수 있습니다.

클라이언트 응용 프로그램은 클라이언트 자격 증명을 검색해야 할 때 소프트웨어 문을 저장하고 사용해야 합니다.

자세한 내용은 [동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) 설명서를 참조하십시오.

#### &#x200B;4. 소프트웨어 명세서를 생성하고 다운로드하는 방법 {#rest-api-v2-access-faq4}

이 작업은 조직 관리자 중 한 사람이나 사용자를 대신하여 활동하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)를 통해 완료할 수 있습니다.

자세한 내용은 [TVE 대시보드 채널 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) 또는 [TVE 대시보드 프로그래머 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications) 설명서를 참조하십시오.

#### &#x200B;5. 소프트웨어 문이 취소되면 어떻게 됩니까? {#rest-api-v2-access-faq5}

소프트웨어 명령문이 취소되는 경우 고려해야 할 중요한 결과 중 하나가 있습니다.

* 해지된 소프트웨어 문을 사용하는 클라이언트 응용 프로그램에서는 더 이상 [권한](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement) 흐름을 진행할 수 없습니다. 즉, 사용자가 콘텐츠를 재생할 수 없게 됩니다.

#### &#x200B;6. 클라이언트 자격 증명은 무엇이며 얼마나 오래 유효합니까? {#rest-api-v2-access-faq6}

클라이언트 자격 증명은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials) 설명서에 정의된 용어입니다.

클라이언트 자격 증명은 클라이언트 등록 끝점에서 검색할 수 있는 클라이언트 식별자 및 클라이언트 암호 쌍으로 구성됩니다.

클라이언트 자격 증명은 제한 없이 유효합니다.

클라이언트 애플리케이션은 클라이언트 자격 증명을 저장하고 액세스 토큰을 검색해야 할 때 무기한 사용해야 합니다.

자세한 내용은 [클라이언트 자격 증명 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) 설명서를 참조하십시오.

#### &#x200B;7. 클라이언트 자격 증명을 관리하는 방법 {#rest-api-v2-access-faq7}

Adobe Pass 인증과 클라이언트-서버 및 서버-서버 통합 모두를 수행하는 경우 각 사용자 애플리케이션 인스턴스에 대해 고유한 클라이언트 자격 증명 쌍을 관리하는 것이 좋습니다.

#### &#x200B;8. 클라이언트 응용 프로그램이 영구 저장소에 클라이언트 자격 증명을 캐시해야 합니까? {#rest-api-v2-access-faq8}

클라이언트 애플리케이션은 클라이언트 자격 증명을 저장하고 액세스 토큰을 검색해야 할 때 무기한 사용해야 합니다.

#### &#x200B;9. 캐시된 클라이언트 자격 증명이 손실되면 어떻게 됩니까? {#rest-api-v2-access-faq9}

캐시된 클라이언트 자격 증명이 손실될 때 고려해야 할 세 가지 중요한 결과가 있습니다.

* 클라이언트 애플리케이션은 새로운 클라이언트 자격 증명 쌍을 획득해야 합니다.
* 클라이언트 애플리케이션은 새로운 클라이언트 자격 증명 쌍을 사용하여 새로운 액세스 토큰을 얻어야 합니다.
* 클라이언트 애플리케이션은 이전에 획득한 인증된 프로필에 대한 액세스 권한을 잃게 되므로 사용자에게 재인증을 요청해야 합니다.

#### &#x200B;10. 액세스 토큰은 무엇이며 유효기간은 얼마나 됩니까? {#rest-api-v2-access-faq10}

액세스 토큰은 [용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token) 설명서에 정의된 용어입니다.

액세스 토큰은 클라이언트 토큰 끝점에서 검색할 수 있는 [전달자 토큰](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)(으)로 구성됩니다.

액세스 토큰은 문제 발생 시 지정된 제한되고 짧은 기간 동안 유효합니다.

클라이언트 애플리케이션은 액세스 토큰을 저장하고 REST API V2를 타깃팅할 때 만료될 때까지 사용해야 합니다.

클라이언트 애플리케이션은 현재 액세스 토큰이 만료되기 전에 새 액세스 토큰을 받아야 승인되지 않은 요청을 방지할 수 있습니다.

자세한 내용은 [액세스 토큰 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) 설명서를 참조하십시오.

#### &#x200B;11. 클라이언트 애플리케이션이 액세스 토큰을 영구 저장소에 캐시해야 합니까? {#rest-api-v2-access-faq11}

클라이언트 애플리케이션은 만료될 때까지 액세스 토큰을 저장 및 사용한 다음 삭제하고 새 액세스 토큰을 얻어야 합니다.

#### &#x200B;12. 클라이언트 애플리케이션은 액세스 토큰을 새로 고침하는 방법 {#rest-api-v2-access-faq12}

클라이언트 애플리케이션은 새 액세스 토큰을 검색하는 것과 동일한 방식으로 액세스 토큰을 새로 고쳐야 하지만 캐시된 클라이언트 자격 증명을 사용해야 합니다.

클라이언트 응용 프로그램은 액세스 토큰을 새로 고치려면 재등록해서는 안 됩니다. 대신 저장된 클라이언트 자격 증명을 사용해야 합니다. 그렇지 않으면 사용자가 재인증해야 합니다.

자세한 내용은 [액세스 토큰 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) 설명서를 참조하십시오.

+++

## 마이그레이션 FAQ {#migration-faqs}

DCR(Dynamic Client Registration)을 사용하도록 기존 애플리케이션을 마이그레이션해야 하는 애플리케이션에서 작업하는 경우 이 섹션을 계속합니다.

>[!MORELIKETHIS]
>
> * [REST API v2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#migration-faqs)

### REST API V2 마이그레이션 FAQ {#rest-api-v2-migration-faqs}

+++REST API V2 마이그레이션 FAQ

#### &#x200B;1. 클라이언트 애플리케이션이 기존의 등록된 애플리케이션(소프트웨어 명령문)을 다시 사용할 수 있습니까? {#rest-api-v2-migration-faq1}

클라이언트 애플리케이션은 기존 등록 애플리케이션(소프트웨어 명령문)을 다시 사용할 수 없으므로 REST API V2 사용 전용의 새로운 등록 애플리케이션(소프트웨어 명령문)을 생성하고 다운로드해야 합니다.

이 작업은 조직 관리자 중 한 사람이나 사용자를 대신하여 활동하는 Adobe Pass 인증 담당자가 Adobe Pass [TVE 대시보드](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)를 통해 완료할 수 있습니다.

자세한 내용은 [TVE 대시보드 채널 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) 또는 [TVE 대시보드 프로그래머 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications) 설명서를 참조하십시오.

이 작업의 자체 관리를 허용하도록 Adobe Pass [TVE 대시보드](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)가 업데이트될 때까지 Adobe Pass 인증 담당자에게 새 등록된 응용 프로그램(소프트웨어 문)에 대한 REST API V2 사용을 활성화하도록 요청해야 합니다.

REST API V2를 사용하는 클라이언트 응용 프로그램에 사용되는 등록된 응용 프로그램(소프트웨어 문)을 구분하려면 등록된 응용 프로그램 이름에 &quot;RESV2&quot;와 같은 특정 접미사를 추가해야 합니다.

#### &#x200B;2. 클라이언트 애플리케이션이 기존 사용자 정의 체계를 다시 사용할 수 있습니까? {#rest-api-v2-migration-faq2}

클라이언트 응용 프로그램은 Adobe Pass [TVE 대시보드](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)를 통해 생성된 기존 사용자 지정 구성표를 다시 사용할 수 있습니다.

자세한 내용은 [TVE 대시보드 채널 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#custom-schemes) 또는 [TVE 대시보드 프로그래머 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#custom-schemes) 설명서를 참조하십시오.

+++
