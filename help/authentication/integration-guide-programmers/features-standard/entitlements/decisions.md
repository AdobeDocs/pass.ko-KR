---
title: 결정
description: 결정
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# 결정 {#decisions}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

[보호된 콘텐츠](#protected-resources)에 대한 액세스 권한 부여 또는 거부 여부를 결정하는 사용자의 MVPD 권한 부여 또는 사전 권한 부여 조회를 기반으로 Adobe Pass 인증 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)에 의해 결정이 생성됩니다.

제공되는 의사 결정에는 호출된 API에 따라 두 가지 유형이 있습니다.

* 유용한 결정 [사전 인증 결정](#preauthorization-decisions)입니다.
* 신뢰할 수 있는 결정 [인증 결정](#authorization-decisions)입니다.

## 사전 인증 결정 {#preauthorization-decisions}

사전 승인 결정은 MVPD이 [보호된 리소스](#protected-resources)에 대한 사용자의 액세스를 허용 또는 거부할 수 있는지 여부를 클라이언트 응용 프로그램에 알릴 수 있도록 하는 유용한 결정입니다.

사전 인가(사전 인가)의 목적은 사용자가 볼 수 있는 콘텐츠에 대한 정확한 정보를 애플리케이션이 표시할 수 있도록 하는 것입니다. 이는 액세스 상태를 반영하도록 잠금 또는 잠금 해제 아이콘과 같은 표시기로 사용자 인터페이스를 향상시킴으로써 달성된다.

>[!IMPORTANT]
>
> 사전 권한 부여 결정은 [권한 부여 결정](#authorization-decisions)의 목적이므로 리소스를 재생하기 위해 신뢰할 수 있는 방식으로 사용할 수 없습니다.

사전 인증 API는 필수가 아니며, 클라이언트 애플리케이션이 필터링하지 않고 리소스 카탈로그를 표시하려는 경우 이 작업을 건너뛸 수 있습니다.

클라이언트 애플리케이션이 이 기능을 사용하려는 경우 API 요청당 제한된 수의 리소스(일반적으로 최대 5개)에 대해서만 사전 권한 부여 결정을 얻을 수 있다는 점을 참고하십시오.

>[!IMPORTANT]
> 
> MVPD 및 Adobe Pass 인증 담당자와 합의한 후에만 최대 리소스 수를 늘릴 수 있습니다. 동의하면 조직의 관리자 또는 Adobe Pass 인증 담당자가 사용자를 대신하여 Adobe Pass TVE 대시보드를 통해 변경 사항을 구현할 수 있습니다.
> 
> 자세한 내용은 [TVE 대시보드 통합 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) 설명서를 참조하십시오.

MVPD는 다양한 메커니즘을 통해 사전 인증을 지원할 수 있으며, 각 메커니즘은 성능에 대한 고유한 의미와 단일 API 요청에서 처리할 수 있는 최대 리소스 수를 가지고 있습니다.

사전 인증을 지원하는 기존 메커니즘에 대한 자세한 내용은 [MVPD Preflight 인증](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md) 설명서를 참조하십시오.

>[!IMPORTANT]
>
> 전체 프리플라이트 권한 부여가 지원되지 않는 MVPD의 경우, 사전 권한 부여의 사용은 성능 문제와 응답 시간 지연을 초래할 수 있으므로 MVPD 및 Adobe Pass 인증 담당자와 미리 합의해야 합니다.

## 인증 결정 {#authorization-decisions}

권한 부여 결정은 클라이언트 응용 프로그램이 [보호된 리소스](#protected-resources)에 대한 사용자의 액세스를 허용 또는 거부하는 MVPD 결정을 준수하도록 허용하는 신뢰할 수 있는 결정입니다.

인증의 목적은 MVPD과의 권한 유효성 검사와 Adobe Pass 인증에서 미디어 토큰을 받은 후, 애플리케이션이 사용자가 요청한 리소스를 재생할 수 있도록 하는 것입니다.

>[!IMPORTANT]
> 
> Adobe Pass 인증은 프로그래머가 미디어 토큰 검증기 라이브러리를 사용하여 권한 부여 결정에 포함된 미디어 토큰의 유효성을 검사하여 비디오 스트림을 시작하기 전에 보안 액세스를 보장하는 것을 권장합니다.
> 
> 자세한 내용은 [미디어 토큰](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md) 설명서를 참조하세요.

인증 API는 필수입니다. 스트림을 릴리스하기 전에 사용자에게 권한이 있는지 MVPD을 통해 확인해야 하므로 클라이언트 애플리케이션은 사용자가 요청하는 리소스를 재생하려는 경우 이 단계를 건너뛸 수 없습니다.

인증 결정은 API 요청당 제한된 리소스(일반적으로 1)에 대해서만 얻을 수 있습니다.

>[!IMPORTANT]
>
> MVPD 및 Adobe Pass 인증 담당자와 합의한 후에만 최대 리소스 수를 늘릴 수 있습니다.

## 보호된 리소스 {#protected-resources}

보호된 리소스는 MVPD와 참여 프로그래머 간의 계약을 통해 정의된 고유 값으로 식별되는 스트리밍 가능한 콘텐츠를 나타냅니다.

보호된 리소스는 계층 트리 구조를 따르며 각 수준은 콘텐츠 인증에 더 많은 세부기간을 제공합니다.

* 네트워크
   * 채널
      * 표시
         * 에피소드
            * 자산

>[!IMPORTANT]
>
> 사전 권한 부여(사전 권한 부여)는 간단한 문자열 또는 MRSS 형식 중 하나를 갖는 식별자가 있는 채널 수준 리소스에 중점을 둡니다.
> 
> 사전 인증의 경우 `CDATA` 섹션이 포함된 식별자의 리소스는 MRSS에서 정의한 에셋 수준 리소스에 주로 사용되므로 사용하지 않는 것이 좋습니다.

### 리소스 식별자 {#resource-identifier}

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

고유 식별자는 주로 Adobe Pass 인증에 불투명하지만 MVPD의 기능 및 요구 사항에 따라 변환기가 적용될 수 있습니다. MVPD에서 리소스 식별자를 인식하거나 구문 분석할 수 없는 경우 Adobe Pass 인증에 오류를 반환하면 [향상된 오류 코드](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)를 사용하여 클라이언트 응용 프로그램에 오류가 전달됩니다.

## REST API V2 {#rest-api-v2}

사전 권한 부여 결정은 다음 API를 사용하여 검색할 수 있습니다.

* [특정 mvpd를 사용하여 사전 인증 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

다음 API를 사용하여 권한 부여 결정을 검색할 수 있습니다.

* [특정 mvpd를 사용하여 권한 부여 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

사전 권한 부여 및 권한 부여 결정의 구조를 이해하려면 위의 API의 **응답** 및 **샘플** 섹션을 참조하십시오.

위의 API를 통합하는 방법 및 시기에 대한 자세한 내용은 다음 문서를 참조하십시오.

* [기본 애플리케이션 내에서 수행되는 기본 사전 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
