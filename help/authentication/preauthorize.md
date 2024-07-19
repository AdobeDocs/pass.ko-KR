---
title: iOS/tvOS API 사전 인증
description: iOS/tvOS API 사전 인증
exl-id: 79c596a4-0e38-4b6c-bb85-f97c6af45ed8
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '391'
ht-degree: 0%

---

# 사전 승인 {#preauthorize}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

사전 권한 부여 API를 사용하여 하나 이상의 리소스에 대한 사전 권한 부여 결정을 얻을 수 있습니다. 이렇게 하면 애플리케이션이 UI 힌트 및/또는 콘텐츠 필터링을 구현할 수 있습니다.

>[!IMPORTANT]
>
>사용자에게 지정된 리소스에 대한 액세스 권한을 부여하기 전에 인증 API **must**&#x200B;을(를) 사용해야 합니다.

사전 권한 부여 API 응답 결과에 사전 권한 부여 거부 결정이 있는 하나 이상의 리소스가 포함된 경우 영향을 받는 각 리소스에 대해 추가 오류 정보가 **(아래 참고 참조)**&#x200B;에 포함될 수 있습니다.

>[!IMPORTANT]
>
>거부된 사전 권한 부여 결정에 대한 추가 오류 정보를 추가하는 향상된 오류 보고 기능은 Adobe Pass 인증 구성 측에서 활성화해야 하므로 요청 시 사용할 수 있습니다.

Adobe Pass 인증 SDK 오류로 인해 사전 승인 API 요청을 처리할 수 없거나 Adobe Pass 인증 서비스 오류가 발생하는 경우 추가 오류 정보(위의 구성과 상관없이)가 포함되며 사전 승인 API 응답 결과의 일부로 리소스가 포함되지 않습니다.

</br>

## `- (void) preauthorize:(nonnull PreauthorizeRequest *)request didCompleteWith:(nonnull AccessEnablerCallback<PreauthorizeResponse *> *)callback;`


**가용성:** v3.6.0+

**매개 변수:**

- PreauthorizeRequest: API 요청 콘텐츠를 전달하는 데 사용되는 요청 개체
- AccessEnablerCallback: API 응답을 반환하는 데 사용되는 콜백 개체
- PreauthorizeResponse: API 응답 콘텐츠를 반환하는 데 사용되는 응답 개체


</br>

## `class PreauthorizeRequest`{#androidpreauthorizerequest}

### **클래스 PreauthorizeRequest.Builder**

```
    ///
    /// Sets the `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// Each element in the list must be a `String` representing either the resource ID value or the media RSS fragment which must be agreed with the MVPD.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    ///
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter resources: The `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
    public func setResources(resources: [String]) -> PreauthorizeRequest.Builder

 

    ///
    /// Sets the features which you want to have them disabled when obtaining preauthorization decisions.
    ///
    /// The list of available features are provided by `PreauthorizeRequest.Feature` enumeration. All features are enabled by default.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    ///
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter features: The set of features which you want to have them disabled.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
    public func disableFeatures(features: Set<PreauthorizeRequest.Feature>) -> PreauthorizeRequest.Builder

 

    ///
    /// Creates and retrieves the reference of a new `PreauthorizeRequest` object instance.
    ///
    /// This function instantiates a new `PreauthorizeRequest` object every time it is called.
    ///
    /// This function uses the values set in advance in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// Bear in mind that this function does not produce any side effects, therefore it does not alter the state of the SDK or the state of the `Builder` object instance which is the receiver of this function call.
    ///
    /// It means that successive calls of this function for the same receiver will create different new `PreauthorizeRequest` object instances, but having the same information, in case the values set to the `Builder` where not modified between the calls.
    ///
    /// In case you do not need to update any of the provided information (resources, features, etc.) you may reuse the `PreauthorizeRequest` instance for multiple uses of the `preauthorize` API.
    ///
    /// - Returns: The reference to a new `PreauthorizeRequest` object instance.
    ///
    public func build() -> PreauthorizeRequest
```


## **enum PreauthorizeRequest.Feature**

```
    ///
    /// This feature controls whether to use the information from the AccessEnabler SDK cache or to bypass it and
    /// rely on Adobe Pass server information via a network call.
    ///
    LOCAL_CACHE

    ///
    /// This feature controls whether to use the information from the Adobe Pass server cache or to bypass it and
    /// rely on MVPD server information via a network call.
    ///
    REMOTE_CACHE
```

</br>

## `interface AccessEnablerCallback<PreauthorizeResponse>` {#accessenablercallback}

```
    /// Response callback called by the SDK when the preauthorize API request was fulfilled. The result is either a successful or an error result containing a status.
    public func onResponse(result: PreauthorizeResponse)


    /// Failure callback called by the SDK when the preauthorize API request could not be serviced. The result is a failure result containing a status. 
    public func onFailure(result: PreauthorizeResponse)
```

</br>


## `class PreauthorizeResponse` {#preauthorizeresponse}

```
    ///
    /// - Returns: Additional status (state) information in case of error or failure.
    ///   Might hold a `nil` value.
    ///
    public Status getStatus()

    ///
    /// - Returns: The list of preauthorization decisions. One decision for each resource.
    ///            The list might be empty in case of error or failure.
    ///
    public List<Decision> getDecisions()
```

### 예:

이 섹션에서는 사용 가능한 일부 PreauthorizeResponse 개체의 JSON 구조를 중점적으로 다룹니다.

>[!IMPORTANT]
>
>다음 예제에서 제공하는 JSON은 이 문서에 표시된 모델 클래스를 통해서만 액세스할 수 있습니다. 공개 메서드의 매체를 통하지 않으면 이러한 JSON의 속성에 액세스할 수 없습니다.

>[!IMPORTANT]
>
>향상된 오류 보고 기능을 통해 검색된 가능한 추가 오류 목록은 [고급 오류 보고](/help/authentication/enhanced-error-codes.md)에 설명되어 있습니다.

#### 성공

요청된 모든 리소스에 긍정적인 사전 승인 결정이 있습니다.

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": true 
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


하나 이상의 리소스에 거부된 사전 권한 부여 결정이 있으며 향상된 오류 보고 기능이 Adobe Pass 인증 구성에서 활성화되지 않았습니다

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": false,
                 
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


하나 이상의 리소스에 거부된 사전 권한 부여 결정이 있으며 Adobe Pass 인증 구성에서 향상된 오류 보고 기능이 활성화됩니다

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": false,
                "error" : {
                   "status" : 403,
                   "code" : "authorization_denied_by_mvpd",
                   "message" : "User not authorized",
                   "details" : "Your subscription package does not include the "TestStream3" channel.",
                   "helpUrl" : "https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                   "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
                   "action" : "none"
                }
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


#### 오류



Adobe Pass 인증 서비스가 API 사전 인증 요청을 제공하는 동안 오류가 발생했습니다.

```JSON
    {
        "resources": [],
        "status": {
            "status": 400,
            "code" : "bad_request",
            "message": "Missing required parameter : deviceId",
            "details": "",
            "helpUrl" : "https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "trace" : "9f115e1c-0158-4a41-8805-9f68923f3646",
            "action" : "none"
        }
    }
```


#### 실패

Adobe Pass 인증 SDK가 사전 인증 API 요청을 제공하는 동안 오류가 발생합니다

```JSON
    {
        "status": 0,
        "code": "requestor_not_configured",
        "message": "The requestor is not yet configured which is a prerequisite for using any API apart from the setRequestor API.",
        "action": "retry"
    }
    
    {
        "status": 0,
        "code": "authentication_session_expired",
        "message": "The current authentication session has expired. The user must re-authenticate with a supported MVPD in order to continue.",
        "action": "authentication"
    }
    
    {
        "status": 0,
        "code": "authentication_session_missing",
        "message": "The authentication session associated with this request could not be retrieved. The user must re-authenticate with a supported MVPD in order to continue.",
        "action": "authentication"
    }
    
    {
        "status": 0,
        "code": "access_token_unavailable",
        "message": "The request failed due to an unexpected error while retrieving the access token. Please check the validity of your software statement and custom scheme.",
        "action": "none"
    }
    
    {
        "status": 0,
        "code": "server_response_format_unknown",
        "message": "The request failed due to an unknown server response format. Please capture the device console logs and contact support.",
        "action": "none"
    }
    
    {
        "status": 0,
        "code": "network_error",
        "message": "The request failed due to a network error. If the issue persists over several retries, please capture the device console logs and contact support.",
        "action": "none"
    }
```

</br>

## **클래스 상태** {#status}

```
    ///
    /// - Returns: The HTTP response status code as documented in RFC 7231.
    ///            Might be 0 in case the `Status` comes from the SDK instead of Adobe Pass Authentication services.
    ///
    public int getStatus()

    ///
    /// - Returns: The standard Adobe Pass Authentication services error code.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getCode()

    ///
    /// - Returns: The human readable message which can be displayed to the end user.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getMessage()

    ///
    /// - Returns: The detailed message which in some cases is provided by the MVPD authorization endpoints or by Programmer degradation rules.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getDetails()

    ///
    /// - Returns: The URL that links to more information about why this state/error occurred and possible solutions.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getHelpUrl()

    ///
    /// - Returns: The unique identifier for this response, which can be used when contacting support to identify specific issues in more complex scenarios.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getTrace()

    ///
    /// - Returns: The recommended action to remediate the situation.
    ///             - none: Unfortunately there is no predefined action to remediate this issue. This might indicate an improper invocation of the public API
    ///             - configuration: A configuration change is needed through TVE dashboard or by contacting support.
    ///             - application-registration: The application must register itself again.
    ///             - authentication: The user must authenticate or re-authenticate.
    ///             - authorization: The user must obtain authorization for the specific resource.
    ///             - degradation: Some form of degradation should be applied.
    ///             - retry: Retrying the request might solve the issue
    ///             - retry-after: Retrying the request after the indicated period of time might solve the issue.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getAction()
```

<br>

## **클래스 결정** {#decision}

```
    ///
    /// This is a getter function.
    ///
    /// - Returns: The resource id for which the decision was obtained.
    ///
    public Status getId()

    ///
    /// This is a getter function.
    ///
    /// - Returns: The value of the flag indicating if the decision is successful or not.
    ///
    public boolean isAuthorized()

    ///
    /// This is a getter function.
    ///
    /// - Returns: Additional status (state) information in case some error has occurred.
    ///            Might hold a `nil` value.
    ///
    public Status getError()
```

</br>


## **샘플 코드** {#sample}

```
let resources: [String] = ["resource_1", "resource_2", "resource_3"];

let disabledFeatures: Set<PreauthorizationRequest.Feature> = [PreauthorizationRequest.Feature.LOCAL_CACHE];

// Build the Preauthorization API request using the builder from PreauthorizationRequest class`

let request: PreauthorizationRequest = PreauthorizationRequest.Builder()

                  .setResources(resources: resources)


                  .disableFeatures(features: disabledFeatures)  // It is **optional** to disable features. If not used all features are enabled by default.

                  .build();

// Build the AccessEnablerCallback by providing the constructor two callbacks for onResponse and onFailure handling  
func onResponseCallback(result: PreauthorizeResponse) -> Void {  //
TODO };

func onFailureCallback(result: PreauthorizeResponse) -> Void {

// TODO

};

let callback: AccessEnablerCallback<PreauthorizeResponse> = AccessEnablerCallback<PreauthorizeResponse>(onResponse: onResponseCallback, onFailure: onFailureCallback);

// Use the preauthorize API
accessEnabler.preauthorize(request, callback);
```
