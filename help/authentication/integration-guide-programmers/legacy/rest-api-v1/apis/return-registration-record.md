---
title: 등록 레코드 반환
description: 등록 레코드 반환
exl-id: 7b9e63a2-59b6-4123-a19b-ee1f021219ea
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 2%

---

# (기존) 등록 레코드 반환 {#return-registration-record}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!NOTE]
>
> REST API 구현은 [조절 메커니즘](/help/authentication/integration-guide-programmers/throttling-mechanism.md)에 의해 제한됩니다.

## REST API 끝점 {#clientless-endpoints}

`<REGGIE_FQDN>`:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)




## 설명 {#description}

등록 코드 UUID, 등록 코드 및 해시된 장치 ID가 포함된 등록 코드 레코드를 반환합니다.






| 엔드포인트 | 호출자: </br>명 | 입력   </br>매개 변수 | HTTP </br>메서드 | 응답 | HTTP </br>응답 |
| --- | --- | --- | --- | --- | --- |
| `<REGGIE_FQDN>`;/reggie/v1/`{requestorId}`/regcode/`{registrationCode}`<p>For example:<p>`<REGGIE_FQDN>`/reggie/v1/sampleRequestorId/regcode/TJJCFK?format=xml | 스트리밍 앱</br></br>또는</br></br>프로그래머 서비스 | 1. 요청자 </br>    (경로 구성 요소)</br>2.  등록 코드 </br>    (경로 구성 요소) | GET | 등록 코드 및 정보가 포함된 XML 또는 JSON입니다. 아래 스키마 및 샘플을 참조하십시오. | 200 |

{style="table-layout:auto"}




| 입력 매개 변수 | 설명 |
| --- | --- |
| 요청자 | 이 작업이 유효한 Programmer requestorId입니다. |
| 등록 코드 | 스트리밍 장치에 표시될(인증 흐름에 입력될) 등록 코드 값입니다. |




## 응답 XML 스키마 {#response-xml-schema}

### 등록 코드 XSD

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="model.mvc.reggie.pass.adobe.com"
            targetNamespace="model.mvc.reggie.pass.adobe.com"
            attributeFormDefault="unqualified"
            elementFormDefault="unqualified">
        <xs:element name="regcode">
            <xs:complexType>
                <xs:all>
                    <xs:element name="id" type="xs:string" />
                    <xs:element name="code" type="xs:string" />
                    <xs:element name="requestor" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="mvpd" type="xs:string" minOccurs="1" maxOccurs="1"/
                    <xs:element name="generated" type="xs:long" />
                    <xs:element name="expires" type="xs:long" />
                    <xs:element name="info" type="infoType" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
        <xs:complexType name="infoType">
            <xs:all>
                <xs:element name="deviceId" type="xs:base64Binary" minOccurs="1" maxOccurs="1"/>
                <xs:element name="deviceType" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="deviceUser" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appId" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appVersion" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="registrationURL" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
            </xs:all>
        </xs:complexType>
    </xs:schema>
```

| 요소 이름 | 설명 |
| --- | --- |
| id | 등록 코드 서비스에서 생성된 UUID |
| 코드 | 등록 코드 서비스에서 생성한 등록 코드 |
| 요청자 | 요청자 ID |
| mvpd | MVPD ID |
| 생성됨 | 등록 코드 생성 타임스탬프(1970년 1월 1일 GMT 이후 밀리초) |
| 만료 | 등록 코드가 만료되는 타임스탬프(1970년 1월 1일 GMT 이후 밀리초) |
| deviceId | 고유 장치 ID(또는 XSTS 토큰) |
| deviceType | 장치 유형 |
| deviceUser | 사용자가 장치에 로그인함 |
| appId | 애플리케이션 ID |
| appVersion | 응용 프로그램 버전 |
| registrationURL | 최종 사용자에게 표시될 로그인 웹 앱의 URL |

{style="table-layout:auto"}

### 샘플 응답 {#sample-response}

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <ns2:regcode xmlns:ns2="model.mvc.reggie.pass.adobe.com">
        <id>678f9fea-a1cafec8-1ff0-4a26-8564-f6cd020acf13</id>
        <code>TJJCFK</code>
        <requestor>sampleRequestorId</requestor>
        <mvpd>sampleMvpdId</mvpd>
        <generated>1348039846647</generated>
        <expires>1348043446647</expires>
        <info>
            <deviceId>dGhpc0lkQUR1bW15RGV2aWNlSWQ=</deviceId>
            <deviceType>xbox</deviceType>
            <deviceUser>JD</deviceUser>
            <appId>2345</appId>
            <appVersion>2.0</appVersion>
            <registrationURL>http://loginwebapp.com</registrationURL>
        </info>
    </ns2:regcode>
```
