---
title: Account IQ 용어
description: 제품 용어집입니다.
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 0%

---

# 제품 개념 및 용어집 {#glossary}

## D2C 및 TV Everywhere의 공통 용어

다음 제품 용어 및 해당 정의는 모든 [버전의 Account IQ](versions-aiq.md)에 공통됩니다.

### [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

현재 세그먼트의 공유 점수를 매우 낮음, 낮음, 중간, 높음 및 매우 높음의 공유 범위 카테고리로 나누는 차트가 있는 대시보드 패널입니다.

### [!UICONTROL Action] {#action-def}

관련 작업 세그먼트의 특성(예: 점수 또는 사용 중인 장치 수 공유)에 영향을 주는 [Operation](#operation-def)과(와) 관련된 직접 또는 간접 이벤트입니다.

### [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

현재 세그먼트의 공유 점수를 매우 낮음, 낮음, 중간, 높음 및 매우 높음의 공유 범위 범주와 함께 각 범주 백분율로 나누는 차트가 있는 대시보드 패널입니다.

### [!UICONTROL AuthN] {#authn-def}

인증 시도 횟수입니다. 인증 시도는 사용자가 D2C 서비스 또는 MVPD로 로그인을 시도하는 프로세스입니다. TV Everywhere 사용자의 경우 사용자가 선택한 MVPD로 리디렉션되며, 여기서 사용자는 일반적으로 사용자 이름과 암호를 사용하여 MVPD로 식별됩니다.

### [!UICONTROL AuthN OK] {#authn-ok-def}

성공한 인증 횟수입니다. 성공적인 인증은 사용자의 식별이 D2C 서비스 또는 MVPD에 의해 확인될 때 발생한다. TV Everywhere 사용자의 경우 이렇게 하면 사용자가 프로그래머 앱 또는 사이트로 다시 리디렉션됩니다.

### [!UICONTROL Cluster] {#cluster-def}

클러스터는 위치와 장치의 컬렉션입니다. 클러스터는 장치 간에 공통적인 위치를 찾아 만들어집니다. 공통 위치에서 본 디바이스는 동일한 클러스터에 속하는 것으로 간주될 것이다. 두 개의 장치가 공통 위치가 없어도 동일한 클러스터에 있을 수 있지만 다른 장치의 위치를 통해 연결할 수 있습니다.

#### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

정적 장치가 없는 클러스터입니다.

#### [!UICONTROL Static cluster] {#static-cluster-def}

하나 이상의 정적 장치가 있는 클러스터입니다.

### [!UICONTROL Concurrency] {#consurrency-def}

동시 실행은 동일한 시간에 재생되거나 매우 가까운 시간에 재생되는 두 개(또는 그 이상) 스트림으로 정의되므로 정상적인 속도로 주행함으로써 이들 스트림 사이의 간격을 정당화할 수 없습니다.
동시 사용량은 서로 다른 두 클러스터 간의 최대 속도(마일/시간)를 사용하여 계산됩니다. 사용자가 124마일 미만의 거리에서 124m/h보다 큰 속도를 가지거나 124마일 이상의 거리에서 400m/h보다 큰 속도를 가지는 경우 동시 사용으로 간주됩니다. 이 거리는 서로 다른 클러스터의 위치 간에 계산됩니다. 동일한 클러스터에서 동시 사용이 허용됩니다.

### [!UICONTROL Device] {#device-def}

업스트리밍 콘텐츠 재생이 가능한 디지털 비디오 하드웨어 제품. 예를 들어, 스마트 폰, 노트북 및 데스크탑 컴퓨터, 게임 콘솔 및 스마트 텔레비전이 있습니다.

### [!UICONTROL Evaluation period] {#evaluation-period-def}

평가 기간은 작업과 관련된 작업의 시작부터 작업 또는 측정이 끝날 때까지의 시간입니다.

### [!UICONTROL Geographical Span] {#geographical-span-def}

위치 세트에서 가장 먼 점 사이의 거리입니다.

### [!UICONTROL Granularity] {#granularity-def}

시간 간격을 기준으로 한 주 또는 한 달과 같은 기간의 크기입니다.

### [!UICONTROL IP] {#ip-def}

인터넷 서비스 공급자가 장치에 할당한 인터넷 프로토콜 주소입니다. 케이블 서비스 제공업체, 셀 서비스 제공업체 등이 이에 해당합니다.

### [!UICONTROL Location] {#location-def}

지구상의 독특한 점. 1000m x 1000m(1평방 km)의 정밀도로 특정 재생 요청에 대한 지리적 위치라고도 합니다.

### [!UICONTROL Media Company] {#media-company-def}

미디어 회사는 미디어 네트워크 그룹을 소유한 회사입니다.

### [!UICONTROL Metric] {#metric}

지표는 구독자 계정의 특성입니다(예: MVPD, 프로그래머 및 스트리밍하는 콘텐츠의 채널, 사용하는 디바이스의 수).

### [!UICONTROL Mobile device] {#mobile-device-def}

이동성이 높은 장치입니다. 예: 휴대폰 및 태블릿.

### 작업 {#operation-def}

작업은 연결된 세그먼트에서 특정 [action](#action-def)의 효과를 추적하기 위해 만들어진 레코드입니다. 작업의 예로는 세그먼트로 식별된 계정에 허용된 동시 스트림 수에 대한 제한이 있을 수 있습니다.

### [!UICONTROL Overall sharing score] {#overall-sharing-score}

사용자가 암호 공유의 크기를 이해하고 사용자에게 이에 대해 조치를 취할 수 있는 긴급성을 제공하는 값입니다.

### [!UICONTROL Play Request] {#play-requests-def}

스트림 시작과 동일합니다. 이 이벤트는 사용자 스트리밍 콘텐츠의 시작을 표시합니다.

### [!UICONTROL Risk Index-Usage] {#risk-index-usage}

공유 계정에서 사용이라고도 하며, 각 계정의 공유 확률에 의해 가중치가 부여된 각 계정에서 수행된 재생 요청 수를 기반으로 계산된 값입니다. 공유 계정별 사용 위험 지수라고도 합니다.

### [!UICONTROL Segment] {#segmet-def}

세그먼트는 선택한 지표에 의해 지정된 사용자 정의 조건을 충족하는 계정 세트입니다. 예를 들어 &quot;장치가 3개 이상인 지역 A, B, C, D 또는 E의 사용자&quot;가 여기에 해당합니다.

### [!UICONTROL Sharing level] {#sharing-level-def}

위험 지수 - 계정 또는 공유 계정 위험 지수라고도 하는 이 값은 선택한 시간 간격 동안 최소 한 번 이상 스트리밍된 현재 세그먼트의 모든 계정에 대해 계산된 공유 확률의 평균을 기반으로 계산된 값입니다.

### [!UICONTROL Static device] {#static-device-def}

이동성이 매우 낮은 장치입니다. 예를 들어, 게임 콘솔, 셋톱 박스, TV 세트 등이 있습니다.

### [!UICONTROL Time interval] {#time-interval-def}

마침표라고도 하는 이 시계는 사용자 인터페이스에 표시된 재생 요청 활동과 처음부터 끝까지 테이블을 포함하는 시간 창입니다.

### [!UICONTROL Trend] {#trend-def}

현재 기간과 이전 기간 사이의 관련 지표의 백분율 차이(예: 총 재생 요청의 백분율)입니다.

### [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

지정된 기간 동안 최소 한 번 이상 스트리밍된 고유한 계정의 수입니다.

### [!UICONTROL Usage] {#usage-defs}

#### [!UICONTROL Avid user] {#avid-user-def}

매월 37회 이상의 재생 요청.

#### [!UICONTROL Infrequent user] {#infrequent-users-def}

매월 9개 미만의 재생 요청.

#### [!UICONTROL Regular user] {#regular-user-def}

매월 9개에서 37개의 재생 요청.

### [!UICONTROL Usage Pattern] {#usage-patern-def}

소셜 그룹 또는 행동(예: 소규모 가족, 여행자 또는 통근자, 소셜 공유 등)과 관련하여 계정 사용자를 가장 잘 특징짓는 계정에 적용되는 유한 범주 레이블 세트 중 하나입니다.

### [!UICONTROL Video category] {#video-category-def}

#### [!UICONTROL For D2C service] {#d2c-service}

비디오 카테고리는 비디오의 특성을 규정하는 특정 레이블입니다. 예를 들어 소스의 영역, 콘텐츠 유형(예: VOD 또는 라이브), 이벤트 또는 특정 레이블(예: 팀) 등이 있습니다.

#### [!UICONTROL For TV Everywhere] {#tv-everywhere}

비디오 카테고리는 스트림과 연관된 프로그래머, 채널 및 MVPD의 조합에 의해 정의된다.

### [!UICONTROL Zip Code] {#zip-code-def}

미국 내 위치와 연관된 미국 우편 번호.
<!--calculated metrics-->


## TV Everywhere 특정 용어

### [!UICONTROL AuthZ] {#authz-def}

인증 또는 인증 요청 수입니다. 인증 요청은 프로그래머가 Adobe을 통해 MVPD에 권한을 요청하여 사용자가 요청한 콘텐츠를 스트리밍하는 프로세스입니다. MVPD는 일반적으로 사용자의 MVPD 가입과 연관된 콘텐츠 권한(예를 들어, 콘텐츠와 연관된 채널이 사용자의 가입에 있는지 여부)에 기초하여 요청을 허가한다. 일부 인증 요청 응답은 Adobe에 의해 캐시되므로 Adobe이 요청을 MVPD에 전달하지 않고 즉시 응답할 수 있습니다.

### [!UICONTROL AuthZ OK] {#authz-ok-def}

성공한 승인 수입니다.

### [!UICONTROL Channel] {#channel-def}

속성이라고도 하는 채널은 비디오 컨텐츠의 이론적으로 관련된 소스입니다. 전통적으로 MVPD로부터 개별적이고, 수치적으로 대응 가능한 연속 비디오 피드를 나타냅니다. 이 채널은 구독자가 STB(Set Top Box)를 통해 사용할 수 있는 액세스 가능한 콘텐츠 채널에 직접 매핑됩니다.

### [!UICONTROL Industry Average Index] {#industry-avg-index-def}

선택한 시간 간격 동안 모든 프로그래머 및 MVPD의 각 위험 지수(계정, 사용량, 전체)에 대해 계산된 값입니다.

### [!UICONTROL Isolation Mode] {#isolation-mode-def}

계정 평가가 선택한 세그먼트의 프로그래머에서 직접 발생한 이벤트로 제한되는 공유 분석 유형입니다.  일반적으로 모든 계정 이벤트가 평가되어 공유에 대한 보다 정확한 예상을 제공합니다.  일부 MVPD 데이터는 격리 모드 분석만 허용하는 방식으로 구성되어 있습니다.

### [!UICONTROL MVPD] {#mvpd-def}

MVPD는 Media Company 비디오 컨텐츠의 집계, 리셀러 및 배포자입니다.

### [!UICONTROL Programmer] {#programmer-def}

Network라고도 하는 프로그래머는 하나 이상의 채널을 소유하고 관리하는 더 큰 회사(법인)의 자회사인 회사이다.

### [!UICONTROL requestorID] {#requestorid-def}

미디어 회사가 자신이나 MVPD의 자회사를 식별하는 데 사용하는 ID입니다.  프로그래머 구현에 따라 미디어 회사, 프로그래머 또는 채널에 매핑될 수 있습니다. 일반적으로 이 폴더는 채널에 매핑됩니다.  MML(March Madness Live)과 같은 의사 채널을 만들고 MVPD 기반 데이터 제한을 해결하기 위한 기술 기반 작업으로 인해 requestorID는 미디어 회사와 점점 더 밀접해지고 있습니다.

### [!UICONTROL resourceID] {#resource-id-def}

최종 사용자가 요청한 콘텐츠. 일반적으로 이는 사용자가 요청한 콘텐츠와 연결된 채널을 식별했습니다.  시스템 개선을 통해 해당 ID는 특정 프로그램을 표현할 수 있지만(예: 특정 등급을 지정), 해당 ID는 연관된 채널을 계속 식별합니다.


