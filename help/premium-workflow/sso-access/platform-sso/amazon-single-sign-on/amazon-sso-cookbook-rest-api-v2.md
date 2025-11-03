---
title: Amazon SSO Cookbook(REST API V2)
description: Amazon SSO Cookbook(REST API V2)
exl-id: 63e4fa63-8ca3-40eb-b49a-84dd75c2ca1d
source-git-commit: e4d243ebf293f3ecc38e532d77116c065a22ebd2
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Amazon SSO Cookbook(REST API V2) {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

Adobe Pass 인증 REST API V2는 FireOS에서 실행되는 클라이언트 애플리케이션의 최종 사용자를 위한 Platform SSO(Single Sign-On)를 지원합니다.

이 문서는 높은 수준의 보기를 제공하는 기존 [REST API V2 개요](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)에 대한 확장 역할을 하며, [플랫폼 ID 흐름을 사용하여 Single Sign-On을 구현하는 방법](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)을 설명하는 문서입니다.

## platform id 흐름을 사용한 Amazon single sign-on {#cookbook}

Adobe Pass 인증은 Amazon과 협력하여 로그인 사용자 경험을 개선하고 TV 구독자를 위한 TV Everywhere 애플리케이션에서 SSO(Single Sign-On)를 용이하게 합니다.

### 사전 요구 사항 {#prerequisites}

플랫폼 ID 흐름을 사용하여 Amazon Single Sign-On을 계속 진행하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

#### Amazon SSO SDK 통합 {#integrate-amazon-sso-sdk}

스트리밍 애플리케이션은 SSO(Single Sign-On)용 [Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) 라이브러리를 빌드에 통합해야 합니다.

* 최신 Amazon SSO SDK 라이브러리를 다운로드하여 응용 프로그램 디렉터리에 평행한 `/SSOEnabler` 폴더에 복사합니다.

* Amazon SSO SDK 라이브러리를 사용하도록 매니페스트 및 Gradle 파일을 업데이트합니다.

  **매니페스트:**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle:**

  저장소에서:

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  종속성 아래에:

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Amazon SSO SDK 사용 {#use-amazon-sso-sdk}

스트리밍 애플리케이션은 Amazon SSO SDK을 사용하여 SSO 토큰(플랫폼 ID) 페이로드를 가져와야 합니다.

Amazon SSO SDK은 SSO 토큰(플랫폼 ID) 페이로드를 얻기 위해 동기 및 비동기 API를 모두 제공합니다.

스트리밍 애플리케이션은 아키텍처에 따라 두 가지 옵션 중 하나를 선택할 수 있습니다.

##### 비동기 API

* `SSOEnabler` 인스턴스를 가져오고 `SSOEnablerCallback`을(를) 설정합니다.

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```

  이 작업은 스트리밍 애플리케이션의 초기화 중에 수행할 수 있습니다.

  ```JAVA
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

  SSO 토큰 성공 응답 번들에는 다음이 포함됩니다.
   * 키가 &quot;SSOToken&quot;인 `string`(으)로 SSO 토큰입니다.

  <br/>

  SSO 토큰 실패 응답 번들에는 다음이 포함됩니다.
   * 키가 &quot;ErrorCode&quot;인 `int`(으)로 오류 코드.
   * 키가 &quot;ErrorDescription&quot;인 `string`(으)로 오류 설명.

  <br/>

* SSO 토큰 가져오기:

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  이 API는 초기화 중에 설정된 콜백을 통해 응답을 제공합니다.

##### 동기 API

* `SSOEnabler` 인스턴스 가져오기:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* SSO 토큰 가져오기:

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  이 API는 호출자 스레드를 차단하고 결과 번들로 응답합니다. 이 호출은 동기 호출이므로 주 스레드에서 이 호출을 사용하지 마십시오.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  이 API는 동기 호출에 대한 시간 초과 값을 설정합니다. 기본 시간 제한 값은 1분입니다.

#### Amazon SSO 대체 {#fallback-amazon-sso}

스트리밍 애플리케이션은 Amazon SSO 흐름에서 일반 인증 흐름으로의 대체 시나리오를 처리해야 합니다.

스트리밍 애플리케이션에서 다음을 처리하고 있는지 확인합니다.

* Amazon 장치에서 실행되어야 하는 Amazon 컴패니언 애플리케이션이 없습니다.
   * 스트리밍 응용 프로그램에서 런타임에 `ClassNotFoundException` 클래스의 `com.amazon.ottssotokenlib.SSOEnabler`이(가) 발생할 수 있습니다.

* 위 API에서 반환해야 하는 SSO 토큰(플랫폼 ID) 페이로드가 없습니다.
   * 스트리밍 애플리케이션은 Amazon 및 Adobe 담당자에게 연락하여 조사할 수 있습니다.

### 워크플로 {#workflow}

Amazon SSO 토큰(플랫폼 ID) 페이로드는 Adobe Pass 인증 REST API V2 끝점에 대해 수행된 모든 HTTP 요청에 있어야 합니다.

```
/api/v2/*
```

Adobe Pass 인증 REST API V2는 다음 방법을 지원하여 장치 범위 또는 플랫폼 범위 식별자인 SSO 토큰(플랫폼 ID) 페이로드를 받습니다.

* `Adobe-Subject-Token`(이)라는 헤더로

>[!IMPORTANT]
> 
> `Adobe-Subject-Token` 헤더에 대한 자세한 내용은 [Adobe-Subject-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) 설명서를 참조하십시오.

#### 샘플

**헤더로 보내기**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> `Adobe-Subject-Token` 헤더 값이 없거나 잘못된 경우 Adobe Pass 인증은 Single Sign-On을 고려하지 않고 요청을 처리합니다.
