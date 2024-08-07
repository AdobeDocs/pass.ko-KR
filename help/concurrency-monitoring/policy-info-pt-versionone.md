---
title: 정책 정보 지점
description: 정책 정보 지점
exl-id: 964bb28d-cfef-4a37-b6c4-10cc59be0b47
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# 정책 정보 지점 {#pip}

>[!NOTE]
>
>이 페이지는 새 통합에 대해 더 이상 권장되지 않는 이전 버전의 API에 적용되므로 더 이상 사용되지 않습니다

다음 다이어그램은 고객이 **정책 정보 지점**&#x200B;을 선택하는 경우의 흐름을 보여 줍니다. 이 경우 CM은 활동 쿼리에 대해서만 사용되며 모든 액세스 논리는 클라이언트 응용 프로그램에 포함됩니다.

![](assets/pip-workflow.png)



아래 다이어그램은 2개의 디바이스에서 콘텐츠를 시청하는 사용자에게 스트림 카운트가 작동하는 방식을 보여 줍니다.

![](assets/pip-sequence.png)

간단히 말해, 일반적인 메시지 흐름은 다음과 같습니다.

1. 처음에 서비스를 사용한 적이 없는 사용자의 경우 웹 페이지가 로드되고 응용 프로그램이 열리며, 여기서 Concurrency Monitoring Service 계측 응용 프로그램은 세션 초기화 호출을 수행합니다.
1. 동시 모니터링 서비스는 하트비트에 대한 새 스트림 리소스와 현재 사용자 활동을 반환합니다.
1. 비디오 재생 중에 계측된 애플리케이션은 동시성 모니터링 서비스에 하트비트를 호출하여 사용자가 현재 비디오를 소비하고 있음을 보여 줍니다.
1. 다른 시점에서는 다른 계측된 응용 프로그램에서 동시 모니터링 서비스에 상태 쿼리를 호출하여 현재 사용자 활동을 반환할 수 있습니다.
1. 비디오 재생 종료 시 계측된 애플리케이션은 &quot;event=stop&quot;으로 하트비트 호출을 수행하여 비디오가 중지되었고 현재 스트림이 더 이상 활성 스트림으로 계산되지 않음을 나타낼 수 있습니다.
