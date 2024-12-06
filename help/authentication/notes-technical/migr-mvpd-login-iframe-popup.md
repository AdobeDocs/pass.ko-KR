---
title: MVPD 로그인 페이지를 iFrame에서 Popup으로 마이그레이션하는 방법
description: MVPD 로그인 페이지를 iFrame에서 Popup으로 마이그레이션하는 방법
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '686'
ht-degree: 0%

---

# MVPD 로그인 페이지를 iFrame에서 Popup으로 마이그레이션하는 방법 {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 팝업과 iFrame 비교 {#popup-vs-iframe}

일부 사용자의 경우 MVPD 로그인 페이지의 iFrame 구현에서 서드파티 쿠키 문제가 발생했습니다.
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

Adobe Pass 인증 팀 **에서는 Firefox 및 Safari에서 iFrame 버전보다 팝업/새 창 로그인 페이지**&#x200B;를 구현하는 것이 좋습니다.  그러나 Internet Explorer용 로그인 페이지를 구현하는 경우 팝업 구현에 문제가 발생할 수 있습니다. IE 문제는 사용자가 팝업 창에서 MVPD로 인증되면 Adobe Pass 인증이 Internet Explorer에서 팝업 차단으로 보이는 상위 페이지 리디렉션을 강제 적용하기 때문에 발생합니다. Adobe Pass 인증 팀 **에서는 Internet Explorer에 대해 iFrame 로그인을 구현할 것을 권장합니다**.

이 기술 노트에 제공된 샘플 코드는 iFrame과 팝업의 하이브리드 구현을 사용하여 Internet Explorer에서는 iFrame을 열고 다른 브라우저에서는 팝업을 엽니다.

iFrame 구현이 이미 있는 것을 고려하여 기술 노트의 첫 번째 부분에는 iFrame 구현을 위한 코드가 표시되고 두 번째 부분에는 팝업 구현을 기본값으로 수용하기 위한 변경 사항이 표시됩니다.


## iFrame에 로그인 페이지가 있는 MVPD 선택기 {#mvpd-pickr-iframe}

이전 코드 예제에서는 iFrame 닫기 버튼과 함께 iFrame이 생성될 &lt;div> 태그가 포함된 HTML 페이지를 보여 주었습니다.

```HTML
<body> 
    <div id="mvpddiv">
        <div style="background: red">
            <input type="button" id="btnCloseIframe" value="X" onclick="closeIframeAction();">
        </div>
        <br/>
        <!-- We use the "about:blank" value so that when the iFrame loads
             we do not see the the parent page. -->
        <iframe id="mvpdframe" name="mvpdframe" src="about:blank"></iframe>
    </div> 
</body>
```

다음은 연결된 **JavaScript** 코드입니다.

```JavaScript
/*
 * Callback indicating that the AccessEnabler swf has initialized
 */
function swfLoaded() {
    // AccessEnabler is loaded so we can use the API function it provides
    accessEnablerObject.setRequestor(requestorID); 
    accessEnablerObject.checkAuthentication();
} 
/*
* The code the correctly closes the opened IFrame and does not leave the
* AccessEnabler in an undefined state.
*/
function closeIframeAction () {
    accessEnablerObject.setSelectedProvider(null);
    /* We use the "about:blank" value so that when the iFrame loads
     * we do not see the the previous MVPD.
     */
    document.getElementById('mvpdframe').src="about:blank";
    document.getElementById('mvpddiv').style.visibility="hidden";
    document.getElementById('mvpddiv').style.display="none";
}
/*
* Some of the supported MVPDs require their login pages to be displayed within an iFrame.
* In your HTML page, you must include the following <div> tag: "<div id="mvpddiv"></div>".
* Called when the selected MVPD requires an iFrame in which to display its authentication UI.
*/
function createIFrame(inWidth, inHeight) {
     // Create the iFrame to the specified width and height for the auth page
    ifrm = document.createElement("IFRAME");
    ifrm.name = "mvpdframe";
    ...
    document.getElementById('mvpddiv').appendChild(ifrm);
    ...
    // Force the name into the DOM since it is still not refreshed, for IE
    window.frames["mvpdframe"].name = "mvpdframe";
}
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
    /* This is an example how previously the MVPD picker was build and will be replaced. */
}
/* 
* Sending the user selected MVPD of the custom dialog is achieved
* by calling the API function setSelectedProvider(providerID:String).
*/
function setSelectedProvider(providerID) {
    accessEnablerObject.setSelectedProvider(providerID);
}
```


## 팝업 창에 로그인 페이지가 있는 MVPD 선택기 {#mvpd-pickr-popup}

**iFrame**&#x200B;을 더 이상 사용하지 않으므로 HTML 코드에는 iFrame 또는 iFrame을 닫는 단추가 포함되지 않습니다. 이전에 iFrame(**mvpddiv**)이 들어 있던 div는 다음 용도로 유지되고 사용됩니다.

* 팝업 포커스가 손실되는 경우 MVPD 로그인 페이지가 이미 열려 있음을 사용자에게 알립니다.
* 팝업에 다시 집중할 수 있는 링크를 제공합니다.

```HTML
<body onload="javascript:loadAccessEnabler();"> 
   <div id="aeHolder">
       <div name="contentAccessEnabler" id="contentAccessEnabler"></div>
   </div>
   <div id="mvpddiv">
      <p>
      <strong>No login window?</strong>
      <br/>
      <a href="javascript:mvpdWindow.focus();">Click here to open it.</a>
      <br/>
      Still not working? Check popup blockers!
      </p>
   </div> 
 
   <div id="picker" style="display:none">
      Choose MVPD: <br/>
      <select id="mvpdList" multiple></select>
      <br/>
         <a id="authenticateButton" onclick="javascript:authenticate();">Authenticate</a>
   </div>
</body>
```

MVPD 목록이 **picker** div에 선택 **-mvpdList**(으)로 표시됩니다.

새 API 콜백이 사용됩니다. **setConfig(configXML)**. 콜백은 setRequestor(requestorID) 함수를 호출한 후 트리거됩니다. 이 콜백은 이전에 설정된 requestorID와 통합된 MVPD 목록을 반환합니다. 콜백 메서드에서 들어오는 XML은 구문 분석되고 MVPD 목록은 캐시됩니다. MVPD 선택기도 만들어졌지만 표시되지 않습니다.

```JavaScript
var mvpdList;  // The list of cached MVPDs
var mvpdWindow;  // The reference to the popup with the MVPD login page
var cancelTimer = 0;
/* Callback indicating that the AccessEnabler swf has initialized */
function swfLoaded() {
   accessEnablerObject = $('#AccessEnabler').get(0);
   // Using a custom implementation of the MVPD picker
   accessEnablerObject.setProviderDialogURL('none');
   accessEnablerObject.setRequestor(requestorID); 
}

function setConfig(configXML) {
   mvpdList = [];
   $.each($($.parseXML(configXML)).find('mvpd'), function (idx, item) {
      var mvpdId = $(item).find('id').text();
      mvpdList[mvpdId] = {
         displayName: $(item).find('displayName').text(),
         logo: $(item).find('logoUrl').text(),
         popup: $(item).find('iFrameRequired').text() == "true",
         width: $(item).find('iFrameWidth').text(),
         height: $(item).find('iFrameHeight').text()
      };

      $('#mvpdList').append($('<option value="' + mvpdId + '" title="' + mvpdId + '">' + mvpdList[mvpdId].displayName + '</option>'));
   });
   accessEnablerObject.getAuthentication();
}
```

getAuthentication() 또는 getAuthorization() 함수가 호출되면 displayProviderDialog() 콜백이 트리거됩니다. 일반적으로 이 콜백 내에서 MVPD 목록이 만들어지고 표시되었을 것입니다. MVPD 선택기가 이미 만들어졌으므로 남은 작업은 사용자에게 표시하는 것입니다.

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

사용자가 선택기에서 MVPD를 선택한 후 팝업을 만들어야 합니다. 팝업이 about:blank로 작성되거나 다른 도메인에 있는 페이지로 작성된 경우 일부 브라우저에서 팝업을 차단할 수 있으므로 AccessEnabler가 로드되는 위치의 호스트 이름으로 여는 것이 좋습니다.

iFrame 구현에서는 btnCloseIframe 버튼과 JavaScript 함수 closeIframeAction()에 의해 인증 흐름 재설정이 수행되었지만 이제 iFrame을 데코레이트할 수 없습니다. 따라서 팝업이 닫힐 때를 관찰하거나(사용자가 또는 인증 흐름을 완료하여) 동일한 동작을 수행합니다. 사용자가 팝업의 포커스를 잃는 경우에도 도움이 되는 코드 조각이 추가되었습니다.

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

createIFrame() 콜백에서 **mvpddiv** div가 표시됩니다.

```JavaScript
function createIFrame(width, height) {
   $('#mvpddiv').show();
   if (useIframeLoginStyle) {
      ...
   }
}

/* Function called when user has selected the MVPD from the picker */
function authenticate() {
   var selectedMvpd = $('#mvpdList').val()[0];
   if (mvpdList[selectedMvpd].popup) {
      mvpdWindow = window.open("http://entitlement.auth-staging.adobe.com", "mvpdframe", "width=" + mvpdList[selectedMvpd].width + ",height=" + mvpdList[selectedMvpd].height, true);
      if (!mvpdWindow) {
         // Message to user that popup blocker is not allowing the authentication process to start
      }
      //watch the mvpd popup for close
      clearInterval(cancelTimer);
      cancelTimer = setInterval(checkClosed, 200);
   }
   accessEnablerObject.setSelectedProvider(selectedMvpd);
}

function checkClosed() {
   try {
      if (mvpdWindow && mvpdWindow["closed"]) {
         clearInterval(cancelTimer);
         accessEnablerObject.setSelectedProvider(null);
         accessEnablerObject.getAuthentication();
         $('#mvpddiv').hide();
      }
   } catch (error) {
      console.log(error);
   }
}
```

>[!IMPORTANT]
>
>* 샘플 코드에는 사용된 requestorID - &#39;REF&#39;에 대해 실제 프로그래머 요청자 ID로 대체해야 하는 하드코딩된 변수가 포함되어 있습니다.
>* 샘플 코드는 사용된 요청자 ID와 연결된 화이트리스트에 있는 도메인에서만 제대로 실행됩니다.
>* 전체 코드를 다운로드할 수 있으므로 이 기술 노트에 표시된 코드는 잘렸습니다. 전체 샘플을 보려면 **JS iFrame과 Popup 샘플**&#x200B;을 참조하십시오.
>* 외부 JavaScript 라이브러리가 [Google Hosted Services](https://developers.google.com/speed/libraries/)에서 연결되었습니다.
