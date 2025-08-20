# Car Service API 문서

## 개요
Car Service는 차량 정보 관리와 차량 목록 조회를 담당하는 REST API 서비스입니다.

- **Base URL**: `http://{host}:8080/api/v1/cars`
- **포트**: 8080
- **응답 형식**: JSON

## API 엔드포인트

### 1. 차량 등록

**POST** `/register`

새로운 차량을 시스템에 등록합니다.

**요청 본문:** `RegisterCarRequest`

**응답:** `ApiResponse<CarResponse>`

**요청 예시:**
```json
{
  "plateNo": "12가3456",
  "carModelId": 1,
  "status": "사용가능",
  "carType": "법인",
  "fuelType": "휘발유",
  "efficiencyKmPerL": 15.5,
  "imageUrl": "https://example.com/car1.jpg",
  "remarks": "신규 등록 차량"
}
```

**응답 예시:**
```json
{
  "success": true,
  "message": "차량이 성공적으로 등록되었습니다.",
  "data": {
    "id": 1,
    "plateNo": "12가3456",
    "carModelName": "현대 아반떼",
    "imageUrl": "https://example.com/car1.jpg",
    "status": "사용가능",
    "carType": "법인",
    "fuelType": "휘발유",
    "efficiencyKmPerL": 15.5,
    "createdAt": "2025-08-20T10:55:20.245523",
    "remarks": "신규 등록 차량"
  },
  "timestamp": "2025-08-20T10:55:20.245523"
}
```

**cURL 예시:**
```bash
curl -X POST "http://localhost:8080/api/v1/cars/register" \
  -H "Content-Type: application/json" \
  -d '{
    "plateNo": "12가3456",
    "carModelId": 1,
    "status": "사용가능",
    "carType": "법인",
    "fuelType": "휘발유",
    "efficiencyKmPerL": 15.5,
    "imageUrl": "https://example.com/car1.jpg",
    "remarks": "신규 등록 차량"
  }'
```

### 2. 차량 목록 조회

**GET** `/`

등록된 차량 목록을 페이지네이션과 필터링을 통해 조회합니다.

**쿼리 매개변수:**
- `searchKeyword` (string, optional) - 검색 키워드 (번호판, 차량명)
- `status` (string, optional) - 차량 상태 필터
- `carType` (string, optional) - 차량 유형 필터
- `page` (integer, optional) - 페이지 번호 (기본값: 0)
- `size` (integer, optional) - 페이지 크기 (기본값: 10)
- `sortBy` (string, optional) - 정렬 기준 (기본값: createdAt)
- `sortDirection` (string, optional) - 정렬 방향 (기본값: DESC)

**응답:** `ShowCarsResponse`

**응답 예시:**
```json
{
  "cars": [
    {
      "id": 3,
      "plateNo": "12가3452",
      "carModelName": "현대 아반떼",
      "imageUrl": "https://example.com/car1.jpg",
      "status": "사용가능",
      "carType": "법인",
      "fuelType": "휘발유",
      "efficiencyKmPerL": "15.50",
      "totalDistance": "0km",
      "monthlyDistance": "0km",
      "createdAt": "2025-08-20T11:05:49.020182",
      "remarks": ""
    },
    {
      "id": 2,
      "plateNo": "12가3456",
      "carModelName": "현대 아반떼",
      "imageUrl": "https://example.com/car1.jpg",
      "status": "사용가능",
      "carType": "법인",
      "fuelType": "휘발유",
      "efficiencyKmPerL": "15.50",
      "totalDistance": "0km",
      "monthlyDistance": "0km",
      "createdAt": "2025-08-20T10:56:35.180226",
      "remarks": ""
    }
  ],
  "pageInfo": {
    "currentPage": 1,
    "pageSize": 10,
    "totalPages": 1,
    "hasNext": false,
    "hasPrevious": false,
    "totalElements": 2
  },
  "totalCount": 2
}
```

**cURL 예시:**
```bash
# 기본 조회
curl -X GET "http://localhost:8080/api/v1/cars"

# 검색 및 필터링
curl -X GET "http://localhost:8080/api/v1/cars?searchKeyword=12가&status=사용가능&page=0&size=5"

# 정렬
curl -X GET "http://localhost:8080/api/v1/cars?sortBy=plateNo&sortDirection=ASC"
```

## 데이터 전송 객체 (DTOs)

### RegisterCarRequest

| 필드 | 타입 | 필수 | 설명 | 예시 |
|------|------|------|------|------|
| `plateNo` | string | ✅ | 차량 번호판 | "12가3456" |
| `carModelId` | integer | ✅ | 차량 모델 ID | 1 |
| `status` | string | ✅ | 차량 상태 | "사용가능" |
| `carType` | string | ❌ | 차량 유형 | "법인" |
| `fuelType` | string | ❌ | 연료 유형 | "휘발유" |
| `efficiencyKmPerL` | number | ❌ | 연비 (km/L) | 15.5 |
| `imageUrl` | string | ❌ | 차량 이미지 URL | "https://..." |
| `remarks` | string | ❌ | 비고 | "신규 등록" |

### CarResponse

| 필드 | 타입 | 설명 | 예시 |
|------|------|------|------|
| `id` | integer | 차량 고유 ID | 1 |
| `plateNo` | string | 차량 번호판 | "12가3456" |
| `carModelName` | string | 차량 모델명 | "현대 아반떼" |
| `imageUrl` | string | 차량 이미지 URL | "https://..." |
| `status` | string | 차량 상태 | "사용가능" |
| `carType` | string | 차량 유형 | "법인" |
| `fuelType` | string | 연료 유형 | "휘발유" |
| `efficiencyKmPerL` | string | 연비 | "15.50" |
| `createdAt` | string | 등록 일시 | "2025-08-20T10:55:20.245523" |
| `remarks` | string | 비고 | "신규 등록" |

### ShowCarsRequest

| 필드 | 타입 | 기본값 | 설명 |
|------|------|--------|------|
| `searchKeyword` | string | "" | 검색 키워드 |
| `status` | string | null | 차량 상태 필터 |
| `carType` | string | null | 차량 유형 필터 |
| `page` | integer | 0 | 페이지 번호 |
| `size` | integer | 10 | 페이지 크기 |
| `sortBy` | string | "createdAt" | 정렬 기준 |
| `sortDirection` | string | "DESC" | 정렬 방향 |

### ShowCarsResponse

| 필드 | 타입 | 설명 |
|------|------|------|
| `cars` | CarListItem[] | 차량 목록 |
| `pageInfo` | PageInfo | 페이지 정보 |
| `totalCount` | integer | 전체 차량 수 |

### CarListItem

| 필드 | 타입 | 설명 | 예시 |
|------|------|------|------|
| `id` | integer | 차량 ID | 1 |
| `plateNo` | string | 번호판 | "12가3456" |
| `carModelName` | string | 모델명 | "현대 아반떼" |
| `imageUrl` | string | 이미지 URL | "https://..." |
| `status` | string | 상태 | "사용가능" |
| `carType` | string | 유형 | "법인" |
| `fuelType` | string | 연료 | "휘발유" |
| `efficiencyKmPerL` | string | 연비 | "15.50" |
| `totalDistance` | string | 총 누적 거리 | "0km" |
| `monthlyDistance` | string | 이번 달 거리 | "0km" |
| `createdAt` | string | 등록일 | "2025-08-20T10:55:20.245523" |
| `remarks` | string | 비고 | "" |

### PageInfo

| 필드 | 타입 | 설명 |
|------|------|------|
| `currentPage` | integer | 현재 페이지 번호 |
| `pageSize` | integer | 페이지 크기 |
| `totalPages` | integer | 전체 페이지 수 |
| `hasNext` | boolean | 다음 페이지 존재 여부 |
| `hasPrevious` | boolean | 이전 페이지 존재 여부 |
| `totalElements` | integer | 전체 요소 수 |

## 차량 상태 및 유형

### 차량 상태 (status)
- `사용가능` - 사용 가능한 상태
- `사용중` - 현재 사용 중인 상태
- `정비중` - 정비 중인 상태
- `폐차` - 폐차된 상태
- `테스트` - 테스트용 차량

### 차량 유형 (carType)
- `법인` - 법인 소유 차량
- `개인` - 개인 소유 차량
- `렌트` - 렌트 차량

### 연료 유형 (fuelType)
- `휘발유` - 휘발유
- `경유` - 경유
- `LPG` - LPG
- `전기` - 전기
- `하이브리드` - 하이브리드

## 에러 응답

에러 발생 시 다음과 같은 형식으로 응답합니다:

```json
{
  "message": "에러 메시지",
  "error": "상세 에러 정보",
  "timestamp": "2025-08-20T10:55:20.245523",
  "status": 400
}
```

## HTTP 상태 코드

| 상태 코드 | 설명 |
|-----------|------|
| 200 | 성공 |
| 201 | 차량 등록 성공 |
| 400 | 잘못된 요청 (검증 오류) |
| 404 | 차량을 찾을 수 없음 |
| 500 | 내부 서버 오류 |

## 사용 예시

### 프론트엔드 연동

```javascript
// 차량 등록
const registerCar = async (carData) => {
  const response = await fetch('/api/v1/cars/register', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(carData),
  });
  
  const result = await response.json();
  
  if (response.ok) {
    return result.data;
  } else {
    throw new Error(result.message);
  }
};

// 차량 목록 조회
const getCarList = async (params = {}) => {
  const queryString = new URLSearchParams(params).toString();
  const response = await fetch(`/api/v1/cars?${queryString}`);
  
  if (response.ok) {
    return await response.json();
  } else {
    throw new Error('차량 목록 조회에 실패했습니다.');
  }
};
```

### Analytics Service 연동

Car Service는 Analytics Service와 연동하여 차량별 운행 통계를 제공합니다:

- 차량 목록 조회 시 Analytics Service에서 통계 정보를 일괄 조회
- 총 누적 거리와 월간 운행 거리를 실시간으로 표시
- 페이지네이션된 차량 목록에 대해 효율적인 통계 데이터 조회

## 참고사항

- 모든 날짜/시간은 ISO-8601 형식으로 표시됩니다
- 거리 정보는 Analytics Service에서 실시간으로 조회됩니다
- 페이지네이션은 0부터 시작합니다
- 정렬 방향은 ASC(오름차순) 또는 DESC(내림차순)을 지원합니다
- 검색은 번호판과 차량명에 대해 부분 일치를 지원합니다
