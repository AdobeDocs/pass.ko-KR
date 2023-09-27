---
title: /authenticate 요청에 '&'reg_code를 사용하지 마십시오.
description: /authenticate 요청에 '&'reg_code를 사용하지 마십시오.
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# /authenticate 요청에 &#39;&amp;&#39;reg_code를 사용하지 마십시오. {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>



## 문제

IE 9 브라우저는 &#39;\®&#39;을(를) 특수 명령으로 해석하고 IE로 ®.

## 설명

다음과 같은 경우 `/authenticate` 요청은 다음과 같이 구성됩니다.


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


...아래와 같이 IE 브라우저에 의해 해석되며 다음 형식으로 Adobe에 전송됩니다.


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


&#39;&amp;&#39;가 없고 Adobe이 을 찾을 수 없으므로 요청자\_id는 univion®\_code=EKAFMFI로 해석됩니다. `regCode` 토큰을 연결할 매개 변수.  AuthN 토큰이 전혀 생성되지 않을 가능성이 있으며, 이 경우 `/checkauthn` 호출에서 토큰을 찾을 수 없습니다.



## 솔루션

다음 옵션 중 하나로 이 문제를 해결할 수 있습니다.

1. 를 사용하지 마십시오. `&reg_code` 다른 쿼리 문자열 매개 변수 간의 매개 변수입니다.  대신 요청 URL의 첫 번째 쿼리 문자열 매개 변수로 이동하여 다음과 같이 요청 URL을 만듭니다.


       &lt;fqdn>authenticate?reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   이렇게 하면 `&reg` 매개 변수가 잘못 해석되지 않습니다.

1. 정규화 `&reg_code` 을 사용하는 경우 `&amp;reg_code`.

1. Adobe은 AuthN 토큰 생성이 실패한 경우 인증 호출에 응답하여 오류 코드를 두 번째 화면으로 다시 전송하는 새 기능을 도입할 수 있습니다.
