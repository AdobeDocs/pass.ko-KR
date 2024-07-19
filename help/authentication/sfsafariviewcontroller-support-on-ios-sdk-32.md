---
title: iOS SDK 3.2+에서 SFSafariViewController 지원
description: iOS SDK 3.2+에서 SFSafariViewController 지원
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# iOS SDK 3.2 이상 {#sfsafariviewcontroller-support-on-ios-sdk-3.2}에서 SFSafariViewController 지원

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

</br>


**보안 요구 사항으로 인해 일부 MVPD의 로그인 페이지는 웹 보기 대신 SFSafariViewController에 표시되어야 합니다.**

일부 MVPD의 경우 로그인 페이지가 SFSafariViewController와 같은 보안 브라우저 컨트롤에 표시되어야 합니다. 웹 보기를 적극적으로 차단하므로 웹 보기를 인증하려면 SVC를 사용해야 합니다.

## 호환성 {#compatiblity}

iOS SDK 버전 3.1부터 AccessEnabler SDK는 서버 구성에 따라 SFSafariViewController의 특정 MVPD에 대한 로그인 페이지를 자동으로 표시합니다.

SDK 버전 3.1은 애플리케이션의 루트 보기 컨트롤러에서 SFSafariViewController를 자동으로 표시합니다. 이렇게 하면 구현자의 로그인 페이지 관리가 간소화되지만, 앱의 특정 구현(예: 이미 표시되는 모달 컨트롤러)으로 인해 루트 보기 컨트롤러에서 SFSafariViewController를 제공할 수 없는 경우가 있습니다.

이러한 경우, 3.2 버전은 프로그래머가 SVC를 수동으로 관리할 수 있는 기능을 도입했다.

## 수동 SVC 관리 {#manual-svc-management}

SVC 를 수동으로 관리하려면 구현자가 다음 단계를 수행해야 합니다.


1. AccessEnabler 초기화 후 **setOptions([&quot;handleSVC&quot;:true])**&#x200B;을(를) 호출합니다(인증이 시작되기 전에 이 호출이 수행되었는지 확인). 이렇게 하면 &quot;수동&quot; SVC 관리가 가능해집니다. SDK는 SVC를 자동으로 표시하지 않습니다. 대신 필요한 경우 를 표시합니다.     **navigate(toUrl:*{url}* useSVC:true)**&#x200B;를 호출합니다.

1. 구현 내에서 선택적 콜백 **`navigateToUrl:useSVC:`**&#x200B;을(를) 구현하려면 제공된 url을 사용하여 SFSafaariViewController 인스턴스를 사용하여 svc 인스턴스를 만들고 화면에 표시해야 합니다.

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***메모:***

   - *원하는 대로 SFSafaariViewController를 사용자 지정할 수 있습니다. 예를 들어 iOS 11+에서 &quot;완료&quot; 레이블을 &quot;취소&quot;로 변경할 수 있습니다.*
   - *svc를 무시하려면 이 참조를 사용해야 합니다.**navigateToUrl:useSVC***의 범위에서 만들지 마십시오.
   - *myController에 고유한 보기 컨트롤러를 사용합니다*


1. 응용 프로그램의 **application(\_app: UIApplication 위임 구현에서 URL: URL, 옵션: \[UIApplicationOpenURLOptionsKey: Any\]) -\> Bool**&#x200B;을(를) 열고 svc를 닫는 코드를 추가하십시오. **accessEnabler.handleExternalURL()**&#x200B;을 호출하는 일부 코드가 있어야 합니다. 바로 아래에 추가:

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   다시 말하지만, svc 는 2단계에서 생성한 SFSafariViewController에 대한 참조입니다.


1. 사용자가 &quot;완료&quot; 단추를 사용하여 svc를 취소한 경우 이를 확인하기 위해 **SFSafariViewControllerDelegate**&#x200B;에서 **safariViewControllerDidFinish(\_ controller: SFSafariViewController)**&#x200B;을(를) 구현합니다. 이 함수에서 인증이 취소되었음을 SDK에 알리려면 다음을 호출해야 합니다.

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
