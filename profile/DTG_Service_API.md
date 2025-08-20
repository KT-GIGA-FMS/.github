# DTG Service API 문서

## 개요
DTG Service는 차량의 실시간 위치 추적과 운행 관리를 담당하는 REST API 서비스입니다.

- **Base URL**: `http://{host}:8085/api/v1/dtg`
- **포트**: 8085
- **WebSocket**: `ws://{host}:8085/ws`
- **응답 형식**: `ApiResponse<T>` 래퍼가 포함된 JSON

## API 엔드포인트

### 1. 운행 시작

**POST** `/trips/start`

차량의 운행을 시작하고 실시간 추적을 시작합니다.

**요청 본문:** `TripStartRequest`

**응답:** `ApiResponse<String>` (Trip ID)

**요청 예시:**
```json
{
  "vehicleId": "CAR001",
  "plateNo": "12가3456",
  "driverId": "DRIVER001",
  "startLatitude": 37.5665,
  "startLongitude": 126.9780,
  "destination": "강남역",
  "purpose": "업무"
}
```

**응답 예시:**
```json
{
  "success": true,
  "message": "운행이 시작되었습니다.",
  "data": "b7291789-e65f-4707-bcc0-08509fba4ee7",
  "timestamp": "2025-08-20T20:44:49.763802"
}
```

**cURL 예시:**
```bash
curl -X POST "http://localhost:8085/api/v1/dtg/trips/start" \
  -H "Content-Type: application/json" \
  -d '{
    "vehicleId": "CAR001",
    "plateNo": "12가3456",
    "driverId": "DRIVER001",
    "startLatitude": 37.5665,
    "startLongitude": 126.9780,
    "destination": "강남역",
    "purpose": "업무"
  }'
```

### 2. 운행 종료

**POST** `/trips/end`

차량의 운행을 종료하고 추적을 중단합니다.

**요청 본문:** `TripEndRequest`

**응답:** `ApiResponse<String>`

**요청 예시:**
```json
{
  "vehicleId": "CAR001",
  "endLatitude": 37.4979,
  "endLongitude": 127.0276,
  "endReason": "목적지 도착",
  "notes": "정상 운행 완료"
}
```

**응답 예시:**
```json
{
  "success": true,
  "message": "운행이 종료되었습니다.",
  "data": "운행이 종료되었습니다.",
  "timestamp": "2025-08-20T20:45:41.317797"
}
```

**cURL 예시:**
```bash
curl -X POST "http://localhost:8085/api/v1/dtg/trips/end" \
  -H "Content-Type: application/json" \
  -d '{
    "vehicleId": "CAR001",
    "endLatitude": 37.4979,
    "endLongitude": 127.0276,
    "endReason": "목적지 도착",
    "notes": "정상 운행 완료"
  }'
```

### 3. 활성 운행 목록 조회

**GET** `/trips/active`

현재 활성 상태인 운행 목록을 조회합니다.

**응답:** `ApiResponse<Map<String, TripSession>>`

**응답 예시:**
```json
{
  "success": true,
  "message": "Success",
  "data": {
    "CAR001": {
      "tripId": "b7291789-e65f-4707-bcc0-08509fba4ee7",
      "vehicleId": "CAR001",
      "plateNo": "12가3456",
      "driverId": "DRIVER001",
      "startLatitude": 37.5665,
      "startLongitude": 126.9780,
      "destination": "강남역",
      "purpose": "업무",
      "startTime": "2025-08-20T20:44:49.747429",
      "endTime": null,
      "endLatitude": null,
      "endLongitude": null,
      "endReason": null,
      "active": true
    }
  },
  "timestamp": "2025-08-20T20:45:26.578053"
}
```

**cURL 예시:**
```bash
curl -X GET "http://localhost:8085/api/v1/dtg/trips/active"
```

### 4. 특정 차량 운행 상태 조회

**GET** `/trips/{vehicleId}`

특정 차량의 현재 운행 상태를 조회합니다.

**경로 매개변수:**
- `vehicleId` (string) - 차량 ID

**응답:** `ApiResponse<TripSession>`

**응답 예시:**
```json
{
  "success": true,
  "message": "Success",
  "data": {
    "tripId": "b7291789-e65f-4707-bcc0-08509fba4ee7",
    "vehicleId": "CAR001",
    "plateNo": "12가3456",
    "driverId": "DRIVER001",
    "startLatitude": 37.5665,
    "startLongitude": 126.9780,
    "destination": "강남역",
    "purpose": "업무",
    "startTime": "2025-08-20T20:44:49.747429",
    "endTime": null,
    "endLatitude": null,
    "endLongitude": null,
    "endReason": null,
    "active": true
  },
  "timestamp": "2025-08-20T20:45:34.520286"
}
```

**cURL 예시:**
```bash
curl -X GET "http://localhost:8085/api/v1/dtg/trips/CAR001"
```

### 5. 서비스 상태 확인

**GET** `/health`

DTG Service의 상태를 확인합니다.

**응답:** `ApiResponse<String>`

**응답 예시:**
```json
{
  "success": true,
  "message": "Success",
  "data": "DTG 서비스가 정상적으로 실행 중입니다.",
  "timestamp": "2025-08-20T20:47:06.073581"
}
```

**cURL 예시:**
```bash
curl -X GET "http://localhost:8085/api/v1/dtg/health"
```

## 데이터 전송 객체 (DTOs)

### TripStartRequest

| 필드 | 타입 | 필수 | 설명 | 예시 |
|------|------|------|------|------|
| `vehicleId` | string | ✅ | 차량 고유 ID | "CAR001" |
| `plateNo` | string | ✅ | 차량 번호판 | "12가3456" |
| `driverId` | string | ✅ | 운전자 ID | "DRIVER001" |
| `startLatitude` | number | ✅ | 시작 위도 | 37.5665 |
| `startLongitude` | number | ✅ | 시작 경도 | 126.9780 |
| `destination` | string | ❌ | 목적지 | "강남역" |
| `purpose` | string | ❌ | 운행 목적 | "업무" |

### TripEndRequest

| 필드 | 타입 | 필수 | 설명 | 예시 |
|------|------|------|------|------|
| `vehicleId` | string | ✅ | 차량 고유 ID | "CAR001" |
| `endLatitude` | number | ✅ | 종료 위도 | 37.4979 |
| `endLongitude` | number | ✅ | 종료 경도 | 127.0276 |
| `endReason` | string | ❌ | 종료 사유 | "목적지 도착" |
| `notes` | string | ❌ | 비고 | "정상 운행 완료" |

### TripSession

| 필드 | 타입 | 설명 | 예시 |
|------|------|------|------|
| `tripId` | string | 운행 고유 ID | "b7291789-e65f-4707-bcc0-08509fba4ee7" |
| `vehicleId` | string | 차량 ID | "CAR001" |
| `plateNo` | string | 번호판 | "12가3456" |
| `driverId` | string | 운전자 ID | "DRIVER001" |
| `startLatitude` | number | 시작 위도 | 37.5665 |
| `startLongitude` | number | 시작 경도 | 126.9780 |
| `destination` | string | 목적지 | "강남역" |
| `purpose` | string | 운행 목적 | "업무" |
| `startTime` | string | 시작 시간 | "2025-08-20T20:44:49.747429" |
| `endTime` | string | 종료 시간 | null |
| `endLatitude` | number | 종료 위도 | null |
| `endLongitude` | number | 종료 경도 | null |
| `endReason` | string | 종료 사유 | null |
| `active` | boolean | 활성 상태 | true |

### ApiResponse<T>

| 필드 | 타입 | 설명 | 예시 |
|------|------|------|------|
| `success` | boolean | 요청 성공 여부 | true |
| `message` | string | 응답 메시지 | "운행이 시작되었습니다." |
| `data` | T | 응답 데이터 | "b7291789-e65f-4707-bcc0-08509fba4ee7" |
| `timestamp` | string | 응답 타임스탬프 | "2025-08-20T20:44:49.763802" |

## WebSocket 연동

### 연결 정보
- **WebSocket URL**: `ws://localhost:8085/ws`
- **SockJS 지원**: `http://localhost:8085/ws` (SockJS)

### 구독 토픽

#### 1. 실시간 추적 데이터
```
/topic/tracking/{vehicleId}
```
**메시지 타입:** `TrackingData`

**예시:**
```json
{
  "vehicleId": "CAR001",
  "plateNo": "12가3456",
  "latitude": 37.5665,
  "longitude": 126.9780,
  "speed": 45.2,
  "heading": 180.5,
  "altitude": 55.0,
  "fuelLevel": 85.0,
  "engineStatus": "ON",
  "timestamp": "2025-08-20T20:47:43.345409",
  "tripId": "b7291789-e65f-4707-bcc0-08509fba4ee7"
}
```

## 실시간 데이터 전송

### 자동 데이터 생성
- **주기**: 1초마다 자동 생성
- **데이터 타입**: GPS 위치, 속도, 방향, 고도, 연료 레벨, 엔진 상태
- **전송 대상**: 
  - Car Tracking Service (REST API)
  - 프론트엔드 (WebSocket)

### 데이터 시뮬레이션
운행 중인 차량에 대해 다음과 같은 데이터를 자동 생성합니다:
- **위치 변화**: 시작 위치 기준 ±0.001도 범위 내 랜덤 이동
- **속도**: 30-70 km/h 범위
- **방향**: 0-360도 범위
- **고도**: 50-70m 범위
- **연료 레벨**: 80-100% 범위

## 서비스 연동

### 1. Car Tracking Service 연동
- **운행 시작/종료 알림**: REST API로 전송
- **실시간 추적 데이터**: 1초마다 REST API로 전송
- **연동 URL**: `http://localhost:8080/api/v1/tracking/*`

### 2. Analytics Service 연동
- **운행 완료 데이터**: 운행 종료 시 자동 전송
- **데이터 형식**: 운행 거리, 시간, 경로 정보

### 3. 프론트엔드 연동
- **WebSocket**: 실시간 추적 데이터 전송
- **토픽**: `/topic/tracking/{vehicleId}`

## 프론트엔드 WebSocket 구현 예시

### React + TypeScript

```typescript
import SockJS from 'sockjs-client';
import { Client } from '@stomp/stompjs';

const useDtgWebSocket = (vehicleId: string) => {
  const [isConnected, setIsConnected] = useState(false);
  const [trackingData, setTrackingData] = useState<TrackingData | null>(null);

  useEffect(() => {
    const client = new Client({
      webSocketFactory: () => new SockJS('http://localhost:8085/ws'),
      debug: (str) => console.log(str),
      reconnectDelay: 5000,
    });

    client.onConnect = () => {
      console.log('DTG WebSocket 연결 성공!');
      setIsConnected(true);
      
      // 실시간 추적 데이터 구독
      client.subscribe(`/topic/tracking/${vehicleId}`, (message) => {
        const data: TrackingData = JSON.parse(message.body);
        setTrackingData(data);
        console.log('실시간 추적 데이터:', data);
      });
    };

    client.onDisconnect = () => {
      console.log('DTG WebSocket 연결 해제');
      setIsConnected(false);
    };

    client.activate();

    return () => {
      client.deactivate();
    };
  }, [vehicleId]);

  return { isConnected, trackingData };
};
```

## 에러 응답

에러 발생 시 다음과 같은 형식으로 응답합니다:

```json
{
  "success": false,
  "message": "운행 시작에 실패했습니다: 차량 ID가 유효하지 않습니다.",
  "data": null,
  "timestamp": "2025-08-20T20:44:49.763802"
}
```

## HTTP 상태 코드

| 상태 코드 | 설명 |
|-----------|------|
| 200 | 성공 |
| 201 | 운행 시작 성공 |
| 400 | 잘못된 요청 (검증 오류) |
| 404 | 차량을 찾을 수 없음 |
| 500 | 내부 서버 오류 |

## 데이터 흐름

### 1. 운행 시작
```
클라이언트 → DTG Service (운행 시작 요청)
    ↓
TripSession 생성 및 활성 운행 목록에 추가
    ↓
Car Tracking Service에 운행 시작 알림 전송
    ↓
1초마다 실시간 데이터 생성 및 전송 시작
    ↓
프론트엔드에 WebSocket으로 실시간 데이터 전송
```

### 2. 실시간 추적
```
DTG Service (1초마다 자동 실행)
    ↓
GPS, 속도, 방향 등 추적 데이터 생성
    ↓
Car Tracking Service로 REST API 전송
    ↓
프론트엔드로 WebSocket 전송
```

### 3. 운행 종료
```
클라이언트 → DTG Service (운행 종료 요청)
    ↓
TripSession 비활성화 및 종료 정보 업데이트
    ↓
Car Tracking Service에 운행 종료 알림 전송
    ↓
Analytics Service에 운행 완료 데이터 전송
    ↓
실시간 데이터 생성 및 전송 중단
```

## 환경 변수

| 변수명 | 기본값 | 설명 |
|--------|--------|------|
| `REDIS_HOST` | localhost | Redis 서버 호스트 |
| `REDIS_PORT` | 6379 | Redis 서버 포트 |
| `CAR_TRACKING_SERVICE_URL` | http://localhost:8080 | Car Tracking Service URL |

## 참고사항

- 모든 날짜/시간은 ISO-8601 형식으로 표시됩니다
- 위도/경도는 소수점 6자리까지 지원합니다
- 속도는 km/h 단위입니다
- 방향은 0-360도 범위입니다
- WebSocket 연결은 자동 재연결을 지원합니다
- 실시간 데이터는 운행 중인 차량에 대해서만 생성됩니다
- 운행 ID는 UUID 형식으로 자동 생성됩니다
- Car Tracking Service와의 연동 실패 시에도 DTG Service는 정상 작동합니다
