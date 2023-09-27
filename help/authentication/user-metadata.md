---
title: 사용자 메타데이터
description: 사용자 메타데이터
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---

# 사용자 메타데이터 {#user-metadata}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## REST API 끝점 {#clientless-endpoints}

`<REGGIE_FQDN>`:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 설명 {#description}

MVPD가 인증된 사용자에 대해 공유한 메타데이터를 검색합니다.


| 엔드포인트 | 호출됨  </br>작성자: | 입력   </br>매개 변수 | HTTP  </br>방법 | 응답 | HTTP  </br>응답 |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | 스트리밍 앱</br></br>또는</br></br>프로그래머 서비스 | 1. 요청자</br>2.  deviceId(필수)</br>3.  device_info/X-Device-Info (필수)</br>4.  deviceType</br>5.  deviceUser(사용되지 않음)</br>6.  appId(사용하지 않음) | GET | 실패한 경우 사용자 메타데이터 또는 오류 세부 정보가 포함된 XML 또는 JSON입니다. | 200 - 성공<p>404 - 메타데이터를 찾을 수 없음<p>412 - 잘못된 AuthN 토큰(예: 만료된 토큰) |


| 입력 매개 변수 | 설명 |
| --- | --- |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |
| deviceId | 장치 ID 바이트입니다. |
| device_info/<p>X-Device-Info | 스트리밍 장치 정보입니다.</br></br> **참고:** 이 URL은 URL 매개 변수로 device_info를 전달할 수 있지만, 이 매개 변수의 잠재적 크기와 GET URL 길이의 제한으로 인해 HTTP 헤더에서 X-Device-Info로 전달해야 합니다. </br></br> 의 전체 세부 정보 보기 [전달 장치 및 연결 정보](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | 디바이스 유형(예: Roku, PC).</br></br> 이 매개 변수가 올바르게 설정되면 ESM은 다음과 같은 지표를 제공합니다. [장치 유형별 분류](/help/authentication/entitlement-service-monitoring-overview.md#progr-filter-metrics) 예: Roku, AppleTV, Xbox 등에 대해 다양한 유형의 분석을 수행할 수 있도록 Clientless를 사용할 때</br></br> 다음을 참조하십시오 [전달 지표에서 클라이언트 없는 장치 유형 매개 변수 사용의 이점](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **참고:** 다음 `device_info` 는 이 매개 변수를 대체합니다. |
| _deviceUser_ | 장치 사용자 식별자.</br></br> **참고:** 사용하는 경우 `deviceUser` 은(는) 과(와) 동일한 값을 가져야 합니다. [등록 코드 만들기](/help/authentication/registration-code-request.md) 요청. |
| _appId_ | 애플리케이션 ID/이름입니다. </br></br> **참고:** 다음 `device_info` 는 이 매개 변수를 대체합니다. 사용하는 경우 `appId` 은(는) 과(와) 동일한 값을 가져야 합니다. [등록 코드 만들기](/help/authentication/registration-code-request.md) 요청. |

>[!NOTE]
> 
>사용자 메타데이터 정보는 인증 흐름이 완료된 후에 사용할 수 있어야 하지만 인증 흐름에서 MVPD 및 메타데이터 유형에 따라 업데이트할 수 있습니다.




## 샘플 응답 {#sample-response}

호출이 성공하면 서버는 아래 표시된 구조와 유사한 구조를 가진 XML(기본값) 또는 JSON 개체로 응답합니다.


```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
              zip: ["12345", "34567"],
              maxRating: { 
                  "MPAA": "PG-13",
                  "VCHIP": "TV-Y", 
                  "URL": "http://exam.pl/e/manage/ratings"
                         },
              householdID: "3456",
              userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
              channelID: ["channel-1", "channel-2"]
              }
    }
```

오브젝트의 루트에는 세 개의 노드가 있습니다.

* *업데이트됨*: 메타데이터가 마지막으로 업데이트된 시간을 나타내는 UNIX 타임스탬프를 지정합니다. 이 속성은 인증 단계 중에 메타데이터를 생성할 때 서버에 의해 초기에 설정됩니다. 메타데이터가 업데이트된 후 후속 호출을 수행하면 타임스탬프가 증가합니다.
* *데이터*: 실제 메타데이터 값을 포함합니다.
* *암호화됨*: 암호화된 속성을 나열하는 배열입니다. 특정 메타데이터 값을 해독하려면 프로그래머는 해당 메타데이터에 대해 Base64 복호화를 수행한 다음 자체 개인 키를 사용하여 결과 값에 RSA 복호화를 적용해야 합니다(Adobe은 프로그래머의 공개 인증서를 사용하여 서버의 메타데이터를 암호화함).

오류가 발생한 경우 서버는 자세한 오류 메시지를 지정하는 XML 또는 JSON 개체를 반환합니다.

자세한 내용은 [사용자 메타데이터](/help/authentication/user-metadata-feature.md).

[REST API 참조로 돌아가기](/help/authentication/rest-api-reference.md)
