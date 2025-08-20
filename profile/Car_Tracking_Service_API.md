# Car Tracking Service API 문서

## 개요
Car Tracking Service는 차량의 실시간 위치 추적과 운행 데이터 관리를 담당하는 REST API 서비스입니다.

- **Base URL**: `http://{host}:8080/api/v1/tracking`
- **포트**: 8080
- **WebSocket**: `ws://{host}:8080/ws`
- **응답 형식**: `ApiResponse<T>` 래퍼가 포함된 JSON

## API 엔드포인트

### 1. DTG 연동 - 운행 시작 알림

**POST** `/trips/start`

DTG Service에서 운행 시작 알림을 받아 처리합니다.

**요청 본문:** `TripStartRequest`

**응답:** `ApiResponse<String>`

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
  "message": "운행 시작이 처리되었습니다.",
  "data": "운행 시작이 처리되었습니다.",
  "timestamp": "2025-08-20T20:47:19.704810"
}
```

**cURL 예시:**
```bash
curl -X POST "http://localhost:8080/api/v1/tracking/trips/start" \
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

### 2. DTG 연동 - 운행 종료 알림

**POST** `/trips/end`

DTG Service에서 운행 종료 알림을 받아 처리합니다.

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
  "message": "운행 종료가 처리되었습니다.",
  "data": "운행 종료가 처리되었습니다.",
  "timestamp": "2025-08-20T20:47:44.326865"
}
```

**cURL 예시:**
```bash
curl -X POST "http://localhost:8080/api/v1/tracking/trips/end" \
  -H "Content-Type: application/json" \
  -d '{
    "vehicleId": "CAR001",
    "endLatitude": 37.4979,
    "endLongitude": 127.0276,
    "endReason": "목적지 도착",
    "notes": "정상 운행 완료"
  }'
```

### 3. DTG 연동 - 실시간 추적 데이터

**POST** `/data`

DTG Service에서 실시간 추적 데이터를 받아 처리합니다.

**요청 본문:** `TrackingData`

**응답:** `ApiResponse<String>`

**요청 예시:**
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
  "tripId": "c9eaa2bf-c38e-4aca-8756-cced7b659bfd"
}
```

**응답 예시:**
```json
{
  "success": true,
  "message": "추적 데이터가 처리되었습니다.",
  "data": "추적 데이터가 처리되었습니다.",
  "timestamp": "2025-08-20T20:47:43.345409"
}
```

**cURL 예시:**
```bash
curl -X POST "http://localhost:8080/api/v1/tracking/data" \
  -H "Content-Type: application/json" \
  -d '{
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
    "tripId": "c9eaa2bf-c38e-4aca-8756-cced7b659bfd"
  }'
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

### TrackingData

| 필드 | 타입 | 필수 | 설명 | 예시 |
|------|------|------|------|------|
| `vehicleId` | string | ✅ | 차량 고유 ID | "CAR001" |
| `plateNo` | string | ✅ | 차량 번호판 | "12가3456" |
| `latitude` | number | ✅ | 현재 위도 | 37.5665 |
| `longitude` | number | ✅ | 현재 경도 | 126.9780 |
| `speed` | number | ❌ | 속도 (km/h) | 45.2 |
| `heading` | number | ❌ | 방향 (0-360도) | 180.5 |
| `altitude` | number | ❌ | 고도 (m) | 55.0 |
| `fuelLevel` | number | ❌ | 연료 레벨 (%) | 85.0 |
| `engineStatus` | string | ❌ | 엔진 상태 | "ON" |
| `timestamp` | string | ✅ | 타임스탬프 | "2025-08-20T20:47:43.345409" |
| `tripId` | string | ✅ | 운행 ID | "c9eaa2bf-c38e-4aca-8756-cced7b659bfd" |

### ApiResponse<T>

| 필드 | 타입 | 설명 | 예시 |
|------|------|------|------|
| `success` | boolean | 요청 성공 여부 | true |
| `message` | string | 응답 메시지 | "운행 시작이 처리되었습니다." |
| `data` | T | 응답 데이터 | "운행 시작이 처리되었습니다." |
| `timestamp` | string | 응답 타임스탬프 | "2025-08-20T20:47:19.704810" |

## WebSocket 연동

### 연결 정보
- **WebSocket URL**: `ws://localhost:8080/ws`
- **SockJS 지원**: `http://localhost:8080/ws` (SockJS)

### 구독 토픽

#### 1. 운행 시작 알림
```
/topic/trips/{vehicleId}/start
```
**메시지 타입:** `TripStartRequest`

**예시:**
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

#### 2. 운행 종료 알림
```
/topic/trips/{vehicleId}/end
```
**메시지 타입:** `TripEndRequest`

**예시:**
```json
{
  "vehicleId": "CAR001",
  "endLatitude": 37.4979,
  "endLongitude": 127.0276,
  "endReason": "목적지 도착",
  "notes": "정상 운행 완료"
}
```

#### 3. 실시간 추적 데이터
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
  "tripId": "c9eaa2bf-c38e-4aca-8756-cced7b659bfd"
}
```

#### 4. 개별 차량 위치 업데이트
```
/topic/vehicle/{vehicleId}
```
**메시지 타입:** `CarTrackingResponse`

#### 5. 전체 차량 업데이트
```
/topic/vehicles/all
```
**메시지 타입:** `CarTrackingResponse`

## 프론트엔드 WebSocket 구현 예시

### React + TypeScript

```typescript
import SockJS from 'sockjs-client';
import { Client } from '@stomp/stompjs';

const useCarTrackingWebSocket = () => {
  const [isConnected, setIsConnected] = useState(false);
  const [trackingData, setTrackingData] = useState<Map<string, TrackingData>>(new Map());

  useEffect(() => {
    const client = new Client({
      webSocketFactory: () => new SockJS('http://localhost:8080/ws'),
      debug: (str) => console.log(str),
      reconnectDelay: 5000,
    });

    client.onConnect = () => {
      console.log('Car Tracking WebSocket 연결 성공!');
      setIsConnected(true);
      
      // 실시간 추적 데이터 구독
      client.subscribe('/topic/tracking/CAR001', (message) => {
        const data: TrackingData = JSON.parse(message.body);
        setTrackingData(prev => new Map(prev).set(data.vehicleId, data));
      });

      // 운행 시작 알림 구독
      client.subscribe('/topic/trips/CAR001/start', (message) => {
        const data: TripStartRequest = JSON.parse(message.body);
        console.log('운행 시작:', data);
      });

      // 운행 종료 알림 구독
      client.subscribe('/topic/trips/CAR001/end', (message) => {
        const data: TripEndRequest = JSON.parse(message.body);
        console.log('운행 종료:', data);
      });
    };

    client.activate();

    return () => {
      client.deactivate();
    };
  }, []);

  return { isConnected, trackingData };
};
```

## 에러 응답

에러 발생 시 다음과 같은 형식으로 응답합니다:

```json
{
  "success": false,
  "message": "운행 시작 처리에 실패했습니다: 차량 ID가 유효하지 않습니다.",
  "data": null,
  "timestamp": "2025-08-20T20:47:19.704810"
}
```

## HTTP 상태 코드

| 상태 코드 | 설명 |
|-----------|------|
| 200 | 성공 |
| 400 | 잘못된 요청 (검증 오류) |
| 500 | 내부 서버 오류 |

## 데이터 흐름

### 1. 운행 시작
```
DTG Service → Car Tracking Service (REST API)
    ↓
Redis에 운행 시작 정보 저장
    ↓
프론트엔드에 WebSocket으로 알림
    ↓
Analytics Service에 운행 시작 알림
```

### 2. 실시간 추적
```
DTG Service → Car Tracking Service (REST API)
    ↓
Redis에 실시간 데이터 저장
    ↓
프론트엔드에 WebSocket으로 실시간 전송
```

### 3. 운행 종료
```
DTG Service → Car Tracking Service (REST API)
    ↓
Redis에 운행 종료 정보 저장
    ↓
프론트엔드에 WebSocket으로 알림
    ↓
Analytics Service에 운행 완료 데이터 전송
```

## Redis 데이터 구조

### 저장되는 데이터
- **운행 시작 정보**: `trip:start:{vehicleId}`
- **운행 종료 정보**: `trip:end:{vehicleId}`
- **실시간 위치**: `vehicle:latest:{vehicleId}`
- **위치 이력**: `vehicle:history:{vehicleId}` (최근 600개)
- **활성 차량 목록**: `vehicles:ids`

## 참고사항

- 모든 날짜/시간은 ISO-8601 형식으로 표시됩니다
- 위도/경도는 소수점 6자리까지 지원합니다
- 속도는 km/h 단위입니다
- 방향은 0-360도 범위입니다
- WebSocket 연결은 자동 재연결을 지원합니다
- Redis에 저장된 데이터는 자동으로 TTL 관리됩니다
- 프론트엔드와의 실시간 통신을 위해 WebSocket을 우선 사용합니다
