---
title: 제품 공지
description: 제품 공지
exl-id: 3c9c66e1-d31d-4af3-8ab2-eb32492f42ca
source-git-commit: 6ff46a124f5f3c78173028ae3efed68d71ee6e41
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 1%

---

# 제품 공지 {#product-announcements}

## 서비스 종료(EOL) {#eol}

이 섹션에서는 서비스 해제가 예정된 특정 Adobe Pass 인증 기능 및 제품의 지원 종료 및 서비스 종료 날짜에 대해 간략히 소개합니다.

서비스 해제 타임라인에 대한 정보를 계속 얻고 아래에 설명된 대로 작업을 수행할 계획인지 확인하십시오.

| 공지 | 공지 날짜 | 지원 종료일 | 서비스 종료 날짜 |   | 설명 |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|---------------------|---------------------------------------------|:--|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adobe Pass 인증 TVE 대시보드 URL EOL <ul><li><a href="https://console.auth.adobe.com">console.auth.adobe.com</a></li><li><a href="https://console.auth-staging.adobe.com">console.auth-staging.adobe.com</a></li><li><a href="https://console-prequal.auth.adobe.com">console-prequal.auth.adobe.com</a></li><li><a href="https://console-prequal.auth-staging.adobe.com">console-prequal.auth-staging.adobe.com</a></li></ul> | 10/02/2024 | 2025년 1월 1일 | 2025년 3월 12일 |   | [2025년 3월 12일](https://console.auth.adobe.com)에 [console.auth.adobe.com](https://console.auth-staging.adobe.com), [console.auth-staging.adobe.com](https://console-prequal.auth.adobe.com), [console-prequal.auth.adobe.com](https://console-prequal.auth-staging.adobe.com) 및 **console-prequal.auth-staging.adobe.com**&#x200B;에서 호스팅되는 Adobe Pass 인증 TVE 대시보드에 대한 수명 종료를 계획합니다.</br></br>이 날짜 이후에는 위에 언급된 URL에 더 이상 액세스할 수 없으며, 통합에 계속 액세스하려면 새 [TVE 대시보드](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)로 리디렉션됩니다.</br></br>서비스를 계속 사용하려면 [experience.adobe.com/pass/authentication](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)에서 호스팅하는 새 [TVE 대시보드](https://experience.adobe.com/pass/authentication)에 액세스해야 합니다. |
| Adobe Pass 인증 AccessEnabler JavaScript SDK v3.5 EOL | 2024년 12월 11일 | 해당 사항 없음 | <span style="color: red;">01/08/2025</span> |   | **2025년 1월 8일**&#x200B;에 Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5의 수명이 종료될 예정입니다. 이 날짜 이후에는 AccessEnabler JavaScript SDK v3.5에서 제공하는 기능 및 서비스가 더 이상 작동하지 않습니다(사용자 인증 및 권한 부여 포함). |
| Adobe Pass 인증 비 DCR 보안 메커니즘 EOL | 2024년 12월 11일 | 해당 사항 없음 | <span style="color: red;">01/20/2025</span> |   | **2025년 1월 20일**&#x200B;에 Adobe Pass 인증 비 DCR 보안 메커니즘의 수명 종료를 계획합니다. 이 메커니즘은 다음 Adobe Pass 인증 API에 대한 액세스를 보호하는 데 사용되었습니다.<ul><li><a href="/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md">임시 패스 API 재설정</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md">API 저하</a></li><li><a href="/help/authentication/integration-guide-mvpds/proxy-mvpd-webserv.md">프록시 MVPD API</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md">자격 서비스 모니터링 API</a></li></ul>이 날짜 이후, 이 보안 메커니즘을 사용하여 액세스되는 위의 API에서 제공하는 기능 및 서비스는 더 이상 작동하지 않습니다.</br></br>서비스를 계속 사용하려면 [동적 클라이언트 등록](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) 메커니즘을 사용하도록 모든 응용 프로그램을 마이그레이션해야 합니다. |
| Adobe Pass 인증 REST API v1 EOL | 2024년 12월 11일 | 12/31/2025 | 2026년 말(TBD) |   | **2025년 12월 31일**&#x200B;에 Adobe Pass 인증 REST API v1에 대한 지원을 중단할 예정입니다. 이 날짜 이후에는 더 이상 업데이트 또는 수정 사항이 제공되지 않습니다.</br></br>계속 지원하려면 모든 응용 프로그램을 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>(으)로 마이그레이션해야 합니다.</br></br>Adobe Pass 인증 REST API v1에 대한 수명 종료를 **2026년 말까지**&#x200B;계획합니다. 이 날짜 이후, REST API v1에서 제공하는 기능 및 서비스는 사용자 인증 및 권한 부여를 포함하여 더 이상 작동하지 않습니다.</br></br>서비스를 계속 사용하려면 모든 응용 프로그램을 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>(으)로 마이그레이션해야 합니다. |
| Adobe Pass 인증 AccessEnabler SDK EOL | 2024년 12월 11일 | 2026/05/31 | 2026년 말(TBD) |   | **2026년 5월 31일**&#x200B;에 Adobe Pass 인증 AccessEnabler SDK에 대한 지원을 중단할 예정입니다. 이 날짜 이후에는 더 이상 업데이트 또는 수정 사항이 제공되지 않습니다.</br></br>계속 지원하려면 모든 응용 프로그램을 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>(으)로 마이그레이션해야 합니다.</br></br>2026년 말까지 **Adobe Pass 인증 AccessEnabler SDK의 사용 수명이 종료될 예정입니다**. 이 날짜 이후에는 AccessEnabler SDK에서 제공하는 기능 및 서비스가 더 이상 작동하지 않습니다(사용자 인증 및 권한 부여 포함).</br></br>서비스를 계속 사용하려면 모든 응용 프로그램을 <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>(으)로 마이그레이션해야 합니다. |

## 제품 릴리스 {#product-releases}

이 섹션에서는 Adobe Pass 인증에 대한 릴리스 내역 및 해당 릴리스 정보에 대한 참조를 컴파일합니다.

### 2025 {#product-releases-2025}

| 릴리스 정보 | 날짜 |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass 인증 3.5.0 릴리스 노트](notes-releases/auth-rn-350.md) | 2025년 12월 9일 - 2025년 12월 11일 |
| [Adobe Pass 인증 Android 3.8.0 릴리스 노트](notes-releases/authn-rn-android-380.md) | 2025년 9월 18일 |
| [Adobe Pass 인증 3.4.0 릴리스 노트](notes-releases/auth-rn-340.md) | 2025년 9월 16일 - 2025년 9월 18일 |
| [Adobe Pass 인증 3.3.0 릴리스 노트](notes-releases/auth-rn-330.md) | 2025년 7월 22일 - 2025년 7월 24일 |
| [Adobe Pass 인증 3.2.0 릴리스 노트](notes-releases/auth-rn-320.md) | 2025년 6월 10일 - 2025년 6월 12일 |
| [Adobe Pass 인증 3.1.0 릴리스 노트](notes-releases/auth-rn-310.md) | 2025년 2월 25일 - 2025년 2월 27일 |
| [Adobe Pass 인증 JavaScript SDK 4.7.1 릴리스 노트](notes-releases/authn-rn-javascript-471.md) | 2025년 2월 25일 - 2025년 2월 27일 |

### 2024 {#product-releases-2024}

| 릴리스 정보 | 날짜 |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass 인증 3.0.3 릴리스 노트](notes-releases/auth-rn-303.md) | 2024년 10월 29일 - 2024년 10월 31일 |
| [Adobe Pass 인증 3.0 릴리스 노트](notes-releases/auth-rn-300.md) | 2024년 9월 10일 - 2024년 9월 12일 |
| [Adobe Pass 인증 2.70 릴리스 노트](notes-releases/auth-rn-270.md) | 2024년 4월 23일 - 2024년 4월 25일 |
| [Adobe Pass 인증 2.69 릴리스 노트](notes-releases/auth-rn-269.md) | 2024년 2월 27일 - 2024년 2월 29일 |
| [Adobe Pass 인증 JavaScript SDK 4.7.0 릴리스 노트](notes-releases/authn-rn-javascript-470.md) | 2024년 2월 27일 - 2024년 2월 29일 |
| [Adobe Pass 인증 iOS/tvOS SDK 3.9.2 릴리스 노트](notes-releases/authn-rn-ios-tvos-392.md) | 2024년 3월 26일 |
| [Adobe Pass 인증 iOS/tvOS SDK 3.8.4 릴리스 노트](notes-releases/authn-rn-ios-tvos-384.md) | 2024/01/26 |

### 2023 {#product-releases-2023}

| 릴리스 정보 | 날짜 |
|---------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass 인증 2.68 릴리스 노트](notes-releases/auth-rn-268.md) | 2023년 12월 5일 - 2023년 12월 7일 |
| [Adobe Pass 인증 2.67 릴리스 노트](notes-releases/auth-rn-267.md) | 2023년 9월 12일 - 2023년 9월 14일 |
| [Adobe Pass 인증 2.66 릴리스 노트](notes-releases/auth-rn-266.md) | 2023년 7월 11일 - 2023년 7월 13일 |
| [Adobe Pass 인증 2.65.1 릴리스 노트](notes-releases/auth-rn-2651.md) | 2023년 6월 20일 - 2023년 6월 22일 |
| [Adobe Pass 인증 2.65 릴리스 노트](notes-releases/auth-rn-265.md) | 25/04/2023 - 27/04/2023 |
| [Adobe Pass 인증 2.64.1 릴리스 노트](notes-releases/auth-rn-2641.md) | 2023년 1월 31일 - 2023년 2월 02일 |
| [Adobe Pass 인증 iOS/tvOS SDK 3.8.3 릴리스 노트](notes-releases/authn-rn-ios-tvos-383.md) | 11/16/2023 |
| [Adobe Pass 인증 iOS/tvOS SDK 3.8.2 릴리스 노트](notes-releases/authn-rn-ios-tvos-382.md) | 2023/02/10 |
| [Adobe Pass 인증 iOS/tvOS SDK 3.8.1 릴리스 노트](notes-releases/authn-rn-ios-tvos-381.md) | 26/05/2023 |
| [Adobe Pass 인증 Android SDK 3.7.3 릴리스 노트](notes-releases/authn-rn-android-373.md) | 2023/09/19 |

### 2022 {#product-releases-2022}

| 릴리스 정보 | 날짜 |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass 인증 2.64 릴리스 노트](notes-releases/auth-rn-264.md) | 2022년 11월 8일 - 2022년 11월 10일 |
| [Adobe Pass 인증 2.63 릴리스 노트](notes-releases/auth-rn-263.md) | 2022년 9월 20일 - 2022년 9월 22일 |
| [Adobe Pass 인증 2.62.1 릴리스 노트](notes-releases/auth-rn-2621.md) | 2022년 8월 2일 - 2022년 8월 4일 |
| [Adobe Pass 인증 JavaScript SDK 4.6.0 릴리스 노트](notes-releases/authn-rn-javascript-460.md) | 2022년 9월 20일 - 2022년 9월 22일 |

### 2021 {#product-releases-2021}

| 릴리스 정보 | 날짜 |
|-----------------------------------------------------------------------------------------------------------|------------|
| [Adobe Pass 인증 JavaScript SDK 4.4.0 릴리스 노트](notes-releases/authn-rn-javascript-440.md) | 2021년 6월 22일 |
| [Adobe Pass 인증 iOS/tvOS SDK 3.7.0 릴리스 노트](notes-releases/authn-rn-ios-tvos-370.md) | 09/03/2021 |
