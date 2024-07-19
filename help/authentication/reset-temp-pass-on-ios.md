---
title: iOS에서 임시 패스 재설정
description: iOS에서 임시 패스 재설정
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---

# iOS에서 임시 패스 재설정 {#reset-temp-pass-on-ios}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>

iOS 데모 앱에는 임시 패스 TTL을 재설정하기 위한 전용 화면이 포함되어 있습니다. 재설정 작업에는 다음 정보가 필요합니다.

- **환경:**&#x200B;은(는) 임시 패스 네트워크 호출 재설정을 수신할 Adobe Pay-TV Pass 서버 끝점을 지정합니다. 가능한 값: **Prequal**(*mgmt-prequal.auth-staging.adobe.com*), **Release**(*mgmt.auth.adobe.com*) 또는 **Custom**(Adobe 내부 테스트용으로 예약됨).
- **OAuth2 전달자 토큰:** OAuth2 토큰은 프로그래머가 Adobe Pay-TV 인증을 사용하도록 인증하는 데 필요합니다. 이러한 토큰은 전용 Pay-TV 인증 OAuth2 엔드포인트(예: *curl -u &quot;\&lt;consumer\_key\>:\&lt;consumer\_secret\_key\>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken?grant\_type=client\_credentials&quot;*)에서 가져올 수 있습니다.
- **요청자 ID:** 현재 프로그래머의 고유 ID입니다. 이 값은 데모 앱의 기본 화면(요청자 필드)에서 읽습니다.
- **임시 패스 ID:** 임시 패스 MVPD에 대한 고유 ID입니다.
- **장치 ID:** 데모 앱에서 계산한 장치 ID를 해시했습니다.
- **일반 키:** 일부 임시 패스 MVPD(즉, 다음 확장 가능한 임시 패스 기능)는 임시 패스 재설정을 위한 일반 키(장치 ID 포함)를 지원합니다.

위의 모든 매개 변수(*일반 키* 제외)는 필수입니다. 다음은 데모 앱에서 수행할 매개 변수 및 관련 네트워크 호출의 예입니다(예제는 *curl *명령의 형식입니다).

- **환경:** 릴리스(*mgmt.auth.adobe.com*)
- **OAuth2 전달자 토큰:** H4j7cF3GtJX81BrsgDa10GwSizVz
- **프로그래머 ID:** 참조
- **임시 통과 ID:** TempPassREF
- **장치 ID:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **일반 키:** null(제공된 값 없음)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

인증 헤더의 *OAuth2 전달자 토큰* 및 매개 변수로 *장치 ID*, *요청자 ID* 및 *임시 패스 ID(MVPD ID)*&#x200B;을(를) 전달하여 **/reset** 끝점에 DELETE HTTP 요청이 이루어집니다.

프로그래머가 *일반 키*&#x200B;에 대한 값을 제공하면 다른 HTTP 호출이 수행되며(이번에는 **/reset/generic** 끝점에 대해), *key* 요청 매개 변수 내에 *일반 키*&#x200B;을(를) 전달합니다.

예를들어, *일반 키*을(를) 전자 메일 주소 해시로 설정합니다.
이러한 기능을 지원하는 임시 통과 MVPD는
다음 HTTP 호출(전자 메일은 SHA-256의 `user@domain.com`입니다.)
해시는 `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`입니다.

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

데모 앱에서 임시 패스를 재설정하면 동일한 장치의 프로그래머 앱에 대해 동일한 효과가 없을 수 있다는 점을 강조하는 것이 중요합니다. 이는 데모 앱 및 AccessEnabler에서 계산한 디바이스 ID가 디바이스의 모든 애플리케이션에 대해 동일하지 않을 수 있기 때문입니다.

- iOS 6 이하: 장치 ID는 MAC 주소(모든 앱에 대해 고유함)를 사용하여 계산되므로 데모 앱에서 임시 패스를 재설정하면 장치의 다른 모든 프로그래머 앱에서 재설정됩니다.

- iOS 7 및 상위: 장치 ID는 IDFV(공급업체의 ID) 값을 기반으로 계산됩니다. 이 값은 동일한 번들 ID 접두사가 있는 모든 응용 프로그램(즉, 마지막 항목을 제외한 모든 구성 요소)에 대해 고유합니다. 데모 앱과 프로그래머 앱에는 서로 다른 번들 ID가 있으므로 데모 앱에서 Temp Pass를 재설정해도 프로그래머 앱에는 영향을 미치지 않습니다.

마지막 사용 사례(iOS 7 이상)가 가장 일반적이므로 프로그래머가 이러한 상황에서 앱에 대한 임시 패스를 재설정하는 방법을 살펴보겠습니다. 다음과 같은 몇 가지 옵션이 있습니다.

1. 데모 앱의 코드를 프로그래머 앱에 연결합니다. *TempPassResetViewController* 및 *DeviceIdDemoApp* 클래스에는 Temp 패스를 재설정하는 코어 논리가 포함되어 있으며 이를 쉽게 수정하여 프로그래머 앱에 포함할 수 있습니다.

1. *curl*&#x200B;을(를) 사용하여 Temp Pass를 재설정하기 위한 HTTP 요청을 실행합니다. device\_Id 매개 변수는 프로그래머 앱의 IDFV를 계산하고 SHA-256 해시를 적용하여 얻을 수 있습니다(*DeviceIdDemoApp* 클래스의 샘플 코드).

1. 프로그래머 앱의 해시된 IDFV를 *일반 키*(으)로 지정하여 데모 앱에서 재설정을 수행하면 됩니다. 이렇게 하면 두 개의 네트워크 호출이 발생합니다(프로그래머와 관련 없는 데모 앱에 대한 임시 패스 재설정 및 프로그래머 앱에 대한 임시 패스 재설정).

위의 모든 옵션은 유사하며, 구현의 용이성에 따라 하나를 선택하는 것은 프로그래머가 결정합니다.
