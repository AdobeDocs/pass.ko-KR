---
title: 개인 정보 보호 요청을 하는 방법
description: 개인 정보 보호 요청을 하는 방법
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# 개인 정보 보호 요청을 하는 방법 {#howto-make-privacy-request}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 식별자 및 네임스페이스 {#identifier-namespace}

액세스 또는 삭제 개인 정보 보호 요청을 보낼 때 고객 애플리케이션은 다음 식별자를 포함해야 합니다.

* **mvpdID** - MVPD의 고유 식별자입니다.
* **userID** - 프로그래머 앱의 사용자를 고유하게 식별하지만 MVPD에서 비롯됩니다. 프로그래머 개요에서 사용자 ID 이해 를 참조하십시오.
* **IMSOrgID** - Adobe Experience Cloud에서 고객을 고유하게 식별하는 Adobe Experience Cloud Identity Management 서비스 조직 ID입니다.


아래 샘플을 확인하십시오.

```JSON
"userIDs": [{
     "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",  -----> "Dish" is the id of the MVPD
     "type":"unregistered",
     "value":"1234-5678-8765-4321" ----> "1234-5678-8765-4321" is the userId associated with the MVPD account
}]
```

>[!IMPORTANT]
>
>Adobe Pass 인증에 대한 개인 정보 요청을 생성하려면 사용자가 인증되어야 합니다. 그렇지 않으면 프로그래머는 MVPD userID를 추출하는 다른 방법을 찾아야 합니다.

## 요청 유형 {#req-type}

Adobe Pass 인증은 액세스 및 삭제 요청을 지원합니다.

### 액세스 {#access-req}

Access 요청의 경우:

해당 데이터 주체에 대해 생성된 인증 및 권한 부여 요청의 총 수에 대한 요약이 포함된 JSON 파일을 다시 제공합니다.
이러한 모든 이벤트는 고객별로 필터링됩니다.


**요청 샘플**

데이터 액세스 요청을 제출할 Adobe Pass 인증 식별자와 함께 JSON을 업로드해야 합니다. 잘 구성된 JSON의 모습을 보려면 다음 샘플을 참조하십시오.

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["access"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**응답 샘플**

```JSON
{
    "jobId": "d9a6b417-f619-4420-82a3-09f61fa8eff3",
    "requestId": "15765127177927284RX-739",
    "userKey": "John Dow",
    "action": "access",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/16/2019 04:11 PM GMT",
    "lastModifiedDate": "12/16/2019 04:15 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/16/2019 04:15 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-16T16:15:23.4Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "6",
                        "numberOfAuthorizationDecisions": "11"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/d9a6b417-f619-4420-82a3-09f61fa8eff3/d9a6b417-f619-4420-82a3-09f61fa8eff3.zip",
    "regulation": "ccpa"
}
```

### 삭제 {#delete-req}

데이터 삭제 요청을 제출하는 Adobe Pass 인증 식별자와 함께 JSON을 업로드해야 합니다. 잘 구성된 JSON의 모습을 보려면 다음 샘플을 참조하십시오.

**요청 샘플**

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["delete"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**응답 샘플**

Delete 요청의 경우:

* 제거된 모든 데이터와 집계된 파일이 아니라 데이터가 제거된 영수증만 공유합니다.
* 응답에 포함된 영수증에는 해당 데이터 주체에 대해 발견된 총 인증 및 권한 부여 토큰 수의 요약이 포함되어 있습니다.

```JSON
{
    "jobId": "aab380d1-a0cd-4a0d-ba95-2649ee90c063",
    "requestId": "15759883098453100RX-074",
    "userKey": "John Dow",
    "action": "delete",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/10/2019 02:31 PM GMT",
    "lastModifiedDate": "12/10/2019 02:34 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/10/2019 02:34 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-10T14:34:55.274Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "2",
                        "numberOfAuthorizationDecisions": "3"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/aab380d1-a0cd-4a0d-ba95-2649ee90c063/aab380d1-a0cd-4a0d-ba95-2649ee90c063.zip",
    "regulation": "ccpa"
}
```

## 요청을 트리거하는 방법 {#trigger-req}

고객이 Adobe에 개인 정보 보호 요청을 보낼 수 있는 옵션은 두 가지가 있습니다.

* **수동으로** - [Privacy Service 사용자 인터페이스 사용](#privacy-service-ui)
* **자동으로** - [Privacy Service API 사용](#privacy-service-api)

### Privacy Service UI 사용 {#privacy-service-ui}

Privacy Service 사용자 인터페이스에 액세스하고 사용하는 방법에 대한 [전체 자습서](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)를 Adobe I/O 서비스를 통해 온라인으로 사용할 수 있습니다. 또한 고객은 이 링크를 사용하여 개인 정보 보호 규정에 대한 비디오 및 문서 라이브러리에 액세스할 수 있습니다. Adobe Experience Cloud 및 GDPR 메뉴를 클릭합니다. 이렇게 하면 많은 비디오가 열립니다. 이 비디오의 사용 방법은 &quot;GDPR UI 방법&quot;을 설명합니다.

UI에서 고객은 각 제품에 대한 GDPR 요청 세부 사항이 포함된 JSON 및 자체 IMSOrgID를 로드해야 합니다.

### Privacy Service API 사용 {#privacy-service-api}

Adobe Experience Platform Privacy Service은 개인 데이터에 대한 액세스/삭제 요청 및 판매 중지 요청에 대한 중앙의 공통 지원 기능을 제공합니다.

**Privacy Service API 설명서**&#x200B;에서는 Adobe 고객이 Adobe API와 통합할 수 있는 방법에 대해 자세히 설명합니다.

**Postman(무료 타사 소프트웨어)를 사용하여 API 호출 시각화:**

* [GitHub에서 API Postman 컬렉션 Privacy Service](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [Postman 환경 만들기에 대한 비디오 안내서](https://video.tv.adobe.com/v/28832)
* [Postman에서 환경 및 컬렉션을 가져오는 단계](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**API 경로:**

* 플랫폼 게이트웨이 URL: `https://platform.adobe.io/`
* 이 API의 기본 경로: `/data/core/privacy/jobs`
* 전체 경로의 예: `https://platform.adobe.io/data/core/privacy/jobs/ping`


**필수 헤더:**

* 모든 호출에는 헤더 `Authorization`, `x-gw-ims-org-id` 및 `x-api-key`이(가) 필요합니다. 이러한 값을 얻는 방법에 대한 자세한 내용은 **인증 자습서**&#x200B;를 참조하십시오.
* 요청 본문에 페이로드가 있는 모든 요청(예: POST, PUT 및 PATCH 호출)에는 값이 `application/json`인 헤더 `Content-Type`이(가) 포함되어야 합니다.

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
