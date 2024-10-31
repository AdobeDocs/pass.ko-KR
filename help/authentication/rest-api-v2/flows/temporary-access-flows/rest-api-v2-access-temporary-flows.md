---
title: 임시 액세스 흐름
description: REST API V2 - 임시 액세스 흐름
exl-id: 387fcdb0-3a42-4893-ba83-e809426f92be
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '3215'
ht-degree: 0%

---

# 임시 액세스 흐름 {#temporary-access-flows}

>[!IMPORTANT]
>
> 이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> REST API V2 구현은 [조절 메커니즘](/help/authentication/throttling-mechanism.md) 설명서에 의해 제한됩니다.

TempPass를 사용하면 프로그래머가 사용자에게 유효한 MVPD 계정으로 인증하도록 요청하지 않고도 보호된 콘텐츠에 일시적으로 액세스할 수 있습니다.

TempPass 기능에 대한 자세한 내용은 [TempPass](../../../temp-pass.md) 설명서를 참조하십시오.

임시 액세스 흐름을 사용하면 다음 시나리오를 쿼리할 수 있습니다.

* [기본 TempPass를 사용하여 인증 결정 검색](#retrieve-authorization-decisions-using-basic-temppass)
* [프로모션 TempPass를 사용하여 인증 결정 검색](#retrieve-authorization-decisions-using-promotional-temppass)
* [프로모션 TempPass를 사용하여 최대 리소스 수 소비](#consume-maximum-number-of-resources-using-promotional-temppass)
* [기본 또는 프로모션 TempPass가 만료되면 인증 결정 검색](#retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires)
* [기본 TempPass에 대한 프로필 검색](#retrieve-profile-for-basic-temppass)
* [프로모션 TempPass에 대한 프로필 검색](#retrieve-profile-for-promotional-temppass)

## 기본 TempPass를 사용하여 인증 결정 검색 {#retrieve-authorization-decisions-using-basic-temppass}

### 전제 조건 {#prerequisites-retrieve-authorization-decisions-using-basic-temppass}

기본 TempPass를 사용하여 인증 결정을 검색하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* 스트리밍 애플리케이션은 사용자에게 인증을 요구하지 않고 재생 콘텐츠에 대한 임시 액세스를 제공하기를 원한다.
* 스트리밍 애플리케이션은 사용자가 선택한 리소스를 재생하기 전에 인증 결정을 검색해야 합니다.

>[!IMPORTANT]
>
> 가정
> 
> <br/>
> 
> * 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 적용되는 기본 TempPass의 올바른 구성 설정이 있어야 합니다.
> * 기본 TempPass에 대해 구성된 TTL(Time-To-Live)이 만료되지 않았습니다.

### 워크플로 {#workflow-retrieve-authorization-decisions-using-basic-temppass}

다음 다이어그램과 같이 기본 TempPass를 사용하여 인증 플로우를 구현하려면 지정된 단계를 따르십시오.

![기본 TempPass를 사용하여 인증 결정 검색](../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-authorization-decisions-using-basic-temppass-flow.png)

*기본 TempPass를 사용하여 인증 결정 검색*

1. **권한 부여 결정 검색:** 스트리밍 응용 프로그램은 결정 권한 부여 끝점을 호출하여 특정 리소스에 대한 권한 부여 결정을 얻는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 mvpd를 사용하여 권한 부여 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   > 
   > * `serviceProvider`, `mvpd` 및 `resources`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization` 및 `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **기본 TempPass 유효성 검사:** Adobe Pass 서버는 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 적용되는 기본 TempPass의 올바른 구성 설정이 있는지 확인합니다.

1. **미디어 토큰이 있는 `Permit` 결정을 반환합니다.** Decisions Authorize 끝점 응답에 `Permit` 결정 및 미디어 토큰이 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 의사 결정 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd를 사용하여 인증 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   >
   > <br/>
   > 
   > 결정 권한 부여 끝점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   > 
   > 승인 결정 끝점은 요청 데이터를 사용하여 임시 액세스 조건이 충족되는지 확인합니다.
   >
   > * 기본 TempPass에 대해 구성된 TTL(Time-To-Live)은 만료되지 않아야 합니다.
   >
   > <br/>
   > 
   > 임시 액세스 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **미디어 토큰으로 스트림 시작:** 스트리밍 응용 프로그램에서 미디어 토큰을 사용하여 콘텐츠를 재생합니다.

## 프로모션 TempPass를 사용하여 인증 결정 검색 {#retrieve-authorization-decisions-using-promotional-temppass}

### 전제 조건 {#prerequisites-retrieve-authorization-decisions-using-promotional-temppass}

프로모션 TempPass를 사용하여 인증 결정을 검색하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* 스트리밍 애플리케이션은 사용자에게 인증을 요구하지 않고 최대 수의 리소스를 재생하기 위한 임시 액세스를 제공하려고 합니다.
* 스트리밍 애플리케이션은 인증 결정을 검색할 때 사용자의 신원에 대한 고유 정보를 포함해야 합니다.
* 스트리밍 애플리케이션은 사용자가 선택한 리소스를 재생하기 전에 인증 결정을 검색해야 합니다.

>[!IMPORTANT]
>
> 가정
>
> <br/>
> 
> * 제공된 `serviceProvider`과(와) `mvpd` 사이의 통합에 적용되는 프로모션 TempPass의 올바른 구성 설정이 있어야 합니다.
> * 프로모션 TempPass에 대해 구성된 TTL(Time-To-Live)이 만료되지 않았습니다.
> * 프로모션 TempPass에 대해 구성된 최대 리소스 수가 사용되지 않았습니다.

### 워크플로 {#workflow-retrieve-authorization-decisions-using-promotional-temppass}

다음 다이어그램과 같이 프로모션 TempPass를 사용하여 인증 플로우를 구현하려면 주어진 단계를 따르십시오.

![프로모션 TempPass를 사용하여 인증 결정 검색](../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-authorization-decisions-using-promotional-temppass-flow.png)

*프로모션 TempPass를 사용하여 인증 결정 검색*

1. **권한 부여 결정 검색:** 스트리밍 응용 프로그램은 결정 권한 부여 끝점을 호출하여 특정 리소스에 대한 권한 부여 결정을 얻는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 mvpd를 사용하여 권한 부여 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   >
   > * `serviceProvider`, `mvpd` 및 `resources`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization` 및 `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더
   >
   > <br/>
   >
   > 판촉 TempPass를 사용할 때 결정 승인 끝점에 `AP-TempPass-Identity` 헤더가 있어야 합니다. 헤더에는 콘텐츠에 액세스하는 사용자의 ID에 대한 고유 정보가 포함됩니다.
   > 
   > <br/>
   > 
   > `AP-TempPass-Identity` 헤더에 대한 자세한 내용은 [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) 설명서를 참조하십시오.

1. **프로모션 TempPass 유효성 검사:** Adobe Pass 서버는 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 적용되는 프로모션 TempPass의 올바른 구성 설정이 있는지 확인합니다.

1. **미디어 토큰이 있는 `Permit` 결정을 반환합니다.** Decisions Authorize 끝점 응답에 `Permit` 결정 및 미디어 토큰이 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 의사 결정 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd를 사용하여 인증 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   > 
   > <br/>
   > 
   > 결정 권한 부여 끝점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   > 
   > 승인 결정 끝점은 요청 데이터를 사용하여 임시 액세스 조건이 충족되는지 확인합니다.
   >
   > * 프로모션 TempPass에 대해 구성된 TTL(Time-To-Live)이 만료되지 않아야 합니다.
   > * 프로모션 TempPass에 대해 구성된 최대 리소스 수를 사용할 수 없습니다.
   >
   > <br/>
   > 
   > 임시 액세스 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **미디어 토큰으로 스트림 시작:** 스트리밍 응용 프로그램에서 미디어 토큰을 사용하여 콘텐츠를 재생합니다.

## 프로모션 TempPass를 사용하여 최대 리소스 수 소비 {#consume-maximum-number-of-resources-using-promotional-temppass}

### 전제 조건 {#prerequisites-consume-maximum-number-of-resources-using-promotional-temppass}

프로모션 TempPass를 사용하여 최대 수의 리소스를 소비하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* 스트리밍 애플리케이션은 사용자에게 인증을 요구하지 않고 최대 수의 리소스를 재생하기 위한 임시 액세스를 제공하려고 합니다.
* 스트리밍 애플리케이션은 인증 결정을 검색할 때 사용자의 신원에 대한 고유 정보를 포함해야 합니다.
* 스트리밍 애플리케이션은 사용자가 선택한 리소스를 재생하기 전에 인증 결정을 검색해야 합니다.

>[!IMPORTANT]
>
> 가정
>
> <br/>
> 
> * 제공된 `serviceProvider`과(와) `mvpd` 사이의 통합에 적용되는 프로모션 TempPass의 올바른 구성 설정이 있어야 합니다.
> * 프로모션 TempPass에 대해 구성된 TTL(Time-To-Live)이 만료되지 않았습니다.
> * 프로모션 TempPass에 대해 구성된 최대 리소스 수는 1개입니다.

### 워크플로 {#workflow-consume-maximum-number-of-resources-using-promotional-temppass}

다음 다이어그램과 같이 프로모션 TempPass를 사용하여 최대 수의 리소스를 사용할 때 인증 플로우를 구현하려면 주어진 단계를 따르십시오.

![프로모션 TempPass를 사용하여 최대 리소스 수를 사용합니다](../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-consume-maximum-number-of-resources-using-promotional-temppass-flow.png)

*프로모션 TempPass를 사용하여 최대 리소스 수를 사용합니다*

1. **프로모션 TempPass에 대한 프로필 검색:** 스트리밍 애플리케이션은 프로필 끝점에 요청을 전송하여 프로모션 TempPass에 대한 프로필 정보를 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 mvpd에 대한 프로필 검색](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) API 설명서를 참조하십시오.
   >
   > * `serviceProvider` 및 `mvpd`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization` 및 `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더
   >
   > <br/>
   > 
   > 프로필 끝점 쿼리는 선택 사항이며 프로모션 TempPass를 사용하여 재생할 수 있는 리소스의 수를 결정하는 데 사용할 수 있습니다.

1. **프로모션 TempPass 유효성 검사:** Adobe Pass 서버는 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 적용되는 프로모션 TempPass의 올바른 구성 설정이 있는지 확인합니다.

1. **임시 프로필에 대한 정보를 반환합니다.** 프로필 끝점 응답에 &quot;temporary&quot;로 설정된 특성 `type`을(를) 포함하여 임시 프로필에 대한 정보가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 프로필 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd에 대한 프로필 검색](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) API 설명서를 참조하십시오.
   > 
   > <br/>
   > 
   > 프로필 끝점은 요청 데이터의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   > 
   > <br/>
   >
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   > 
   > 프로필 끝점은 요청 데이터를 사용하여 임시 액세스 조건이 충족되는지 확인합니다.
   >
   > * 프로모션 TempPass에 대해 구성된 TTL(Time-To-Live)이 만료되지 않아야 합니다.
   > * 프로모션 TempPass에 대해 구성된 최대 리소스 수를 사용할 수 없습니다.
   >
   > <br/>
   > 
   > 임시 액세스 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **의사 결정 흐름을 진행합니다.** Profiles 끝점 응답에 프로필이 포함되어 있으면 스트리밍 응용 프로그램에서 임시 프로필 정보를 사용하여 후속 의사 결정 흐름을 계속합니다.

1. **권한 부여 결정 검색:** 스트리밍 응용 프로그램은 결정 권한 부여 끝점을 호출하여 특정 리소스에 대한 권한 부여 결정을 얻는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   > 
   > 자세한 내용은 [특정 mvpd를 사용하여 권한 부여 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   >
   > * `serviceProvider`, `mvpd` 및 `resources`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization` 및 `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더
   >
   > <br/>
   > 
   > 판촉 TempPass를 사용할 때 결정 승인 끝점에 `AP-TempPass-Identity` 헤더가 있어야 합니다. 헤더에는 콘텐츠에 액세스하는 사용자의 ID에 대한 고유 정보가 포함됩니다.
   > 
   > <br/>
   > 
   > `AP-TempPass-Identity` 헤더에 대한 자세한 내용은 [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) 설명서를 참조하십시오.

1. **프로모션 TempPass 유효성 검사:** Adobe Pass 서버는 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 적용되는 프로모션 TempPass의 올바른 구성 설정이 있는지 확인합니다.

1. **미디어 토큰이 있는 `Permit` 결정을 반환합니다.** Decisions Authorize 끝점 응답에 `Permit` 결정 및 미디어 토큰이 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 의사 결정 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd를 사용하여 인증 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   > 
   > <br/>
   > 
   > 결정 권한 부여 끝점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   > 
   > <br/>
   > 
   > 승인 결정 끝점은 요청 데이터를 사용하여 임시 액세스 조건이 충족되는지 확인합니다.
   >
   > * 프로모션 TempPass에 대해 구성된 TTL(Time-To-Live)이 만료되지 않아야 합니다.
   > * 프로모션 TempPass에 대해 구성된 최대 리소스 수를 사용할 수 없습니다.
   >
   > <br/>
   > 
   > 임시 액세스 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **권한 부여 결정 검색:** 스트리밍 응용 프로그램은 결정 권한 부여 끝점을 호출하여 특정 리소스에 대한 권한 부여 결정을 얻는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 mvpd를 사용하여 권한 부여 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   >
   > * `serviceProvider`, `mvpd` 및 `resources`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization` 및 `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더
   >
   > <br/>
   > 
   > 판촉 TempPass를 사용할 때 결정 승인 끝점에 `AP-TempPass-Identity` 헤더가 있어야 합니다. 헤더에는 콘텐츠에 액세스하는 사용자의 ID에 대한 고유 정보가 포함됩니다.
   >
   > <br/>
   > 
   > `AP-TempPass-Identity` 헤더에 대한 자세한 내용은 [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) 설명서를 참조하십시오.

1. **프로모션 TempPass 유효성 검사:** Adobe Pass 서버는 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 적용되는 프로모션 TempPass의 올바른 구성 설정이 있는지 확인합니다.

1. **세부 정보가 포함된 `Deny` 결정 반환:** Decisions Authorize Endpoint 응답에 `Deny` 결정 및 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 오류 페이로드가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 의사 결정 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd를 사용하여 인증 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   > 
   > <br/>
   > 
   > 결정 권한 부여 끝점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   > 
   > 승인 결정 끝점은 요청 데이터를 사용하여 임시 액세스 조건이 충족되는지 확인합니다.
   >
   > * 프로모션 TempPass에 대해 구성된 TTL(Time-To-Live)이 만료되지 않아야 합니다.
   > * 프로모션 TempPass에 대해 구성된 최대 리소스 수를 사용할 수 없습니다.
   >
   > <br/>
   > 
   > 임시 액세스 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **`Deny` 결정 정보 처리:** 스트리밍 응용 프로그램에서 응답의 오류 정보를 처리하고 이를 사용하여 사용자 인터페이스에 특정 메시지를 선택적으로 표시할 수 있습니다.

   >[!TIP]
   >
   > 제안: 스트리밍 애플리케이션은 최대 리소스 수가 초과되었음을 사용자에게 알리고 계속 시청하기 위해 일반 MVPD를 사용하여 기본 인증 흐름을 시작하도록 권장할 수 있습니다.

## 기본 또는 프로모션 TempPass가 만료되면 인증 결정 검색 {#retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

### 전제 조건 {#prerequisites-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

기본 또는 프로모션 TempPass가 만료되는 경우 인증 결정을 검색하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* [기본 TempPass를 사용하여 권한 부여 결정을 검색하기 전의 필수 구성 요소](#prerequisites-retrieve-authorization-decisions-using-basic-temppass).
* [프로모션 TempPass를 사용하여 권한 부여 결정을 검색하기 전 필수 구성 요소](#prerequisites-retrieve-authorization-decisions-using-promotional-temppass).

>[!IMPORTANT]
>
> 가정
> 
> <br/>
> 
> * 제공된 `serviceProvider`과(와) `mvpd` 사이의 통합에 적용되는 기본 또는 프로모션 TempPass의 올바른 구성 설정이 있어야 합니다.
> * 기본 또는 프로모션에 대해 구성된 TTL(Time-To-Live)입니다. 임시 액세스 기간 제한을 초과했습니다.

### 워크플로 {#workflow-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

다음 다이어그램과 같이 기본 또는 프로모션 TempPass가 만료되는 경우 지정된 단계에 따라 인증 플로우를 구현합니다.

![기본 또는 프로모션 TempPass가 만료되면 인증 결정 검색](../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires-flow.png)

*기본 또는 프로모션 TempPass가 만료되면 인증 결정 검색*

1. **권한 부여 결정 검색:** 스트리밍 응용 프로그램은 결정 권한 부여 끝점을 호출하여 특정 리소스에 대한 권한 부여 결정을 얻는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 mvpd를 사용하여 권한 부여 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   > 
   > * `serviceProvider`, `mvpd` 및 `resources`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization` 및 `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더
   >
   > <br/>
   > 
   > 판촉 TempPass를 사용할 때 결정 승인 끝점에 `AP-TempPass-Identity` 헤더가 있어야 합니다. 헤더에는 콘텐츠에 액세스하는 사용자의 ID에 대한 고유 정보가 포함됩니다.
   > 
   > <br/>
   > 
   > `AP-TempPass-Identity` 헤더에 대한 자세한 내용은 [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) 설명서를 참조하십시오.

1. **기본 또는 프로모션 TempPass 유효성 검사:** Adobe Pass 서버는 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 적용되는 기본 또는 프로모션 TempPass의 올바른 구성 설정이 있는지 확인합니다.

1. **세부 정보가 포함된 `Deny` 결정 반환:** Decisions Authorize Endpoint 응답에 `Deny` 결정 및 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 오류 페이로드가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 의사 결정 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd를 사용하여 인증 결정 검색](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 설명서를 참조하십시오.
   > 
   > <br/>
   > 
   > 결정 권한 부여 끝점은 기본 조건이 충족되는지 확인하기 위해 요청 데이터를 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   > 
   > 승인 결정 끝점은 요청 데이터를 사용하여 임시 액세스 조건이 충족되는지 확인합니다.
   >
   > * 기본 또는 프로모션 TempPass에 대해 구성된 TTL(Time-To-Live)이 만료되지 않아야 합니다.
   > * 프로모션 TempPass에 대해 구성된 최대 리소스 수를 사용할 수 없습니다.
   >
   > <br/>
   > 
   > 임시 액세스 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **`Deny` 결정 정보 처리:** 스트리밍 응용 프로그램에서 응답의 오류 정보를 처리하고 이를 사용하여 사용자 인터페이스에 특정 메시지를 선택적으로 표시할 수 있습니다.

   >[!TIP]
   >
   > 제안: 스트리밍 애플리케이션은 임시 액세스가 만료되었음을 사용자에게 알리고 일반 MVPD를 사용하여 기본 인증 흐름을 시작하여 계속 시청하도록 권장할 수 있습니다.

## 기본 TempPass에 대한 프로필 검색 {#retrieve-profile-for-basic-temppass}

>[!IMPORTANT]
>
> 프로필 끝점 쿼리는 기본 TempPass에 대해 선택 사항입니다.

### 전제 조건 {#prerequisites-retrieve-profile-for-basic-temppass}

기본 TempPass에 대한 프로필을 검색하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* 스트리밍 애플리케이션은 임시 액세스가 만료되지 않았는지 확인하기 위해 임시 프로필을 검색하려고 합니다.

>[!IMPORTANT]
>
> 가정
> 
> <br/>
> 
> * 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 적용되는 기본 TempPass의 올바른 구성 설정이 있어야 합니다.
> * 기본 TempPass에 대해 구성된 TTL(Time-To-Live)은 만료되지 않아야 합니다.

### 워크플로 {#workflow-retrieve-profile-information-for-basic-temppass}

지정된 단계에 따라 다음 다이어그램과 같이 기본 TempPass에 대한 프로필 검색 플로우를 구현합니다.

![기본 TempPass에 대한 프로필 검색](../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-profile-for-basic-temppass-flow.png)

*기본 TempPass에 대한 프로필 검색*

1. **기본 TempPass에 대한 프로필 검색:** 스트리밍 응용 프로그램은 프로필 끝점에 요청을 보내 기본 TempPass에 대한 프로필 정보를 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 mvpd에 대한 프로필 검색](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) API 설명서를 참조하십시오.
   > 
   > * `serviceProvider` 및 `mvpd`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization` 및 `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **기본 TempPass 유효성 검사:** Adobe Pass 서버는 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 적용되는 기본 TempPass의 올바른 구성 설정이 있는지 확인합니다.

1. **임시 프로필에 대한 정보를 반환합니다.** 프로필 끝점 응답에 &quot;temporary&quot;로 설정된 특성 `type`을(를) 포함하여 임시 프로필에 대한 정보가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 프로필 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd에 대한 프로필 검색](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) API 설명서를 참조하십시오.
   > 
   > <br/>
   > 
   > 프로필 끝점은 요청 데이터의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   > 
   > 프로필 끝점은 요청 데이터를 사용하여 임시 액세스 조건이 충족되는지 확인합니다.
   >
   > * 기본 TempPass에 대해 구성된 TTL(Time-To-Live)은 만료되지 않아야 합니다.
   >
   > <br/>
   > 
   > 임시 액세스 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **의사 결정 흐름을 진행합니다.** Profiles 끝점 응답에 프로필이 포함되어 있으면 스트리밍 응용 프로그램에서 임시 프로필 정보를 사용하여 후속 의사 결정 흐름을 계속합니다.

## 프로모션 TempPass에 대한 프로필 검색 {#retrieve-profile-for-promotional-temppass}

>[!IMPORTANT]
>
> 프로필 끝점 쿼리는 프로모션 TempPass에 대해 선택 사항입니다.

### 전제 조건 {#prerequisites-retrieve-profile-for-promotional-temppass}

프로모션 TempPass에 대한 프로필을 검색하기 전에 다음 전제 조건이 충족되는지 확인하십시오.

* 스트리밍 애플리케이션은 임시 액세스가 만료되지 않았는지 확인하거나 재생할 수 있는 리소스의 수를 결정하기 위해 임시 프로필을 검색하려고 합니다.

>[!IMPORTANT]
>
> 가정
>
> <br/>
> 
> * 제공된 `serviceProvider`과(와) `mvpd` 사이의 통합에 적용되는 프로모션 TempPass의 올바른 구성 설정이 있어야 합니다.
> * 프로모션 TempPass에 대해 구성된 TTL(Time-To-Live)이 만료되지 않았습니다.
> * 프로모션 TempPass에 대해 구성된 최대 리소스 수가 사용되지 않았습니다.

### 워크플로 {#workflow-retrieve-profile-information-for-promotional-temppass}

다음 다이어그램과 같이 프로모션 TempPass에 대한 프로필 검색 플로우를 구현하려면 주어진 단계를 따르십시오.

![프로모션 TempPass에 대한 프로필 검색](../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-profile-for-promotional-temppass-flow.png)

*프로모션 TempPass에 대한 프로필 검색*

1. **프로모션 TempPass에 대한 프로필 검색:** 스트리밍 애플리케이션은 프로필 끝점에 요청을 전송하여 프로모션 TempPass에 대한 프로필 정보를 검색하는 데 필요한 모든 데이터를 수집합니다.

   >[!IMPORTANT]
   >
   > 자세한 내용은 [특정 mvpd에 대한 프로필 검색](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) API 설명서를 참조하십시오.
   > 
   > * `serviceProvider` 및 `mvpd`과(와) 같은 모든 _필수_ 매개 변수
   > * `Authorization` 및 `AP-Device-Identifier`과(와) 같은 모든 _required_ 헤더
   > * 모든 _선택적_ 매개 변수 및 헤더

1. **프로모션 TempPass 유효성 검사:** Adobe Pass 서버는 제공된 `serviceProvider`과(와) `mvpd` 간의 통합에 적용되는 프로모션 TempPass의 올바른 구성 설정이 있는지 확인합니다.

1. **임시 프로필에 대한 정보를 반환합니다.** 프로필 끝점 응답에 &quot;temporary&quot;로 설정된 특성 `type`을(를) 포함하여 임시 프로필에 대한 정보가 포함되어 있습니다.

   >[!IMPORTANT]
   >
   > 프로필 응답에 제공된 정보에 대한 자세한 내용은 [특정 mvpd에 대한 프로필 검색](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) API 설명서를 참조하십시오.
   > 
   > <br/>
   > 
   > 프로필 끝점은 요청 데이터의 유효성을 검사하여 기본 조건이 충족되는지 확인합니다.
   >
   > * _required_ 매개 변수와 헤더가 유효해야 합니다.
   > * 입력한 `serviceProvider`과(와) `mvpd` 간의 통합이 활성화되어 있어야 합니다.
   >
   > <br/>
   > 
   > 기본 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.
   >
   > <br/>
   > 
   > 프로필 끝점은 요청 데이터를 사용하여 임시 액세스 조건이 충족되는지 확인합니다.
   >
   > * 프로모션 TempPass에 대해 구성된 TTL(Time-To-Live)이 만료되지 않아야 합니다.
   > * 프로모션 TempPass에 대해 구성된 최대 리소스 수를 사용할 수 없습니다.
   >
   > <br/>
   > 
   > 임시 액세스 유효성 검사가 실패하면 오류 응답이 생성되고 [향상된 오류 코드](../../../enhanced-error-codes.md) 설명서를 준수하는 추가 정보가 제공됩니다.

1. **의사 결정 흐름을 진행합니다.** Profiles 끝점 응답에 프로필이 포함되어 있으면 스트리밍 응용 프로그램에서 임시 프로필 정보를 사용하여 후속 의사 결정 흐름을 계속합니다.
