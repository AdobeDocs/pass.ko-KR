---
title: 부록 B "디버깅 팁"
description: 부록 B "디버깅 팁"
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 0%

---

# (기존) 부록 B: 디버깅 팁 {#appendix-b-debugging-tips}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 임시 데이터 지우기 {#clearing-temporary-data}

Adobe Pass 인증은 브라우저 캐시, LSO 캐시 및 쿠키와 같은 임시 데이터를 저장합니다. 테스트할 때 깨끗한 슬레이트를 얻을 수 있도록 임시 데이터를 지우는 것이 중요합니다.

- [브라우저 캐시 및 쿠키 지우기](#clearing-the-browser-cache-and-cookies)
- [LSO 캐시 지우기](#clearing-lsos-cache)


## 브라우저 캐시 및 쿠키 지우기 {#clearing-the-browser-cache-and-cookies}

Firefox에서는 브라우저를 신뢰할 수 있지만, &quot;도구&quot; -\> &quot;최근 내역 지우기...&quot; -\> &quot;지울 시간 범위:&quot;에서 &quot;모두&quot;를 선택하고, &quot;세부 정보&quot;에서 &quot;쿠키&quot; 및 &quot;캐시&quot; -\> &quot;지금 지우기&quot;를 클릭합니다.


## LSO 캐시 지우기 {#clearing-lsos-cache}

[Flash Player 도움말](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html)에 액세스합니다.

```entitlement.\*```을(를) 선택하고(테스트 내용에 따라) &quot;웹 사이트 삭제&quot;를 클릭합니다.


## 디버깅 도구 {#tools}

Adobe Pass 인증 엔지니어는 다음 디버깅 도구를 사용합니다.

- Firebug - <http://www.getfirebug.com/>
- Flashbug(Flash Player의 디버그 버전에서 작동)
- Fiddler - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>
