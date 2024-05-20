---
title: 채널
description: TVE 대시보드 내의 채널과 다양한 구성에 대해 알아봅니다.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 0%

---


# 채널 {#channels}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

다음 **채널** tve 대시보드 의 섹션에서 특정 프로그래머와 연관된 채널에 대한 설정을 보고 관리할 수 있습니다. 다음을 수행할 수도 있습니다. [새 채널 추가](#add-new-channel) 귀하의 요구 사항에 따라.

다음 **채널** 왼쪽 패널의 탭에는 연결된 채널 목록이 다음 세부 정보와 함께 표시됩니다.

* **표시 이름**: 상업적 목적으로 사용되는 채널의 브랜드 이름입니다.
* **채널 ID**: 요청자 ID라고도 하는 고유 식별자입니다.
* **통합**: 다음으로 설정된 연결 수 [MVPD](/help/authentication/glossary.md#mvpd).

![기존 채널 목록](assets/channels-list.png)

*기존 채널 목록*

에 채널 이름을 입력합니다. **검색** 채널에 대한 자세한 내용은 목록 위에 있는 막대를 참조하십시오.

## 채널 구성 관리 {#manage-channel-conf}

특정 채널의 다양한 설정을 관리하려면 단계를 따르십시오.

1. 다음 항목 선택 **채널** 왼쪽 패널의 탭입니다.
1. 사용 가능한 목록에서 채널을 선택합니다.
1. 다음 탭 중 하나를 선택하여 선택한 채널의 해당 설정을 보고 편집합니다.

   * [일반 설정](#general-settings)
   * [통합](#integrations)
   * [인증서](#certificates)
   * [도메인](#domains)
   * [등록된 응용 프로그램](#registered-applications)
   * [사용자 지정 체계](#custom-schemes)

   ![채널 설정](assets/channel-settings.png)

   *채널 설정*

>[!IMPORTANT]
>
> 보기 [변경 사항 검토 및 푸시](/help/authentication/tve-dashboard-review-push-changes.md) 구성 변경 사항 활성화에 대한 자세한 내용은 을 참조하십시오.

### 일반 설정 {#general-settings}

이 탭에는 다음이 표시됩니다 **채널 정보** 및 **Analytics 구성**.

#### 채널 정보 {#channel-information}

이 섹션에서는 다음 세부 정보를 편집할 수 있습니다.

* **표시 이름**: 상업적 목적으로 사용되는 채널의 브랜드 이름입니다.

* **기본 리디렉션 URL**: 인증 및 로그아웃을 위한 백업 리디렉션 URL.

* **오류 보고**: 선택 시 **예**, Adobe Pass SDK는 analytics를 위해 Adobe Pass 백엔드에 오류 보고서를 전송합니다.

![채널 정보 편집](assets/channel-information.png)

*채널 정보 편집*

#### Analytics 구성 {#analytics-configuration}

이 섹션에서는 Adobe Pass 인증 이벤트를 Adobe Analytics에 전달하도록 구성할 수 있습니다.

활성화하려면 **Analytics 구성**, 보고서 세트 ID(RSID) 설정에 대한 자세한 내용은 기술 계정 관리자(TAM)에게 문의하십시오.

![Analytics 구성 활성화](assets/channel-analytical-conf.png)

*Analytics 구성 활성화*

선택 **새 분석 구성 추가** 여러 구성을 추가합니다.

새 구성 변경이 생성되었으며 서버를 업데이트할 준비가 되었습니다. 에서 새 Analytics 구성을 사용하려면 **Analytics 구성** 섹션, 다음 작업을 계속 [변경 사항 검토 및 푸시](/help/authentication/tve-dashboard-review-push-changes.md) 흐름.

### 통합 {#integrations}

이 탭에는 현재 선택한 채널과 MVPD 간의 사용 가능한 통합 목록이 표시됩니다. 이 목록에는 각 통합이 활성화 여부를 나타내는 상태와 함께 표시됩니다. 이 목록에서 특정 통합을 선택하여 의 자세한 정보에 액세스합니다. [통합](tve-dashboard-integrations.md) 섹션.

![사용 가능한 통합 목록](assets/channel-integrations.png)

*사용 가능한 통합 목록*

### 인증서 {#certificates}

이 탭에는 다음 목록이 표시됩니다. [사용 가능한 인증서](#available-certificates) 및 [상속된 사용 가능한 인증서](#inherited-avail-certificates) 사용자 메타데이터 암호화 흐름에 사용됩니다. 다음을 포함하는 각 인증서에 대한 세부 정보가 표시됩니다.

* 상태(활성화 여부) **사용자 메타데이터 암호화** 사용 여부)
* 시리얼 번호
* 발급자 조직의 이름
* 주체 조직의 이름
* 발행 날짜
* 만료일
* 사용자 메타데이터를 암호화하는 드롭다운 메뉴(선택하는 경우) **예**&#x200B;인증서는 우편 번호 값과 같은 중요한 사용자 정보를 암호화합니다.

#### 사용 가능한 인증서 {#available-certificates}

이러한 인증서는 개인 또는 공개 키 역할을 하며 사용자 메타데이터 암호화에 사용됩니다.
사용 가능한 인증서 섹션에서 다음과 같이 변경할 수 있습니다.

* [새 인증서 추가](#add-new-certificate)
* [인증서 삭제](#delete-certificate)

##### 새 인증서 추가 {#add-new-certificate}

새 인증서를 추가하려면 다음 단계를 따르십시오.

1. 선택 **새 인증서 추가** 의 맨 위에 **사용 가능한 인증서** 섹션.

   ![새 인증서 추가](assets/add-new-certificate.png)

   *새 인증서 추가*

1. 인증서의 공개 키를 **새 인증서** 대화 상자.
1. 선택 **인증서 추가**.

   새 구성 변경이 생성되었으며 서버를 업데이트할 준비가 되었습니다. 다음에 나열된 새 인증서를 사용하려면 **사용 가능한 인증서** 섹션, 다음 작업을 계속 [변경 사항 검토 및 푸시](/help/authentication/tve-dashboard-review-push-changes.md) 흐름.

1. 목록에서 새 인증서 찾기 **사용 가능한 인증서**.

   >[!IMPORTANT]
   >
   > 시스템이 최신 상태이고 새 인증서를 사용할 준비가 되었는지 확인하십시오.

1. 선택 **예** 출처: **암호화된 사용자 메타데이터에 사용됨** 드롭다운 메뉴를 사용하여 새 인증서를 활성화합니다.

##### 인증서 삭제 {#delete-certificate}

인증서를 삭제하려면 다음 단계를 따르십시오.

1. 다음 목록에서 삭제할 인증서를 마우스로 가리킵니다. **사용 가능한 인증서**.
1. 선택 **제거**.

   ![선택한 인증서 제거](assets/channel-delete-certificate.png)

   *선택한 인증서 제거*

1. 선택 **삭제** 다음에서 **활성 인증서 삭제** 대화 상자.

새 구성 변경이 생성되었으며 서버를 업데이트할 준비가 되었습니다. 인증서가 다음에서 삭제됩니다 **사용 가능한 인증서** 다음 이후에만 섹션 추가 [변경 사항 검토 및 푸시](/help/authentication/tve-dashboard-review-push-changes.md).

#### 상속된 사용 가능한 인증서 {#inherited-avail-certificates}

미디어 회사들은 이러한 인증서를 자체 수준에서 정의합니다. 동일한 미디어 회사와 연결된 모든 채널은 이 인증서를 사용할 수 있습니다.

![상속된 사용 가능한 인증서](assets/inherited-available-certificates.png)

*상속된 사용 가능한 인증서*

### 도메인 {#domains}

이 탭에는 각 채널이 Adobe Pass 인증과 통신하는 데 사용할 수 있는 도메인 목록이 표시됩니다.

도메인을 다음과 같이 변경할 수 있습니다.

* [새 도메인 추가](#add-domains)
* [도메인 삭제](#delete-domain)

>[!TIP]
>
> 목록에 더 일반적인 도메인이 있는 경우 새 하위 도메인을 추가하지 마십시오.

#### 새 도메인 추가 {#add-domains}

도메인을 추가하려면 다음 단계를 따르십시오.

1. 선택 **새 도메인 추가** 의 오른쪽 위 모서리 **사용 가능한 도메인** 섹션.

   ![새 도메인 추가](assets/add-new-domain.png)

   *새 도메인 추가*

1. 에 도메인 이름을 입력합니다. **새 도메인** 대화 상자.

1. 선택 **도메인 추가** 을 눌러 선택한 채널에 대한 새 도메인을 추가합니다.

새 구성 변경이 생성되었으며 서버를 업데이트할 준비가 되었습니다. 에 나열된 새 도메인을 사용하려면 **사용 가능한 도메인** 섹션, 다음 작업을 계속 [변경 사항 검토 및 푸시](/help/authentication/tve-dashboard-review-push-changes.md) 흐름.

#### 도메인 삭제 {#delete-domain}

도메인을 삭제하려면 다음 단계를 따르십시오.

1. 다음 목록에서 삭제할 도메인을 마우스로 가리킵니다. **사용 가능한 도메인**.
1. 선택 **제거**.

   ![선택한 도메인 제거](assets/remove-domain.png)

   *선택한 도메인 제거*

1. 선택 **삭제** 다음에 있음 **도메인 삭제** 대화 상자.

새 구성 변경이 생성되었으며 서버를 업데이트할 준비가 되었습니다. 도메인이 다음에서 삭제됩니다. **사용 가능한 도메인** 다음 이후에만 섹션 추가 [변경 사항 검토 및 푸시](/help/authentication/tve-dashboard-review-push-changes.md).

선택한 도메인은 더 이상 사용할 수 없습니다. 따라서 이 도메인과 연결된 애플리케이션은 Adobe Pass 인증 서비스에 액세스할 수 없게 됩니다.

### 등록된 응용 프로그램 {#registered-applications}

이 탭에는 응용 프로그램 등록 목록이 제공됩니다. 보기 [동적 클라이언트 등록 관리](/help/authentication/dynamic-client-registration-management.md) 추가 정보.

### 사용자 지정 체계 {#custom-schemes}

이 탭에는 사용자 지정 체계 목록이 표시됩니다. 보기 [iOS/tvOS 애플리케이션 등록](/help/authentication/iostvos-application-registration.md) 및 [동적 클라이언트 등록 관리](/help/authentication/dynamic-client-registration-management.md) 추가 정보.

## 새 채널 추가 {#add-new-channel}

다음 단계에 따라 새 채널을 추가합니다.

1. 다음 항목 선택 **채널** 왼쪽 패널의 탭입니다.
1. 선택 **새 채널 추가** 의 오른쪽 위 모서리 **채널** 섹션.

   ![새 채널 추가](assets/add-new-channel.png)

   *새 채널 추가*

1. 선택 **프로그래머 ID** 드롭다운 메뉴에서 **새 채널** 대화 상자.

1. 에 고유 식별자 입력 **채널 ID**.
1. 상업용으로 사용되는 채널의 브랜드 이름을에 입력하십시오. **표시 이름**.
1. 선택 **채널 추가**.

새 구성 변경이 생성되었으며 서버를 업데이트할 준비가 되었습니다. 다음에 나열된 새 채널을 사용하려면 **채널** 섹션, 다음 작업을 계속 [변경 사항 검토 및 푸시](/help/authentication/tve-dashboard-review-push-changes.md) 흐름.

