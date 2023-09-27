---
title: 인증 시작
description: 인증 시작
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 0%

---

# 인증 시작 {#initiate-authentication}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## REST API 끝점 {#clientless-endpoints}

&lt;reggie_fqdn>:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* 프로덕션 - [api.auth.adobe.com](http://api.auth.adobe.com/)
* 스테이징 - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## 설명 {#description}

MVPD 선택 이벤트를 알려 인증 프로세스를 시작합니다. MVPD에서 성공적인 응답을 받으면 조정되는 Adobe Pass 인증 데이터베이스에 레코드를 만듭니다.



| 엔드포인트 | 호출됨  </br>작성자: | 입력   </br>매개 변수 | HTTP  </br>방법 | 응답 | HTTP  </br>응답 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate | AuthN 모듈 | 1. requestor_id (필수)</br>2.  mso_id(필수)</br>3.  reg_code(필수)</br>4.  domain_name(필수)</br>5.  noflash=true -  </br>    (필수, 잔여 매개 변수)</br>6.  no_iframe=true (필수, 잔차 매개 변수)</br>7.  추가 매개 변수(선택 사항)</br>8.  redirect_url(필수) | GET | 로그인 웹 앱은 MVPD 로그인 페이지로 리디렉션됩니다. | 전체 리디렉션 구현의 경우 302 |

{style="table-layout:auto"}


| 입력 매개 변수 | 설명 |
| --- | --- |
| requestor_id | 이 작업이 유효한 프로그래머 요청자입니다. |
| mso_id | 이 작업이 유효한 MVPD ID. |
| reg_code | Reggie 서비스에서 생성한 등록 코드. |
| domain_name | 원래 도메인. |
| redirect_url | 인증 완료 후 로그인 Webapp 리디렉션 URL. |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**중요: 필수 매개 변수 -** 클라이언트측 구현에 관계없이 위의 모든 매개 변수는 필수입니다.
>
>
>예:
>
>```
>domain_name=loginwebapp.com
>mso_id=sampleMvpdId
>reg_code=RO0885W
>requestor_id=sampleRequestorId
>noflash=true
>redirect_url=http://loginwebapp.com
>```

>[!IMPORTANT]
> 
>**중요: 선택적 매개 변수**
>
>호출에는 다음과 같은 다른 기능을 활성화하는 선택적 매개 변수도 포함될 수 있습니다.
>
> * generic\_data - 다음을 사용할 수 있습니다. [프로모션 TempPass](/help/authentication/promotional-temp-pass.md)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **메모** {#notes}

* 값 `domain_name` 매개 변수는 Adobe Pass 인증에 등록된 도메인 이름 중 하나로 설정해야 합니다. 자세한 내용은 다음을 참조하십시오. [등록 및 초기화](/help/authentication/programmer-overview.md).

* [/authenticate 요청에 &#39;&amp;&#39;reg\_code를 사용하지 마십시오(기술 노트).](/help/authentication/clientless-avoid-using-reg-code-in-authenticate-request.md)

* 다음 `redirect_url` 매개 변수는 순서대로 마지막 매개 변수여야 합니다.

* 값 `redirect_url` 매개 변수는 URL로 인코딩해야 합니다.
