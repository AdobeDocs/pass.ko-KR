---
title: 선택 대화 상자에서 MVPD 허용
description: 선택 대화 상자에서 MVPD 허용
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# (레거시) 선택 대화 상자에서 MVPD 허용 {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 문제 {#issue}

프로그래머는 최종 사용자에게 공개하기 전에 새로운 MVPD 통합의 사용자 경험을 테스트하거나 확인해야 할 수 있습니다.

## 솔루션 {#solution}

`displayProviderDialog()` 콜백에서 Adobe Pass 인증은 선택한 프로그래머(요청자 ID)와 통합된 모든 MVPD를 반환합니다. 그러나 프로그래머는 MVPD의 반환 배열에 필터를 적용하고 두 목록에 있는 필터만 표시할 수 있습니다.

## 예 {#example}

이 예에서는 MVPD 선택기 대화 상자 내에 CableCompany_1 및 CableCompany_2만 표시하고 CableCompany_NewIntegration은 표시하지 않는 방법을 보여 줍니다.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if ( isAllowListed(currentMvpd.ID) ) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isAllowListed(mvpdID) {
    // Implement allowlisting on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
}
```

<!--
**Related Information**
* [Prevent MVPDs from appearing in the Selection Dialog](/help/authentication/prevent-mvpd-selectn-dialog.md)
* **Code Samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
