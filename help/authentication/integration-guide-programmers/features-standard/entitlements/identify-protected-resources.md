---
title: 보호된 리소스 식별
description: 보호된 리소스 식별
exl-id: e96aea02-54b2-491d-ba91-253c0d0e681c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# 보호된 리소스 식별 {#identifying-protected-resources}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

각 인증 요청(또는 인증 확인 요청)에는 사용자가 액세스를 요청하는 보호된 리소스에 대한 고유 식별자가 포함되어야 합니다. 보호된 리소스는 MVPD와 참여 프로그래머 사이에 합의된 대로 모든 수준의 승인된 콘텐츠가 될 수 있습니다. 보호되는 리소스가 점점 세분화되는 이 트리 구조에 적합해야 합니다.

- 네트워크
   - 채널
      - 표시
         - 에피소드
            - 자산

</br>

## 미디어 RSS 형식 {#media_rss}

리소스는 간단한 문자열(채널의 고유 식별자)로 식별하거나, Adobe(또는 Adobe Pass 인증 승인 파트너)와 참여 MVPD 및 프로그래머 간에 합의된 대로 MRSS(미디어 RSS 형식)로 나타낼 수 있습니다. 리소스 지정자로 사용되는 RSS 문자열에는 등급 및 자녀 보호 메타데이터와 같은 추가 정보가 포함될 수 있습니다.


&quot;TNT&quot;와 같은 단순 리소스 식별자를 사용하는 경우 이 리소스 식별자가 채널을 나타낸다고 가정하고 이 RSS 리소스 지정자로 변환됩니다.

```RSS
    <rss version="2.0"> 
        <channel>
            <title>TNT</title>
        </channel>
    </rss>
```


예를 들어, 보다 복잡한 지정자에는 추가 등급 정보가 포함될 수 있습니다. 리소스 ID가 필요한 Access Enabler 함수(예: [`getAuthorization()`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md))에 전체 RSS 문자열을 전달할 수 있습니다.

```rss
    var resource = 
        '<rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
             <channel>
                 <title>TNT</title>
                 <media:rating scheme="urn:mpaa">pg</media:rating>
             </channel>
         </rss>'; 
    getAuthorization(resource);
```

리소스 지정자는 Adobe Pass 인증에 불투명하며 단순히 MVPD로 전달됩니다. MVPD가 리소스 지정자를 인식하지 못하거나 구문 분석할 수 없으면 Adobe Pass 인증에 오류를 반환하여 `tokenRequestFailed()` 콜백에 오류를 다시 전달합니다.

<!--
## Related Information {#related}

-  User Metadata
-  Preflight Authorization
-->
