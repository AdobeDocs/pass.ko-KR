---
title: 보호된 리소스
description: 보호된 리소스
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# 보호된 리소스 {#protected-resources}

>[!IMPORTANT]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

보호된 리소스는 사용자가 스트리밍할 수 있는 콘텐츠를 나타내며 MVPD와 참여 프로그래머 간의 계약을 통해 설정된 고유한 값에 의해 식별됩니다.

보호된 리소스는 계층 트리 구조를 따르며 각 수준은 콘텐츠 인증에 더 많은 세부기간을 제공합니다.

* 네트워크
   * 채널
      * 표시
         * 에피소드
            * 자산

## 보호된 리소스 식별자 {#identifiers}

리소스 고유 식별자는 두 가지 형식을 가질 수 있습니다.

* 채널(브랜드)의 고유 식별자와 같은 간단한 문자열 형식입니다.
* 제목, 등급 및 자녀 보호 메타데이터와 같은 추가 정보가 포함된 미디어 RSS(MRSS) 형식입니다.

&quot;REF30&quot;(채널을 나타낸다고 가정)과 같은 간단한 리소스 식별자의 경우 다음과 같이 RSS 리소스 식별자로 변환할 수 있습니다.

```RSS
    <rss version="2.0"> 
        <channel>
            <title>REF30</title>
        </channel>
    </rss>
```

보다 복잡한 자원 식별자의 경우, RSS 자원 식별자는 다음과 같이 추가 등급 정보를 포함할 수 있다:

```RSS
    <rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
        <channel>
            <title>REF30</title>
            <media:rating scheme="urn:mpaa">pg</media:rating>
        </channel>
    </rss>
```

## REST API V2 {#rest-api-v2}

사전 권한 부여 및 권한 부여 결정을 검색하는 Adobe Pass 인증 API를 사용하려면 클라이언트 애플리케이션이 보호된 리소스의 식별자를 매개 변수로 포함해야 합니다.

사전 권한 부여 및 권한 부여 결정은 다음 API를 사용하여 검색할 수 있습니다.

* [특정 mvpd를 사용하여 사전 인증 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [특정 mvpd를 사용하여 권한 부여 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

보호된 리소스의 고유 식별자를 제공하는 데 필요한 형식을 이해하려면 위의 API의 **요청** 및 **샘플** 섹션을 참조하십시오.

고유 식별자는 Adobe Pass에 직접 전달되므로 MVPD 인증에 불투명합니다. MVPD에서 리소스 식별자를 인식하거나 구문 분석할 수 없는 경우 Adobe Pass 인증에 오류를 반환한 다음 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)를 사용하여 클라이언트 응용 프로그램으로 오류를 다시 전달합니다.

위의 API를 통합하는 방법 및 시기에 대한 자세한 내용은 다음 문서를 참조하십시오.

* [기본 애플리케이션 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
