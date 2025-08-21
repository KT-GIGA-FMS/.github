## 🚗 법인 차량 관제 서비스 (KT DTG FMS) 프로젝트

실시간 차량 위치 추적 · 예약 관리 · 운행 이력 분석을 제공하는 MSA 기반 법인 차량 관제 플랫폼
- [프로젝트 기획서](https://docs.google.com/document/d/1HPhWYdSCaW_UycXsQGsRFQ6wy3SvAwCacChCETTccKY/edit?tab=t.0)

  
---

## 1. FP (Function Point) 산출물

- [FP 산출표](https://docs.google.com/spreadsheets/d/11p7Wmf7TJH4ZsfxWPAp8_TwIe4qGc4qaDQb3Sh7QISA/edit?usp=sharing)

  
---
## 2. 프로젝트 특장점 및 근거

- 🚀 **스트리밍 데이터 처리**: Redis Stream & Pub/Sub 기반

  ```웹 소켓 기반 실시간 푸시를 통해 1-2초 내 시각화로 연결을 안정적이고 빠르게 처리해 보여줄 수 있습니다.```


- 🛰 **실시간 위치 추적**: WebSocket + 카카오/네이버 맵 API

  
  ```실시간 좌표를 받아 폴리라인으로 경로를 그려 재생·하이라이트합니다. NAVER JS SDK의 폴리라인·오버레이 이벤트를 활용해 인터랙션을 강화했습니다```


- 🧩 **MSA 기반 설계**: 서비스별 독립 배포 + Azure APIM

  ``` 마이크로서비스를 독립 배포 가능하게 설계해 민첩성과 확장성을 확보했습니다. APIM의 rate-limit(및 by-key) 정책으로 등급별 트래픽 제어와 안전한 공개를 적용했습니다.```


- 📊 **분석 대시보드**:

  
  ``` 운행 거리, 경로 추적, 출발지 및 도착지 관리와 통계 데이터를 보여줍니다. 출발·도착 지점과 이동 거리는 DB에서 정확하게 계산해, 운전자·차량별 핵심 지표를 실시간으로 확인할 수 있습니다.```


  
## 3. 핵심 이벤트 플로우 & MSA 보드
👉 [MSA 보드 보기](URL_기입)  

---

## 4. 각 서비스 리포 링크 & One-line Mission

| 서비스                     | 리포지토리                                                                       | One-line Mission              |
| ----------------------- | --------------------------------------------------------------------------- | ----------------------------- |
| 🌐 Front-End            | [front-end](https://github.com/KT-GIGA-FMS/front-end)                       | 관리자와 운전자가 이용할 수 있는 웹 인터페이스 제공 |
| 🚘 Car Service          | [car-service](https://github.com/KT-GIGA-FMS/car-service)                   | 차량 정보 등록/조회/상태 관리             |
| 👤 User Service         | [user-service](https://github.com/KT-GIGA-FMS/user-service)                 | 사용자 계정/권한 관리                  |
| 📍 Car Tracking Service | [car-tracking-service](https://github.com/KT-GIGA-FMS/car-tracking-service) | 차량 위치 수집 및 실시간 스트리밍           |
| 📊 Analytics Service    | [analytics-service](https://github.com/KT-GIGA-FMS/analytics-service)       | 운행 데이터 분석 및 대시보드 제공           |
| 🤖 AI Agent             | [ai-agent](https://github.com/KT-GIGA-FMS/ai-agent)                         | 차량 예약/배차 요청 처리 AI 대화형 에이전트    |
| 🚗 DTG                  | [DTG](https://github.com/KT-GIGA-FMS/DTG)                                   | 차량 디지털 운행기록장치 연동              |
| ⚙️ .github              | [.github](https://github.com/KT-GIGA-FMS/.github)                           | 공통 워크플로우 및 설정 관리              |


## 5. ERD 산출물

<img width="880" height="812" alt="ERD_kt-dtg-fms" src="https://github.com/user-attachments/assets/a91a0be9-28a8-4194-8a5c-615d6811b4c8" />



---

## 6.아키텍처 & API 계약
![헤드셋 원정대 - 프레임 1](https://github.com/user-attachments/assets/4fbee7e5-7483-411e-a870-4e6e4922c3c8)


- **API 계약 (엔드포인트 + DTO)**  
### [🚗 Car Service (8080)](https://github.com/KT-GIGA-FMS/.github/blob/main/profile/Car_Service_API.md) 
```
POST   /api/v1/cars/register          - 차량 등록
GET    /api/v1/cars                   - 차량 목록 조회 (페이지네이션)
```

### [📊 Analytics Service (8083)](https://github.com/KT-GIGA-FMS/.github/blob/main/profile/Analytics_Service_API.md)
```
GET    /api/v1/analytics/vehicles/{id}/statistics    - 차량 통계 조회
POST   /api/v1/analytics/vehicles/statistics/batch  - 차량 통계 일괄 조회
```

### [🚀 DTG Service (8085)](https://github.com/KT-GIGA-FMS/.github/blob/main/profile/DTG_Service_API.md)
```
POST   /api/v1/dtg/trips/start        - 운행 시작
POST   /api/v1/dtg/trips/end          - 운행 종료
GET    /api/v1/dtg/trips/active       - 활성 운행 목록
GET    /api/v1/dtg/trips/{id}         - 특정 차량 운행 상태
GET    /api/v1/dtg/health             - 서비스 상태
```

### [📍 Car Tracking Service (8080)](https://github.com/KT-GIGA-FMS/.github/blob/main/profile/Car_Tracking_Service_API.md)
```
POST   /api/v1/tracking/trips/start   - DTG 운행 시작 수신
POST   /api/v1/tracking/trips/end     - DTG 운행 종료 수신
POST   /api/v1/tracking/data          - DTG 실시간 데이터 수신
```

### 🌐 WebSocket Endpoints
```
DTG Service:        ws://localhost:8085/ws
Car Tracking:       ws://localhost:8080/ws
```

### 🧰Swagger UI
```
Car Service:        http://localhost:8080/swagger-ui.html
Analytics:          http://localhost:8083/swagger-ui.html
DTG Service:        http://localhost:8085/swagger-ui.html
```

---

## 7. ADR (Architecture Decision Records)

**(1) ADR-001: RDB로 PostgreSQL 선택**

- Context: 예약 중복 방지와 시간 범위 쿼리가 많음
- Alternatives: MySQL(팀에 익숙), 기능적 차이는 크지 않음
- Decision: 수업에서 다룬 경험과 학습을 고려해 PostgreSQL 선택
- Consequence: 안정적인 트랜잭션 처리와 시계열 쿼리 지원



**(2) ADR-002: 실시간 파이프는 Redis (Stream + ZSET)**

- Context: 법인 차량 약 100대 관제 규모에는 Redis 성능 충분
- Alternatives: Kafka(대규모에 적합) → 현재는 오버엔지니어링
- Decision: Redis Streams + ZSET
- Consequence: 운영 단순, 필요 시 대규모 확장 가능



**(3) ADR-003: API Gateway는 Azure APIM**

- Context: 키 관리, 레이트리밋, CORS 중앙화 필요
- Alternatives: Kong, NGINX, Tyk, AWS API Gateway 등 존재
- Decision: 가장 빠르게 도입 가능한 Azure APIM 선택
- Consequence: 서비스별 독립 배포, 보안정책 일원화



**(4) ADR-004: 실시간 전송은 WebSocket, 대체는 SSE**

- Context: 지도 실시간 업데이트
- Alternatives: Polling(비효율)
- Decision: WS 우선, 장애 시 SSE 폴백
- Consequence: 방화벽/프록시 호환성 개선



**(5) ADR-005: 예약 겹침 방지는 코드 로직으로 처리**

- Context: 분산 환경에서 동일 시간대에 중복 예약 시도 가능성 존재
- Alternatives: DB 제약 조건(EXCLUDE USING GIST)으로 강제, 다만 적용 난이도와 학습 부담 있음
- Decision: 현재는 애플리케이션 코드 레벨에서 겹침 검사 로직을 구현하고, 추후 운영 안정화 단계에서 DB 제약 조건 적용 예정
- Consequence: 초기 개발과 적용이 용이, 단 코드 레벨에서의 레이스 컨디션 위험은 존재



---

## 🌐 공유 링크
- [github projects](https://github.com/orgs/KT-GIGA-FMS/projects/3)
  - <img width="1912" height="1036" alt="image" src="https://github.com/user-attachments/assets/6df38afe-7d37-4b4e-8b93-cf450789523c" />

- [Notion](https://www.notion.so/KT-250de2e870a180fbb45ef1ea86d7874e?source=copy_link)
- [미로보드](https://miro.com/app/board/uXjVJT7T_I8=/)
- [DB/ERD](https://www.erdcloud.com/d/aqyjwmcWrcmSoYD4S)
- [기획서](https://docs.google.com/document/d/1HPhWYdSCaW_UycXsQGsRFQ6wy3SvAwCacChCETTccKY/edit?tab=t.0)
- [FP 산정 시트](https://docs.google.com/spreadsheets/d/11p7Wmf7TJH4ZsfxWPAp8_TwIe4qGc4qaDQb3Sh7QISA/edit?gid=2082954959#gid=2082954959)

---

- [아이디어 모음집](https://github.com/KT-GIGA-FMS/.github/blob/main/profile/ROADMAP.md)
- 컨벤션
  - 이슈 컨벤션
  ```
  [라벨명] 이슈 이름
  
  ### 💬 기능 설명
  > 기능에 대해 간략히 적어주세요.
  
  ### 🎯 구현 내용
  - [ ] TODO
  - [ ] TODO
  - [ ] TODO
  ```
  - 🌱 브랜치 컨벤션

  ```
  main                  : 메인 브랜치
  deploy                : 단위 배포 브랜치
  dev                   : 개발 통합 브랜치
  feat/#번호-설명         : 기능 개발 브랜치
  ```

  - 📝 커밋 컨벤션
  
  ```
  [타입/#이슈번호]: 작업 내용
  
  - 상세 변경 사항 1
  - 상세 변경 사항 2
  ```
  
