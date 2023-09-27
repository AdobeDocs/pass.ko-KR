---
title: 프록시 MVPD 웹 서비스
description: 프록시 MVPD 웹 서비스
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '955'
ht-degree: 0%

---

# 프록시 MVPD 웹 서비스 {#proxy-mvpd-wbservice}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview-proxy-mvpd-webserv}

&quot;프록시 MVPD&quot;는 Adobe Pass 인증과의 자체 통합을 관리하는 것 외에도 연결된 &quot;프록시화된 MVPD&quot; 그룹을 대신하여 권한 부여 프로세스를 관리하는 MVPD입니다. 이러한 배열은 프로그래머에게는 투명합니다.

ProxyMVPD 기능을 구현하기 위해 Adobe Pass 인증은 ProxyMVPD가 ProxyedMVPD 목록을 제출하고 검색할 수 있는 RESTful 웹 서비스를 제공합니다. 이 공개 API에 사용되는 프로토콜은 다음 가정과 함께 REST HTTP입니다.

* 프록시 MVPD는 HTTP GET 메서드를 사용하여 현재 통합된 MVPD 목록을 검색합니다.
* 프록시 MVPD는 HTTP POST 메서드를 사용하여 지원되는 MVPD 목록을 업데이트합니다.

## 프록시 MVPD 서비스 {#proxy-mvpd-services}

* [프록시화된 MVPD 검색](#retriev-proxied-mvpds)
* [프록시화된 MVPD 제출](#submit-proxied-mvpds)

### 프록시화된 MVPD 검색 {#retriev-proxied-mvpds}

apikey 매개 변수로 식별된 ProxyMVPD에 대한 프록시화된 MVPD의 현재 목록을 검색합니다.

| 엔드포인트 | 호출자 | 요청 헤더 | HTTP 메서드 | HTTP 응답 |
|---|---|---|---|---|
| &lt;fqdn>/control/v1/proxyedMvpds | ProxyMVPD | apikey(필수) | GET | <ul><li> 200(ok) - 요청이 성공적으로 처리되었으며 응답에 XML 형식의 ProxyedMVPD 목록이 포함되어 있습니다.</li><li>401(권한 없음) - 제공된 자격 증명에 대해 사용자 인증이 필요하거나 권한이 부여되지 않았습니다.  다음 중 하나를 나타냅니다.<ul><li>apikey 토큰이 요청 헤더에 없습니다.</li><li>허용 목록에 없는 IP 주소에서 요청을 가져옵니다.</li><li>토큰이 잘못되었습니다.</li></ul></li><li>403(사용할 수 없음) - 제공된 매개 변수에 대해 작업이 지원되지 않거나 프록시 MVPD가 프록시로 설정되지 않았거나 누락되었음을 나타냅니다.</li><li>405(메서드가 허용되지 않음) - GET 또는 POST 이외의 HTTP 메서드가 사용되었습니다. HTTP 메서드는 일반적으로 지원되지 않거나 이 특정 끝점에 대해 지원되지 않습니다.</li><li>500(내부 서버 오류) - 요청 프로세스 중에 서버 측에서 오류가 발생했습니다.</li></ul> |

Curl 예:

`curl -X GET -H "apikey: ???provided-by-adobe???" "https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds"`


XML 응답 예:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```

### 프록시화된 MVPD 제출 {#submit-proxied-mvpds}

apikey 매개 변수로 식별된 프록시 MVPD와 통합된 MVPD 배열을 푸시합니다.

| 엔드포인트 | 호출자 | 요청 헤더 | HTTP 메서드 | HTTP 응답 |
|:------------------------------:|:---------:|:--------------------------------------------:|:-----------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;fqdn>/control/v1/proxyedMvpds | ProxyMVPD | apikey (필수) proxied-mvpds (필수) | POST | <ul><li>201(생성됨) - 푸시가 정상적으로 처리되었습니다.</li><li>400(잘못된 요청) - 서버가 요청을 처리하는 방법을 모릅니다.<ul><li>들어오는 XML이 이 사양에 게시된 스키마를 따르지 않음</li><li>프록시화된 mvpd에 고유 ID가 없습니다.</li><li>푸시된 requestorIds가 존재하지 않음 400 응답 코드에 대한 다른 서블릿 컨테이너 이유</li></ul><li>401(권한 없음) - api 키가 잘못되었거나 호출자 IP가 허용 목록에 없습니다.</li><li>403(사용할 수 없음) - 제공된 매개 변수에 대해 작업이 지원되지 않거나 프록시 MVPD가 프록시로 설정되지 않았거나 누락되었음을 나타냅니다.</li><li>405(메서드가 허용되지 않음) - GET 또는 POST 이외의 HTTP 메서드가 사용되었습니다. HTTP 메서드는 일반적으로 지원되지 않거나 이 특정 끝점에 대해 지원되지 않습니다.</li><li>500(내부 서버 오류) - 요청 프로세스 중에 서버 측에서 오류가 발생했습니다.</li></ul> |

Curl 예:

`curl -X POST -H "apikey: <API_KEY>" "https://mgmt-prequal.auth.adobe.com/control/v1/proxiedMvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



XML 예:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```


### 게시 빈도 {#posting-frequency}

Adobe Pass 인증은 이전 푸시가 변경된 경우에만 ProxyMVPD가 ProxyedMVPD 목록을 푸시하도록 권장합니다.

### 프록시화된 MVPD 삭제 중 {#delete-proxied-freqency}

ProxyMVPD가 빈 ProxyedMVPD 목록이 있는 XML 레코드를 푸시하면 해당 빈 목록이 다른 목록과 마찬가지로 시스템에 저장되므로 이전 목록을 효과적으로 삭제합니다.



## XSD 형식 {#xsd-format}

Adobe은 당사의 공개 웹 서비스에서/로 프록시화된 MVPD를 게시/검색하기 위해 다음과 같은 허용 형식을 정의했습니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns:pxm="http://tve.adobe.com/data/proxiedmvpd"
           targetNamespace="http://tve.adobe.com/data/proxiedmvpd"
           elementFormDefault="qualified"
           version="1.0">
    <xs:complexType name="iframeSize">
        <xs:all>
            <xs:element name="iframeHeight" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeWidth" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
        </xs:all>
    </xs:complexType>
    <xs:complexType name="requestorIds">
        <xs:annotation>
            <xs:documentation>List of requestors/programmers integrated with the proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:sequence>
            <xs:element name="requestorId" type="xs:string" minOccurs="1" maxOccurs="unbounded" nillable="false">
                <xs:annotation>
                    <xs:documentation>The requestor/programmer identifier recognized by Adobe</xs:documentation>
                </xs:annotation>
            </xs:element>
        </xs:sequence>
    </xs:complexType>
    <xs:complexType name="proxiedMvpd">
        <xs:all>
            <xs:element name="id" minOccurs="1" maxOccurs="1" nillable="false">
                <xs:annotation>
                    <xs:documentation>The id must conform to the regular expression: ([a-zA-Z0-9]+((\-)|[_])*)</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:simpleContent>
                        <xs:extension base="xs:string">
                            <xs:attribute name="ProviderID">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:minLength value="1"/>
                                        <xs:maxLength value="128"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:attribute>
                        </xs:extension>
                    </xs:simpleContent>
                </xs:complexType>
            </xs:element>
            <xs:element name="displayName" type="xs:string" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="logoURL" type="xs:anyURI" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeSize" type="pxm:iframeSize" minOccurs="0" maxOccurs="1"/>
            <xs:element name="requestorIds" type="pxm:requestorIds" minOccurs="0" maxOccurs="1"/>
        </xs:all>
    </xs:complexType>
    <xs:element name="proxiedMvpds">
        <xs:annotation>
            <xs:documentation>List of Proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:complexType>
            <xs:sequence>
                <xs:element name="proxiedMvpd" type="pxm:proxiedMvpd" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

**요소에 대한 참고 사항:**

* `id` (필수) - 프록시화된 MVPD ID는 다음 문자 중 하나를 사용하여 MVPD의 이름과 관련된 문자열이어야 합니다(추적 목적으로 프로그래머에게 노출됨).
   * 모든 영숫자, 밑줄(&quot;_&quot;) 및 하이픈(&quot;-&quot;).
   * idID는 다음 정규 표현식을 준수해야 합니다.
     `(a-zA-Z0-9((-)|_)*)`

     따라서 하나 이상의 문자가 있어야 하며, 문자로 시작하고, 문자, 숫자, 대시 또는 밑줄로 계속해야 합니다.

* `iframeSize` (선택 사항) - iframeSize 요소는 선택 사항이며 MVPD 인증 페이지가 iFrame에 있어야 하는 경우 iFrame의 크기를 정의합니다. 그렇지 않으면 iframeSize 요소가 없으면 전체 브라우저 리디렉션 페이지에서 인증이 발생합니다.
* `requestorIds` (선택 사항) - requestorIds 값은 Adobe에서 제공합니다. 프록시화된 MVPD를 하나 이상의 requestorId와 통합해야 합니다. 프록시화된 MVPD 요소에 &quot;requestorIds&quot; 태그가 없으면 프록시화된 MVPD는 프록시 MVPD 아래에 통합된 사용 가능한 모든 요청자와 통합됩니다.
* `ProviderID` (선택 사항) - ProviderID 특성이 id 요소에 있으면 SAML 인증 요청 시 ProviderID 값이 프록시 MVPD에 ID 값 대신 프록시 MVPD/SubMVPD ID로 전송됩니다. 이 경우 id 값은 프로그래머 페이지에 표시된 MVPD 선택기에서만 사용되며 내부적으로 Adobe Pass 인증에 의해 사용됩니다. ProviderID 특성의 길이는 1자에서 128자 사이여야 합니다.

## 보안 {#security}

요청이 유효한 것으로 간주되려면 다음 규칙을 준수해야 합니다.

* 요청 헤더에 보안 API 매개 변수가 포함되어야 합니다. 프록시 MVPD의 호출을 고유하게 식별하는 응용 프로그램 키입니다.
* 허용된 특정 IP 주소에서 요청을 가져와야 합니다.
* SSL 프로토콜을 통해 요청을 전송해야 합니다.

Adobe은 토큰의 (정적) 값을 제공합니다. 이 값은 인증 및 권한 부여 프로세스에 사용됩니다.  위에 나열되지 않은 요청 헤더에 있는 모든 매개 변수는 무시됩니다.

Curl 예:

`curl -X GET -H "apikey: ???provided-by-adobe???" "https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds"`

## Adobe Pass 인증 환경에 대한 프록시 MVPD 웹 서비스 끝점 {#proxy-mvpd-wevserv-endpoints}

* **프로덕션 URL:** https://mgmt.auth.adobe.com/control/v1/proxiedMvpds
* **스테이징 URL:** https://mgmt.auth-staging.adobe.com/control/v1/proxiedMvpds
* **프로덕션 전 URL:** https://mgmt-prequal.auth.adobe.com/control/v1/proxiedMvpds
* **사전 준비 URL:** https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
