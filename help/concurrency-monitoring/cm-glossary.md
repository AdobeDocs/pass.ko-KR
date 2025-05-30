---
title: 용어집
description: 동시성 모니터링 용어 목록
exl-id: 3b3b36fe-9f04-4de9-bd84-9f8d766bbc71
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# 용어집 {#glossary}

## 계정 ID {#accid-defn}

* 가입자의 MVPD 계정(일반적으로 실제 청구 계정에 해당). 이 계정은 자체 시스템에서 MVPD가 식별할 수 있어야 합니다.

## 작업 {#action-defn}

* 주체가 요청하는 액세스 유형입니다. CM에 대해 가능한 값은 스트리밍 세션의 ***시작*** 또는 ***계속***&#x200B;입니다.

## 활성 스트림 {#active-stream-defn}

* 지난 90초 동안 1개 이상의 이벤트(하트비트)를 받은 스트림입니다.

* ***참고:*** 스트림의 마지막 이벤트가 중지(`?event=stop`) 유형인 경우 계산되지 않습니다. 더 이상 &quot;활성&quot;으로 간주되지 않도록 플레이어에서 스트림을 명시적으로 닫을 수 있는 최적화입니다.

## 애플리케이션 {#application-defn}

* 비디오 콘텐츠 액세스를 위해 테넌트가 개발
* 동시성 모니터링 서비스에서 제공한 정보를 기반으로 콘텐츠 액세스에 대한 결정을 내리고 적용합니다([정책 정보 지점](/help/concurrency-monitoring/policy-info-pt-versionone.md) 사례에서 유효).
* Adobe에서 제공한 고유한 **응용 프로그램 ID**&#x200B;이(가) 있습니다.

## 동시성 모니터링 서비스 {#cm-service-defn}

* 구독자를 위한 모니터링 시스템 역할을 하여 교차 애플리케이션 정책 시행 요구 사항에 MVPD 및 프로그래머를 지원합니다.
* 스트림 활동을 나타내는 하트비트를 받습니다.
* 사용자 활동을 기반으로 권한 부여 요청을 평가하고 허용/거부 응답을 제공하여 _정책 결정 지점_ 역할을 합니다.
* 구독자에 대한 활성 스트림 수(및 추가 스트림 메타데이터)를 보고하여 _정책 정보 지점_ 역할을 합니다.

## 환경 {#env-defn}

* 구성 설정 또는 시스템 시간과 같은 요청과 관련된 추가 정보입니다.

## MVPD {#mvpd-defn}

* 멀티채널 비디오 프로그래밍 배포자입니다.
* 인증 및 권한 부여 공급자 역할을 하지만 서비스 또는 콘텐츠 공급자일 수도 있습니다.

## 정책 {#policy-defn}

* 대상으로 정의된 CM의 코어 액세스 제어 개념과 고유한 이름으로 그룹화된 하나 이상의 규칙입니다.

## 정책 관리 지점(PAP) {#policy-admin-pt-defn}

* 액세스 권한 부여 정책을 관리하는 지점입니다. 이 내용은 여기에 문서화되지 않지만 고객이 액세스 정책을 관리할 수 있는 셀프서비스 콘솔을 제공할 예정입니다.

## PDP(정책 결정 지점) {#policy-decn-pt-defn}

* 액세스 결정을 내리기 전에 권한 부여 정책에 대해 액세스 요청을 평가하는 포인트입니다.

## 정책 시행 지점(PEP) {#policy-ef-pt-defn}

* 리소스에 대한 사용자의 액세스 요청을 가로채고 PDP에 결정 요청을 수행하고 요청에 대한 해당 결정을 집행하는 지점입니다. 현재 클라이언트 응용 프로그램이며 이 역할을 동시 모니터링으로 전송할 계획이 없습니다.

## 정책 정보 지점(PIP) {#policy-info-pt-defn}

* 속성 값의 소스. 동시성 모니터링은 다음을 제공하여 정보 포인트 역할을 합니다.
   * 통과 스트림 메타데이터.
   * 동시 스트림에 대한 활동 지표입니다.

## 프로그래머 {#programmer-defn}

* 서비스 및 콘텐츠 공급자 역할을 합니다.
* Concurrency Monitoring 서비스와 통합되는 배포된 클라이언트 애플리케이션을 사용하여 위에서 언급한 서비스 데이터를 기반으로 정의된 보안 정책을 적용합니다.
* MVPD를 지원하여 구독자 활동을 수집하고 해당 속성에 제한 규칙을 적용해야 합니다.
* 또한 별도의 규칙으로 모든 대상 포털에서 컨텐츠에 대한 동시 액세스를 제한할 수도 있습니다.

  *Q: 나머지 Adobe Pass 인증과 같이 프로그래머가 요청자 ID가 아닌 이유는 무엇입니까?*

  *A: 프로그래머가 이 매개 변수를 유연하게 사용하여 사용 사례에 따라 속성 간에 데이터를 전달하거나 격리할 수 있기 때문입니다.*

## 리소스 {#resource-defn}

* 주체가 사용하려는 실제 콘텐츠. 리소스는 일반적으로 소유자(게시자)와 관련된 속성을 전달하며 장르 또는 기타 정보와 같은 추가 정보를 제공할 수도 있습니다.

## 규칙 {#rule-defn}

* 해당 스트림에 대한 액세스를 허용할지 또는 거부할지 여부를 결정하기 위해 특정 스트림 및 관련 사용자 활동에 대해 평가되는 부울 함수입니다.

## 스트리밍 세션 {#streaming-session-defn}

* 특정 리소스를 사용하기 위해 주체가 시작한 스트리밍 비디오 세션입니다.

## 제목 {#subj-defn}

* 인터넷을 통한 (비디오) 콘텐츠의 소비자. Concurrency Monitoring은 일반적으로 MVPD 계정 ID(동일한 계약을 공유하는 여러 실제 사용자(예: 가정의 가족 구성원)를 다루므로 _&#x200B;**사용자**&#x200B;_&#x200B;라는 용어를 의도적으로 피하고 있습니다.

* 각 스트림에 대해 서비스를 사용하는 실제 사용자, 네트워크 연결 장치 등과 관련된 속성으로 주체를 개선할 수 있습니다.

## 구독자 {#subscriber-defn}

* MVPD의 결제 고객 또는 결제 고객의 자격 증명을 공유하는 사람
* 위에서 언급한 서비스를 사용하는 클라이언트 애플리케이션에서 Concurrency Monitoring Service에 의해 콘텐츠 보기를 중지할 수 있습니다.
* 최상의 사례 시나리오에서는 동시성 모니터링 서비스의 존재를 절대 인식하지 못합니다

## Target {#target-defn}

* 규칙이 지정된 스트림에 적용되는지 여부를 반환하는 스트림 조건자입니다. CM의 암시적 대상은 해당 정책을 참조하는 응용 프로그램에서 만든 모든 스트림입니다. 또한 규칙을 적용하기 전에 활동 필터링을 미세 조정하기 위해 속성 값 조건을 추가할 수 있습니다.

## 임차인 {#tenant-defn}

* 동시성 모니터링 고객 조직.
