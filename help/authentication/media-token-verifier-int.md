---
title: 미디어 토큰 검증기 통합
description: 미디어 토큰 검증기 통합
exl-id: 1688889a-2e30-4d66-96ff-1ddf4b287f68
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '887'
ht-degree: 0%

---

# 미디어 토큰 검증기 통합

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 미디어 토큰 검증기 정보 {#about-media-token-verifier}

인증이 성공하면 Adobe Pass 인증에서 오래 지속되는 인증(AuthZ) 토큰을 만듭니다.  AuthZ 토큰은 클라이언트의 플랫폼에 따라 클라이언트측에 전달되거나 서버측에 저장됩니다.  자세한 내용은 [토큰 이해](/help/authentication/programmer-overview.md#understanding-tokens)에서 다른 클라이언트 시스템에 토큰을 저장하는 방법을 참조하십시오.


AuthZ 토큰은 사이트 사용자에게 주어진 리소스를 볼 수 있는 권한을 부여합니다.  이는 토큰이 만료된 후 6~24시간의 일반적인 TTL(Time-to-Live)을 가집니다. **실제 보기 액세스를 위해 Adobe Pass 인증은 AuthZ 토큰을 사용하여 미디어 서버로 가져와서 전달하는 단기 미디어 토큰을 생성합니다**. 이러한 단기 미디어 토큰은 매우 짧은 TTL(일반적으로 몇 분)이 있습니다.


AccessEnabler 통합에서는 `setToken()` 콜백을 통해 수명이 짧은 미디어 토큰을 얻습니다. Clientless API 통합의 경우 `<SP_FQDN>/api/v1/tokens/media` API 호출을 사용하여 수명이 짧은 미디어 토큰을 얻습니다. 토큰은 PKI(공개 키 인프라)에 기반한 토큰 보호를 사용하여 Adobe이 서명한 일반 텍스트로 전송된 문자열입니다. 이 PKI 기반 보호를 사용하면 토큰이 인증 기관에서 Adobe에 발급한 비대칭 키를 사용하여 서명됩니다.


토큰에 대한 클라이언트 측의 유효성 검사가 없으므로 악의적인 사용자는 도구를 사용하여 가짜 `setToken()` 호출을 삽입할 수 있습니다. 따라서 **은(는) 사용자에게 권한이 부여되었는지 여부를 고려할 때 `setToken()`이(가) 트리거되었다는 사실에만 의존할 수**&#x200B;없습니다. 단기 토큰 자체가 합법적인지 확인해야 합니다. 유효성 검사를 수행하는 도구는 미디어 토큰 검증기 라이브러리입니다.


>[!TIP]
>
>유효성 검사를 위해 반환된 토큰 문자열의 전체 길이를 미디어 토큰 검증기에 전달해야 합니다.

## 미디어 토큰 검증기를 사용하여 단기 토큰 확인 {#validate-short-livedttokens}

비디오 스트림을 실제로 시작하기 전에 토큰의 유효성을 검사하려면 프로그래머가 미디어 토큰 검증기 라이브러리를 사용하는 웹 서비스에 토큰을 보내는 것이 좋습니다. 단기 미디어 토큰의 매우 짧은 TTL은 토큰을 생성하는 서버와 토큰을 확인하는 서버 간의 클럭 동기화 문제를 충분히 허용하도록 정의되지만, 더 이상 그렇지 않습니다.



[미디어 토큰 검증기 라이브러리](https://adobeprimetime.zendesk.com/auth/v2/login/signin?return_to=https%3A%2F%2Ftve.zendesk.com%2Fhc%2Fen-us%2Farticles%2F204963159-Media-Token-Verifier-library&amp;theme=hc&amp;locale=en-us&amp;brand_id=343429&amp;auth_origin=343429%2Cfalse%2Ctrue){target=_blank}는 Adobe Pass 인증 파트너에서 사용할 수 있습니다.



미디어 토큰 검증기 라이브러리가 Java 아카이브 `mediatoken-verifier-VERSION.jar`에 포함되어 있습니다. 라이브러리는 다음을 정의합니다.

* JavaDoc 설명서가 포함된 토큰 확인 API(`ITokenVerifier` 인터페이스)
* 토큰이 실제로 Adobe에서 왔는지를 확인하는 데 사용되는 Adobe 공개 키
* Verifier API를 사용하는 방법과 라이브러리에 포함된 Adobe 공개 키를 사용하여 원본을 확인하는 방법을 보여 주는 참조 구현(`com.adobe.entitlement.test.EntitlementVerifierTest.java`)입니다


아카이브에는 모든 종속성 및 인증서 키 저장소가 포함되어 있습니다. 포함된 인증서 키 저장소의 기본 암호는 &quot;123456&quot;입니다.

* 확인 라이브러리를 사용하려면 JDK 버전 1.5 이상이 필요합니다.
* 서명 알고리즘 &quot;SHA256WithRSA&quot;에 대해 선호하는 JCE 공급자를 사용하십시오.


**확인 프로그램 라이브러리만 토큰 콘텐츠를 분석하는 데 사용해야 합니다. 토큰 형식은 보장되지 않으며 향후 변경될 수 있으므로 프로그래머는 토큰을 구문 분석하고 데이터를 직접 추출해서는 안 됩니다.** 검증자 API만 올바르게 작동합니다. 문자열을 직접 구문 분석하면 일시적으로 작동할 수 있지만, 나중에 형식이 변경될 수 있을 때 문제를 일으킬 수 있습니다. 검증자 API는 토큰에서 다음과 같은 정보를 검색합니다.

* 토큰이 유효합니까(`isValid()` 메서드)?
* 토큰(`getResourceID()` 메서드)에 연결된 리소스 ID입니다. 이는 `setToken()` 함수 콜백의 다른 매개 변수와 비교할 수 있으며 일치해야 합니다. 일치하지 않으면 사기성이 있음을 나타낼 수 있습니다.
* 토큰을 발급한 시간(`getTimeIssued()` 메서드).
* TTL(`getTimeToLive()` 메서드)입니다.
* MVPD(`getUserSessionGUID()` 메서드)에서 받은 익명 인증 GUID입니다.
* 사용자를 인증한 배포자의 ID(인증한 경우) - 배포자에 대한 인증을 제공한 프록시-MVPD입니다.

## 검증자 API 사용 {#using-verifier-api}

`ITokenVerifier` 클래스는 지정된 리소스에 대한 토큰 인증의 유효성을 검사하는 데 사용하는 메서드를 정의합니다. `setToken()` 요청에 대한 응답으로 받은 토큰을 분석하려면 `ITokenVerifier` 메서드를 사용하십시오.


`isValid()` 메서드는 토큰을 확인하는 기본 방법입니다. 하나의 인수, 리소스 ID가 필요합니다. Null 리소스 ID를 전달하면 메서드는 토큰 신뢰성 및 유효 기간만 확인합니다.


`isValid()` 메서드가 다음 상태 값 중 하나를 반환합니다.



| VALID_TOKEN | 모든 유효성 검사 성공 |
|--------------------|-----------------------------------------|
| INVALID_TOKEN_FORMAT | 토큰 형식이 잘못되었습니다. |
| INVALID_SIGNATURE | 토큰 신뢰성을 확인할 수 없습니다. |
| TOKEN_EXPIRED | 토큰 TTL이 잘못되었습니다. |
| INVALID_RESOURCE_ID | 토큰이 지정된 리소스에 유효하지 않음 |
| 오류 알 수 없음 | 토큰이 아직 확인되지 않았습니다. |

추가 메서드는 리소스 ID, 발급된 시간 및 주어진 토큰에 대한 TTL(Time-to-Live)에 대한 특정 액세스를 제공합니다.

* `getResourceID()`을(를) 사용하여 토큰과 연결된 리소스 ID를 검색하고 이를 setToken() 요청에서 반환된 ID와 비교합니다.
* 토큰을 발급한 시간을 검색하려면 `getTimeIssued()`을(를) 사용하십시오.
* TTL을 검색하려면 `getTimeToLive()`을(를) 사용하십시오.
* `getUserSessionGUID()`을(를) 사용하여 MVPD에서 설정한 익명화된 GUID를 검색하십시오.
* `getMvpdId()`을(를) 사용하여 사용자를 인증한 MVPD의 ID를 검색합니다.
* `getProxyMvpdId()`을(를) 사용하여 사용자를 인증한 프록시 MVPD의 ID를 검색합니다.

## 샘플 코드 {#sample-code}

미디어 토큰 검증기 보관에는 참조 구현(`com.adobe.entitlement.test.EntitlementVerifierTest.java`)과 테스트 클래스로 API를 호출하는 예가 포함되어 있습니다. 이 샘플(`com.adobe.entitlement.text.EntitlementVerifierTest.java`)은 토큰 확인 라이브러리를 미디어 서버에 통합하는 방법을 보여 줍니다.


```Java
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
