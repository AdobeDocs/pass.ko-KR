---
title: 사용자 메타데이터
description: 사용자 메타데이터
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '1793'
ht-degree: 0%

---

# 사용자 메타데이터 {#user-metadata}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

사용자 메타데이터는 MVPD에 의해 유지되고 Adobe Pass 인증 [REST API V2](#apis)을(를) 통해 프로그래머에게 제공되는 사용자별 [특성](#attributes)(예: 우편 번호, 자녀 보호 등급, 사용자 ID 등)을 참조합니다.

사용자 메타데이터는 사용자의 개인화를 향상시키는 데 사용할 수 있지만 분석에 사용할 수도 있습니다. 예를 들어 프로그래머는 사용자의 우편 번호를 사용하여 현지화된 뉴스 또는 날씨 업데이트를 제공하거나 자녀 보호를 적용할 수 있습니다.

Adobe Pass 인증은 MVPD가 데이터를 다양한 형식으로 제공할 때 사용자 메타데이터 값을 표준화합니다. 또한 특정 특성(예: 우편 번호)의 경우 프로그래머 인증서를 사용하여 값을 [암호화](#encryption)할 수 있습니다.

Adobe Pass 인증을 사용하면 프로그래머가 MVPD 통합에서 사용할 수 있는 사용자 메타데이터를 검토하고 [Adobe Pass TVE 대시보드](https://experience.adobe.com/#/pass/authentication)를 통해 [관리](#management)할 수 있습니다.

## 사용자 메타데이터 속성 {#attributes}

다음 표에는 프로그래머가 사용할 수 있는 사용자 메타데이터 특성 중 일부가 나열되어 있습니다.

| 키 | 유형 | 샘플 | 암호화 필요 | 설명 | 세부 사항 |
|------------------|---------|--------------------------------------------------------------|---------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `userID` | 문자열 | &quot;1o7241p&quot; | 아니요 | 계정 식별자. | 속성 값은 가정용 식별자 또는 하위 계정 식별자일 수 있습니다. MVPD이 하위 계정을 지원하고 현재 사용자가 기본 계정 소유자가 아닌 경우 `userID` 값이 `householdID`과(와) 다릅니다. |
| `upstreamUserID` | 문자열 | &quot;1o7241p&quot; | 아니요 | 동시 모니터링을 위한 계정 식별자. | 속성 값을 사용하여 MVPD 및 프로그래머 사이트 및 앱에서 동시 실행 제한을 적용할 수 있습니다. `upstreamUserID` 값은 대부분의 MVPD에 대한 `userID` 값과 동일합니다. |
| `householdID` | 문자열 | &quot;1o7241p&quot; | 아니요 | 자녀 보호 계정 식별자. | 속성 값은 가계 사용과 하위 계정 사용을 구분하는 데 사용할 수 있습니다. 경우에 따라 true 등급을 사용할 수 없는 경우 보호자 통제 대용품으로 사용할 수 있으며, 사용자가 가구 계정으로 로그인한 경우 시청할 수 있습니다. 그렇지 않으면 등급이 지정된 콘텐츠가 표시되지 않습니다. MVPD의 표시 방법에 대한 다양한 변형이 있습니다(예: 가구 사용자 ID, 가구 주 ID, 가구 주 플래그 등). MVPD에서 하위 계정을 지원하지 않는 경우 `userID`과(와) 동일합니다. |
| `primaryOID` | 문자열 | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | 아니요 | 계정 식별자. | 속성은 AT&amp;T에만 적용됩니다. `typeID` 값이 &quot;기본&quot;으로 설정된 경우 `primaryOID` 값은 `userID` 값과 동일합니다. |
| `typeID` | 문자열 | &quot;기본&quot; | 아니요 | 현재 사용자가 기본 또는 보조 계정 소유자인지 여부를 나타내는 속성입니다. | 속성은 AT&amp;T에만 적용됩니다. `typeID` 값이 &quot;기본&quot;으로 설정된 경우 `primaryOID` 값은 `userID` 값과 동일합니다. |
| `is_hoh` | 문자열 | &quot;1&quot; | 아니요 | 현재 사용자가 세대주인지 여부를 나타내는 속성. | 속성은 Synacor에 따라 다릅니다. |
| `hba_status` | 부울 | &quot;true&quot; | 아니요 | 현재 사용자가 HBA를 통해 인증되었는지 여부를 나타내는 속성입니다. |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `allowMirroring` | 부울 | &quot;true&quot; | 아니요 | 현재 디바이스가 화면을 미러링할 수 있는지 여부를 나타내는 속성입니다. | 속성은 스펙트럼에 따라 다릅니다. |
| `zip` | 배열 | \[&quot;77754&quot;, &quot;12345&quot;\] | 예 | 사용자의 우편 번호입니다. | 속성 값을 사용하여 현지화된 뉴스, 날씨 업데이트 또는 스포츠 이벤트를 전달할 수 있습니다. `zip` 값은 MVPD과 법적 합의가 필요한 중요한 데이터를 나타냅니다. 암호화되면 `zip` 키의 표현은 `Array`이(가) 아닌 `String`이(가) 됩니다. |
| `encryptedZip` | 문자열 | &quot;&quot; | 예 | 사용자의 암호화된 우편 번호입니다. | 속성은 Comcast에만 적용됩니다. |
| `channelID` | 배열 | \[&quot;channel-1&quot;, &quot;channel-2&quot;\] | 아니요 | 사용자가 볼 수 있는 채널 목록입니다. | 속성 값을 사용하여 여러 네트워크를 집계하는 포털에서 다양한 채널을 필터링할 수 있습니다. 이 사용자 메타데이터 특성 대신 [사전 권한 부여 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)를 사용하여 사용자가 사용할 수 없는 채널을 필터링하는 것이 좋습니다. |
| `maxRating` | 오브젝트 | { MPAA: &quot;NR&quot;, VCHIP: &quot;X&quot;, URL: &quot;http://manage.my/parental&quot; } | 아니요 | 현재 사용자의 최대 자녀 보호 등급입니다. | 속성 값을 사용하여 &quot;MPAA&quot; 또는 &quot;VCHIP&quot; 등급을 기반으로 현재 사용자에게 적합하지 않은 콘텐츠를 필터링할 수 있습니다. |
| `language` | 문자열 | &quot;영어&quot; | 아니요 | 언어 설정. | 속성 값을 사용하여 사용자의 언어 환경 설정에 따라 메시지를 표시할 수 있습니다. |

프로그래머가 사용할 수 있는 사용자 메타데이터 특성은 MVPD이 제공하는 항목에 따라 다릅니다. 다음 표에는 다양한 MVPD에서 사용할 수 있는 특성이 나열되어 있습니다.

|                         | **서명된 사용권 계약(zip만 해당)** | **AuthN의 사용자 ID** | **AuthN의 업스트림 사용자 ID** | **AuthN/Z의 세대 ID** | **AuthN의 기본 OID** | **AuthN의 유형 ID** | **AuthN의 세대주** | **HBA 상태** | **AuthZ에서 미러링 허용** | **AuthN/Z의 우편번호** | **AuthN의 채널 ID** | **AuthN/Z의 평가** | **언어** | **onNet** | **inHome** | **메모** |
|-------------------------|---------------------------------------|----------------------|-------------------------------|-----------------------------|--------------------------|----------------------|--------------------------------|----------------|------------------------------|-------------------------|-------------------------|-----------------------|--------------|-----------|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **정식 이름** | 해당 사항 없음 | `userID` | `upstreamUserID` | `householdID` | `primaryOID` | `typeID` | `is_hoh` | `hba_status` | `allowMirroring` | `zip` | `channelID` | `maxRating` | `language` | `onNet` | `inHome` |                                                                                                                                           |
| **암호화 필요** | 해당 사항 없음 | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| **중요** | 해당 사항 없음 | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| Adobe IdP | **예** | **예** | **예** | **예(AuthN만 해당)** | **예** | **예** | **예** | **아니요** | **아니요** | **예(AuthN만 해당)** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | 법적 합의가 필요하지 않습니다. |
| 시나코 | **예** | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **예(AuthN만 해당)** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | 모든 프록시화된 MVPD를 다루지 않는 법적 합의. 이는 Synacor에 대한 일반 지원이며 모든 MVPD로 롤업되지는 않을 수 있습니다. |
| 접시 | **아니요** | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | 모든 Synacor MVPD와 `upstreamUserID` 목록을 공유합니다. |
| 컴캐스트 | **아니요** | **예** | **예** | **예(AuthZ만 해당)** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **예(AuthZ만 해당)** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| AT&amp;T | **예** | **예** | **예** | **예(AuthN만 해당)** | **예** | **예** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | 법률 협정에 서명했습니다. |
| DTV | **예** | **예** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| 콕스 | **아니요** | **예** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| 케이블 비전 | **예** | **예** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | 법률 협정에 서명했습니다. |
| 스펙트럼 | **예** | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| 헌장 | **예** | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| 버라이즌 | **아니요** | **예** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| HTC | **아니요** | **예** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| 로저스 | **아니요** | **예** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| RCN | **예** | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| 이스트링크 | **아니요** | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| 코게코 | **아니요** | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| 비디오트론 | **아니요** | **예** | **예** | **예*** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | `userID`과(와) 동일한 값으로 `householdID`을(를) 노출합니다. |
| 프록시 Massilon | **예** | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | 법률 협정에 서명했습니다. |
| 프록시 Clearleap | **예** | **예** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **아니요** | **예(AuthZ만 해당)** | **예** | **아니요** | **아니요** | 법률 협정에 서명했습니다. |
| 프록시 GLDS | **아니요** | **예** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** |                                                                                                                                           |
| 기타 MVPD | **아니요** | **예** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | 아직 사용권 계약이 없습니다. 중요한 메타데이터는 프로덕션에 사용할 수 없습니다. 모든 MVPD에 대해 추가 작업 없이 `userID`을(를) 사용할 수 있습니다. |

>[!IMPORTANT]
>
> 중요한 사용자 메타데이터(예: 우편 번호)를 사용할 수 있으려면 먼저 MVPD와 법적 계약을 체결해야 합니다.

## 사용자 메타데이터 암호화 {#encryption}

사용자 메타데이터 특성을 암호화하고 해독하려면 프로그래머가 인증서(공개/개인 키 쌍)를 생성하고 [Adobe Pass TVE 대시보드](https://experience.adobe.com/#/pass/authentication)를 통해 인증서를 [자체 구성](#management)하거나 Adobe Pass 인증 담당자와 공개 키를 공유해야 합니다.

인증서가 올바르게 생성 및 구성되었는지 확인하려면 아래 단계를 따르십시오.

1. OpenSSL 툴킷(http://www.openssl.org)을 다운로드하여 설치합니다.

1. CSR(인증서 서명 요청) 생성:

   * 키 쌍을 생성합니다. 명령 터미널에서 다음을 실행합니다.

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * CSR을 생성합니다. 명령 터미널에서 다음을 실행합니다.

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     개인 키의 암호를 입력하라는 메시지가 표시됩니다.

   * 개인 키 및 암호의 백업 사본을 만듭니다. 샘플 CSR:

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. CSR을 인증 기관(CA)에 보냅니다(예: Verisign).

1. CA는 .p7b 형식(PKCS#7, Cryptographic Message Syntax Standard)으로 인증서를 전송합니다.

1. .p7b 인증서 배포 개인 키를 사용하여 PKCS#7(.p7b) 파일을 PKCS#12(PFX 파일, Personal Information Exchange Syntax Standard)로 변환하고 PEM 파일(연결된 인증서 컨테이너 파일)을 생성합니다.

   * PKCS#7 파일을 임시 PEM 파일로 변환합니다. 명령줄에서 다음을 실행합니다.

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * 임시 PEM 파일을 PFX 파일로 변환합니다. 명령줄에서 다음을 실행합니다.

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * 임시 PEM 파일을 최종 PEM 파일로 변환합니다. 명령줄에서 다음을 실행합니다.

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. PEM 파일을 사용하여 [Adobe Pass TVE 대시보드](https://experience.adobe.com/#/pass/authentication)를 통해 인증서를 [구성](#management)하거나 Adobe Pass 인증 담당자에게 PEM 파일을 보냅니다.

   * [Adobe Pass TVE 대시보드](https://experience.adobe.com/#/pass/authentication)를 통해 인증서를 관리하는 방법에 대한 자세한 내용은 다음 섹션을 참조하십시오.

   * Adobe Pass 인증은 기본 인증서와 백업 인증서를 모두 지원합니다. 어떤 식으로든 기본 인증서가 손상되면 이를 취소하고 보조 인증서로 전환할 수 있습니다. 이를 통해 고객에게 미치는 영향을 최소화하면서 인증서 간의 원활한 전환을 보장합니다.

## 사용자 메타데이터 관리 {#management}

>[!IMPORTANT]
>
> Adobe Pass TVE 대시보드에 액세스할 수 없는 경우 [Zendesk](https://adobeprimetime.zendesk.com)를 통해 티켓을 만들고 TAM(기술 계정 관리자)에게 적절한 변경을 요청하십시오.

Adobe Pass TVE Dashboard는 Adobe Pass 인증 고객(프로그래머)이 구성 및 데이터를 관리하는 도구입니다. 이 셀프 서비스 대시보드를 사용하면 [Adobe Pass TVE 대시보드 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) 설명서에 설명된 다양한 기능을 사용할 수 있습니다.

MVPD에서 사용할 수 있는 사용자 메타데이터 특성을 검토하고 관리하려면 [통합을 위한 TVE Dashboard 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#user-metadata) 설명서의 단계를 따릅니다.

사용자 메타데이터 특성 암호화에 사용되는 인증서를 검토하고 관리하려면 [프로그래머를 위한 TVE 대시보드 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#certificates) 또는 [채널에 대한 TVE 대시보드 사용 안내서](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#certificates) 문서의 단계를 따릅니다.

## REST API V2 {#rest-api-v2}

사용자 메타데이터 속성은 다음 API를 사용하여 검색할 수 있습니다.

* [프로필 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [특정 mvpd에 대한 프로필 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [특정 코드에 대한 프로필 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

사용자 메타데이터 특성의 구조를 이해하려면 위의 API의 **응답** 및 **샘플** 섹션을 참조하십시오.

위의 API를 통합하는 방법 및 시기에 대한 자세한 내용은 다음 문서를 참조하십시오.

* [기본 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [보조 애플리케이션 내에서 수행되는 기본 프로필 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
