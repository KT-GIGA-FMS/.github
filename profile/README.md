# 🚗 법인 차량 관제 서비스 (KT DTG FMS) 프로젝트
## 🚀 실시간 차량 관제 · 예약 관리 · 운행 이력 분석 서비스를 제공하는 MSA 기반 법인 차량 관제 플랫폼입니다.


* DTG : Digital Tacho Graph (디지털 운행 기록 장치)
* FMS : Fleet Management System (통합 관리 서비스)

---

## 📑 목차

- [0. 소개](#0-소개)
- [1. 서비스 아키텍처 및 핵심 이벤트 플로우](#1-서비스-아키텍처-및-핵심-이벤트-플로우)
- [2. 서비스별 Repo 링크 및 실행 방법](#2-서비스별-Repo-링크-및-실행-방법)
- [3. ERD 계약](#3-ERD-계약)
- [4. 데이터 모델](#4-데이터-모델)
- [5. 회복탄력성 & 관측](#5-회복탄력성-&-관측)
- [6. 데모 시나리오](#6-데모-시나리오)
- [7. 설계 - 구현 정합성, 변경 이력](#7-설계-구현-정합성-변경-이력)
- [8. 협업, 품질 운영](#8-협업-품질-운영)
- [9. 공유 자료 모음](#9-공유-자료-모음)

---

## 0. 소개
- **주요 기능**
  - 실시간 차량 운행 현황 확인
  - 차량 예약 AI Agent
  - 차량 정보 관리
  - 운전자 정보 관리
  - 운행 정보 분석
  
- [데모 사이트](https://front-end-service-g6hghwc7ecgkathb.koreacentral-01.azurewebsites.net/car/cars)

- 팀원 소개
  - [길가은](https://github.com/rlfrkdms1)
  - [김예은](https://github.com/yeni28)
  - [장현우](https://github.com/J-nowcow)

### 프로젝트 특장점
  
  - 🚀 **스트리밍 데이터 처리**: Redis Stream & Pub/Sub 기반
  
    ```웹 소켓 기반 실시간 푸시를 통해 1-2초 내 시각화로 연결을 안정적이고 빠르게 처리해 보여줄 수 있습니다.```
  
  
  - 🛰 **실시간 위치 추적**: WebSocket + 카카오/네이버 맵 API
  
    
    ```실시간 좌표를 받아 폴리라인으로 경로를 그려 재생·하이라이트합니다. NAVER JS SDK의 폴리라인·오버레이 이벤트를 활용해 인터랙션을 강화했습니다```
  
  
  - 🧩 **MSA 기반 설계**: 서비스별 독립 배포 + Azure APIM
  
    ``` 마이크로서비스를 독립 배포 가능하게 설계해 민첩성과 확장성을 확보했습니다. APIM의 rate-limit(및 by-key) 정책으로 등급별 트래픽 제어와 안전한 공개를 적용했습니다.```
  
  
  - 📊 **분석 대시보드**:
  
    
    ``` 운행 거리, 경로 추적, 출발지 및 도착지 관리와 통계 데이터를 보여줍니다. 출발·도착 지점과 이동 거리는 DB에서 정확하게 계산해, 운전자·차량별 핵심 지표를 실시간으로 확인할 수 있습니다.```
  
  - 🤖 **AI Agent 기반 차량 예약**:
  
  
    ``` 자연어 입력만으로 예약·취소가 가능하며, LLM 기반 의도 파악으로 사용성을 크게 개선했습니다. Redis 캐시를 활용하여 대화의 맥락을 이어갑니다. 또한 필요 시 Agent가 자체 툴(ex: 차량 가능 여부 조회, 기존 예약 검증)을 스스로 호출하여 단순 LLM 추론보다 정확하고 일관된 결과를 제공합니다. ```

---

## 1. 서비스 아키텍처 및 핵심 이벤트 플로우

- **서비스 아키텍처**
![헤드셋 원정대 - 시스템 아키텍처](https://github.com/user-attachments/assets/9dc58135-5e4e-4c8c-bd9a-49a12f86ff24)

- **핵심 이벤트 플로우**
![헤드셋 원정대 - 핵심 이벤트 플로우 (1)](https://github.com/user-attachments/assets/986200c7-cd11-4c0b-a037-e7cc8266df82)

- icon 출처
  - <a href="https://www.flaticon.com/kr/free-icons/" title="관리자 아이콘">관리자 아이콘 제작자: itim2101 - Flaticon</a>
  - <a href="https://www.flaticon.com/free-icons/car" title="car icons">Car icons created by Freepik - Flaticon</a>
  - <a href="https://www.flaticon.com/kr/free-icons/" title="운전 아이콘">운전 아이콘 제작자: Freepik - Flaticon</a>
  

- Azure 서비스 스펙
  - Azure App Service
    - <img width="1676" height="219" alt="image" src="https://github.com/user-attachments/assets/5543ab23-593c-4364-ae33-ccda7cb9197d" />
  - Azure Cache Redis
    - <img width="1739" height="103" alt="image" src="https://github.com/user-attachments/assets/d3cc4213-a0ae-40c9-b7f7-2929a0064eae" />
  - Azure Openai
    - <img width="1639" height="98" alt="image" src="https://github.com/user-attachments/assets/2475fb9e-5866-4c20-a26c-fe6f7a9e2929" />
    - <img width="1682" height="297" alt="image" src="https://github.com/user-attachments/assets/9981bb34-06a3-41d4-89fc-4242ed7f4b44" />


---

## 2. 서비스별 Repo 링크 및 실행 방법

- **각 서비스 리포지토리 링크 & One-line Mission**

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

CI/CD의 주요 특징
1. 자동화된 배포 파이프라인
  - Git push → 자동 빌드 → 자동 배포
  - 수동 실행도 가능 (workflow_dispatch)
2. 컨테이너 기반 배포
  - Docker 이미지로 일관된 환경 보장
  - Azure Web App for Containers 활용
3. 품질 보증
  - 헬스체크를 통한 서비스 상태 모니터링
  - API 테스트를 통한 기능 검증
  - 배포 실패 시 자동 롤백 가능
4. 보안 강화
  - GitHub Secrets를 통한 민감 정보 관리
  - non-root 사용자로 컨테이너 실행
  - 환경 변수를 통한 설정 분리
---

## 3. API 계약

- [car-tracking-service Swagger](https://car-tracking-service-hmarhqf6a0f8abb4.koreacentral-01.azurewebsites.net/swagger-ui/index.html)
- [user-service Swagger](https://user-service-fbh3ctfghxhzerbq.koreacentral-01.azurewebsites.net/swagger-ui/index.html)
- [car-services Swagger](https://car-services-e6bmdpbjcffqfzd2.koreacentral-01.azurewebsites.net/swagger-ui/index.html)
- [analytics-service Swagger](https://analytics-service-dngpgmgkbph2adbu.koreacentral-01.azurewebsites.net/swagger-ui/index.html)
- [ai-agent Swagger](https://ai-agent-b9asf5aebwajamfy.koreacentral-01.azurewebsites.net/docs#/)

- API 계약 (엔드포인트 + DTO)
  ### [🚗 Car Service](https://github.com/KT-GIGA-FMS/.github/blob/main/profile/Car_Service_API.md) 
  ```
  POST   /api/v1/cars/register          - 차량 등록
  GET    /api/v1/cars                   - 차량 목록 조회 (페이지네이션)
  ```
  
  ### [📊 Analytics Service](https://github.com/KT-GIGA-FMS/.github/blob/main/profile/Analytics_Service_API.md)
  ```
  GET    /api/v1/analytics/vehicles/{id}/statistics    - 차량 통계 조회
  POST   /api/v1/analytics/vehicles/statistics/batch  - 차량 통계 일괄 조회
  ```
  
  ### [🚀 DTG Service](https://github.com/KT-GIGA-FMS/.github/blob/main/profile/DTG_Service_API.md)
  ```
  POST   /api/v1/dtg/trips/start        - 운행 시작
  POST   /api/v1/dtg/trips/end          - 운행 종료
  GET    /api/v1/dtg/trips/active       - 활성 운행 목록
  GET    /api/v1/dtg/trips/{id}         - 특정 차량 운행 상태
  GET    /api/v1/dtg/health             - 서비스 상태
  ```
  
  ### [📍 Car Tracking Service](https://github.com/KT-GIGA-FMS/.github/blob/main/profile/Car_Tracking_Service_API.md)
  ```
  POST   /api/v1/tracking/trips/start   - DTG 운행 시작 수신
  POST   /api/v1/tracking/trips/end     - DTG 운행 종료 수신
  POST   /api/v1/tracking/data          - DTG 실시간 데이터 수신
  ```

## 4. 데이터 모델

- [ERDCloud](https://www.erdcloud.com/d/aqyjwmcWrcmSoYD4S)

<img width="880" height="812" alt="ERD_kt-dtg-fms" src="https://github.com/user-attachments/assets/a91a0be9-28a8-4194-8a5c-615d6811b4c8" />


---

## 5. 회복탄력성 & 관측
**1) APIM 전역 정책 적용**
- CORS 설정: 모든 Origin(*), 주요 HTTP 메서드(GET/POST/PUT/PATCH/DELETE/OPTIONS), 헤더(*) 허용.
- Correlation-Id 전파: X-Correlation-Id가 없으면 UUID 생성 후 요청·응답·에러에 헤더 삽입.
- 백엔드 타임아웃: forward-request timeout="10" 적용.
- Preflight(OPTIONS) 처리: 프리플라이트 요청은 바로 응답(set-body("")).

**실제 적용한 Policy**
  ```
  <policies>
    <inbound>
      <set-variable name="cid" value="@(
          context.Request.Headers.ContainsKey("X-Correlation-Id")
            ? context.Request.Headers.GetValueOrDefault("X-Correlation-Id")
            : System.Guid.NewGuid().ToString()
        )" />
      <set-header name="X-Correlation-Id" exists-action="override">
        <value>@((string)context.Variables["cid"])</value>
      </set-header>
  
      <cors allow-credentials="false">
        <allowed-origins><origin>*</origin></allowed-origins>
        <allowed-methods>
          <method>GET</method><method>POST</method><method>PUT</method>
          <method>PATCH</method><method>DELETE</method><method>OPTIONS</method>
        </allowed-methods>
        <allowed-headers><header>*</header></allowed-headers>
        <expose-headers><header>*</header></expose-headers>
      </cors>
  
      <choose>
        <when condition="@((string)context.Request.Method == \"OPTIONS\")">
          <return-response>
            <set-body>@("")</set-body>
          </return-response>
        </when>
      </choose>
    </inbound>
  
    <backend>
      <forward-request timeout="10" />
    </backend>
  
    <outbound>
      <set-header name="X-Correlation-Id" exists-action="override">
        <value>@((string)context.Variables["cid"])</value>
      </set-header>
    </outbound>
  
    <on-error>
      <set-header name="X-Correlation-Id" exists-action="override">
        <value>@((string)context.Variables["cid"])</value>
      </set-header>
    </on-error>
  </policies>
  ```

**2) 실제 구성 요약**
- 게이트웨이 레벨에서 회복탄력성:
  - 요청이 지연되면 10초 후 타임아웃.
  - 요청 전후, 오류 응답에도 Correlation-Id를 삽입하여 추적 가능.
  - 프리플라이트 요청(OPTIONS)은 빠르게 응답 처리.
  - CORS 전역 허용으로 프론트엔드 개발 환경 대응.
 
---

## 6. 데모 시나리오

A. 차량 등록·조회·예약 (Agent 포함)
1. 등록 · 관리자
    - 차량 목록 화면에서 차량 기본정보(번호, 모델, 연료, 연비, 상태)를 입력·저장
    - 성공 시 목록에 AVAILABLE 상태로 즉시 반영(중복 번호 검증)
2. 조회 · 사용자
    - 차량 조회 화면에서 전체 목록 확인, 필터(가용/모델/연료)와 정렬·페이징으로 탐색
    - 선택한 차량의 주요 스펙·상태를 카드/리스트로 확인
3. 예약 · 사용자(AI Agent)
    - 자연어로 “내일 오전 10시 인근에서 이용 가능한 SUV 예약”처럼 요청
    - Agent가 가용 차량/시간대를 추천 → 사용자 선택 → 예약 생성 및 예약번호 발급
    - 실패 시 대안 시간대/차량을 재추천( 멱등키로 중복 확정 방지)

B. 실시간 차량 확인·관제 (관리자)
1. 실시간 조회
    - 개별 차량: 차량 ID(또는 목록 선택)로 현재 위치·속도·이동 경로를 실시간 확인. 정지/이동 상태 배지와 최근 수신 시각 표시.
    - 전체 차량: 운행 중 차량을 한눈에 확인
2. 관제 대시보드
  - 요약 KPI: 금일 운행 건수, 총 주행거리, 평균 속도, 가동률 등 핵심 지표 제공.
  - 기록 조회: 차량·기간별 운행 로그/그래프(속도 추이·구간별 거리)와 직전 운행 상세 확인.
  - 업무 효율화: 과속·장시간 정차 등 이상 이벤트 표시로 조치 우선순위 파악.

데모 포인트: 차량 클릭 → 실시간 경로 하이라이트, 대시보드에서 운행 기록으로 드릴다운.
---

## 7. 설계 - 구현 정합성, 변경 이력
- ADR (Architecture Decision Records)

- (1) ADR-001: RDB로 PostgreSQL 선택
  - Context: 예약 중복 방지와 시간 범위 쿼리가 많음
  - Alternatives: MySQL(팀에 익숙), 기능적 차이는 크지 않음
  - Decision: 수업에서 다룬 경험과 학습을 고려해 PostgreSQL 선택
  - Consequence: 안정적인 트랜잭션 처리와 시계열 쿼리 지원



- (2) ADR-002: 실시간 파이프는 Redis (Stream + ZSET)
  - Context: 법인 차량 약 100대 관제 규모에는 Redis 성능 충분
  - Alternatives: Kafka(대규모에 적합) → 현재는 오버엔지니어링
  - Decision: Redis Streams + ZSET
  - Consequence: 운영 단순, 필요 시 대규모 확장 가능



- (3) ADR-003: API Gateway는 Azure APIM
  - Context: 키 관리, 레이트리밋, CORS 중앙화 필요
  - Alternatives: Kong, NGINX, Tyk, AWS API Gateway 등 존재
  - Decision: 가장 빠르게 도입 가능한 Azure APIM 선택
  - Consequence: 서비스별 독립 배포, 보안정책 일원화


- (4) ADR-004: 차량 관제 데이터 실시간 전송은 WebSocket (대안으로 SSE)
  - Context: 지도 실시간 업데이트
  - Alternatives: Polling(비효율)
  - Decision: WS 우선, 장애 시 SSE 폴백
  - Consequence: 방화벽/프록시 호환성 개선


- (5) ADR-005: 예약 겹침 방지는 코드 로직으로 처리
  - Context: 분산 환경에서 동일 시간대에 중복 예약 시도 가능성 존재
  - Alternatives: DB 제약 조건(EXCLUDE USING GIST)으로 강제, 다만 적용 난이도 있음
  - Decision: 현재는 애플리케이션 코드 레벨에서 겹침 검사 로직을 구현하고, 추후 운영 안정화 단계에서 DB 제약 조건 적용 예정
  - Consequence: 초기 개발과 적용이 용이, 단 코드 레벨에서의 레이스 컨디션 위험은 존재

---

## 8. 협업, 품질 운영
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

- [github projects 운영](https://github.com/orgs/KT-GIGA-FMS/projects/3)
  - <img width="1912" height="1036" alt="image" src="https://github.com/user-attachments/assets/6df38afe-7d37-4b4e-8b93-cf450789523c" />

---
  
## 9. 공유 자료 모음
- [프로젝트 기획서](https://docs.google.com/document/d/1HPhWYdSCaW_UycXsQGsRFQ6wy3SvAwCacChCETTccKY/edit?tab=t.0)
- [FP 산정 시트](https://docs.google.com/spreadsheets/d/11p7Wmf7TJH4ZsfxWPAp8_TwIe4qGc4qaDQb3Sh7QISA/edit?gid=2082954959#gid=2082954959)

- [github projects](https://github.com/orgs/KT-GIGA-FMS/projects/3)
- [Notion](https://www.notion.so/KT-250de2e870a180fbb45ef1ea86d7874e?source=copy_link)
- [Miro Board](https://miro.com/app/board/uXjVJT7T_I8=/)
- [DB/ERD](https://www.erdcloud.com/d/aqyjwmcWrcmSoYD4S)
- [아이디어 모음집](https://github.com/KT-GIGA-FMS/.github/blob/main/profile/ROADMAP.md)

  
