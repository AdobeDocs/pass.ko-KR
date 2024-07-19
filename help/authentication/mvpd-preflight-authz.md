---
title: MVPD Preflight 인증
description: MVPD Preflight 인증
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 0%

---

# MVPD Preflight 인증

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 소개 {#mvpd-preflight-authz-intro}

&quot;Preflight 인증&quot;은 여러 리소스에 대한 간단한 인증 검사입니다. 프로그래머는 주로 UI를 장식하는 데 사용합니다(예: 잠금 및 잠금 해제 아이콘으로 액세스 상태 표시).

Adobe Pass 인증은 현재 AuthN 응답 특성 또는 다중 채널 AuthZ 요청을 통해 MVPD에 대해 두 가지 방법으로 Preflight 인증을 지원할 수 있습니다.  다음 시나리오에서는 프리플라이트 인증을 구현할 수 있는 다양한 방법의 비용/이점에 대해 설명합니다.

* **최상의 사례 시나리오** - MVPD가 권한 부여 단계(다중 채널 AuthZ) 동안 사전 승인된 리소스 목록을 제공합니다.
* **최악의 경우 시나리오** - MVPD가 여러 리소스 권한 부여를 지원하지 않는 경우 Adobe Pass 인증 서버는 리소스 목록의 각 리소스에 대해 MVPD에 대한 권한 부여 호출을 수행합니다. 이 시나리오는 프리플라이트 인증 요청에 대한 응답 시간에 영향을 미칩니다(리소스 수에 비례). Adobe 및 MVPD 서버 모두에서 로드를 늘려 성능 문제를 일으킬 수 있습니다. 또한 실제 플레이가 필요 없이 인증 요청/응답 이벤트를 생성합니다.
* **사용하지 않음** - MVPD는 인증 단계 중에 사전 승인된 리소스 목록을 제공하므로, 목록이 클라이언트에 캐시되므로 사전 전송 요청도 필요 없이 네트워크 호출이 필요하지 않습니다.

MVPD는 Preflight 인증을 지원하지 않아도 되지만, 다음 섹션에서는 위의 최악의 시나리오로 되돌아가기 전에 Adobe Pass 인증이 지원할 수 있는 일부 Preflight 인증 방법에 대해 설명합니다.

## AuthN에서 Preflight {#preflight-authn}

이 프리플라이트 시나리오는 OLCA 호환 시나리오(케이블)입니다. &quot;인증 어설션 내의 속성 문&quot;이라는 인증 및 권한 부여 인터페이스 1.0 사양 섹션 7.5.2에서는 SAML 인증 응답이 사전 승인된 리소스 목록을 포함하는 방법에 대해 설명합니다. IdP가 이를 지원하는 경우 Adobe Pass 인증 서버는 인증 시 사전 설정된 리소스 목록을 생성하고 인증 토큰과 함께 클라이언트에 캐시할 수 있습니다. 이 메서드는 또한 최상의 사례 시나리오를 달성하며, 모든 것이 이미 클라이언트에 있으므로 프로그래머가 checkPreauthorizedResources()를 호출할 때 네트워크 호출이 수행되지 않습니다.

### SAML 속성 문의 사용자 지정 리소스 목록 {#custom-res-saml-attr}

IdP의 SAML 인증 응답에는 AdobePass에서 인증해야 하는 리소스 이름이 포함된 AttributeStatement가 포함되어야 합니다.  일부 MVPD는 다음 형식으로 이를 제공합니다.

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

위의 샘플은 사전 승인된 두 리소스 &quot;MMOD&quot;와 &quot;Olympics2012&quot;가 포함된 목록을 제공합니다.

이렇게 하면 최상의 사례 시나리오가 효과적으로 달성되며, 모든 항목이 이미 클라이언트에 있으므로 프로그래머가 checkPreauthorizedResources()를 호출할 때 네트워크 호출이 수행되지 않습니다.

## AuthZ의 다중 채널 Preflight {#preflight-multich-authz}

이 프리플라이트 구현은 OLCA 호환 가능 (케이블 맵)입니다.  인증 및 권한 부여 인터페이스 1.0 사양(섹션 7.5.3 및 7.5.4)은 SAML 어설션 또는 XACML을 사용하여 MVPD로부터 권한 부여 정보를 요청하는 방법에 대해 설명합니다. 이는 인증 흐름의 일부로 승인 상태를 지원하지 않는 MVPD에 대해 쿼리하는 권장 방법입니다. Adobe Pass 인증은 승인된 리소스 목록을 검색하기 위해 MVPD에 대해 단일 네트워크 호출을 실행합니다.


Adobe Pass 인증은 프로그래머 애플리케이션에서 리소스 목록을 수신합니다. 그런 다음 Adobe Pass 인증의 MVPD 통합은 이러한 모든 리소스를 포함하는 하나의 AuthZ 호출을 만든 다음 응답을 구문 분석하고 여러 허용/거부 결정을 추출할 수 있습니다.  다중 채널 AuthZ 시나리오가 있는 프리플라이트에 대한 플로우는 다음과 같이 작동합니다.

1. 프로그래머 앱은 preflight 클라이언트 API를 통해 쉼표로 구분된 리소스 목록을 보냅니다(예: &quot;TestChannel1,TestChannel2,TestChannel3&quot;).
1. MVPD Preflight AuthZ 요청 호출은 여러 리소스를 포함하며 다음과 같은 구조를 가지고 있습니다.

```XML
<?xml version="1.0" encoding="UTF-8"?><soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/"> 
<soap11:Header/> 
<soap11:Body> 
  <xacml-samlp:XACMLAuthzDecisionQuery xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol" 
                                       CombinePolicies="false" Destination="https://login.idpexmaple.net/" ID="_3576604f382455d6495f342d9e07b69c" 
                                       IssueInstant="2013-02-07T10:31:40.333Z" Version="2.0"> 
  <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com/on-behalf-of/TestDistributors</saml2:Issuer> 
  <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os"> 
  <xacml-context:Subject SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject"> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VFZTAQEAABQCe[...]</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Subject> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel2</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                xsi:type="xacml-context:AttributeValueType">TestChannel3</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Action> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VIEW</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Action> 
  <xacml-context:Environment> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address" 
                           DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">127.0.0.1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Environment> 
  </xacml-context:Request> 
  </xacml-samlp:XACMLAuthzDecisionQuery> 
</soap11:Body> 
</soap11:Envelope>
```

## 여러 리소스에 대한 사용자 지정 인증 {#custom-authz}

일부 MVPD에는 한 요청에서 여러 리소스에 대한 인증을 지원하는 인증 끝점이 있지만, 다중 채널 AuthZ에 설명된 시나리오에 해당되지 않습니다. 이러한 특정 MVPD에는 사용자 지정 작업이 필요합니다.

Adobe은 기존 구현을 변경하지 않고 여러 채널 인증을 지원할 수도 있습니다.  이 접근 방식은 Adobe과 MVPD 기술 팀 간에 검토되어야 예상대로 작동하는지 확인할 수 있습니다.

## Preflight 인증을 지원하는 MVPD {#mvpds-supp-preflight-authz}

다음 표에는 Preflight Authorization을 지원하는 MVPD와 함께 Preflight가 지원하는 Preflight 유형 및 알려진 제한 사항이 나와 있습니다.

| Preflight 접근 방식 | MVPD | 메모 |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| 다중 채널 AuthZ | Comcast AT&amp;T 프록시 Clearleap Charter_Direct 프록시 GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne |                                                                    |
| 사용자 메타데이터의 채널 라인업 | 서든링크 | 모든 Synacor 직접 통합은 이 접근 방식도 지원할 수 있습니다. |
| 포크 및 조인 | 위에 나열되지 않은 다른 모든 항목 | 선택된 기본 최대 리소스 수는 5입니다. |

<!--
![RelatedInformation]
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Authorization](/help/authentication/authz-usecase.md)
>* [MVPD Integration Features](/help/authentication/mvpd-integr-features.md)
>* [MVPD User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Preflight Authorization - Programmer Integration Guide](/help/authentication/preflight-authz.md)
>* [AuthN and AuthZ Interface 1.0 Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank} 
-->
