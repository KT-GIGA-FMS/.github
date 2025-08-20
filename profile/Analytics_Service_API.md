# Analytics Service API 문서

## 개요
Analytics Service는 차량의 운행 통계를 제공하는 REST API 서비스입니다.

- **Base URL**: `http://{host}:8083/api/v1/analytics`
- **포트**: 8083
- **응답 형식**: `ApiResponse<T>` 래퍼가 포함된 JSON

## 공통 응답 형식

모든 API 응답은 `ApiResponse<T>`로 래핑됩니다:

```json
{
  "success": true,
  "message": "Success", 
  "data": {},
  "timestamp": "2025-08-20T12:34:56.789"
}
```

## API 엔드포인트

### 1. 차량 통계 조회 (단건)

**GET** `/vehicles/{vehicleId}/statistics`

특정 차량의 통계 정보를 조회합니다.

**경로 매개변수:**
- `vehicleId` (string) - 차량의 고유 ID

**응답:** `ApiResponse<VehicleStatisticsResponse>`

**응답 예시:**
```json
{
  "success": true,
  "message": "Success",
  "data": {
    "vehicleId": "1",
    "vehicleName": "12가3456",
    "totalDistance": 12345.67,
    "monthlyDistance": 321.5,
    "currentYearMonth": "2025-08",
    "totalTrips": 87,
    "monthlyTrips": 5
  },
  "timestamp": "2025-08-20T12:34:56.789"
}
```

**cURL 예시:**
```bash
curl -X GET "http://localhost:8083/api/v1/analytics/vehicles/1/statistics"
```

### 2. 차량 통계 일괄 조회

**POST** `/vehicles/statistics/batch`

여러 차량의 통계 정보를 한 번에 조회합니다. 페이지네이션 시나리오에 유용합니다.

**요청 본문:** `string[]` - 차량 ID 배열

**응답:** `ApiResponse<VehicleStatisticsResponse[]>`

**요청 예시:**
```json
["1", "2", "3"]
```

**응답 예시:**
```json
{
  "success": true,
  "message": "Success",
  "data": [
    {
      "vehicleId": "1",
      "vehicleName": "12가3456",
      "totalDistance": 12345.67,
      "monthlyDistance": 321.5,
      "currentYearMonth": "2025-08",
      "totalTrips": 87,
      "monthlyTrips": 5
    },
    {
      "vehicleId": "2",
      "vehicleName": "34가5678",
      "totalDistance": 0,
      "monthlyDistance": 0,
      "currentYearMonth": "2025-08",
      "totalTrips": 0,
      "monthlyTrips": 0
    }
  ],
  "timestamp": "2025-08-20T12:34:56.789"
}
```

**cURL 예시:**
```bash
curl -X POST "http://localhost:8083/api/v1/analytics/vehicles/statistics/batch" \
  -H "Content-Type: application/json" \
  -d '["1","2","3"]'
```

## 데이터 전송 객체 (DTOs)

### VehicleStatisticsResponse

| 필드 | 타입 | 설명 | 예시 |
|------|------|------|------|
| `vehicleId` | string | 차량의 고유 식별자 | "1" |
| `vehicleName` | string | 차량의 표시 이름 | "12가3456" |
| `totalDistance` | number | 총 누적 거리 (킬로미터) | 12345.67 |
| `monthlyDistance` | number | 이번 달 운행 거리 | 321.5 |
| `currentYearMonth` | string | 현재 년월 (yyyy-MM 형식) | "2025-08" |
| `totalTrips` | integer | 총 완료된 운행 횟수 | 87 |
| `monthlyTrips` | integer | 이번 달 운행 횟수 | 5 |

### ApiResponse<T>

| 필드 | 타입 | 설명 | 예시 |
|------|------|------|------|
| `success` | boolean | 요청 성공 여부 | true |
| `message` | string | 사람이 읽을 수 있는 메시지 | "Success" |
| `data` | T | 실제 응답 데이터 | {} |
| `timestamp` | string | ISO-8601 형식의 타임스탬프 | "2025-08-20T12:34:56.789" |

## 에러 응답

에러가 발생하면 `success: false`와 에러 메시지가 포함된 응답을 반환합니다:

```json
{
  "success": false,
  "message": "차량 통계 일괄 조회에 실패했습니다: Internal error",
  "data": null,
  "timestamp": "2025-08-20T12:34:56.789"
}
```

## HTTP 상태 코드

| 상태 코드 | 설명 |
|-----------|------|
| 200 | 성공 |
| 400 | 잘못된 요청 (검증 오류) |
| 404 | 차량을 찾을 수 없음 |
| 500 | 내부 서버 오류 |

## 사용 예시

### 프론트엔드 연동

```javascript
// 단일 차량 통계 조회
const getVehicleStats = async (vehicleId) => {
  const response = await fetch(`/api/v1/analytics/vehicles/${vehicleId}/statistics`);
  const result = await response.json();
  
  if (result.success) {
    return result.data;
  } else {
    throw new Error(result.message);
  }
};

// 차량 통계 일괄 조회
const getBatchVehicleStats = async (vehicleIds) => {
  const response = await fetch('/api/v1/analytics/vehicles/statistics/batch', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(vehicleIds),
  });
  
  const result = await response.json();
  
  if (result.success) {
    return result.data;
  } else {
    throw new Error(result.message);
  }
};
```

### Car Service 연동

```java
// CarService에서 사용 예시
List<String> vehicleIds = carPage.getContent().stream()
    .map(car -> car.getId().toString())
    .toList();

List<VehicleStatisticsDto> statistics = analyticsClientService.getVehicleStatisticsBatch(vehicleIds);
```

## 참고사항

- 모든 거리는 킬로미터 단위로 측정됩니다
- 월간 통계는 현재 월을 기준으로 계산됩니다
- 운행 횟수는 완료된 운행만 포함합니다
- 서비스는 자동으로 시간대 변환을 처리합니다 (Asia/Seoul)
- 페이지네이션 시나리오에서는 API 호출을 줄이기 위해 배치 요청을 권장합니다
