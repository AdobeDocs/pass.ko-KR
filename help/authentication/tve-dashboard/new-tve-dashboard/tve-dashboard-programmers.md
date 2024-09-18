---
title: 프로그래머
description: TVE 대시보드 내 프로그래머 및 해당 구성에 대해 알아봅니다.
exl-id: b450d7cc-d5b5-4454-8f95-8047856bfb98
source-git-commit: acff285f7db1bdd32d5da3e01a770d9581d3ba75
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---

# 프로그래머 {#programmers}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

TVE 대시보드의 **프로그래머** 섹션에서 계정 권한에 연결된 [프로그래머](/help/authentication/glossary.md#programmer)에 대한 설정을 보고 관리할 수 있습니다. 필요에 따라 [새 프로그래머를 추가](#add-new-programmer)할 수도 있습니다.

왼쪽 패널의 **프로그래머** 탭에는 다음 세부 정보가 포함된 기존 프로그래머 목록이 표시됩니다.

* **프로그래머 ID**: 시스템 내의 미디어 회사 식별자입니다.
* **채널**: 프로그래머와 연결된 채널 수입니다.

![기존 프로그래머 목록](../../assets/tve-dashboard/new-tve-dashboard/programmers/programmers-list-view.png)

*기존 프로그래머 목록*

프로그래머에 대해 자세히 알아보려면 목록 위의 **Search** 표시줄에 프로그래머 이름을 입력하십시오.

## 프로그래머 구성 관리 {#manage-programmer-conf}

특정 프로그래머의 다양한 설정을 관리하려면 다음 단계를 따르십시오.

1. 왼쪽 패널에서 **프로그래머** 탭을 선택합니다.
1. 목록에서 프로그래머를 선택하십시오.
1. 다음 탭 중 하나를 선택하여 선택한 프로그래머의 해당 설정을 보고 편집합니다.

   * [채널](#channels)
   * [인증서](#certificates)
   * [등록된 응용 프로그램](#registered-applications)
   * [사용자 지정 체계](#custom-schemes)

   ![프로그래머 설정](../../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-tabs-view.png)

   *프로그래머 설정*

>[!IMPORTANT]
>
> 구성 변경 내용을 활성화하는 방법에 대한 자세한 내용은 [변경 내용 검토 및 푸시](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md)를 참조하십시오.

### 채널 {#channels}

이 탭에는 현재 프로그래머와 연결된 채널 목록이 표시됩니다. [채널](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md) 섹션의 자세한 정보에 액세스하려면 이 목록에서 특정 채널을 선택하십시오.

선택한 프로그래머에 대한 새 채널을 추가하려면 **사용 가능한 채널** 섹션의 오른쪽 상단에서 **새 채널 추가**&#x200B;를 선택하십시오. [새 채널을 추가하는 방법](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#add-new-channel)을 알아보세요.

![새 채널 추가](../../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-channel-button.png)

*새 채널 추가*

### 인증서 {#certificates}

이 탭에는 사용자 메타데이터 암호화 흐름에 사용된 [사용 가능한 인증서](#available-certificates) 목록이 표시됩니다. 다음을 포함하는 각 인증서에 대한 세부 정보가 표시됩니다.

* 상태(**사용자 메타데이터 암호화** 사용에 대해 사용할지 여부)
* 시리얼 번호
* 발급자 조직의 이름
* 주체 조직의 이름
* 발행 날짜
* 만료일
* 사용자 메타데이터를 암호화하는 드롭다운 메뉴(**예**&#x200B;를 선택하는 경우, 인증서는 우편 번호 값과 같은 중요한 사용자 정보를 암호화합니다).

#### 사용 가능한 인증서 {#available-certificates}

이러한 인증서는 개인 또는 공개 키 역할을 하며 사용자 메타데이터 암호화에 사용됩니다. 동일한 미디어 회사와 연결된 모든 채널은 이 인증서를 사용할 수 있습니다.

사용 가능한 인증서에 대해 다음과 같이 변경할 수 있습니다.

* [새 인증서 추가](#add-new-certificate)
* [인증서 삭제](#delete-certificate)

##### 새 인증서 추가 {#add-new-certificate}

새 인증서를 추가하려면 다음 단계를 따르십시오.

1. **사용 가능한 인증서** 섹션의 오른쪽 상단 모서리에서 **새 인증서 추가**&#x200B;를 선택합니다.

   ![새 인증서 추가](../../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-certificate-button.png)

   *새 인증서 추가*

1. 인증서의 공개 키를 **새 인증서** 대화 상자에 붙여 넣으십시오.

1. **인증서 추가**&#x200B;를 선택합니다.

1. **사용 가능한 인증서** 목록에서 새 인증서를 찾습니다.

   >[!IMPORTANT]
   >
   > 시스템이 최신 상태이고 새 인증서를 사용할 준비가 되었는지 확인하십시오.

1. **사용자 메타데이터를 암호화하는데 사용** 드롭다운 메뉴에서 **예**&#x200B;를 선택하여 새 인증서를 활성화합니다.

새 구성 변경이 생성되었으며 서버를 업데이트할 준비가 되었습니다. **사용 가능한 인증서** 섹션에 나열된 새 인증서를 사용하려면 [변경 내용 검토 및 푸시](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md) 흐름을 진행하십시오.

##### 인증서 삭제 {#delete-certificate}

인증서를 삭제하려면 다음 단계를 따르십시오.

1. **사용 가능한 인증서** 목록에서 삭제할 인증서를 마우스로 가리킵니다.

1. **제거**&#x200B;를 선택합니다.

   ![선택한 인증서 제거](../../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-remove-certificate-button.png)

   *선택한 인증서 제거*

1. **인증서 삭제** 대화 상자에서 **삭제**&#x200B;을 선택합니다.

새 구성 변경이 생성되었으며 서버를 업데이트할 준비가 되었습니다. [변경 내용 검토 및 푸시](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md)한 후에만 **사용 가능한 인증서** 섹션에서 인증서가 삭제됩니다.

### 등록된 응용 프로그램 {#registered-applications}

이 탭에는 응용 프로그램 등록 목록이 제공됩니다.

### 사용자 지정 체계 {#custom-schemes}

이 탭에는 사용자 지정 체계 목록이 표시됩니다. [iOS/tvOS 응용 프로그램 등록](/help/authentication/iostvos-application-registration.md) 보기

## 새 프로그래머 추가 {#add-new-programmer}

새 프로그래머 엔티티를 추가하려면 다음 단계를 따르십시오.

1. 왼쪽 패널에서 **프로그래머** 탭을 선택합니다.

1. **프로그래머** 섹션의 오른쪽 상단에서 **새 프로그래머 추가**&#x200B;를 선택합니다.

   ![새 프로그래머 추가](../../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-programmer-button.png)

   *새 프로그래머 추가*

1. **새 프로그래머** 대화 상자의 **프로그래머 ID**&#x200B;에 미디어 회사 식별자를 입력하십시오.

1. **표시 이름** 아래의 콘솔에 표시할 상용 브랜드 이름을 입력하십시오.

1. **프로그래머 추가**&#x200B;를 선택하십시오.

새 구성 변경이 생성되었으며 서버를 업데이트할 준비가 되었습니다. **프로그래머** 섹션에 나열된 새 프로그래머를 사용하려면 [변경 내용 검토 및 푸시](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md) 흐름을 진행하십시오.
