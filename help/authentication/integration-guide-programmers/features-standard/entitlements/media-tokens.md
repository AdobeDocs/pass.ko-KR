---
title: 미디어 토큰
description: 미디어 토큰
exl-id: 7e486d2c-e078-464d-90b1-14e2cfb4d20a
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---

# 미디어 토큰 {#media-tokens}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

미디어 토큰은 보호된 콘텐츠(리소스)에 대한 보기 액세스를 제공하기 위한 권한 부여 결정의 결과로 Adobe Pass 인증 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)에서 생성된 토큰입니다. 미디어 토큰은 문제가 발생한 시점에 지정된 제한되고 짧은 기간(몇 분) 동안 유효하며, 이는 클라이언트 애플리케이션에서 확인하고 사용해야 하는 시간을 나타냅니다.

미디어 토큰은 일반 텍스트로 전송된 PKI(공개 키 인프라)를 기반으로 하는 서명된 문자열로 구성됩니다. PKI 기반 보호를 사용하면 CA(인증 기관)에서 Adobe에 발급한 비대칭 키를 사용하여 토큰에 서명합니다.

미디어 토큰이 프로그래머에게 전달되면 해당 리소스에 대한 액세스 보안을 보장하기 위해 비디오 스트림을 시작하기 전에 미디어 토큰 검증기를 사용하여 확인할 수 있습니다.

미디어 토큰 검증기는 미디어 토큰의 신뢰성을 확인하는 역할을 하는 Adobe Pass 인증에서 배포한 라이브러리입니다.

## 미디어 토큰 검증기 {#media-token-verifier}

Adobe Pass 인증은 프로그래머가 비디오 스트림을 시작하기 전에 보안 액세스를 보장하기 위해 미디어 토큰 검증기 라이브러리를 통합하는 백엔드 서비스에 미디어 토큰을 보낼 것을 권장합니다. 미디어 토큰의 TTL(Time-to-Live)은 토큰 생성 서버와 유효성 검사 서버 간의 잠재적인 클럭 동기화 문제를 고려하도록 설계되었습니다.

토큰 형식이 보장되지 않으며 향후 변경될 수 있으므로 Adobe Pass 인증은 미디어 토큰을 구문 분석하고 데이터를 직접 추출하지 않도록 강력하게 조언합니다. 미디어 토큰 검증기 라이브러리는 토큰의 콘텐츠를 분석하는 데 사용되는 유일한 도구여야 합니다.

미디어 토큰 검증기 라이브러리는 다음 링크에서 다운로드할 수 있습니다.

* https://tve.zendesk.com/hc/en-us/articles/204963159-Media-Token-Verifier-library

미디어 토큰 검증기 라이브러리에는 JDK 버전 1.5 이상이 필요하며 서명 알고리즘(`SHA256WithRSA`)에 기본 JCE(Java Cryptography Extension) 공급자 사용을 지원합니다.

`mediatoken-verifier-VERSION.jar` Java 아카이브로 표시되는 미디어 토큰 검증자 라이브러리에는 다음이 포함됩니다.

* Adobe 공개 키입니다.
* 토큰 확인 API(`ITokenVerifier.java`).
* 참조 구현(`com.adobe.entitlement.test.EntitlementVerifierTest.java`)입니다.
* 종속성 및 인증서 키 저장소.

>[!IMPORTANT]
> 
> 포함된 인증서 키 저장소의 기본 암호는 `123456`입니다.

### 메서드 {#methods}

`ITokenVerifier` 클래스는 다음 메서드를 정의합니다.

* 미디어 토큰의 유효성을 검사하는 데 사용되는 `isValid()` 메서드입니다. 단일 인수 [리소스 식별자](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)을(를) 허용합니다. 입력한 리소스 식별자가 `null`인 경우 메서드는 미디어 토큰의 정품 여부 및 유효 기간만 확인합니다.

  `isValid()` 메서드가 다음 상태 값 중 하나를 반환합니다.

  | VALID_TOKEN | 토큰 유효성 검사 성공 |
  |----------------------|-------------------------------------------|
  | INVALID_TOKEN_FORMAT | 토큰 형식이 잘못되었습니다. |
  | INVALID_SIGNATURE | 토큰 신뢰성을 확인할 수 없습니다. |
  | TOKEN_EXPIRED | 토큰 TTL이 잘못되었습니다. |
  | INVALID_RESOURCE_ID | 토큰이 지정된 리소스에 유효하지 않음 |
  | 오류 알 수 없음 | 토큰이 아직 확인되지 않았습니다. |

* 미디어 토큰과 연결된 리소스 식별자를 검색하고 이를 인증 결정 응답에서 반환된 식별자와 비교하는 데 사용되는 `getResourceID()` 메서드입니다.

* 미디어 토큰이 발급된 시간을 검색하는 데 사용된 `getTimeIssued()` 메서드입니다.

* 미디어 토큰의 TTL을 검색하는 데 사용된 `getTimeToLive()` 메서드입니다.

* MVPD에서 설정한 익명화된 GUID를 검색하는 데 사용되는 `getUserSessionGUID()` 메서드입니다.

* 사용자를 인증한 MVPD의 식별자를 검색하는 데 사용되는 `getMvpdId()` 메서드입니다.

* 사용자를 인증한 프록시 MVPD의 식별자를 검색하는 데 사용되는 `getProxyMvpdId()` 메서드입니다.

### 샘플 {#sample}

미디어 토큰 검증기 보관에는 참조 구현(`com.adobe.entitlement.test.EntitlementVerifierTest.java`)과 테스트 클래스로 API를 호출하는 예가 포함되어 있습니다. 이 샘플(`com.adobe.entitlement.text.EntitlementVerifierTest.java`)은 미디어 토큰 검증기 라이브러리를 미디어 서버에 통합하는 방법을 보여 줍니다.

```JAVA
package com.adobe.entitlement.test;

import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.tve.crypto.SignatureVerificationCredential; 
import java.io.InputStream; 

public class EntitlementVerifierTest { 
    String mRequestorID = null;
    String mTokenToVerify = null;
    String mPathToCertificate = null;
    String mKeystoreType = null;
    String mKeystorePasswd = null;
    String mResourceID = null;

    public static void main(String[] args) { 
        if (args == null || args.length < 2 ) {
            System.out.println("Incorrect args: Usage: EntitlementVerifierTest requestorID tokenToVerify [resourceID]");
            return;
        } 
        String requestorID = args[0];
        String tokenToVerify = args[1];
        String pathToCertificate = "media_token_keystore.jks"; // the default keystore provided in the entitlement jar 
        String keystoreType = "jks";
        String keystorePasswd = "123456"; // password for the default keystore 
        if (requestorID == null || tokenToVerify == null) {
            System.out.println("One or more arguments is null");
            return;
        } 
        System.out.println("RequestorID: " + requestorID);
        System.out.println("token: " + tokenToVerify);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
        System.out.println("keystore passwd: " + keystorePasswd);
        String resourceID = null;
        if (args.length > 2) {
            resourceID = args[2];
        }
        System.out.println("Resource ID: " + resourceID);
        EntitlementVerifierTest verifier = new EntitlementVerifierTest(requestorID,
            tokenToVerify, pathToCertificate, keystoreType, keystorePasswd, resourceID);
        verifier.verifyToken();
    } 

    protected EntitlementVerifierTest(String inRequestorID,
                                      String inTokenToVerify,
                                      String inPathToCertificate,
                                      String inKeystoreType,
                                      String inKeystorePasswd, String inResourceID) {
        mRequestorID = inRequestorID;
        mTokenToVerify = inTokenToVerify;
        mPathToCertificate = inPathToCertificate;
        mKeystoreType = inKeystoreType;
        mKeystorePasswd = inKeystorePasswd;
        mResourceID = inResourceID;
    } 

    protected void verifyToken() {
        // It is expected that the SignatureVerificationCredential and 
        // CryptoDataHolder could be created at Init time in a web application 
        // and be reused for all token verifications. 
        CryptoDataHolder cryptoData = createCryptoDataHolder(mPathToCertificate, mKeystoreType, mKeystorePasswd);
        ITokenVerifierFactory tokenVerifierFactory = new SimpleTokenPKISignatureVerifierFactory();
        ITokenVerifier tokenVerifier = tokenVerifierFactory.getInstance(mRequestorID, mTokenToVerify, cryptoData);
        ITokenVerifier.eReturnValue status = tokenVerifier.isValid(mResourceID);
        System.out.println("Is token Valid? : " + status.toString());
        System.out.println("Token User ID: " + tokenVerifier.getUserSessionGUID());
        System.out.println("Token was generated at: " + tokenVerifier.getTimeIssued());

        System.out.println("Token Mvpd ID: " + tokenVerifier.getMvpdId());
        System.out.println("Token Proxy Mvpd ID: " + tokenVerifier.getProxyMvpdId());
    } 
    
    protected CryptoDataHolder createCryptoDataHolder(String pathToCertificate,
                                                      String keystoreType, String keystorePasswd) {
        SignatureVerificationCredential verificationCredential =
            readShortTokenVerificationCredential(pathToCertificate, keystoreType, keystorePasswd);
        CryptoDataHolder cryptoData = new CryptoDataHolder();
        cryptoData.setCertificateInfo(verificationCredential);
        return cryptoData;
    } 
    
    protected SignatureVerificationCredential readShortTokenVerificationCredential(String keystoreFile,
                                                                                   String keystoreType,
                                                                                   String keystorePasswd) {
        SignatureVerificationCredential cred = null; 
        if (keystoreFile != null){
            try {
                // load the keystore file 
                ClassLoader loader = EntitlementVerifierTest.class.getClassLoader();
                InputStream certInputStream =  loader.getResourceAsStream(keystoreFile);
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null) {
            System.out.println("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```

## REST API V2 {#rest-api-v2}

다음 API를 사용하여 미디어 토큰을 검색할 수 있습니다.

* [특정 mvpd를 사용하여 권한 부여 결정 검색](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

인증 결정 및 미디어 토큰의 구조를 이해하려면 위의 API의 **응답** 및 **샘플** 섹션을 참조하십시오.

위의 API를 통합하는 방법 및 시기에 대한 자세한 내용은 다음 문서를 참조하십시오.

* [기본 애플리케이션 내에서 수행되는 기본 인증 흐름](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!IMPORTANT]
>
> 클라이언트 응용 프로그램은 유효성 검사를 위해 반환된 `token`의 `serializedToken` 값을 [미디어 토큰 검증기](#media-token-verifier)에 전달해야 합니다.
