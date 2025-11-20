---
title: API 참조 개요
description: 끝점, 인증 및 응답 형식을 포함한 동시 모니터링 API에 대한 전체 참조
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# API 참조 개요 {#api-reference-overview}

동시 모니터링 API는 스트리밍 세션을 관리하고 동시 사용 정책을 적용하기 위한 RESTful 인터페이스를 제공합니다. 이 참조는 모든 엔드포인트, 인증 방법, 요청/응답 형식 및 오류 처리에 대한 완전한 설명서를 제공합니다.

## API 기본 URL

### 프로덕션 환경

```
https://streams.adobeprimetime.com/v2/
```

### 스테이징 환경

```
https://streams-stage.adobeprimetime.com/v2/
```

**참고:** 개발 및 테스트를 위해 항상 스테이징 환경을 사용하십시오. 프로덕션 자격 증명은 스테이징 통합이 완료된 후에만 제공됩니다.

## 인증

모든 API 호출에는 애플리케이션 자격 증명을 사용하여 HTTP 기본 인증이 필요합니다.

- **사용자 이름:** 응용 프로그램 ID(Adobe 제공)
- **암호:** 빈 문자열

### 인증 헤더 예

```bash
curl -u "<your-app-id>:" https://streams-stage.adobeprimetime.com/v2/sessions

For an application with id "demo-app" the authentication header would be exactly as shown below, including the quotes and colon:
curl -u "demo-app:" https://streams-stage.adobeprimetime.com/v2/sessions
```

## 응답 형식 표준

### 성공 응답

성공한 모든 응답은 다음 구조를 따릅니다.

```json
{
  "status": "success",
  "data": {
    // Response-specific data
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### 오류 응답

모든 오류 응답은 다음 구조를 따릅니다.

```json
{
  "associatedAdvice": [
    {
      "policyName": "string",
      "ruleName": "string",
      "scope": {},
      "attribute": "string",
      "threshold": 0,
      "conflicts": [
        {}
      ]
    }
  ],
  "obligations": [
    {
      "namespace": "string",
      "action": "string",
      "arguments": [
        "string"
      ]
    }
  ]
}
```

### 평가 결과 형식

정책을 평가할 때(특히 409개 충돌의 경우) 응답에는 평가 결과가 포함됩니다.

```json
{
  "evaluationResult": {
    "decision": "DENY",
    "obligations": [
      {
        "id": "obligation-id",
        "fulfillOn": "DENY",
        "attributes": {
          "attribute1": "value1"
        }
      }
    ],
    "associatedAdvice": [
      {
        "id": "advice-id",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "rule-name",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## 일반 HTTP 상태 코드

| 코드 | 설명 | 반환 시 |
|------|----------------------|------------------------------------------------|
| 200 | 확인 | 성공한 GET 요청 |
| 202 | 생성됨/수락됨 | 성공적인 세션 생성/하트비트 기록됨 |
| 400 | 잘못된 요청 | 잘못된 매개변수 또는 누락된 필수 필드 |
| 401 | 승인되지 않음 | 인증이 잘못되었거나 없습니다. |
| 403 | 금지됨 | 권한 부족 |
| 404 | 세션 ID를 찾을 수 없음 | CM 서비스에서 생성되지 않은 세션 ID |
| 409 | 충돌 | 정책 위반(동시 제한에 도달함) |
| 410 | 없어짐 | 세션이 만료되었거나 종료되었습니다. |
| 429 | 요청이 너무 많음 | 요금 한도 초과됨 |

## 매개 변수 전달 메서드

### 경로 매개 변수

URL 경로의 일부인 필수 매개 변수:

1. `{idp}` - ID 공급자 식별자
2. `{subject}` - 사용자 식별자(일반적으로 Adobe Pass)
3. `{sessionId}` - 세션 식별자(위치 헤더에서 반환됨)

### 추가 매개 변수

선택적 매개 변수가 URL에 전달됩니다.

```bash
GET /sessions/{idp}/{subject}?platform=test
```

### 양식 데이터(POST/PUT)

요청 본문의 메타데이터 및 세션 데이터:

```bash
POST /sessions/{idp}/{subject}
Content-Type: application/x-www-form-urlencoded

channel=Channel1&deviceId=device-123&contentType=live
```

### 헤더

HTTP 헤더에서 전달된 특수 매개 변수:

```bash
X-Terminate: termination-code-123
X-Client-Version: 1.0.0
```

## 오류 처리 우수 사례

### 409 충돌 처리

409 충돌 응답을 받으면

1. 정책 위반을 이해하려면 **평가 결과를 구문 분석**&#x200B;합니다.
2. **에서**&#x200B;충돌 정보 추출`associatedAdvice`
3. LIFO/FIFO 전략에 따라 **사용자에게 옵션 제공**
4. LIFO 동작을 구현하는 경우 **종료 코드 사용**

### 410 이동 처리

410 Gone 응답을 받으면

1. **응답에 본문이 있는지 확인** - 원격 종료를 나타냅니다.
2. 세션이 종료된 이유를 이해하려면 **조언을 구문 분석하십시오**
3. 세션 종료를 반영하도록 **UI 업데이트**
4. **정상적으로 처리** - 세션이 자연스럽게 시간 초과되었을 수 있습니다.
5. **새 세션 시작** - 필요한 경우 새 세션을 시작합니다.

### 속도 제한

429개의 너무 많은 요청을 받는 경우:

1. **통화 빈도 제한**(분당 최대 요청 수 200개)(CM에서 허용하는 최대 수준)
2. **필요한 간격으로 하트비트를 보냅니다**(분당 1회).

## 테스트 도구

### 대화형 API 탐색기

대화형 테스트에는 [Swagger UI](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)를 사용하십시오.

1. 오른쪽 상단 모서리에 애플리케이션 ID를 입력합니다
2. 인증을 설정하려면 &quot;탐색&quot;을 클릭하십시오.
3. 실제 매개 변수를 사용하여 엔드포인트 테스트
4. 요청/응답 예 보기
