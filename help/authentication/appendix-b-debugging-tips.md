---
title: 부록 B "디버깅 팁"
description: 부록 B "디버깅 팁"
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# 부록 B: 디버깅 팁 {#appendix-b-debugging-tips}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.


## 임시 데이터 지우기 {#clearing-temporary-data}

Adobe Pass 인증은 브라우저 캐시, LSO 캐시 및 쿠키와 같은 임시 데이터를 저장합니다. 테스트할 때 깨끗한 슬레이트를 얻을 수 있도록 임시 데이터를 지우는 것이 중요합니다.

- [브라우저 캐시 및 쿠키 지우기](#clearing-the-browser-cache-and-cookies)
- [LSO 캐시 지우기](#clearing-lsos-cache)


## 브라우저 캐시 및 쿠키 지우기 {#clearing-the-browser-cache-and-cookies}

Firefox에서는 브라우저를 신뢰할 수 있지만, &quot;도구&quot; -\> &quot;최근 내역 지우기...&quot; -\> &quot;지울 시간 범위:&quot;에서 &quot;모두&quot;를 선택하고, &quot;세부 정보&quot;에서 &quot;쿠키&quot; 및 &quot;캐시&quot; -\> &quot;지금 지우기&quot;를 클릭합니다.


## LSO 캐시 지우기 {#clearing-lsos-cache}

액세스 [Flash Player 도움말](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html).

다음 항목 선택 ```entitlement.\*``` (테스트되는 내용에 따라) 을(를) 클릭하고 &quot;웹 사이트 삭제&quot;를 클릭합니다.


## 디버깅 도구 {#tools}

Adobe Pass 인증 엔지니어는 다음 디버깅 도구를 사용합니다.

- 파이어버그 - <http://www.getfirebug.com/>
- Flashbug(Flash Player의 디버그 버전에서 작동)
- 바이올린 - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- 와이어샤크 <http://www.wireshark.org/>


<!--
## Related Information

- [Programmer Integration Guide](/help/authentication/programmer-integration-guide-overview.md)

- [Using Charles Proxy (Tech Note)](https://tve.zendesk.com/hc/en-us/articles/204962849-Using-Charles-Proxy)
-->
