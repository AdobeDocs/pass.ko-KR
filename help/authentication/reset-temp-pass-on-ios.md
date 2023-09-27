---
title: iOS에서 임시 패스 재설정
description: iOS에서 임시 패스 재설정
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# iOS에서 임시 패스 재설정 {#reset-temp-pass-on-ios}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>

iOS 데모 앱에는 임시 패스 TTL을 재설정하기 위한 전용 화면이 포함되어 있습니다. 재설정 작업에는 다음 정보가 필요합니다.

- **환경:** 임시 패스 네트워크 호출 재설정을 수신할 Adobe Pay-TV Pass 서버 끝점을 지정합니다. 가능한 값: **프리쿠알** (*mgmt-prequal.auth-staging.adobe.com*), **릴리스** (*mgmt.auth.adobe.com*) 또는 **사용자 정의** (Adobe 내부 테스트용으로 예약됨).
- **OAuth2 전달자 토큰:** oauth2 토큰은 프로그래머가 Adobe Pay-TV 인증을 받도록 인증하는 데 필요합니다. 이러한 토큰은 전용 Pay-TV 인증 OAuth2 엔드포인트에서 얻을 수 있습니다 (예: *curl -u &quot;\&lt;consumer _key=&quot;&quot;>:\&lt;consumer _secret=&quot;&quot; _key=&quot;&quot;>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken?grant\_type=client\_credentials&quot;*).
- **요청자 ID:** 현재 프로그래머의 고유 ID. 이 값은 데모 앱의 기본 화면(요청자 필드)에서 읽습니다.
- **임시 통과 ID:** 임시 통과 MVPD에 대한 고유 ID.
- **장치 ID:** 데모 앱에서 계산한 해시된 장치 ID입니다.
- **일반 키:** 일부 임시 패스 MVPD(즉, 다음 확장 가능한 임시 패스 기능)는 임시 패스 재설정을 위한 일반 키(장치 ID와 함께)를 지원합니다.

위의 모든 매개 변수(제외) *일반 키*)은 필수입니다. 다음은 데모 앱에서 수행할 매개 변수 및 관련 네트워크 호출의 예입니다(예제는 *curl *명령의 형식입니다).

- **환경:** 릴리스(*mgmt.auth.adobe.com*)
- **OAuth2 전달자 토큰:** H4j7cF3GtJX81BrsgDa10GwSizVz
- **프로그래머 ID:** 참조
- **임시 통과 ID:** TempPassREF
- **장치 ID:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **일반 키:** null(제공된 값 없음)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

에 대한 DELETE HTTP 요청이 수행됩니다. **/reset** 끝점, 전달 *OAuth2 전달자 토큰* Authorization 헤더 및 *장치 ID*, *요청자 ID* 및 *임시 패스 ID(MVPD ID)* 를 매개 변수로 사용하십시오.

프로그래머가 다음에 대한 값을 제공하는 경우 *일반 키*&#x200B;을 사용하여 다른 HTTP 호출이 수행됩니다(이번에는 **/reset/generic** endpoint), 전달 *일반 키* 의 내부 *key* 요청 매개 변수.

예를 들어, *일반 키* (이러한 기능을 지원하는 Temp Pass MVPD의 경우) 전자 메일 주소 해시에 대해 다음과 같은 HTTP 호출이 발생합니다(전자 메일은 다음과 같음). `user@domain.com` SHA-256 해시는 `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

데모 앱에서 임시 패스를 재설정하면 동일한 장치의 프로그래머 앱에 대해 동일한 효과가 없을 수 있다는 점을 강조하는 것이 중요합니다. 이는 데모 앱 및 AccessEnabler에서 계산한 디바이스 ID가 디바이스의 모든 애플리케이션에 대해 동일하지 않을 수 있기 때문입니다.

- iOS 6 이하: 장치 ID는 MAC 주소(모든 앱에 대해 고유함)를 사용하여 계산되므로 데모 앱에서 임시 패스를 재설정하면 장치의 다른 모든 프로그래머 앱에서 재설정됩니다.

- iOS 7 및 상위: 장치 ID는 IDFV(공급업체의 ID) 값을 기반으로 계산됩니다. 이 값은 동일한 번들 ID 접두사가 있는 모든 응용 프로그램(즉, 마지막 항목을 제외한 모든 구성 요소)에 대해 고유합니다. 데모 앱과 프로그래머 앱에는 서로 다른 번들 ID가 있으므로 데모 앱에서 Temp Pass를 재설정해도 프로그래머 앱에는 영향을 미치지 않습니다.

마지막 사용 사례(iOS 7 이상)가 가장 일반적이므로 프로그래머가 이러한 상황에서 앱에 대한 임시 패스를 재설정하는 방법을 살펴보겠습니다. 다음과 같은 몇 가지 옵션이 있습니다.

1. 데모 앱의 코드를 프로그래머 앱에 연결합니다. 다음 *TempPassResetViewController* 및 *DeviceIdDemoApp* 클래스에는 Temp Pass를 재설정하기 위한 핵심 논리가 포함되어 있으며 이를 쉽게 수정하여 프로그래머 앱에 포함할 수 있습니다.

1. 다음을 사용하여 임시 전달 재설정에 대한 HTTP 요청 실행 *컬*. device\_Id 매개 변수는 프로그래머 앱의 IDFV를 계산하고 그 위에 SHA-256 해시를 적용하여 얻을 수 있습니다(의 샘플 코드) *DeviceIdDemoApp* class).

1. 프로그래머 앱의 해시된 IDFV를 로 지정하여 데모 앱에서 재설정을 수행하면 됩니다. *일반 키*. 이렇게 하면 두 개의 네트워크 호출이 발생합니다(프로그래머와 관련 없는 데모 앱에 대한 임시 패스 재설정 및 프로그래머 앱에 대한 임시 패스 재설정).

위의 모든 옵션은 유사하며, 구현의 용이성에 따라 하나를 선택하는 것은 프로그래머가 결정합니다.
