## 0. 🚗 법인 차량 관제 서비스 (KT DTG FMS) 프로젝트

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


  
## 🗂 (4) 핵심 이벤트 플로우 & MSA 보드
👉 [MSA 보드 보기](URL_기입)  

---

## 3. 각 서비스 리포 링크 & One-line Mission

| 서비스 | 리포지토리 | One-line Mission |
|--------|------------|------------------|
| 🚘 Car Service | [car-service](URL_기입) | 차량 정보 등록/조회/상태 관리 |
| 👤 User Service | [user-service](URL_기입) | 사용자·권한 관리 |
| 📅 Reservation Service | [reservation-service](URL_기입) | 차량 예약/배차 관리 |
| 📍 Tracking Service | [tracking-service](URL_기입) | 실시간 위치 데이터 수집 |
| 🔌 Realtime Gateway | [realtime-gateway](URL_기입) | 실시간 WS/SSE 브로드캐스트 |
| 📊 Driving Service | [driving-service](URL_기입) | 운행 이력 및 통계 관리 |
| ⚙️ ETL Worker | [etl-worker](URL_기입) | Redis→RDB/NoSQL ETL 파이프라인 |

---

## 4. ERD 산출물
- **ERD (RDB - PostgreSQL)**  
  👉 [ERD 보기](URL_기입)  

- **Document Schema (NoSQL - CosmosDB/Mongo)**  
  👉 [Schema 보기](URL_기입)  

- **Redis/Memcached Key 설계표**  
  👉 [Key 설계표](URL_기입)  

---

## 5.아키텍처 & API 계약

- **아키텍처 설계도**  
  👉 [아키텍처 다이어그램](URL_기입)  

- **API 계약 (엔드포인트 + DTO)**  
  👉 [API 명세](URL_기입)  

---

## 6. ADR (Architecture Decision Records)

- ADR-001: PostgreSQL 선택 이유  
- ADR-002: Redis Stream/ZSET 사용 근거  
- ADR-003: NoSQL 활용 목적  
- ADR-004: Azure APIM 도입  
- ADR-005: WebSocket/SSE 실시간 처리  

👉 [ADR 전체 보기](URL_기입)  

---

## 🌐 공유 링크
- [github projects](https://github.com/orgs/KT-GIGA-FMS/projects/3)
- [Notion](https://www.notion.so/KT-250de2e870a180fbb45ef1ea86d7874e?source=copy_link)
- [미로보드](https://miro.com/app/board/uXjVJT7T_I8=/)
- [DB/ERD](https://www.erdcloud.com/d/aqyjwmcWrcmSoYD4S)
- [기획서 초안](https://docs.google.com/document/d/1HPhWYdSCaW_UycXsQGsRFQ6wy3SvAwCacChCETTccKY/edit?tab=t.0)
- [FP 산정 시트 초안](https://docs.google.com/spreadsheets/d/11p7Wmf7TJH4ZsfxWPAp8_TwIe4qGc4qaDQb3Sh7QISA/edit?gid=2082954959#gid=2082954959)

---

- [아이디어 모음집](https://github.com/KT-GIGA-FMS/.github/blob/main/profile/ROADMAP.md)
- 컨벤션
  - <img width="300" height="120" alt="image" src="https://github.com/user-attachments/assets/b45a1627-055f-4919-b00e-7c92a51a6660" />
  - <img width="240" height="85" alt="image" src="https://github.com/user-attachments/assets/6d113334-b8cf-4f21-b36b-aa502fdc4e9e" />

