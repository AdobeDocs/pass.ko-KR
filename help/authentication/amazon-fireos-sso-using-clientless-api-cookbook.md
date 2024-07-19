---
title: Clientless API Cookbook을 사용한 Amazon FireOS SSO
description: Clientless API Cookbook을 사용한 Amazon FireOS SSO
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '755'
ht-degree: 0%

---

# Clientless API Cookbook을 사용한 Amazon FireOS SSO {#amazon-fireos-sso-using-clientless-api-cookbook}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>

## 소개 {#Introduction}

이 문서에서는 Clientless API를 사용하여 Amazon의 SSO 버전의 Adobe Pass 인증 흐름을 구현하는 방법을 설명합니다. 이 문서의 첫 번째 부분은 Amazon 버전의 아키텍처가 갖는 특수성에 중점을 두며, 이는 구현에 익숙하고 경험이 풍부한 많은 파트너에게 적합합니다.

이 문서의 두 번째 부분에서는 Adobe Pass 인증 클라이언트 없는 API를 구현하는 주요 단계를 설명합니다.

Clientless 솔루션이 작동하는 방식에 대한 광범위한 기술 개요는 [REST API 개요](/help/authentication/rest-api-overview.md)를 참조하십시오. Adobe은 전체 아키텍처 및 첫 번째 구현에 대한 지원을 위한 기본 연락처입니다.

## Amazon 클라이언트 없는 SSO {#AMZ-Clientless-SSO}

### 높은 수준의 아키텍처 {#High-Level-Arch}

Amazon Clientless SSO 구현은 단순하며 대부분 일반 Adobe Primetime 인증 Clientless API와 동일합니다.

Amazon SDK를 사용하여 개인화된 페이로드를 검색하고 Adobe 클라이언트 없는 API를 호출할 때 사용해야 합니다.

페이로드가 인식되고 인증된 세션에 해당하는 경우 클라이언트 없는 API는 세션 토큰과 함께 즉시 반환됩니다.

### Amazon SDK를 사용하기 위해 애플리케이션을 빌드하는 방법 {#Build-entries}

* 최신 [Amazon 스텁 SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar)를 다운로드하여 앱 디렉터리에 평행한 /SSOEnabler 폴더로 복사합니다.
* 라이브러리 를 사용하도록 매니페스트/gradle 파일을 업데이트합니다.

  **매니페스트 파일에 다음 줄을 추가합니다.**

  ```Java
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false"/\>
  ```

  **Gradle 파일 항목:**

  저장소에서:

  ```java
  flatDir {
       dirs '../SSOEnabler'
  }
  ```

  종속성 아래에 다음을 추가합니다.

  ```Java
  provided fileTree(include: \['ottSSOTokenStub.jar'\], dir: '../SSOEnabler')
  ```


* Amazon 컴패니언 앱의 부재 처리:

  응용 프로그램이 실행 중인 Amazon 장치에 동반자가 없을 수도 있지만 런타임에 `com.amazon.ottssotokenlib.SSOEnabler` 클래스의 ClassNotFoundException이 발생해야 합니다.

  이 경우 페이로드 단계를 건너뛰고 일반 PrimeTime 플로우로 되돌아가기만 하면 됩니다. SSO는 활성화되지 않지만 일반 인증 흐름은 정상적으로 발생합니다.

</br>

### Amazon SDK를 사용하여 Amazon SSO 페이로드를 가져오는 방법 {#UseAmazonSSO}

응용 프로그램을 초기화하는 동안 SSOEnabler의 인스턴스를 가져옵니다. 애플리케이션 아키텍처를 기반으로 동기식 구현과 비동기식 구현 중 하나를 결정해야 합니다.

어떤 이유로든 API 호출이 페이로드를 반환하지 않는 경우 SSO가 아닌 일반 흐름을 사용하여 Amazon 및 Adobe 파트너에게 문의하여 확인하십시오.

**비동기 API**

* SSO Enabler 인스턴스 가져오기:

  ```Java
  ssoEnabler = SSOEnabler.getInstance(context);
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```


* 콜백 설정

  ```java
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

   * 성공 응답 번들에는 다음이 포함됩니다.
      * 키가 &quot;SSOToken&quot;인 문자열로서의 SSO 토큰
   * 실패 응답 번들에는 다음이 포함됩니다.
      * 키가 &quot;ErrorCode&quot;인 int로 오류 코드
      * 오류 설명을 &quot;ErrorDescription&quot; 키가 있는 문자열로 표시


* SSO 토큰 가져오기

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

* 이 API는 초기화 중에 설정된 콜백을 통해 응답을 제공합니다.

  **예**. 초기화 중에 생성된 singleton 인스턴스를 사용한 호출:

  ```JAVA
  ssoEnabler.getSSOTokenAsync().
  ```


**동기 API**

* SSO Enabler 인스턴스 가져오기 및 콜백 설정

  ```JAVA
  ssoEnabler = SSOEnabler.getInstance(context);</span>
  ```

* SSO 토큰 가져오기

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

   * 이 API는 호출자 스레드를 차단하고 결과 번들로 응답합니다. 이 호출은 동기 호출이므로 주 스레드에서 사용하지 마십시오.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

   * 값(밀리초). 설정된 경우 동기화 API에 대해 기본 시간 제한 값 1분을 재정의합니다.


### Dynamic Client Registration을 사용하도록 Adobe Pass Clientless API 업데이트 {#clientlessdcr}

구현이 처음이라면 **클라이언트 없는 기술 개요**&#x200B;를 보고 지원이 필요한 경우 Adobe에 문의하십시오.

Adobe 클라이언트 없는 API를 사용하려면 애플리케이션이 Adobe 서버를 호출하기 위해 Dynamic Client Registration을 사용해야 합니다.

* 응용 프로그램에서 Dynamic Client Registration을 사용하려면 [Dynamic Client Registration Management의 지침에 따라 응용 프로그램을 등록](/help/authentication/dynamic-client-registration-management.md)합니다.

* Adobe Pass 서버에 대한 인증 및 권한 부여 요청을 수행하기 위해 Dynamic Client Registration API를 구현하려면 [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)의 지침을 따르십시오.

### Amazon SSO를 사용하도록 Adobe Pass Clientless API 업데이트 {#clientlesssso}

Amazon SDK에서 가져온 Amazon SSO 페이로드는 Adobe Pass 인증 종단점에 대한 요청에 존재해야 합니다.

```
      /adobe-services/*
      /reggie/*
      /api/*
```


모든 Adobe Pass 인증 종단점은 다음 방법을 지원하여 장치 범위 식별자 또는 플랫폼 범위 식별자(Amazon SSO 페이로드에 있음)를 받습니다.

* 헤더로서 : &quot;Adobe-주체-토큰&quot;
* 쿼리 매개 변수로서: &quot;ast&quot;
* 게시물 매개 변수로 : &quot;ast&quot;


>[!NOTE]
>
>장치 범위 식별자 또는 플랫폼 범위 식별자가 쿼리/게시물 매개 변수로 전송된 경우 요청 서명을 생성할 때 이를 포함해야 합니다.

>[!NOTE]
>
>쿼리 매개 변수 &quot;ast&quot;를 사용하면 전체 URL이 매우 길어지고 거부될 수 있습니다. /authenticate 호출 시 /regcode 호출에서 제공되었으므로 이 매개 변수를 건너뛸 수 있습니다.

**예:**

**사용자 지정 헤더로 보내기**

```HTTPS
GET /adobe-services/config/requestor HTTP/1.1 Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**쿼리 매개 변수로 보내기**

```HTTPS
GET /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com
```


**게시물 매개 변수로 보내기**


```HTTPS
POST /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com Content-Type: multipart/form-data;
boundary=---- WebKitFormBoundary7MA4YWxkTrZu0gW
```

>[!NOTE]
>
>Amazon SSO가 없거나 잘못된 경우 Adobe Pass 인증은 속성을 무시하고 SSO가 없는 것처럼 호출을 실행합니다.
