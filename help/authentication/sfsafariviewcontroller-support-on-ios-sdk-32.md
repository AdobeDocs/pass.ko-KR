---
title: iOS SDK 3.2+에서 SFSafariViewController 지원
description: iOS SDK 3.2+에서 SFSafariViewController 지원
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# iOS SDK 3.2+에서 SFSafariViewController 지원 {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>


**보안 요구 사항으로 인해 일부 MVPD의 로그인 페이지는 웹 보기가 아닌 SFSafariViewController에 표시되어야 합니다.**

일부 MVPD의 경우 로그인 페이지가 SFSafariViewController와 같은 보안 브라우저 컨트롤에 표시되어야 합니다. 웹 보기를 적극적으로 차단하므로 웹 보기를 인증하려면 SVC를 사용해야 합니다.

## 호환성 {#compatiblity}

iOS SDK 버전 3.1부터 AccessEnabler SDK는 서버 구성에 따라 SFSafariViewController의 특정 MVPD에 대한 로그인 페이지를 자동으로 표시합니다.

SDK 버전 3.1은 애플리케이션의 루트 보기 컨트롤러에서 SFSafariViewController를 자동으로 표시합니다. 이렇게 하면 구현자의 로그인 페이지 관리가 간소화되지만, 앱의 특정 구현(예: 이미 표시되는 모달 컨트롤러)으로 인해 루트 보기 컨트롤러에서 SFSafariViewController를 제공할 수 없는 경우가 있습니다.

이러한 경우, 3.2 버전은 프로그래머가 SVC를 수동으로 관리할 수 있는 기능을 도입했다.

## 수동 SVC 관리 {#manual-svc-management}

SVC 를 수동으로 관리하려면 구현자가 다음 단계를 수행해야 합니다.


1. 호출 **setOptions([&quot;handleSVC&quot;:true])** AccessEnabler 초기화 후(인증이 시작되기 전에 이 호출이 수행되었는지 확인). 이렇게 하면 &quot;수동&quot; SVC 관리가 가능해집니다. SDK는 SVC를 자동으로 표시하지 않습니다. 대신 필요한 경우 를 호출합니다. **navigate(toUrl:*{url}* useSVC:true)**.

1. 선택적 콜백 구현 **navigateToUrl:useSVC:** 구현 내에서 제공된 url을 사용하여 SFSafariViewController 인스턴스를 사용하여 svc 인스턴스를 만들고 화면에 표시해야 합니다.

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***참고:***

   - *원하는 방식으로 SFSafaariViewController를 사용자 지정할 수 있습니다. 예를 들어 iOS 11+에서 &quot;완료&quot; 레이블을 &quot;취소&quot;로 변경할 수 있습니다.*
   - *svc를 해지하려면 이 문서를 참조해야 합니다.**navigateToUrl:useSVC***
   - *&quot;myController&quot;에 고유한 보기 컨트롤러 사용*


1. 애플리케이션의 위임 구현 **application(\_app: UIApplication, open url: URL, options: \[UIApplicationOpenURLOptionsKey: Any\]) -\> 부울**&#x200B;를 클릭하고 코드를 추가하여 svc를 닫습니다. 이미 호출한 코드가 있을 것입니다. **accessEnabler.handleExternalURL()**. 바로 아래에 추가:

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   다시 말하지만, svc 는 2단계에서 생성한 SFSafariViewController에 대한 참조입니다.


1. 구현 **safariViewControllerDidFinish(\_ controller: SFSafariViewController)** 출처: **SFSafariViewControllerDelegate** 사용자가 &quot;완료&quot; 단추를 사용하여 svc를 취소한 시점을 추적하기 위해 이 함수에서 인증이 취소되었음을 SDK에 알리려면 다음을 호출해야 합니다.

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
