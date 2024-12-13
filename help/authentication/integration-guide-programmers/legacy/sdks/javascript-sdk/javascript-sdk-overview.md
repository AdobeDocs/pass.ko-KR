---
title: JavaScript SDK 개요
description: JavaScript SDK 개요
exl-id: 8756c804-a4c1-4ee3-b2b9-be45f38bdf94
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 0%

---

# (기존) JavaScript SDK 개요 {#javascript-sdk-overview}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 소개

Adobe은 AccessEnabler 라이브러리의 최신 JS v4.x로 마이그레이션하는 것이 좋습니다.

Adobe Pass 인증 JavaScript 통합은 익숙한 JS 웹 애플리케이션 개발 환경에서 프로그래머에게 TV-Everywhere 솔루션을 제공합니다. 통합의 주요 구성 요소는 &quot;높은 수준의&quot; 애플리케이션(사용자 상호 작용, 비디오 프레젠테이션)과 자격 흐름에 대한 항목을 제공하고 Adobe Pass 인증 서버와의 통신을 처리하는 Adobe 제공 &quot;낮은 수준의&quot; AccessEnabler 라이브러리입니다.

일반 Adobe Pass 인증 권한 흐름은 [프로그래머 권한 흐름](/help/authentication/integration-guide-programmers/entitlement-flow.md)에 포함되어 있으며 JavaScript 통합 Cookbook은 구현을 안내합니다. 다음 섹션에서는 JavaScript AccessEnabler 통합에 대한 설명과 샘플을 제공합니다.

>[!IMPORTANT]
>
>이 문서에서는 데스크탑 웹 솔루션의 구현에 대해 설명합니다. JavaScript 라이브러리는 모바일 플랫폼(예: iOS의 Safari, Android의 Chrome)에서 지원되지 않습니다. 모바일 플랫폼(iOS, Android, Windows)을 타깃팅하려면 기본 SDK를 사용하십시오.

## MVPD 선택 대화 상자 만들기 {#creating-the-mvpd-selection-dialog}

사용자가 MVPD에 로그인하여 인증되려면 페이지나 플레이어에서 사용자가 자신의 MVPD을 식별할 수 있는 방법을 제공해야 합니다. 개발을 위해 MVPD 선택 대화 상자의 기본 버전이 제공됩니다. 프로덕션을 사용하려면 자체 MVPD 선택기를 구현해야 합니다.

고객의 공급자가 누구인지 이미 알고 있는 경우 사용자 상호 작용 없이 [프로그래밍 방식으로 MVPD을 설정](/help/authentication/home.md)할 수 있습니다. 이 기술은 동일하지만 공급자 선택기 대화 상자를 호출하고 고객에게 MVPD을 선택하라고 요청하는 단계를 거치지 않습니다.

## 서비스 공급자 표시 {#displaying-the-service-provider}

다음 코드 샘플은 현재 고객의 서비스 공급자를 검색하고 표시하는 방법을 보여 줍니다.

**HTML** - 고객이 선택한 공급자가 이미 로그인한 경우 이 페이지는 해당 공급자를 표시하는 페이지에 섹션을 추가합니다.

```HTML
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
            "http://www.w3.org/TR/html4/loose.dtd"> 
    <html>
    <head>
        <title>Get the distributor that is used for the authentication</title>
        <script type="text/javascript" src="libs/jquery-1.4.4.min.js"></script>
        <script type="text/javascript" src="libs/swfobject.js"></script>
        <script type="text/javascript" src="scripts/sample6.js"></script>
    </head>
    <body>
        <div id="alternative">
        <a href="http://www.adobe.com/go/getflashplayer"> 
            <img src="http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif" 
                 alt="Get Adobe Flash player"/> </a>
        </div> 
        <div id="user_authenticated">Please wait while we verify your authentication status</div> 
        <div id="control">
        <button id="sign_in" style="display:none;">Sign in</button>
        <button id="sign_out" style="display:none;">Sign out</button>
        </div> 
        <div id="distributor"></div> 
        <div id="provider_selector" style="display:none;">
        <select id="provider_list"></select>
        <button id="login">Login</button>
        <button id="cancel">Cancel</button>
        </div> 
        <div id="video_player">This is a video player showing an image as a preview.  You are not yet authorized to see this content.  Make sure that you first sign in.
        </div> 
        <div id="get_authorization">
        <button id="request_access" style="display:none;">Request access</button>
        </div> 
    </body>
    </html>
```


**JavaScript** 이 JavaScript 파일은 사용자가 이미 로그인한 경우 현재 공급자에 대한 Access Enabler를 쿼리하고 예약된 페이지 섹션에 결과를 표시합니다. 또한 MVPD 선택기 대화 상자를 구현합니다.

```JS
    $(function() {
        embedAE();
    }); 
    function embedAE() {
        var flashvars = {};
        var params = {
            bgcolor: "#869ca7",
            quality: "high",
            allowscriptaccess: "always"
        };
        var attributes = {
            id: "ae_swf",
            align: "middle",
            name: "ae_swf"
        };
        swfobject.embedSWF(
                "https://entitlement.auth-staging.adobe.com/entitlement/AccessEnabler.swf",      
                "alternative", "1", "1", "10.1.53", "", flashvars, params, attributes);
    } 
    function getAE() {
        return document.getElementById("ae_swf");
    } 
    function swfLoaded() {
        var swf = getAE();
        initBindings();
        // don't load the default provider-selection dialog 
        swf.setProviderDialogURL("none");
        // identify yourself 
        swf.setRequestor("sample_requestor_Id");
        swf.checkAuthentication();
    } 
    // Define the button actions for the provider dialog
    function initBindings() {
        var provider_selector = $('#provider_selector');
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var login = $('#login');
        var cancel = $('#cancel');
        var request_access = $('#request_access'); 
        sign_out.click(function() {
            getAE().logout();
        });
        sign_in.click(function() {
            getAE().getAuthentication();
        }); 
        login.click(function() {
            var selected_mvpd_id = $("#provider_list option:selected").val();
            getAE().setSelectedProvider(selected_mvpd_id);
        }); 
        cancel.click(function() {
            getAE().setSelectedProvider(null);
            provider_selector.hide();
        }); 
        request_access.click(function(){
            getAE().getAuthorization("sample_requestor_Id");
        });
    }
    // Called if user is already authenticated; queries AE to find the current user's MVPD, 
    // Adds the result to the UI 
    function setDistributor() {
        var distributor = $("#distributor");
        var selectedProvider = getAE().getSelectedProvider();
        distributor.replaceWith('<div id="distributor">' +
                                'Courtesy of ' +
                                selectedProvider.MVPD +
                                '</div>');
    } 
    // Adjust the contents of the provider dialog, based on authentication status 
    // Adds the call to check and display the current distributor, if the user is already
    // logged in. 
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        var user_authenticated = $("#user_authenticated");
        var video_player = $("#video_player");
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var request_access = $('#request_access'); 
        if (isAuthenticated == 1) {
            setDistributor();
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are authenticated - if you wish you can sign out
                                           </div>');
            sign_out.show();
            sign_in.hide();
            video_player.replaceWith('<div id="video_player">Now you can ask for access.</div>');
            request_access.show();
        } else {
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are not authenticated please sign in
                                           </div>');
            sign_in.show();
            sign_out.hide();
        }
    } 
    // Allow user to choose a provider 
    function displayProviderDialog(providers) {
        var provider_list = $("#provider_list");
        var options = provider_list.attr('options');
        for (var index in providers) {
            options[index] = new Option(providers[index].displayName,
                                        providers[index].ID);
        }
        //select the first one by default 
        if (!$("#provider_list option:selected").length)
            $("#provider_list option[0]").attr('selected', 'selected'); 
        $('#provider_selector').show();
    } 
    // Handle the authorization response by sending token to video player 
    function setToken(requested_resource_id, token) {
        var video_player = $("#video_player");
        var request_access = $("#request_access");
        video_player.replaceWith('<div id="video_player">Now you are authorized to ' +
                                 requested_resource_id +
                                 ' content with ' + token + ' . </div>');
    }
```

## 로그아웃 {#logout}

로그아웃 프로세스를 시작하려면 `logout()`을(를) 호출하십시오. 이 메서드는 인수를 사용하지 않습니다. 현재 사용자를 로그아웃하여 해당 사용자에 대한 모든 인증 및 권한 부여 정보를 지우고 로컬 시스템에서 모든 AuthN 및 AuthZ 토큰을 삭제합니다.

플레이어가 사용자 로그아웃을 처리할 책임이 없는 경우가 있습니다.



- **Adobe Pass 인증과 통합되지 않은 사이트에서 로그아웃이 시작되면** 이 경우 MVPD은 브라우저 리디렉션을 통해 Adobe Pass 인증 단일 로그아웃 서비스를 호출할 수 있습니다. (백채널 호출을 통한 SLO 호출은 현재 지원되지 않습니다.)

>[!NOTE]
>
>토큰이 만료될 때까지 사용자가 컴퓨터를 유휴 상태로 두면 여전히 세션으로 돌아가서 로그아웃을 성공적으로 시작할 수 있습니다. Adobe Pass 인증은 모든 토큰이 삭제되도록 하며 MVPD에 해당 세션을 삭제하도록 알립니다.

다음 JavaScript 코드는 현재 인증된 사용자를 로그아웃(비인증)하는 방법을 보여 줍니다.

```JS
    [...]
    /*
     * @param isAuthenticated authentication status: 1 - Authenticated, 0 - not authenticated 
     * @param errorCode Any error that occurred when determining the authentication status.
     * An empty string if no error occurred.
     */
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        if (isAuthenticated == 1 ) {
            /* User is authenticated – we can logout / deauthenticate */
            accessEnablerObject.logout();
            /* Logs out the current user, clearing all authentication
             * and authorization information for that user. Deletes
             * all authN and authZ tokens from the user's system.
             */
        } else {
            /*
             * User is NOT authenticated – we do not need to logout;
             * Show the log-in image/button/message
             */
        }
    }
```

<!--
**Related Information**

- [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
- [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
- [JavaScript Code Samples](#javascript-code-samples)
- [Understanding Tokens](#understanding_tokens)
-->
