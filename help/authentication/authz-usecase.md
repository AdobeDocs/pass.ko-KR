---
title: MVPD 인증
description: MVPD 인증
exl-id: 215780e4-12b6-4ba6-8377-4d21b63b6975
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---

# MVPD 인증

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#mvpd-authz-overview}

인증(AuthZ)은 Adobe이 호스팅하는 백엔드 서버와 MVPD AuthZ 끝점 간의 백 채널(서버 간) 통신을 통해 수행됩니다.

AuthZ 요청의 경우 권한 부여 끝점이 최소한 다음 매개 변수를 처리할 수 있어야 합니다.

* **Uid**. 인증 단계에서 수신한 사용자 ID입니다.

* **리소스 ID**. 주어진 콘텐츠 리소스를 식별하는 문자열입니다. 이 리소스 ID는 프로그래머가 지정하며 MVPD는 이러한 리소스에 대한 비즈니스 규칙을 보강해야 합니다(예: 사용자가 특정 채널을 구독하고 있는지 확인).

사용자에게 권한이 부여되었는지 여부를 확인하는 것 외에도 응답에는 이 권한의 TTL(Time-to-Live), 즉 권한이 만료되는 시점이 포함되어야 합니다. TTL이 설정되지 않으면 AuthZ 요청이 실패합니다.  이러한 이유로, **ttl은 Adobe Pass 인증 측의 필수 구성 설정입니다**&#x200B;를 사용하면 MVPD가 요청에 TTL을 포함하지 않을 때 사례를 다룰 수 있습니다.

## 인증 요청 {#authz-req}

AuthZ 요청에는 요청이 수행되는 주체, 주체가 액세스하려는 리소스, 주체가 리소스에서 수행하려는 작업 및 작업이 수행되려고 하는 환경이 포함되어야 합니다. Adobe Pass 인증의 특별한 경우 이러한 요소는 다음과 같습니다.

| XXML 요소 | 다음에 해당 |
|---------------|--------------------------------------------------------------------------------------------------------------------------------|
| 제목 | 인증된 세션에서 식별되고 SAML 어설션의 &#39;주체-토큰&#39; AttributeValue에 의해 참조되는 주도자입니다. |
| 리소스 | 보호된 리소스의 URI입니다. |
| 작업 | 보기. |
| 환경 | SP에 표시되는 것과 같이 요청 클라이언트의 IP 주소를 포함합니다. |



이 시점에서 SP는 XACML Authorization DecisionQuery를 준비하고 HTTP POST을 통해 IDP에 대한 (이전에 합의된) PDP(정책 결정 지점)로 보내야 합니다. 다음은 간단한 XACML 요청의 예입니다(XACML 코어 사양 참조).

```XML
POST https://authz.site.com/XACML_endpoint
<Request  xmlns="urn:oasis:names:tc:xacm:2.0:context:schema:os"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="urn:oasis:names:tc:xacml:2.0:context:schema:os
http://docs.oasis-open.org/xacml/access_control-xacml-2.0-context-schema-os.xsd">
<Subject>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-token"
        DataType="http://www.w3.org/2001/XMLSchema#base64Binary">
      <AttributeValue>{Base64 Data}</AttributeValue>
   </Attribute>
</Subject>
<Resource>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"
        DataType="http://www.w3.org/2001/XMLSchema#anyURI">
<AttributeValue>urn:tve:tms:1234</AttributeValue>
   </Attribute>
</Resource>
<Action>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id"
        DataType="http://www.w3.org/2001/XMLSchema#string">
       <AttributeValue>VIEW</AttributeValue>
   </Attribute>
</Action>
<Environment>
   <Attribute
       AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address"
       DataType="http://www.w3.org/2001/XMLSchema#string">
      <AttributeValue>1.2.3.4</AttributeValue>
   </Attribute>
</Environment>
</Request>
```


AuthZ 요청을 수신한 후, MVPD의 PDP는 요청을 평가하고 주체가 리소스에 대해 요청된 작업을 수행하도록 허용되어야 하는지 여부를 결정합니다. 그런 다음 MVPD는 아래 인증 응답에 설명된 대로 결정, 상태 코드 및 메시지가 포함된 응답을 반환합니다.

## 인증 응답 {#authz-response}

AuthZ 요청에 대한 응답은 MVPD가 요청을 평가하고 요청된 비즈니스 규칙을 적용하여 주체가 리소스에 대해 요청된 작업을 수행할 수 있는지 여부를 확인한 후 제공됩니다. Adobe Pass 인증에 대해 반환된 응답은 SP가 PEP(정책 시행 지점)로 갖는 결정, 상태 코드, 메시지 및 의무와 함께 XACML 코어 사양 다음에 다시 표시됩니다. 다음은 샘플 응답입니다.

```XML
<Response xmlns="urn:oasis:names:tc:xacml:2.0:context:schema:os">
  <Result>
  <Decision>Permit</Decision>
  <Status>
     <StatusCode Value="urn:oasis:names:tc:xacml:1.0:status:ok"/>
     <StatusMessage>ok</StatusMessage>
  </Status>
  <xacml:Obligations     
          xmlns:xacml="urn:oasis:names:tc:xacml:2.0:policy:schema:os">
     <xacml:Obligation    
              ObligationId="urn:cablelabs:olca:1.0:obligations:log"
              FulfillOn="Permit" />
  </xacml:Obligations>
 </Result>
</Response>
```

다음은 Adobe Pass 인증이 지원하고 프로그래머가 이행할 수 있도록 하는 DENY 의무 목록입니다.

* **항아리:tve:xacml:2.0:obligations:restrict-pc** - 구독자가 자녀 보호 확인에 실패했으며 SP가 이 콘텐츠에 대한 액세스를 제한하기 위해 적절한 조치를 취해야 합니다.

* **항아리:tve:xacml:2.0:obligations:업그레이드** - 구독자에게 적절한 구독 수준이 없습니다.  콘텐츠에 액세스하려면 구독을 업그레이드해야 합니다.

Adobe Pass 인증은 다음을 지원합니다 **허용** 의무를 준수하고 프로그래머가 이를 이행할 수 있도록 합니다.

* **항아리:cablelabs:olca:1.0:obligations:log** - Adobe Pass은 트랜잭션을 기록하며, 합의된 보고 메커니즘을 통해 사용할 수 있습니다.

* **항아리:cablelabs:olca:1.0:obligations:재작성** - Adobe Pass 인증은 인증을 n초 만에 다시 새로 고칩니다(XACML AttributeAssignment를 통해 Duty에 대한 인수로 지정됨 - XACML 코어 사양 , 섹션 5.46 참조).

<!--
>![RelatedInformation]
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
>* [Authentication](/help/authentication/authn-usecase.md)
-->
