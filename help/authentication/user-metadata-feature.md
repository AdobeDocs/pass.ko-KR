---
title: 사용자 메타데이터 기능
description: 사용자 메타데이터 기능
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1649'
ht-degree: 0%

---

# 사용자 메타데이터 {#user-metadata}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>
</br>

## 소개 {#intro}

사용자 메타데이터 기능을 사용하면 프로그래머가 MVPD에서 유지 관리되는 다양한 유형의 사용자별 데이터에 액세스할 수 있습니다.  사용자 메타데이터 유형에는 우편 번호, 보호자 등급, 사용자 ID 등이 포함됩니다.  *사용자* 메타데이터는 이전에 사용할 수 있었던 확장입니다. *정적* 메타데이터(인증 토큰 TTL, 인증 토큰 TTL 및 장치 ID).


사용자 메타데이터 핵심 사항:

- 인증 및 권한 부여 흐름 중에 프로그래머의 애플리케이션으로 전달됨
- 값이 토큰에 저장됩니다.
- 상이한 MVPD들이 상이한 포맷들로 데이터를 제공하는 경우 값들은 정규화될 수 있다
- 일부 매개 변수는 프로그래머 키(예: zip 코드)를 사용하여 암호화할 수 있습니다
- 특정 값은 구성 변경을 통해 Adobe에서 사용할 수 있습니다

## 사용자 메타데이터 가져오기 {#obtaining}

프로그래머는 AccessEnabler를 통해 사용자 메타데이터를 사용할 수 있습니다. `getMetadata()` 함수 및 를 통해 `/usermetadata` clientless API의 종단점입니다.  사용에 대한 자세한 내용은 플랫폼 API 설명서 를 참조하십시오 `getMetadata()` 및 해당 콜백 `setMetadataStatus()` (또는 클라이언트 없는 API에 사용되는 끝점 및 매개 변수의 경우).

프로그래머는 가져오려는 메타데이터 유형에 대한 키를 제공하여 메타데이터를 가져옵니다. `getMetadata(key)`.

메타데이터는 다음과 같이 반환됩니다. `setMetadataStatus(key, encrypted, data)`:

| 매개 변수 | 유형 | 설명 |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | 문자열 | 요청한 메타데이터 유형을 지정합니다. |
| `encrypted` | 부울 | &quot;value&quot;의 암호화 여부를 나타내는 부울 플래그입니다. &quot;true&quot;이면 &quot;value&quot;는 실제로 실제 값의 JSON 웹 암호화 표현입니다. |
| `data` | 오브젝트 | 메타데이터의 표현을 포함하는 JSON 개체 |



데이터 매개 변수의 구조입니다. 값은 유형에 따라 다릅니다.

| 키 | 값 유형 | 샘플 | 설명 |
| --- | --- | --- | --- |
| `zip` | JSON 배열 | \[&quot;77754&quot;, &quot;12345&quot;\] | 우편번호 |
| `householdID` | JSON 문자열 | &quot;1o7241p&quot; | 세대 식별자. MVPD가 하위 계정을 지원하지 않는 경우에는 다음과 같습니다. `userID` |
| `maxRating` | JSON 개체 | { MPAA: &quot;NR&quot;, <br>  VCHIP: &quot;X&quot;,  <br>  URL: &quot;http://manage.my/parental&quot; } | 사용자의 최대 자녀 보호 등급 |
| `userID` | JSON 문자열 | &quot;1o7241p&quot; | 사용자 식별자. MVPD가 하위 계정을 지원하고 사용자가 주 계정이 아닌 경우 `userID` 이(가) 다음과 다름 `householdID`. |
| `channelID` | JSON 배열 | \[&quot;channel-1&quot;, &quot;channel-2&quot; \] | 사용자가 볼 수 있는 채널 목록 |
| `is_hoh` | JSON 문자열 | &quot;1&quot; | 사용자가 세대주인지 여부를 식별하는 플래그 |
| `encryptedZip` | JSON 문자열 | &quot;&quot; | Comcast는 암호화된 우편 번호를 노출합니다. |
| `typeID` | JSON 문자열 | 기본 | 사용자 계정이 기본/보조 계정인지 식별하는 플래그입니다. |
| `primaryOID` | JSON 문자열 | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | 세대 식별자. If `typeID` 은(는) 기본이며 다음 값을 포함합니다. `userID` |
| hba_status | 부울 | &quot;true&quot; &quot;false&quot; | 특정 통합에 대해 홈 기반 인증이 활성화되었는지 여부를 나타내는 부울 플래그 |
| allowMirror | 부울 | &quot;true&quot; &quot;false&quot; | 이 디바이스에 화면 미러링이 허용되는지 여부를 나타냅니다. |

>[!NOTE]
>
> **참고:** data 매개 변수가 암호화되어 있는 경우, 일반적으로 **zip 키**&#x200B;를 입력하면 메타데이터 키의 표현은 배열 또는 개체 대신 JSON 문자열이 됩니다.


**중요 사항:** 프로그래머가 사용할 수 있는 실제 사용자 메타데이터는 MVPD가 사용할 수 있도록 하는 내용에 따라 다릅니다.  프로덕션 환경에서 중요한 사용자 메타데이터(예: zip 코드)를 사용하려면 먼저 MVPD와 법적 계약을 체결해야 합니다.

</br>


| 이름 | 세부 사항 | 암호화 필요 | 댓글 |
| --- | --- | --- | --- |
| 사용자 ID | MVPD에서 제공한 대로 | 아니요 | 그런 다음 Adobe에 의해 해시되고 미디어 토큰과 sendTrackingData() 콜백에 노출되는 값입니다.<br><br>이 사건의 해싱은 역사적인 이유로 이루어졌다<br><br>이 ID는 세대 ID 또는 하위 계정 ID일 수 있습니다. 이는 일반적으로 지정되지 않으며, 당시 사용된 로그인에 연결된 ID일 뿐입니다(기본 계정 또는 하위 계정일 수 있음) |
| 업스트림 사용자 ID | 동시성 모니터링 흐름에만 사용할 수 있도록 MVPD에서 제공 | 아니요 | 이 값은 MVPD 및 프로그래머 사이트 및 앱에서 동시 실행 제한을 적용할 때 사용됩니다. <br><br>ID에는 동시성 모니터링 정책도 포함될 수 있습니다<br><br>대부분의 MVPD에서 이 값은 사용자 ID와 같습니다 |
| 세대 사용자 ID | MVPD가 제공하므로 대부분 자녀 보호 흐름에 사용됩니다. | 아니요 | 프로그래머가 가구와 하위 계정 사용을 이해할 수 있는 ID입니다.<br><br>실제 등급을 사용할 수 없는 경우 자녀 보호 대용품으로 사용되는 경우가 있습니다(사용자가 시청할 수 있는 가족 계정으로 로그인한 경우). 그렇지 않으면 등급이 매겨진 컨텐츠가 표시되지 않음)<br><br>MVPD에는 이를 나타내는 방법에 대한 많은 변형이 있습니다. - 가구 사용자 ID, 가구주 ID, 가구주 플래그 등. |
| 세대주 | 현재 계정이 세대주인지 여부를 보여 주는 플래그 | 아니요 | 위 참조 |
| 유형 ID / 기본 ID | 가구 계정 식별자 | 아니요 | AT&amp;T에서 가구주를 나타내는 특정 지표.<br><br>유형 ID = 사용자 계정이 기본/보조 계정인지 식별하는 플래그입니다.<br><br>기본 OID = 가정용 식별자. TypeID가 기본이면 에는 userID 값이 포함됩니다. |
| 최대 등급 | 현재 계정에 대해 허용되는 최대 등급 | 아니요 | 프로그래머가 계정에 적합하지 않은 콘텐츠를 필터링할 수 있습니다.<br><br>MPAA 또는 VCHIP 등급 있음 |
| 채널 라인 업 | 사용자 패키지에서 사용할 수 있는 채널 목록 | 아니요 | 여러 네트워크를 통합하는 포털에서 다양한 채널을 신속하게 허용/제거하는 데 사용됩니다.</br></br> *Preflight 권한 부여는 일반적으로 이 사용 사례에 더 많은 유연성을 허용하므로 대신 사용해야 합니다* <br><br>OLCA 사양은 AuthN 응답에서 이것을 속성 문으로 허용합니다 |
| HBA 상태 | HBA를 통한 인증 여부를 나타냅니다. | 아니요 |     |
| 우편번호 | 사용자의 청구 우편 번호 | 예 | 방송 또는 스포츠행사에 사용된다<br><br>빠른 업데이트를 위한 AuthZ 응답도 제공 가능<br><br>중요한 데이터, MVPD 법적 동의 필요 |
| 암호화된 우편 번호 | 사용자의 청구 우편 번호(Comcast) | 예 | 위와 같으나 Comcast에 의해 암호화됨 |
| 언어 | 사용자 언어 설정 | 아니요 | 사용자의 환경 설정에 따라 메시지를 표시하는 데 사용됨 |
| 미러링 허용 | 이 디바이스에 화면 미러링이 허용되는지 여부를 나타냅니다. | 아니요 |     |



## 사용 가능한 메타데이터 {#available_metadata}

다음 표는 Adobe Pass 인증 에코 시스템에서 사용자 메타데이터의 현재 상태를 보여 줍니다.


|     | **리걸&#x200B;**<br><br>**계약&#x200B;**<br><br>**서명됨(zip만 해당)** | **사용자 ID **<br><br>**인증 시** | **우편번호&#x200B;**<br><br>**인증/Z에서** | **등급&#x200B;**<br><br>**인증/Z에서** | **세대&#x200B;**<br><br>**AuthN/Z의 ID** | **AuthN의 채널 ID** | **AuthN의 세대주** | **AuthN에 ID 입력** | **AuthN의 기본 OID** | 언어 | 업스트림 사용자 ID **인증 시** | HBA 상태 | OnNet | inHome | AuthZ에서 미러링 허용 | **메모** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **정식 이름** | 해당 사항 없음 | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | 언어 | 업스트림 사용자 ID | hba_status | 온넷 | inHome | allowMirror | 1. AuthN의 경우 - 모든 구문 분석기가 이 새 속성을 사용하도록 OiosamlMetadataParser를 변경해야 합니다. <br>2.  AuthZ의 경우 권한 부여 구현이 MVPD와 관련되므로 일반적인 방법이 없습니다. |
| **암호화 필요** | 해당 사항 없음 | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** |     |
| **중요** | 해당 사항 없음 | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** |     |     |
| **Adobe IdP** | **예** | **예** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **예** | **예** | **예** | **예** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | 법적 계약이 필요하지 않으며 활성화할 수 있습니다. |
| **시나코** | **예** | **예** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **예** | **예** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **모든 프록시화된 MVPD를 다루지 않는 법적 합의.**   <br>이는 Synacor에 대한 일반 지원이며 일부 MVPD로 롤업되지는 않을 수 있습니다. |
| 접시 | **아니요** | **예** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | 모든 Synacor MVPD 및 upstreamUserID와 동일한 목록을 공유합니다. |
| 컴캐스트 | **아니요** | **예** | **아니요** | **예(AuthZ만 해당)** | **예(AuthZ만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **예** | **아니요** | **아니요** | **아니요** |     |
| **AT&amp;T** | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **예** | **예** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | 법률 협정에 서명했습니다. |
| **케이블 비전** | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | 법률 협정에 서명했습니다. |
| **HTC** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** |     |
| **프록시 Massilon** | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | 법률 협정에 서명했습니다. |
| **프록시 Clearleap** | **예** | **예** | **예(AuthN만 해당)** | **예(AuthZ만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | 법률 협정에 서명했습니다. |
| 로저스 | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** |     |
| RCN | **예** | **예** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** |     |
| 헌장 | **예** | **예** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** |     |
| 버라이즌 | **아니요** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **예** | **아니요** | **아니요** | **아니요** |     |
| 이스트링크 | **아니요** | **예** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** |     |
| 프록시 GLDS | **아니요** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** |     |
| DTV | **예** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** |     |
| 콕스 | **아니요** | **예** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** |     |
| 코게코 | **아니요** | **예** | **예(AuthN만 해당)** | **아니요** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** |     |
| 비디오트론 | **아니요** | **예** | **예(AuthN만 해당)** | **아니요** | **예*** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | userID와 동일한 값으로 householdID를 노출합니다. |
| 스펙트럼 | **예** | **예** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **예(AuthN만 해당)** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **예** | **아니요** | **아니요** | **예** |     |
| **모든 기타&#x200B;**<br><br>**MVPD** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **아니요** | **예** | **아니요** | **아니요** | **아니요** | **아니요** | **아직 사용권 계약이 없습니다. 중요한 메타데이터는 프로덕션에 사용할 수 없습니다.**  <br>모든 MVPD의 경우 추가 작업 없이 사용자 ID를 사용할 수 있습니다. |


새로운 유형을 사용할 수 있고 Adobe Pass 인증 시스템에 추가되면 사용자 메타데이터 유형 목록이 확장됩니다.

## 코드 샘플 {#code_samples}

- [코드 샘플 1](#code_sample1)
- [코드 샘플 2 (Mock getMetadata)](#code_sample2)


### 코드 샘플 1 {#code_sample1}

```
    // Assuming a reference to the AccessEnabler has been previously obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if(status ==1) {
        // User is authenticated, request metadata
        ae.getMetadata("zip");
        ae.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if(encrypted) {
        // The metadata value is encrypted
        // Needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```


### 코드 샘플 2 (Mock getMetadata) {#code_sample2}

```
    // Assuming a reference to the AccessEnabler has been
    //   previously obtained and stored in the "ae" variable
     
    // Mock the getMetadata() method
    var aeMock = {
        getMetadata: function(key) {
          var data = null;
          // Set mock data based on the received key,
          // according to the format in the spec
          switch(key) {
            case 'zip': 
              data = [ "1235", "23456" ];
              break;
            case 'maxRating': 
              data = { "MPAA": "PG-13", "VCHIP": "TV-14" };
              break;
            default:
              break;
          }
          // Call the metadata status just like AccessEnabler does
          setMetadataStatus(key, false, data);
        }
     }
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if (status == 1) {
        // User is authenticated, request metadata using mock object
        aeMock.getMetadata("zip");
        aeMock.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the  setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if (encrypted) {
        // The metadata value is encrypted, so it
        //   needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```

<!---

For details on your particular platform, or to gain some insight into how User Metadata is processed on the MVPD side, see the appropriate link in Related Information below.  

## Related Information {#related}

- ActionScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- JavaScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- iOS - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaStatus)
- Android - [getMetadata()](#getMetadata), [setMetadataStatus()](#setMetadaStatus)
- Clientless - [AuthN Metadata](#authn_metadata)
- [MVPD Integration Guide: User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
-->
