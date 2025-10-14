---
title: 표준 메타데이터 속성
description: 표준 메타데이터 속성
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '1053'
ht-degree: 0%

---

# 표준 메타데이터 속성 {#std-metadata-attributes}

이 페이지에서는 동시 모니터링 서비스가 처리할 수 있고 구현할 수 있는 정책의 기반으로 사용할 수 있는 메타데이터 속성의 전체 목록을 제공하는 것을 목적으로 합니다. 표준 메타데이터 속성은 다음과 같이 분류할 수 있습니다.

* 설계에 포함된 속성(URL 경로에 필요하므로 세션 초기화 호출마다 전송됨). 이러한 값 없이 유효한 호출을 수행할 수 없습니다.
* 메타데이터 속성: 세션 초기화 호출 동안 양식 데이터로 전달해야 하는 값(백엔드 정책에 해당 값이 필요한 경우).

## 디자인에 필요한 속성 {#attr-req-by-design}

동시성 모니터링 API는 클라이언트가 유효한 초기화 호출의 일부로 다음 값을 보내도록 강제합니다. [세션 시작 호출](/help/concurrency-monitoring/restrict-concurr-usage-mult-apps.md#api-calls-descr).

| 필드 이름 | 예제 값 | 사용 위치 | 다음에서 획득됨: |
|---------------|-----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | 인증 헤더 | 통합 시 Zendesk 티켓 |
| mvpdName | Sample_MVPD | URI 경로 | 사용자가 MVPD을 선택할 때 구성 끝점에서 Adobe Pass 인증 |
| accountId | 12345 | URI 경로 | 사용자 로그인 후 Adobe Pass 인증 upstreamUserID 메타데이터 [사용자 메타데이터 upstreamUserID - Adobe Pass 인증](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) |


## 메타데이터 속성 {#metadata-attr}

아래 표의 필드는 프로그래머 및 MVPD가 동시 모니터링에서 구현할 정책을 만드는 데 사용할 수 있습니다.

[API v2.0](http://docs.adobeptime.io/cm-api-v2/)을(를) 사용하면 정의된 정책에 이러한 특성이 필요한 경우 해당 특성이 없는 세션 초기화 시도에 의해 400 잘못된 요청이 발생합니다.


| 엔티티 | 속성 이름 | 데이터 유형 | 설명 | 외부 참조(예: EIDR, OATC) | 예제 값 | 유효성 검사 규칙 |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 미디어 회사 | 프로그래머 이름 | 문자열 | 프로그래머의 이름 |                                                   | 프로그래머 |                                                                                   |
| 리소스 | channel | 문자열 | 더 TV 채널 |                                                   | ChannelY |                                                                                   |
|                 | assetId | 문자열 | 이 콘텐츠에 대해 표시할 &quot;친숙한&quot; 또는 소비자가 읽을 수 있는 제목 | [EIDR 2.0 데이터 필드 참조](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | 벤허 |                                                                                   |
|                 | 유형 | 열거 | TveItem으로 표시되는 콘텐츠의 일반 유형을 설명하는 값입니다. 열거된 값은 다음과 같습니다. movie broadcastEpisode nonBroadcastEpisode musicVideo awardsShow 클립 콘서트 컨퍼런스 뉴스이벤트 sportingEvent trailer | [OATC 메타데이터 피드 권장 방법](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | 필드는 열거형의 항목 중 하나에 해당해야 합니다 |
|                 | contentType | 문자열 | 이 필드는 요청된 콘텐츠가 라이브인지 VOD인지 여부를 결정합니다. | 해당 사항 없음 | 라이브, vod | live 또는 vod |
|                 | 장르 | 문자열 | 스트리밍되는 콘텐츠의 장르입니다. 일반 프로그래밍 형식을 설명합니다. | [OATC 메타데이터 피드 권장](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} 연습 | 코메디 | 유효한 장르 유형 |
|                 | 지속 시간 | 숫자 | 미디어 항목의 기간(초) | [OATC 메타데이터 피드 권장 방법](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | 숫자 순서 |
| 장치/브라우저 | deviceId | 문자열 | 고유 디바이스 식별자. | [Device Atlas 속성](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | 문자열 | 이 장치의 알기 쉬운 이름. |                                                   | 조의 iPad |                                                                                   |
|                 | 마케팅 이름 | 문자열 | 장치의 마케팅 이름(또는 고객에게 친숙한 이름) | [Device Atlas 속성](https://deviceatlas.com/device-data/properties){target=_blank} | IPHONE 6 | 유효한 마케팅 이름 |
|                 | mobileDevice | 부울 | 이동 시 디바이스를 사용하는 경우 True입니다. | [Device Atlas 속성](https://deviceatlas.com/device-data/properties){target=_blank} | true, false | true, false |
|                 | deviceModel | 문자열 | 디바이스, 브라우저 또는 기타 구성 요소의 모델 이름 | [Device Atlas 속성](https://deviceatlas.com/device-data/properties){target=_blank} | 태블릿, 휴대폰, xbox. 셋톱 박스 | 유효한 장치 모델 이름 |
|                 | osName | 문자열 | 장치가 실행 중인 운영 체제 | [Device Atlas - OS 사전 정의된 속성 값](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android, Windows 10, OS X, Linux, 기타 참고: 속성 값을 보려면 Device Atlas에서 사용자 이름과 암호로 로그인해야 합니다 | 예상 값은 Device Atlas 사전 정의된 속성에 있는 값 중 하나입니다 |
|                 | browserName | 문자열 | 장치의 브라우저 이름 또는 유형 | [Device Atlas - 브라우저에서 미리 정의된 속성 값](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | 디바이스에 있는 브라우저의 이름 또는 유형입니다.  참고: 속성 값을 보려면 장치 아틀라스에 사용자 이름과 암호로 로그인해야 합니다 | 예상 값은 Device Atlas 사전 정의된 속성에 있는 값 중 하나입니다 |
|                 | browserVersion | 문자열 | 장치의 브라우저 버전 | [Device Atlas 속성](https://deviceatlas.com/device-data/properties){target=_blank} | 장치의 브라우저 버전 |                                                                                   |
| 애플리케이션 | applicationName | 문자열 | 애플리케이션에 대한 &quot;사용자에게 친숙한&quot; 이름 또는 사용자가 읽을 수 있는 이름 | 해당 사항 없음 | Sample_Application |                                                                                   |
|                 | applicationId | 문자열 | 클라이언트 응용 프로그램을 고유하게 식별하는 응용 프로그램 ID입니다. | 해당 사항 없음 | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | 애플리케이션 플랫폼 | 문자열 | 애플리케이션의 기본 플랫폼 | 해당 사항 없음 | ios, android |                                                                                   |
|                 | applicationVersion | 문자열 | 이 값은 분석 목적으로 사용할 수 있습니다. | 해당 사항 없음 | 1.0, 2.0 |                                                                                   |
| 제목 | accountId | 문자열 | 동시 모니터링 제목의 계정 ID(MVPD 범위) | 해당 사항 없음 | test-account |                                                                                   |
|                 | contractType | 문자열 | 프리미엄, 기본. 고객은 이 메타데이터를 사용자 정의 메타데이터로 추가하고 자신의 영역 내에서 사용할 수 있습니다 | 해당 사항 없음 | premium, 기본 |                                                                                   |
| 사용자 | 이름 | 문자열 | 일부 MVPD는 콘텐츠를 재생하는 특정 사용자와 관련된 정보를 제공합니다. | 해당 사항 없음 |                                                                                                                                                         |                                                                                   |
|                 | hba | 부울 | 사용자가 홈 위치에서 스트림을 시작할지 여부를 식별합니다 | 해당 사항 없음 | true, false | true 또는 false |
| 위치 | 대륙 | 문자열 | 재생 요청을 보내는 deviceID가 시작되는 대륙입니다. | 해당 사항 없음 | 북아메리카 | 유효한 대륙 이름 |
|                 | 국가 | 문자열 | 재생 요청을 보내는 deviceID의 출처 국가 | 해당 사항 없음 | 미국 | 유효한 국가 이름 |
|                 | 상태 | 문자열 | 재생 요청을 보내는 deviceID의 출처 상태 | 해당 사항 없음 | CA | 유효한 상태 이름 |
|                 | 도시 | 문자열 | 재생 요청을 보내는 deviceID의 출처 도시 | 해당 사항 없음 | 쿠퍼티노 | 유효한 도시 이름 |
|                 | 우편번호 | 숫자 | 재생 요청을 보내는 deviceID의 출처 우편번호 | 해당 사항 없음 | 95014 | 유효한 우편번호 |
| 스트림 | streamId | 문자열 | 고객 제어가 아닌 CM 서비스에서 생성됨. maxstreams 유형의 규칙이 정의된 경우 묵시적으로 사용됩니다. | 해당 사항 없음 | 해당 사항 없음 | 해당 사항 없음 |
|                 | streamCDN | 문자열 | 스트림을 가져온 CDN을 나타냅니다. | 해당 사항 없음 | 해당 사항 없음 | 해당 사항 없음 |

## 정책 작성에 메타데이터 속성을 사용하는 예제 {#examples-metadata-attr}

표준 메타데이터 필드는 필드 값에 따라 서버측 정책을 정의하는 데 사용할 수 있습니다.

* 특정 필드 값에만 적용되도록 정책을 구성할 수 있습니다(예: 전용 iOS 정책: 여기서 `osType`은(는) `iOS`임).
* 특정 필드에 대해 고유한 값의 수를 제한할 수 있습니다. 몇 가지 예는 다음과 같습니다.
   * 고유 장치가 X개 이하입니다. `HAVING DISTINCT COUNT(deviceId) <= 2`
   * X개 이하의 고유한 우편번호: `HAVING DISTINCT COUNT(zipcode) <= 3`
* 필드 값당 활성 스트림 수를 제한할 수 있습니다. 몇 가지 예는 다음과 같습니다.
   * 단일 장치 형식에 대해 활성 스트림이 X개 이하입니다. `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3` Live 콘텐츠 스트림에 대한 활성 스트림 수 제한

[Zendesk에서 티켓을 만들기](mailto:tve-support@adobe.com)하여 동시성 모니터링 팀에 연락하여 구현할 정책을 지정하십시오.

정책 및 통합 Cookbook의 더 많은 예는 다음에서 찾을 수 있습니다.

* [정책 결정 지점](/help/concurrency-monitoring/cm-policy-decision-point.md)
* [API 콘솔 - 동시 Adobe 모니터링](http://docs.adobeptime.io/cm-api-v2/)
