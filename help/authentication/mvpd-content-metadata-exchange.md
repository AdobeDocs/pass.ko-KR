---
title: MVPD 콘텐츠 메타데이터 교환
description: MVPD 콘텐츠 메타데이터 교환
exl-id: d17e60dc-6c61-4ca2-bad8-1840c95261e0
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# MVPD 콘텐츠 메타데이터 교환

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#content-metadat-exchange-overview}

이 페이지에서는 Adobe Pass Authentication에서 인증 요청 시 구조화된 데이터를 MVPD로 전송하는 데 사용하는 두 가지 표준 구현에 대해 설명합니다.  구조화된 데이터는 요청을 하는 리소스(프로그래머)와 콘텐츠 등급과 같은 추가 데이터를 나타낼 수 있습니다.

프로그래머 측면에서 Adobe Pass 인증은 다음과 같이 구조화된 MRSS 데이터 리소스를 지원합니다.

1. 프로그래머는 리소스를 MRSS 문자열로 보냅니다. Adobe Pass 인증은 웹 장치나 기본 장치에 대해 클라이언트측에서 인코딩하지 않습니다. MRSS는 Adobe Pass 인증 서버에 일반 문자열로 전송됩니다.
1. 서버측에서 MRSS는 사전 정의된 스키마(http://search.yahoo.com/mrss/)에 대해 검증됩니다.  유효성 검사가 성공하면 Adobe Pass 인증이 MRSS 필드에서 다음 정보를 추출합니다.
   * 채널 제목
   * 항목 제목
   * 리소스 식별자
   * 등급 값 및 유형
1. MRSS에서 추출된 값은 MVPD에 전달되는 인증 요청을 작성하는 데 사용됩니다.

Adobe Pass 인증은 MRSS를 MVPD에서 지원하는 형식으로 변환하는 두 가지 방법을 지원합니다.

* **XACML**.  첫 번째 접근 방식은 OLCA 표준을 따릅니다.  MRSS 값을 추출하는 XACML을 사용하여 MRSS 요소에 매핑되는 특성이 있는 XACMLResource를 작성합니다.  그런 다음 MVPD로 전달됩니다.
* **나머지**.  두 번째 방법은 REST 기반입니다.  MRSS는 base64로 인코딩되고 REST 호출에서 URL 매개 변수로 전달됩니다.

두 접근법들에서, MVPD는 그 자신의 논리 플로우 내에 추출된 값들을 포함시키고 인가 응답을 반환함으로써 인가 요청을 처리한다.

## 통합 세부 정보 {#integration-details}

* OLCA 기반 XACML 구조화된 리소스
* REST 기반 구조 리소스

### OLCA 기반 XACML 구조화된 리소스 {#olca-based-xacml-struc-resource}

대부분의 케이블 지향 MVPD는 XACML 기반 접근 방식을 사용하지만 아직 전체 구조화된 데이터 접근 방식을 지원하지 않습니다.  XACML을 지원하는 다른 MVPD는 채널 제목을 사용하여 ResourceID 속성에 대해 해당 제목을 수락합니다. 아래 예는 전체 구조화된 XACML 기반 접근 방식을 보여줍니다. Adobe Pass 인증 팀은 XACML을 사용하지만 자녀 보호 기능과 같은 기능을 아직 지원하지 않는 MVPD의 경우 XACML 통합을 다음 예제로 조정할 것을 권장합니다.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap11:Envelope xmlns:soap11=">
    <soap11:Header/>
    <soap11:Body>
        <xacml-samlp:XACMLAuthzDecisionQuery
                xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol"
                Destination="
                ID="_f1dd34469c5aeac016760e51dbba007d" IssueInstant="2012-06-26T16:30:24.879Z" Version="2.0">
            <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
                https://saml.sp.auth.adobe.com/
            </saml:Issuer>
            <ds:Signature xmlns:ds=">.......</ds:Signature>
            <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                [....info skipped for brevity....]
                <xacml-context:Resource>
 
        // The MRSS item GUID is passed as the XACML Resource resource-id
                    <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id">
                        <xacml-context:AttributeValue>DISNEY_GUID_12345</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
        // The MRSS channel title is passed as the XACML Resource tv-network
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:tv-network">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // Adobe doesn't yet support an explicit namespace for the GUID, so we reuse the channel title as the GUID.  
        // We expect to add an explicit namespace later next year pulling it from the GUID scheme attribute.
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:id:namespace">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS item title is passed as the XACML Resource content title
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:title">
                        <xacml-context:AttributeValue>Disney Program X</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS media rating is passed as the XACML Resource content rating 
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:rating:vchip">
                        <xacml-context:AttributeValue>TV-Y</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
                </xacml-context:Resource>
 
                <xacml-context:Action>
                    <xacml-context:Attribute>
                        <xacml-context:AttributeValue>VIEW</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
                </xacml-context:Action>
 
                [.....info skipped for brevity....]
            </xacml-context:Request>
        </xacml-samlp:XACMLAuthzDecisionQuery>
    </soap11:Body>
</soap11:Envelope>
 
//formatted for readability
```

### REST 기반 구조 리소스 {#rest-based-struct-resource}

일부 MVPD는 인증을 위해 다음과 같은 REST 기반 프로토콜을 표준화했습니다. 이 접근 방식은 XACML 접근 방식과 마찬가지로 모든 기능을 제공하지만, &quot;더 가벼운 가중치&quot; 구현을 제공합니다.

`// The MRSS is base64 encoded by Adobe Pass Authentication, and passed in that format to the REST-based Authorization endpoint.`

`https://auth.somedomain.net/mediation/1/rest/client/authz?uuID=AC82CE4&mrss=base64encodedstring&IPAddress=123.456.78.901`

<!--
>[!RELATEDINFORMATION]
>* [User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Programmer Integration Guide: Identifying Protected Resources](/help/authentication/identify-protected-resources.md)
>* [Programmer Integration Guide: User Metadata Exchange](/help/authentication/user-metadata.md)
-->
