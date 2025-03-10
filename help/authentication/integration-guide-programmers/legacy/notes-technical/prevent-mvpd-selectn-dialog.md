---
title: 선택 대화 상자에 MVPD가 표시되지 않도록 합니다.
description: 선택 대화 상자에 MVPD가 표시되지 않도록 합니다.
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 0%

---

# (레거시) 선택 대화 상자에 MVPD가 표시되지 않도록 합니다.

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 문제 {#issue-prevent-mvpd-sel-dialog}

특정 MVPD(&quot;차단 목록&quot;)가 MVPD 선택기에 표시되지 않도록 해야 합니다.


## 솔루션 {#solution-prevent-mvpd-sel-dialog}

해결 방법은 `displayProviderDialog()`이(가) 호출될 때 차단 목록을 작성하는 것입니다.

예를 들어 CableCompany_1 및 CableCompany_2가 MVPD 선택기 내에 표시되지 않도록 하려면 다음 예제와 같은 작업을 수행해야 합니다.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if (!isBlocklisted(currentMvpd.ID)) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isBlocklisted(mvpdID) {
    // Implement block-listing on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
} 
```
