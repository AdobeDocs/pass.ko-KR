---
title: Adobe Pass 인증에서 Experience Cloud ID 사용
description: Adobe Pass 인증에서 Experience Cloud ID 사용
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: 26245e019afac2c0844ed64b222208cc821f9c6c
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---

# Adobe Pass 인증에서 Experience Cloud ID 사용

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## Experience Cloud ID란 무엇이며 어떻게 얻습니까? {#what-exp-cloud-id-obtain}

Experience Cloud ID(줄여서 ECID)는 Adobe Experience Cloud이 애플리케이션/웹 사이트의 각 개별 사용자에 대해 생성한 고유 ID입니다. ECID는 여러 애플리케이션/웹 사이트에서 특정 사용자에 대한 정보를 함께 연결하는 데 사용되는 모든 Experience Cloud 보고서에 많이 사용됩니다.

방문자 ID를 제공하는 시스템이 이미 있는 경우 이 문서의 범위에 동일한 ID를 사용해야 합니다.

ECID를 얻는 한 가지 방법은 Experience Cloud ID 서비스를 사용하는 것입니다. TDM, JS 라이브러리, 서버측, 직접 통합 또는 모바일 플랫폼용 기본 라이브러리를 기반으로 원하는 구현 유형을 사용할 수 있습니다. 사용 가능한 서비스, 라이브러리, SDK 및 구현 안내서를 종합적으로 보려면 <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html?lang=ko>을(를) 참조하십시오.

## Adobe Pass 인증에서 Experience Cloud ID를 사용하면 어떤 이점이 있습니까? {#benefit-ex-cloud-id}

ECID를 사용하도록 SDK 및 Clientless REST API를 구성하는 경우 나중에 Adobe Pass 인증으로 수집된 데이터를 기존 Experience Cloud 솔루션에 연결할 수 있습니다. 이렇게 하면 Adobe에서 제공하는 모든 솔루션에서 고객의 여정 및 경험을 더 잘 이해할 수 있습니다.

## Adobe Pass 인증에서 Experience Cloud ID를 사용하는 방법 {#how-to-ex-cloud-id-authn}

ECID(위에 설명)를 가져온 후에는 이 정보를 SDK 및 Clientless REST API에 전달해야 합니다. 이 정보는 나중에 SDK에서 수행하는 각 네트워크 호출의 서버에 전달됩니다. 구성 프로세스는 다음과 같이 모든 SDK에 대해 다릅니다.

### JS SDK {#js-sdk}

JavaScript의 경우 맵의 ECID를 setRequestor 호출에 세 번째 매개 변수로 전달해야 합니다.

**사용 예:**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### iOS/tvOS SDK {#ios-sdk}

iOS/tvOS SDK의 경우 setOptions라는 전용 메서드가 있습니다.

**사용 예:**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/fireTV SDK {#android-sdk}

Android/fireTV SDK의 경우 메커니즘은 iOS과 유사합니다. 매개 변수 이름만 다릅니다. API는 여기에 설명되어 있습니다.

**사용 예:**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### 클라이언트 없는 API {#clientless-api}

REST API v1을 통해 Adobe Pass을 사용하는 경우 **ECID** 값을 **모든 API에서**(이)라는 매개 변수 **&#39;ap_vi&#39;**(으)로 전송해야 합니다.

**사용 예:**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`

### REST API V2 {#rest-api-v2}

REST API v2를 통해 Adobe Pass을 사용하는 경우 **ECID** 값을 **모든 API에서**(이)라는 헤더로 **&#39;AP-Visitor-Identifier&#39;**(으)로 전송해야 합니다.

**사용 예:**

`POST: https://api.auth.adobe.com/api/v2/${serviceProvider}/sessions/`\
헤더:\
`AP-Visitor-Identifier: THE_ECID_VALUE`

