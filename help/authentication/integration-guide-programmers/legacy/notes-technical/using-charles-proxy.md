---
title: Charles Proxy 사용
description: Charles Proxy 사용
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: 175755aa7463257487b29c5f4da989cf34e91bfd
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---

# (기존) Charles Proxy 사용 {#using-charles-proxy}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

**Charles:** <http://charlesproxy.com>


## Charles Proxy 다운로드, 설치 및 시작 {#download-install-and-get-stared-with-charles-proxy}

- **다운로드** - <http://www.charlesproxy.com/download/>
- **설치** - <http://www.charlesproxy.com/documentation/installation/>
- **시작** - <http://www.charlesproxy.com/documentation/getting-started/>


## 구조 및 시퀀스 탭 {#structure-vs-sequence-tabs}

트래픽을 보는 방법에는 두 가지가 있습니다.

1. **구조** - 요청이 호스트별로 그룹화됨
1. **시퀀스** - 요청이 호출된 순서대로 나열됩니다.


## SSL 및 인증서 {#ssl-and-certificates}

`\[ *Proxy -\> Proxy Settings... -\> SSL* \]` SSL 프록시 사용

&quot;SSL 프록시 활성화&quot; 확인란을 선택하고 모든 HTTPS 위치를 추가합니다.

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)
-->

- SSL 프록시 - <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- SSL 인증서 - <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- 모바일 장치에서 SSL 프록시 설정 - 아래를 참조하십시오.


## 호스트 {#ignore-/-exclude-hosts} 무시/제외

출력이 너무 복잡해지면 위치를 무시하거나 제외하도록 선택할 수 있습니다. 다음 중 하나를 수행하여 위치를 무시하거나 제외할 수 있습니다.

- 무시하려는 요청을 마우스 오른쪽 단추로 클릭한 다음 &quot;무시&quot;를 선택합니다
- `\[ *Proxy -\> Recording Settings... -\> Exclude* \]`에서 제외할 위치를 수동으로 추가하십시오.


## DNS 스푸핑 {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



DNS 스푸핑은 요청을 다른 IP로 리디렉션하려고 할 때, 특히 모바일 디바이스에서 작업할 때 매우 유용합니다.

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## 원격 매핑 {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



맵 리모컨을 사용하면 &quot;들어오는&quot; 요청을 다른 끝점으로 리디렉션할 수 있습니다. 이 기능의 가장 일반적인 사용 사례는 `AccessEnabler.swf`을(를) `AccessEnablerDebug.swf:`에 &quot;매핑&quot;하는 것입니다.

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## 역방향 프록시 {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## 모바일 {#mobile}

### iOS 장치(iPhone/iPad)에서 Charles 사용 {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### iPhone의 SSL 연결 {#ssl-connection-from-iphone}

iOS 장치에서 <http://charlesproxy.com/charles.crt>(으)로 이동합니다.  이렇게 하면 인증서 설치 대화 상자가 시작됩니다.

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)
-->

</br>

인증서 설치를 완료하려면 `\[ *Install*... *Install*... *Done* \]`을(를) 클릭하십시오.

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### iOS 디바이스에서 Charles 사용 {#using-charles-from-an-ios-device}

iOS 장치에서 `\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]`을(를) 선택합니다. 네트워크 옆에 있는 파란색 작은 화살표를 클릭한 다음 HTTP 프록시 로 내려가서 &quot;수동&quot;을 선택합니다.


</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)
-->

</br>
여기서 Charles를 실행하는 시스템의 IP와 포트를 지정해야 합니다. <span style="line-height: 1.6em;">이제 iOS 장치에서 Safari를 열고 웹 페이지를 열려고 하면 Charles를 실행하는 컴퓨터에서 다음 팝업이 표시됩니다.

</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)
-->

</br>
"허용"을 클릭하여 장치가 Charles를 사용하여 모든 해당 항목을 프록시하도록 허용합니다.
요청.

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS - 모든 인증서 신뢰 {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### iOS 인증 오류 - adobepass.ios.app을 찾을 수 없음

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## Android에 Charles 사용

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


Android 장치에서 [Charles 프록시](http://charlesproxy.com/charles.crt)(으)로 이동합니다.
