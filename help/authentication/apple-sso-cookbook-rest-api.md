---
title: Apple SSO Cookbook (REST API)
description: Apple SSO Cookbook (REST API)
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1435'
ht-degree: 0%

---

# Apple SSO Cookbook (REST API) {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 소개 {#Introduction}

Adobe Pass 인증 REST API는 Apple SSO 워크플로라고 하는 기능을 통해 iOS, iPadOS 또는 tvOS에서 실행되는 클라이언트 애플리케이션의 최종 사용자를 위한 플랫폼 SSO(Single Sign-On) 인증을 지원할 수 있습니다.

이 문서는 기존 REST API 설명서에 대한 확장 역할을 하며, 해당 설명서는 다음과 같습니다 [여기](/help/authentication/rest-api-reference.md).

## 요리책 {#Cookbooks}

Apple SSO 사용자 경험을 활용하려면 애플리케이션 하나가 [비디오 구독자 계정](https://developer.apple.com/documentation/videosubscriberaccount) Apple에서 개발한 프레임워크이며 Adobe Pass 인증 REST API 통신에 대해서는 아래에 제시된 팁 시퀀스를 따라야 할 것입니다.

### 인증 {#Authentication}

- [유효한 Adobe 인증 토큰이 있습니까?](#Is_there_a_valid_Adobe_authentication_token)
- [사용자가 Platform SSO를 통해 로그인되어 있습니까?](#Is_the_user_logged_in_via_Platform_SSO)
- [Adobe 구성 가져오기](#Fetch_Adobe_configuration)
- [Adobe 구성으로 Platform SSO 워크플로 시작](#Initiate_Platform_SSO_workflow_with_Adobe_config)
- [사용자 로그인에 성공했습니까?](#Is_user_login_successful)
- [Adobe에서 선택한 MVPD에 대한 프로필 요청 얻기](#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD)
- [프로필을 가져오려면 Adobe 요청을 Platform SSO로 전달하십시오.](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile)
- [Platform SSO 프로필을 Adobe 인증 토큰으로 교환](#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token)
- [Adobe 토큰이 생성되었습니까?](#Is_Adobe_token_generated_successfully)
- [두 번째 화면 인증 워크플로 시작](#Initiate_second_screen_authentication_workflow)
- [인증 흐름 진행](#Proceed_with_authorization_flows)


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/qu/platform-sso.jpeg)


#### 단계: &quot;유효한 Adobe 인증 토큰이 있습니까?&quot; {#Is_there_a_valid_Adobe_authentication_token}

>[!TIP]
>
> **<u>팁:</u>** 을(를) 통해 구현 [Adobe Pass 인증](/help/authentication/check-authentication-token.md) 서비스.


#### 단계: &quot;사용자가 Platform SSO를 통해 로그인했습니까?&quot; {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>팁:</u>** 을(를) 통해 구현 [비디오 구독자 계정](https://developer.apple.com/documentation/videosubscriberaccount) 프레임워크.

- 애플리케이션이 다음을 확인해야 합니다. [액세스 권한](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) 사용자의 구독 정보를 참조하고 사용자가 이를 허용한 경우에만 계속합니다.
- 지원서는 다음을 제출해야 합니다 [요청](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) 구독자 계정 정보.
- 응용 프로그램은 대기하고 다음을 처리해야 합니다. [메타데이터](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 정보.



>[!TIP]
>
> **<u>Pro 팁:</u>** 코드 스니펫을 따라 댓글에 주의를 기울입니다.

```swift
...
let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();

videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
            switch (accessStatus) {
            // The user allows the application to access subscription information.
            case VSAccountAccessStatus.granted:
                    // Construct the request for subscriber account information.
                    let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();

                    // This is actually the SAML Issuer not the channel ID.
                    vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAccountProviderIdentifier = true;
                    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                    
                    // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
                    vsaMetadataRequest.isInterruptionAllowed = false;
                    
                    // Submit the request for subscriber account information - accountProviderIdentifier.
                    videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in        
                        if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                            // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                            // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                            ...
                            // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                            // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                            ...
                            // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                            // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                            ...
                            // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                            ...
                            // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Fallback to regular authentication workflow.
                ...
            }
}
...  
```

#### 단계: &quot;Adobe 구성 가져오기&quot; {#Fetch_Adobe_configuration}

>[!TIP]
>
> **<u>팁:</u>** 을(를) 통해 구현 [Adobe Pass 인증](/help/authentication/provide-mvpd-list.md) 서비스.


>[!TIP]
>
> **<u>Pro 팁:</u>** MVPD 속성에 대해 알아보십시오. *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* 또한 다른 단계에서 코드 조각에 표시되는 주석에 각별히 주의하십시오.

#### &quot;Adobe 구성으로 Platform SSO 워크플로 시작&quot; 단계 {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>팁:</u>** 을(를) 통해 구현 [비디오 구독자 계정](https://developer.apple.com/documentation/videosubscriberaccount) 프레임워크.

- 애플리케이션이 다음을 확인해야 합니다. [액세스 권한](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) 사용자의 구독 정보를 참조하고 사용자가 이를 허용한 경우에만 계속합니다.
- 응용 프로그램에서 다음을 제공해야 합니다. [위임](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) VSAccountManager용
- 지원서는 다음을 제출해야 합니다 [요청](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) 구독자 계정 정보.
- 응용 프로그램은 대기하고 다음을 처리해야 합니다. [메타데이터](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 정보.



>[!TIP]
>
> **<u>Pro 팁:</u>** 코드 스니펫을 따라 댓글에 주의를 기울입니다.


```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    // This must be a class implementing the VSAccountManagerDelegate protocol.
    let videoSubscriberAccountManagerDelegate: VideoSubscriberAccountManagerDelegate = VideoSubscriberAccountManagerDelegate();
    
    videoSubscriberAccountManager.delegate = videoSubscriberAccountManagerDelegate;
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account framework to prompt the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = true;
                        
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
    
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
                        
                        // Submit the request for subscriber account information - accountProviderIdentifier.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in                        
                            // This represents the checks for the "Is user login successful?" step.
                            if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                                // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                                // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                                ...
                                // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                                // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                                ...
                                // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                                // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                                ...
                                // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                                ...
                                // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
    
                                        // The requestor.mvpds must be computed during the "Fetch Adobe configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
                                        
                                        if mvpd != nil {
                                            // Continue with the "Initiate second screen authentcation workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate second screen authentcation workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate second screen authentcation workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate second screen authentcation workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### 단계: &quot;사용자 로그인이 성공했습니까?&quot; {#Is_user_login_successful}

>[!TIP]
>
> **<u>Pro 팁:</u>** 의 코드 스니펫에 유의하십시오. [&quot;Adobe 구성으로 Platform SSO 워크플로 시작&quot;](#Initiate_Platform_SSO_workflow_with_Adobe_config) 단계. 다음의 경우 사용자 로그인이 성공했습니다. *`vsaMetadata!.accountProviderIdentifier`* 에는 유효한 값이 포함되어 있으며 현재 날짜는 을(를) 통과하지 못했습니다. *`vsaMetadata!.authenticationExpirationDate`* 값.

#### 단계 &quot;선택한 MVPD에 대한 Adobe에서 프로필 요청 얻기&quot; {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>팁:</u>** Adobe Pass 인증을 통해 구현 [프로필 요청](/help/authentication/retrieve-profilerequest.md) 서비스.

>[!TIP]
>
> **<u>Pro 팁:</u>** 비디오 구독자 계정 프레임워크에서 가져온 공급자 식별자는 *`platformMappingId`* Adobe Pass 인증 구성 측면입니다. 따라서 응용 프로그램은 다음을 사용하여 MVPD id 속성 값을 결정해야 합니다. *`platformMappingId`* 값, Adobe Pass 인증 매체 이용 [MVPD 목록 제공](/help/authentication/provide-mvpd-list.md) 서비스.

#### 단계: &quot;프로필을 가져오기 위해 Adobe 요청을 Platform SSO로 전달&quot; {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>팁:</u>** 을(를) 통해 구현 [비디오 구독자 계정](https://developer.apple.com/documentation/videosubscriberaccount) 프레임워크.


- 애플리케이션이 다음을 확인해야 합니다. [액세스 권한](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) 사용자의 구독 정보를 참조하고 사용자가 이를 허용한 경우에만 계속합니다.
- 지원서는 다음을 제출해야 합니다 [요청](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) 구독자 계정 정보.
- 응용 프로그램은 대기하고 다음을 처리해야 합니다. [메타데이터](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 정보.



>[!TIP]
>
> **<u>Pro 팁:</u>** 코드 스니펫을 따라 댓글에 주의를 기울입니다.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = false;
    
                        // This are the user metadata fields expected to be available on a successful login and are determined from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service. Look for the requiredMetadataFields associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = requiredMetadataFields;
    
                        // This is the payload from [Adobe Pass Authentication](/help/authentication/retrieve-profilerequest.md) service.
                        vsaMetadataRequest.verificationToken = profileRequestPayload;
                        
                        // Submit the request for subscriber account information.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in
                            if (vsaMetadata != nil && vsaMetadata!.samlAttributeQueryResponse != nil) {
                                var samlResponse: String? = vsaMetadata!.samlAttributeQueryResponse!;
                                
                                // Remove new lines, new tabs and spaces.
                                samlResponse = samlResponse?.replacingOccurrences(of: "[ \\t]+", with: " ", options: String.CompareOptions.regularExpression);
                                samlResponse = samlResponse?.components(separatedBy: CharacterSet.newlines).joined(separator: "");
                                samlResponse = samlResponse?.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines);
                                
                                // Base64 encode.
                                samlResponse = samlResponse?.data(using: .utf8)?.base64EncodedString(options: []);
                                
                                // URL encode. Please be aware not to double URL encode it further.
                                samlResponse = samlResponse?.addingPercentEncoding(withAllowedCharacters: CharacterSet.init(charactersIn: "!*'();:@&=+$,/?%#[]").inverted);
                                
                                // Continue with the "Exchange the Platform SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Fallback to regular authentication workflow.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### 단계: &quot;플랫폼 SSO 프로필을 Adobe 인증 토큰으로 교환&quot; {#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token}

>[!TIP]
>
> **<u>팁:</u>** Adobe Pass 인증을 통해 구현 [토큰 교환](/help/authentication/token-exchange.md) 서비스.


>[!TIP]
>
> **<u>Pro 팁:</u>** 의 코드 스니펫에 유의하십시오. [&quot;프로필을 가져오려면 Adobe 요청을 Platform SSO로 전달&quot;](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile) 단계. 이 *`vsaMetadata!.samlAttributeQueryResponse!`* 다음 표현: *`SAMLResponse`*, 전달해야 함 [토큰 교환](/help/authentication/token-exchange.md) 문자열 조작 및 인코딩이 필요합니다(*Base64* 인코딩되고 *URL* 호출 전 인코딩)을 참조하십시오.

</br>

#### 단계: &quot;Adobe 토큰이 생성되었습니까?&quot; {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>팁:</u>** Adobe Pass 인증 매체를 통해 구현 [토큰 교환](/help/authentication/token-exchange.md) 성공적인 응답, 다음 작업을 수행합니다. *`204 No Content`*: 토큰이 성공적으로 만들어졌으며 인증 흐름에 사용할 준비가 되었음을 나타냅니다.

</br>

#### 단계: &quot;두 번째 화면 인증 워크플로 시작&quot; {#Initiate_second_screen_authentication_workflow}

**중요 사항:** &quot;두 번째 화면 인증 워크플로&quot; 용어는 AppleTV에 적절하지만, &quot;첫 번째 화면 인증 워크플로&quot; / &quot;일반 인증 워크플로&quot; 용어는 iPhone 및 iPad에 더 적절합니다.


>[!TIP]
>
> **<u>팁:</u>** Adobe Pass 인증을 통해 구현

[등록 코드 요청](/help/authentication/registration-code-request.md), [인증 시작](/help/authentication/initiate-authentication.md) 및 [REST API 검색 인증 토큰](/help/authentication/retrieve-authentication-token.md) 또는 [인증 토큰 확인](/help/authentication/check-authentication-token.md) 서비스.


>[!TIP]
>
> **<u>Pro 팁:</u>** tvOS 구현에 대해 아래 단계를 따르십시오.

- 응용 프로그램은 다음을 수행해야 합니다. [등록 코드 받기](/help/authentication/registration-code-request.md) 및 를 첫 번째 장치(화면)에서 최종 사용자에게 제공합니다.
- 응용 프로그램을 시작해야 합니다. [인증 상태를 확인하는 폴링](/help/authentication/retrieve-authentication-token.md) 등록 코드를 받은 후 첫 번째 장치(화면)에서
- 다른 응용 프로그램은 다음을 수행해야 합니다. [인증 시작](/help/authentication/initiate-authentication.md) 두 번째 장치(화면)에서 등록 코드를 사용할 때
- 응용 프로그램을 중지해야 합니다. [폴링](/help/authentication/retrieve-authentication-token.md) 인증 토큰이 생성될 때 첫 번째 장치(화면)에서



>[!TIP]
>
> **<u>Pro 팁:</u>** iOS/iPadOS 구현에 대한 아래 절차를 따르십시오.

- 응용 프로그램은 다음을 수행해야 합니다. [등록 코드 받기](/help/authentication/registration-code-request.md) 첫 번째 장치(화면)에서는 최종 사용자에게 제시해서는 안 됩니다.
- 응용 프로그램은 다음을 수행해야 합니다. [인증 시작](/help/authentication/initiate-authentication.md) 등록 코드 및 를 사용하여 첫 번째 장치(화면)에서 [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) 또는 [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) 구성 요소.
- 응용 프로그램을 시작해야 합니다. [인증 상태를 알기 위해 폴링](/help/authentication/retrieve-authentication-token.md) 다음 시간 이후에 첫 번째 장치(화면)에서 [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) 또는 [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) 구성 요소가 닫힙니다.
- 응용 프로그램을 중지해야 합니다. [폴링](/help/authentication/retrieve-authentication-token.md) 인증 토큰이 생성될 때 첫 번째 장치(화면)에서

</br>

#### 단계: &quot;인증 흐름 진행&quot; {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>팁:</u>** Adobe Pass 인증을 통해 구현 [인증 시작](/help/authentication/initiate-authorization.md) 및 [짧은 미디어 토큰 가져오기](/help/authentication/obtain-short-media-token.md) 서비스.

</br>

### 로그아웃 {#Logout}

다음 [비디오 구독자 계정](https://developer.apple.com/documentation/videosubscriberaccount) 프레임워크는 장치 시스템 수준에서 TV 공급자 계정에 로그인한 사용자를 프로그래밍 방식으로 로그아웃시키는 API를 제공하지 않습니다. 따라서 로그아웃이 완전히 적용되려면 최종 사용자가에서 명시적으로 로그아웃해야 합니다 *`Settings -> TV Provider`* iOS/iPadOS에서 또는 *`Settings -> Accounts -> TV Provider`* tvOS에서 사용자가 가질 수 있는 다른 옵션은 특정 응용 프로그램 설정 섹션(TV 공급자 액세스)에서 사용자의 구독 정보에 액세스할 수 있는 권한을 철회하는 것입니다.

>[!TIP]
>
> **<u>팁:</u>** Adobe Pass 인증을 통해 구현 [사용자 메타데이터 호출](/help/authentication/user-metadata.md) 및 [로그아웃](/help/authentication/initiate-logout.md) 서비스.


>[!TIP]
>
> **<u>Pro 팁:</u>** tvOS 구현에 대해 아래 단계를 따르십시오.


- 애플리케이션은 &quot;&quot;을(를) 사용하여 플랫폼 SSO를 통해 로그인한 결과로서 인증이 발생했는지 여부를 확인해야 합니다&#x200B;*tokenSource&quot;* [사용자 메타데이터](/help/authentication/user-metadata.md) Adobe Pass 인증 서비스에서.
- 애플리케이션에서 사용자가에서 명시적으로 로그아웃하도록 지시/프롬프트를 표시해야 합니다. *`Settings -> Accounts -> TV Provider`* tvOS에서 **전용** 다음의 경우에 *&quot;tokenSource&quot;* 값이 &quot;*Apple&quot;.*
- 응용 프로그램은 다음을 수행해야 합니다. [로그아웃 시작](/help/authentication/initiate-logout.md) 직접 HTTP 호출을 사용하여 Adobe Pass 인증 서비스에서 이렇게 하면 MVPD 측의 세션 정리가 용이하지 않습니다.



>[!TIP]
>
> **<u>Pro 팁:</u>** iOS/iPadOS 구현에 대한 아래 절차를 따르십시오.

- 애플리케이션은 &quot;&quot;을(를) 사용하여 플랫폼 SSO를 통해 로그인한 결과로서 인증이 발생했는지 여부를 확인해야 합니다&#x200B;*tokenSource&quot;* [사용자 메타데이터](/help/authentication/user-metadata.md) Adobe Pass 인증 서비스에서.
- 애플리케이션에서 사용자가에서 명시적으로 로그아웃하도록 지시/프롬프트를 표시해야 합니다. *`Settings -> TV Provider`* iOS/iPadOS에서 **전용** 다음의 경우에 *&quot;tokenSource&quot;* 값이 다음과 같음 *&quot;Apple&quot;*.
- 응용 프로그램은 다음을 수행해야 합니다. [로그아웃 시작](/help/authentication/initiate-logout.md) 를 사용하여 Adobe Pass 인증 서비스에서 [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) 또는 [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) 구성 요소. 이렇게 하면 MVPD 측의 세션 정리가 용이해집니다.

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
