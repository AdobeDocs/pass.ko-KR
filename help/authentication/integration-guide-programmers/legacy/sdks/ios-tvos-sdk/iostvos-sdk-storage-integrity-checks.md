---
title: iOS/tvOS 스토리지 무결성 검사 메커니즘
description: iOS/tvOS 무결성 검사 메커니즘
exl-id: 5d7cdc46-3e51-4e14-9e30-d7f48bc87506
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# (레거시) iOS/tvOS 무결성 검사 메커니즘 {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

>[!IMPORTANT]
>
> [제품 알림](/help/authentication/product-announcements.md) 페이지에서 집계한 최신 Adobe Pass 인증 제품 알림 및 서비스 중단 타임라인에 대한 정보를 계속 받아 보십시오.

## 소개 {#Intro}

iOS/tvOS AccessEnabler SDK 버전 3.8.3부터 AccessEnabler 초기화 시 스토리지 무결성 검사를 수행하는 옵션을 사용할 수 있습니다.

이 메커니즘을 사용하기 위해 AccessEnabler 클래스의 추가 초기화 메서드를 사용하여 api를 확장했습니다.

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## 무결성 검사 {#Checks}

스토리지 무결성 검사는 AccessEnabler 스토리지 손상이 의심되는 경우(예: 읽기/쓰기 스토리지 작업 중에 경합 상태가 발생한 경우) 유용합니다.

AccessEnabler 초기화 시 다음 검사를 수행할 수 있습니다.
- 스토리지 작동 가능성: 읽기 및 쓰기 작업의 성공 여부 확인
- 저장된 값 무결성: 모든 값이 유효하고 예상 형식입니다

>[!IMPORTANT]
> 
>검사 중 하나가 실패하면 저장소의 모든 값이 지워지고 사용자가 로그아웃되므로 사용자 환경이 나쁠 수 있습니다. 필요한 경우에만 스토리지 무결성 검사를 사용하십시오.


## 기본 비헤이비어 {#Default}

기본 초기화 방법을 사용하여 AccessEnabler를 초기화할 때 기본적으로 스토리지 무결성 검사가 꺼져 있습니다.

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

AccessEnabler 초기화 시 수행할 스토리지 무결성 검사를 명시적으로 지정하려면 다음 초기화 방법을 사용하십시오.

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## 무결성 검사 유형 {#Switcher}

IntegrityCheckType 열거형은 클라이언트 응용 프로그램에 노출되며 다음 값을 가집니다.

| 값 | 확인 수행됨 | 스토리지 지워짐 | 설명 | 권장 사용 사례 |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | 없음 | 사용 안 함 | 스토리지 초기화 시 무결성 검사가 수행되지 않습니다. | SDK 흐름이 예상대로 작동하는 경우 |
| INTEGRITY_CHECK_ALL | 저장된 값의 저장소 작동 가능성 <br/> | 확인 실패 시 | 스토리지 초기화 시 사용 가능한 모든 무결성 검사가 수행됩니다. | SDK 스토리지 손상이 의심되는 경우 <br/> 무결성 검사에 실패하면 사용자가 로그아웃됩니다. |
| INTEGRITY_CHECK_CLEAR | 없음 | 항상 | 스토리지 초기화 시 스토리지가 지워짐 | SDK 흐름을 예상대로 완료할 수 없는 경우 |
