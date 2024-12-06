---
title: 선택 대화 상자에 MVPD가 표시되지 않도록 합니다.
description: 선택 대화 상자에 MVPD가 표시되지 않도록 합니다.
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---

# 선택 대화 상자에 MVPD가 표시되지 않도록 합니다.

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 문제 {#issue-prevent-mvpd-sel-dialog}

(&quot;차단 목록&quot;) 특정 MVPD가 MVPD 선택기에 표시되지 않도록 해야 합니다.


## 솔루션 {#solution-prevent-mvpd-sel-dialog}

해결 방법은 `displayProviderDialog()`이(가) 호출될 때 차단 목록을 작성하는 것입니다.

예를 들어 CableCompany_1 및 CableCompany_2가 MVPD 선택기 내부에 표시되지 않도록 하려면 다음 예제와 같은 작업을 수행합니다.

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

<!--
**Related Information**

* [Allow MVPDs in the Selection Dialog](/help/authentication/allow-mvpd-selectn-dialog.md)
* **Code samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
