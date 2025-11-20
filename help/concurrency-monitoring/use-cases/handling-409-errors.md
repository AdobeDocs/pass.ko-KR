---
title: 409 충돌 오류 처리
description: 동시 사용 제한에 도달할 때 409 충돌 오류를 처리하는 방법에 대해 알아봅니다
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---


# 409 충돌 오류 처리 {#handling-409-errors}

사용자가 새 스트림을 시작하려고 할 때 동시 사용 제한에 도달하면 동시성 모니터링에서 **409 충돌** 응답을 반환합니다. 이 오류를 처리하는 방법을 이해하는 것은 올바른 사용자 경험을 제공하는 데 중요합니다.

## 409 충돌이란 무엇입니까? {#what-is-a-409-conflict}

다음과 같은 경우 409 충돌이 발생합니다.

1. **사용자가 새 스트림을 시작하려고 합니다**
2. **정책 평가 결정** 요청이 한도를 초과합니다.
3. 자세한 충돌 정보가 포함된 **시스템이 409**&#x200B;을(를) 반환함
4. 충돌을 처리하는 방법을 **응용 프로그램에서 결정**&#x200B;해야 합니다.

### 409 응답 구조

```json
{
  "status": "error",
  "error": {
    "code": "POLICY_VIOLATION",
    "message": "Concurrent usage limit exceeded"
  },
  "evaluationResult": {
    "decision": "DENY",
    "associatedAdvice": [
      {
        "id": "advice-1",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1",
                "contentType": "live"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## 응답 이해 {#understanding-the-response}

### 키 필드

- **`decision`**: 409개 충돌에 대해 항상 &quot;거부&quot;
- **`associatedAdvice`**: 위반을 설명하는 권고 개체 배열
- **`conflicts`**: 충돌을 일으키는 활성 세션 목록
- **`terminationCode`**: 특정 세션을 종료하는 고유 코드

### 권고 사항 유형

#### 규칙 위반 권고 사항

```json
{
  "adviceType": "rule-violation",
  "attributes": {
    "rule": "max_streams",
    "threshold": 3,
    "current": 4,
    "conflicts": [...]
  }
}
```

#### 원격 종료 조언

```json
{
  "adviceType": "remote-termination",
  "attributes": {
    "terminatedBy": "session-456",
    "reason": "New session requested with X-Terminate header"
  }
}
```

## 처리 전략 {#handling-strategies}

- FIFO 모드에서 CM은 기존의 세션을 종료하여 새로운 세션을 시작할 수 있도록 합니다.
- LIFO 모드에서 CM은 새 세션을 차단하고 사용자에게 알립니다.


## 우수 사례 {#best-practices}

### &#x200B;1. 사용자 통신 지우기

- **제한 설명** - 사용자는 차단된 이유를 이해해야 합니다.
- **사용 가능한 옵션 표시** - 충돌을 해결하기 위해 수행할 수 있는 작업
- **컨텍스트를 제공** - 활성 상태인 세션을 표시합니다.

### &#x200B;2. 정상적인 오류 처리

- **충돌 안 함** - 409 오류를 정상적으로 처리합니다.
- **대체 요소 제공** - 충돌을 해결하는 방법을 제공합니다.
- **사용자 상태 저장** - 선택한 콘텐츠를 잃지 않음

### &#x200B;3. 사용자 경험 고려 사항

- **빠른 해결** - 충돌을 쉽게 해결할 수 있습니다.
- **선택 항목 지우기** - 사용자가 해당 옵션을 이해해야 합니다.
- **일관된 동작** - 충돌을 항상 같은 방식으로 처리합니다.

### &#x200B;4. 기술적 고려 사항

- **응답을 신중하게 구문 분석** - 모든 관련 정보 추출
- **에지 사례 처리** - 충돌이 반환되지 않으면 어떻게 합니까?
- **로그 충돌** - 분석을 위해 정책 위반 추적


