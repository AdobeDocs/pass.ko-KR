---
title: DCR(Dynamic Client Registration) 용어집
description: DCR(Dynamic Client Registration) 용어집
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# DCR(Dynamic Client Registration) 용어집 {#rest-api-dcr-glossary}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

이 문서에서는 Adobe Pass 인증 DCR(Dynamic Client Registration)을 통합할 때 사용되는 용어에 대한 정의를 제공합니다.

>[!MORELIKETHIS]
> 
> * [REST API v2 용어집](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## 용어집 용어 {#glossary-terms}

### {#a}

#### 액세스 토큰 {#access-token}

액세스 토큰은 보호된 API에 대한 액세스를 보장하기 위한 [DCR(Dynamic Client Registration)](#dcr) 프로세스의 결과로 Adobe Pass 인증에 의해 생성된 토큰입니다.

### C {#c}

#### 클라이언트 자격 증명 {#client-credentials}

클라이언트 자격 증명은 [DCR(Dynamic Client Registration)](#dcr) 프로세스 중에 생성된 고유한 값 집합이며 [액세스 토큰](#access-token)을(를) 얻는 데 사용됩니다.

#### 사용자 지정 체계 {#custom-scheme}

사용자 지정 구성표는 Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)에서 생성 및 다운로드할 수 있는 [프로그래머](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) 응용 프로그램을 참조하는 고유한 값이며 iOS 장치에서 실행되는 응용 프로그램에서 최종 리디렉션으로 사용됩니다.

### D {#d}

#### DCR {#dcr}

DCR(Dynamic Client Registration)은 [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591)에서 정의한 인증 메커니즘이며 [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)에서 설명한 OAuth 2.0 인증 프레임워크를 기반으로 합니다.

DCR은 보호된 API에 대한 액세스를 추가로 활성화할 수 있는 Adobe Pass 인증 서비스로 [프로그래머](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer)에게 전달됩니다.

자세한 내용은 [동적 클라이언트 등록 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) 설명서를 참조하십시오.

### R {#r}

#### 등록된 응용 프로그램 {#registered-application}

등록된 응용 프로그램은 [DCR(Dynamic Client Registration)](#dcr) 프로세스를 진행해야 하는 [Programmer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) 응용 프로그램에 대한 정보를 저장하는 Adobe Pass 인증 개념입니다.

### S {#s}

#### 소프트웨어 구문 {#software-statement}

소프트웨어 문은 Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)에서 다운로드할 수 있는 JSON 웹 토큰(JWT)이며 [DCR(Dynamic Client Registration)](#dcr) 프로세스의 일부로 사용됩니다.
