# airotServerIntegrationGuide

Himpel REST API Developer Guide — Draft (v0.1)

목적:
본 문서는 Himpel IoT 플랫폼의 User Control 및 Device Control 관련 REST API 규격으로 모든 요청 및 응답은 JSON 형식을 사용.

# REAT API
## SNS User 생성

### API

| **항목** | **설명** |
| -------- | -------- |
| API 명 | `/himpel/api/user/sns` |
| Method | `POST` |
| Content-Type | `application/json` |
| 설명 | SNS 인증정보 기반으로 신규 사용자를 생성합니다. |

### request body

| **1 depth** | **설명** |
| -------- | -------- |
| userId | sns 로그인을 통해 조회된 사용자 이메일 |

  - 예제
```code
{
  userId: "himpel@gmail.com"
}
```

### response body

| **1 depth** | **2 depth** | **설명** |
| -------- | -------- | -------- |
| result | | 성공여부 true/false |
| body | userId |서버에서 발급한 사용자 uuid|

- 예제
```code
{
  result: true,
  body: {
    userId: "b479f800-9XXX-11f0-a4fe-c55396113c73",
  }
}
```

⸻

## SNS User Login

### API

| **항목** | **설명** |
| -------- | -------- |
| API 명 | `/himpel/api/user/login/sns` |
| Method | `POST` |
| Content-Type | `application/json` |
| 설명 | SNS 인증정보 기반으로 신규 사용자를 생성합니다. |

### request body

| **1 depth** | **설명** |
| -------- | -------- |
| userId | sns 로그인을 통해 조회된 사용자 이메일 |
| fcmToken | 앱 푸쉬를 위한 fcmToken |

  - 예제
```code
{
  userId: "himpel@gmail.com",
  fcmToken: "XXXXXXXXX"
}
```

### response body

| **1 depth** | **2 depth** | **설명** |
| -------- | -------- | -------- |
| result | | 성공여부 true/false |
| body | userUUID | 서버에서 발급한 사용자 uuid,사용자 생성시 발급받은 useerId 와 동일|
|  | customerUUID | 사용자 그룹 uuid |


- 예제
```code
{
  result: true,
  body: {
    userUUID: "b479f800-9XXX-11f0-a4fe-c55396113c73",
    customerUUID: "b479f800-9ZZZ-11f0-a4fe-c55396113c73",
}
```


⸻

## User 정보 조회

### API

| **항목** | **설명** |
| -------- | -------- |
| API 명 | `/himpel/api/user/{userUUID}` |
| Method | `GET` |
| Content-Type | `application/json` |
| 설명 | SNS 인증정보 기반으로 신규 사용자를 생성합니다. |

### request parameter
| **항목** | **설명** |
| -------- | -------- |
| userUUID | 로그인시 응답받은 userUUID |

### response body

| **1 depth** | **2 depth** | **설명** |
| -------- | -------- | -------- |
| result | | 성공여부 true/false |
| body | name | 사용자 계정 (email) |
|  | customerUUID | 사용자 그룹 uuid |


- 예제
```code
{
  result: true,
  body: {
    customerUUID: "b479f800-9ZZZ-11f0-a4fe-c55396113c73",
    name: "himpel@gmail.com"
}
```

⸻

## User Extension Data 저장

### API

| **항목** | **설명** |
| -------- | -------- |
| API 명 | `/himpel/api/user/{userUUID}/telemetry` |
| Method | `POST` |
| Content-Type | `application/json` |
| 설명 | 사용자를 위한 기타 정보를 저장 |

### request body

| **1 depth** | **설명** |
| -------- | -------- |
| key | 저장할 데이터의 이름 |
| value | 저장할 데이터 |

  - 예제
```code
{
  key: "pushSetting",
  value: true
}
```

### response body

| **1 depth** | **2 depth** | **설명** |
| -------- | -------- | -------- |
| result | | 성공여부 true/false |


- 예제
```code
{
  result: true,
}
```

⸻

## User Extension Data 조회

### API

| **항목** | **설명** |
| -------- | -------- |
| API 명 | `/himpel/api/user/{userUUID}/telemetry?keys=KEYS` |
| Method | `GET` |
| Content-Type | `application/json` |
| 설명 | 사용자 기타 데이터 조회 |

### request parameter
| **항목** | **설명** |
| -------- | -------- |
| keys | 조회하고자 하는 데이터 key 모음 ( , 로 구분 )|

- 예제

```code
/himpel/api/user/{userUUID}/telemetry?keys=pushSetting,mapSetting
```
### response body

| **1 depth** | **2 depth** | **설명** |
| -------- | -------- | -------- |
| result | | 성공여부 true/false |

- 예제
```code
{
  result: true,
}
```


⸻

## 장치 생성

### API

| **항목** | **설명** |
| -------- | -------- |
| API 명 | `/himpel/api/device` |
| Method | `POST` |
| Content-Type | `application/json` |
| 설명 | 새 장치 생성, 기존에 동일한 이름의 장치가 있다면 삭제후 생성 |

### request body

| **1 depth** | **설명** |
| -------- | -------- |
| deviceName | 생성할 장치 이름 |
| customerUUID | 사용자 login 시에 발급받은 customerUUID |
| gateway | 생성할 기기가 gateway 인지 |

  - 예제
```code
{
  "deviceName": "H-1234",
  "customerUUID": "doorlock",
  "gateway": false,
}
```

### response body

| **1 depth** | **2 depth** | **설명** |
| -------- | -------- | -------- |
| result | | 성공여부 true/false |
| body | deviceId | 생성된 장치의 uuid |
|  | credentialsId | 생성된 장치의 accessToken, MQTT 접속시 user ID 로 사용 ( Client ID 아님 ) |


- 예제
```code
{
  result: true,
  body: {
    deviceId: "05af5f40-afeb-11f0-9fd8-a399e6e87405",
    credentialsId: "xutWRKO9zZyCKenC2stA"

  }
}
```

⸻

## 장치 조회

### API

| **항목** | **설명** |
| -------- | -------- |
| API 명 | `/himpel/api/device/{deviceId}` |
| Method | `POST` |
| Content-Type | `application/json` |
| 설명 | 장치 정보 조회 |

### request parameter

| **1 depth** | **설명** |
| -------- | -------- |
| deviceId | 장치 생성시 발급받은 장치 uuid |


### response body

| **1 depth** | **2 depth** | **설명** |
| -------- | -------- | -------- |
| result | | 성공여부 true/false |
| body |  | 조회된 장치 정보 |

⸻

## 사용자에게 할당된 장치들 조회

### API

| **항목** | **설명** |
| -------- | -------- |
| API 명 | `/himpel/api/devices?customerUUID=CUSTOMER_UUID` |
| Method | `GET` |
| Content-Type | `application/json` |
| 설명 | 장치 정보 조회 |

### request parameter

| **1 depth** | **설명** |
| -------- | -------- |
| customerUUID | 사용자 로그인시 발급 받은 customerUUID |


### response body

| **1 depth** | **2 depth** | **설명** |
| -------- | -------- | -------- |
| result | | 성공여부 true/false |
| body |  | 조회된 장치 정보, Array |


⸻


## 장치의 Telemetry Data 조회

### API

| **항목** | **설명** |
| -------- | -------- |
| API 명 | `/himpel/api/device/{deviceId}/telemetry` |
| Method | `GET` |
| Content-Type | `application/json` |
| 설명 | 장치의 telemetry 데이터 조회 ( MQTT 로 publish 한 데이터 ) |

### request parameter

| **1 depth** | **설명** |
| -------- | -------- |
| deviceId | 장치 생성시 발급 받은 device uuid |


### response body

| **1 depth** | **2 depth** | **설명** |
| -------- | -------- | -------- |
| result | | 성공여부 true/false |
| body |  | 장치의 telemetry data |



⸻

# MQTT
* MQTT 통신은 보안을 위해 mqtts 로 수행
* client ID 는 장치마다 모두 상이해야 함
* user ID 는 REST API 의 "장치 생성" 시 발급 받은 credentialsId 로 설정
* telemetry 를 위한 topic 은 v1/devices/me/telemetry
* publish 시 payload 는 json 이어야 하며, key value 로 전달
  ```code
  {
    "filter":123,
    "mode":"ventilation"
  }
 
