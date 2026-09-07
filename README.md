# 남도ON (NamdoON)

> 남도장터의 업무·상담 이력, 내부 지식, 데이터 분석을 연결하는 통합 업무지원 플랫폼

![Status](https://img.shields.io/badge/status-PoC-orange)
![Frontend](https://img.shields.io/badge/frontend-React-61DAFB)
![Backend](https://img.shields.io/badge/backend-Spring%20Boot-6DB33F)
![AI](https://img.shields.io/badge/AI-Python%20RAG-3776AB)
![Database](https://img.shields.io/badge/database-PostgreSQL%20%2B%20pgvector-4169E1)

## 1. 프로젝트 소개

남도ON은 남도장터의 프로젝트·업무·고객상담 이력을 한 곳에서 관리하고, 완료된 업무와 검증된 문서를 조직 지식으로 전환하여 RAG(Retrieval-Augmented Generation) 기반 검색·질의응답에 활용하기 위한 내부 업무지원 플랫폼입니다.

본 저장소에서는 정식 구축에 앞서 핵심 업무 흐름과 기술 적합성을 검증하는 **3일 PoC**를 우선 진행합니다. PoC 결과를 바탕으로 업무 운영, 지식·AI, 데이터 분석 영역을 단계적으로 확장합니다.

### 핵심 가치

- 흩어진 업무·상담·문서 이력을 하나의 흐름으로 연결
- 완료 업무와 상담 결과를 재사용 가능한 지식으로 축적
- 승인된 지식과 출처를 기반으로 신뢰할 수 있는 RAG 답변 제공
- 개인정보와 업무 권한을 고려한 내부 AI 활용 기반 마련
- 소규모 내부 개발을 통한 기술 적합성 및 후속 투자범위 검증

## 2. PoC 개요

| 구분 | 내용 |
| --- | --- |
| 프로젝트명 | 남도ON 통합 업무·지식·데이터 플랫폼 PoC |
| 추진기간 | 3일 |
| 추진인력 | 개발 3명, 관리 1명 |
| 개발방식 | Vertical Slice, 경량 Git-flow, Pull Request 및 Peer Review |
| 인프라 | 네이버클라우드 크레딧 범위 내 단일 서버 |
| 배포방식 | Docker Compose 기반 통합 배포 |
| 주요사용자 | 남도장터 내부직원 및 지정된 위탁용역사 |
| PoC 목표 | 핵심 통합 시나리오 구현, RAG 품질 및 협업·배포 절차 검증 |

## 3. 핵심 시나리오

PoC에서는 다음 흐름이 처음부터 끝까지 동작하는지를 검증합니다.

1. 사용자가 업무 또는 고객상담을 등록합니다.
2. 담당자가 상태, 처리내용 및 결과를 기록합니다.
3. 완료된 업무·상담과 관련 문서를 지식 후보로 등록합니다.
4. 검토자가 공개범위와 내용을 확인하고 지식을 승인합니다.
5. 사용자가 자연어로 질문하면 승인된 지식에서 관련 근거를 검색합니다.
6. 시스템이 답변과 함께 문서명, 구간 등 출처를 표시합니다.
7. 관리자가 업무·상담·지식 현황을 간단한 대시보드에서 확인합니다.

```mermaid
flowchart TD
    A[업무·상담 등록] --> B[처리 및 결과 기록]
    B --> C[지식 후보 생성]
    C --> D{담당자 검토}
    D -->|승인| E[임베딩 및 검색 반영]
    D -->|보완| C
    E --> F[RAG 질문·답변]
    F --> G[출처 확인 및 업무 재활용]
```

## 4. PoC 범위

### 포함 범위

- 업무·고객상담 등록, 조회, 상태 및 처리결과 관리
- 문서 또는 처리결과의 지식 등록과 승인상태 관리
- 승인 지식의 Chunk 분할, Embedding 및 Vector 저장
- 사용자 질문에 대한 RAG 답변과 출처 표시
- 업무·상담·지식 건수 중심의 간단한 현황 조회
- GitHub Issue, Branch, Pull Request, Review를 이용한 협업
- 네이버클라우드 단일 서버 배포 및 반복 시연

### PoC 제외 범위

- 고객에게 직접 공개되는 챗봇
- 실사용 개인정보를 이용한 AI 처리
- 기존 남도장터 운영계 MSA와의 직접 연계
- 실시간 ETL, DW 및 자유형 Text-to-SQL
- 예측·추천 AI와 고급 BI 대시보드
- Kubernetes, 다중 서버, 이중화 및 무중단 배포
- 정식 운영 수준의 성능·재해복구 체계

## 5. 시스템 구성

```mermaid
flowchart TD
    U[내부 사용자] --> N[Nginx]
    N --> W[React Web]
    N --> B[Spring Boot API]
    N --> R[Python RAG API]
    B --> P[(PostgreSQL)]
    R --> P
    P --> V[pgvector]
```

| 계층 | 기술 | 주요 역할 |
| --- | --- | --- |
| Frontend | React | 업무·상담·지식·RAG 화면 |
| Business API | Java 17+, Spring Boot 3 | 업무 로직, 권한, 데이터 처리 |
| AI API | Python | 문서 분할, 임베딩, 검색, 답변 생성 |
| Database | PostgreSQL, pgvector | 업무 데이터와 Vector 통합 저장 |
| Web/Proxy | Nginx | 정적 파일 제공 및 API Reverse Proxy |
| Runtime | Docker Compose | 서비스 실행 및 환경 표준화 |
| Collaboration | GitHub | Issue, PR, Review, Actions, Release 관리 |
| Infrastructure | NAVER Cloud Platform | PoC 통합 서버 운영 |

> PoC에서는 단순성과 비용 통제를 위해 PostgreSQL과 pgvector를 함께 사용합니다. 데이터 규모와 검색 요구가 확대될 경우 전문 검색엔진 또는 별도 Vector DB 도입을 재검토합니다.

## 6. 저장소 구조

```text
namdo-on/
├── frontend/                 # React 웹 애플리케이션
├── backend/                  # Spring Boot 업무 API
├── ai/                       # Python RAG API 및 평가 코드
├── infra/
│   ├── docker-compose.yml    # 통합 실행 구성
│   ├── nginx/                # Reverse Proxy 설정
│   └── scripts/              # 배포·백업·점검 스크립트
├── docs/
│   ├── api/                  # OpenAPI 및 연계 명세
│   ├── adr/                  # 주요 기술 의사결정 기록
│   ├── test/                 # UAT 및 Golden Query 결과
│   └── images/               # 문서용 이미지
├── .github/
│   ├── workflows/            # CI/CD Workflow
│   ├── ISSUE_TEMPLATE/       # Issue 양식
│   └── pull_request_template.md
├── .env.example              # 환경변수 예시
├── .gitignore
└── README.md
```

## 7. 역할 분담

기능 단절을 줄이기 위해 화면·API·DB·테스트를 하나의 업무 흐름으로 구현하는 Vertical Slice 방식을 적용합니다.

| 역할 | 주 책임 | 주요 작업 |
| --- | --- | --- |
| 개발자 1 | 공통 플랫폼·통합·배포 | 프로젝트 골격, 공통 규격, Docker Compose, Nginx, CI/CD, NCP 배포 |
| 개발자 2 | 업무·상담 영역 | 업무·상담 DB, API, 화면, 상태변경 및 현황 조회 |
| 개발자 3 | 지식·RAG 영역 | 지식 등록·승인, Chunk, Embedding, pgvector 검색, 답변·출처 표시 |
| 관리자 | PM·PO·QA | 범위·우선순위, 샘플 데이터, 완료조건, UAT, 릴리스 승인, 시연·보고 |

### 공통 변경 책임

- DB Migration: 개발자 2 주관, 영향 개발자 사전 리뷰
- API 공통 규격: 개발자 1 주관, OpenAPI 우선 합의
- RAG 데이터 구조: 개발자 3 주관, 개인정보·공개범위 관리자 확인
- 배포 및 환경변수: 개발자 1 관리, 관리자 최종 배포 승인

## 8. 3일 WBS

| 일자 | 단계 | 개발 작업 | 관리·검증 | 주요 산출물 |
| --- | --- | --- | --- | --- |
| Day 1 오전 | 착수 | 저장소, 프로젝트 골격, DB·API·화면 계약 정의 | 범위·시나리오·완료조건 확정 | 백로그, ERD, API 초안 |
| Day 1 오후 | 병렬 개발 | 공통 기반, 업무·상담 Slice, RAG Slice 개발 | 샘플 문서·Golden Query 준비 | Draft PR, 실행 가능한 골격 |
| Day 1 종료 | 1차 통합 | `develop` 병합 및 통합 실행 | 범위 변경 통제, 충돌사항 정리 | 통합본 v0.1 |
| Day 2 오전 | 핵심 구현 | 등록→처리→지식화→RAG 흐름 연결 | 중간 시연 및 기능 검수 | E2E 시나리오 |
| Day 2 오후 | 배포 후보 | 오류 보완, `release/poc-v0.1` 생성, NCP 배포 | UAT 및 RAG 답변 평가 | `poc-v0.1.0-rc1` |
| Day 3 오전 | 안정화 | 신규 기능 중단, 결함 수정 및 회귀시험 | 반복 시연, 결과 기록 | 결함조치·시험결과 |
| Day 3 오후 | 릴리스 | `release` → `main`, 태그 및 최종 배포 | 릴리스 승인, 결과보고·회고 | `poc-v0.1.0` |

## 9. Git-flow 운영

### 브랜치 정책

| 브랜치 | 용도 | 생성 기준 | 병합 대상 |
| --- | --- | --- | --- |
| `main` | 최종 시연·릴리스 | 기본 브랜치 | 릴리스 및 Hotfix만 반영 |
| `develop` | 개발 통합 | `main`에서 생성 | Feature와 Fix 반영 |
| `feature/*` | 기능 개발 | `develop`에서 생성 | `develop` |
| `release/poc-v0.1` | 통합시험·배포 후보 | Day 2에 `develop`에서 생성 | `main`, 이후 `develop` 재반영 |
| `hotfix/*` | 배포 후 긴급 수정 | `main`에서 생성 | `main`과 `develop` |

### 브랜치 이름

```text
feature/BE-101-work-api
feature/FE-102-work-screen
feature/AI-103-rag-source
fix/BE-201-ticket-validation
hotfix/OPS-301-startup-error
```

### 작업 절차

```bash
# 통합 브랜치 최신화
git switch develop
git pull --ff-only origin develop

# 작업 브랜치 생성
git switch -c feature/BE-101-work-api

# 작업 후 커밋·푸시
git add <changed-files>
git commit -m "feat: 업무 등록 API 추가"
git push -u origin feature/BE-101-work-api
```

푸시 후 GitHub에서 `feature/*` → `develop` Pull Request를 생성합니다.

### Pull Request 기준

- 한 PR은 하나의 Issue 또는 하나의 완료조건만 처리합니다.
- 작업 시작 후 가능한 한 빠르게 Draft PR을 생성합니다.
- 변경 목적, 주요 변경사항, 테스트 결과, 영향범위를 작성합니다.
- 화면 변경은 Before/After 또는 캡처를 첨부합니다.
- DB·API·권한·환경변수 변경 여부를 명시합니다.
- CI 통과와 개발자 1명 이상의 리뷰 후 병합합니다.
- Feature PR은 `Squash merge`하고 병합 후 브랜치를 삭제합니다.
- `main` 직접 Push와 검증되지 않은 강제 병합을 금지합니다.

### 커밋 규칙

| 접두어 | 용도 | 예시 |
| --- | --- | --- |
| `feat` | 기능 추가 | `feat: 상담 등록 화면 추가` |
| `fix` | 오류 수정 | `fix: 빈 검색어 검증 추가` |
| `test` | 테스트 | `test: RAG 출처 평가 추가` |
| `docs` | 문서 | `docs: API 명세 갱신` |
| `refactor` | 기능변경 없는 구조개선 | `refactor: 검색 서비스 분리` |
| `chore` | 설정·빌드 | `chore: Docker 환경 설정` |

## 10. 개발 환경 구성

### 요구사항

- Git 2.40+
- Docker Engine 및 Docker Compose
- Node.js LTS 및 npm
- JDK 17+
- Python 3.11+

Docker Compose만 사용할 경우 Node.js, JDK, Python을 호스트에 별도로 설치하지 않는 구성을 권장합니다.

### 환경변수

`.env.example`을 복사해 로컬 전용 `.env`를 생성합니다.

```bash
cp .env.example .env
```

예상 환경변수는 다음과 같습니다.

```dotenv
# Application
APP_ENV=local
WEB_PORT=80

# PostgreSQL
POSTGRES_DB=namdoon
POSTGRES_USER=namdoon_app
POSTGRES_PASSWORD=change-me
DATABASE_URL=postgresql://namdoon_app:change-me@postgres:5432/namdoon

# AI / RAG
EMBEDDING_MODEL=
LLM_MODEL=
LLM_API_KEY=
RAG_TOP_K=5

# Security
JWT_SECRET=change-me
```

> `.env`, API Key, DB 비밀번호, SSH Private Key 및 클라우드 인증정보는 저장소에 커밋하지 않습니다.

### 통합 실행

```bash
docker compose -f infra/docker-compose.yml up -d --build
docker compose -f infra/docker-compose.yml ps
```

### 로그 확인 및 종료

```bash
docker compose -f infra/docker-compose.yml logs -f --tail=200
docker compose -f infra/docker-compose.yml down
```

데이터까지 초기화하는 명령은 기존 데이터가 삭제될 수 있으므로 담당자 확인 후 실행합니다.

## 11. API 초안

구체적인 요청·응답 구조는 `docs/api/`의 OpenAPI 문서를 기준으로 관리합니다.

| 영역 | Method | Endpoint | 설명 |
| --- | --- | --- | --- |
| Health | `GET` | `/api/health` | 서비스 상태 확인 |
| 업무 | `POST` | `/api/tasks` | 업무 등록 |
| 업무 | `GET` | `/api/tasks` | 업무 목록 조회 |
| 업무 | `PATCH` | `/api/tasks/{id}/status` | 업무 상태 변경 |
| 상담 | `POST` | `/api/tickets` | 고객상담 등록 |
| 상담 | `PATCH` | `/api/tickets/{id}/resolve` | 상담 처리결과 등록 |
| 지식 | `POST` | `/api/knowledge` | 지식 후보 등록 |
| 지식 | `POST` | `/api/knowledge/{id}/approve` | 지식 승인 |
| RAG | `POST` | `/api/rag/query` | 지식 질의 및 출처 조회 |
| 현황 | `GET` | `/api/dashboard/summary` | PoC 요약 현황 |

## 12. 테스트 및 완료 기준

### PoC 목표지표

| 구분 | 목표 |
| --- | --- |
| 통합 시나리오 | 최소 3개 정상 동작 |
| 업무·상담 데이터 | 합계 10건 이상 |
| 승인 지식 | 10건 이상 |
| Golden Query | 10건 이상 |
| 답변 출처 표시율 | 100% |
| 사용 가능 답변율 | 80% 이상 |
| 반복 시연 | 5회 연속 치명적 오류 없음 |

### Definition of Done

Issue는 다음 조건을 모두 충족하면 완료로 처리합니다.

- 완료조건에 기재된 기능이 동작함
- 정상·오류·권한 시나리오를 확인함
- 관련 테스트가 통과함
- 개인정보 또는 민감정보가 로그·화면에 불필요하게 노출되지 않음
- API, DB, 환경변수 변경사항이 문서에 반영됨
- 동료 리뷰와 CI 검증을 통과함
- `develop` 통합 후 다른 핵심 기능이 정상 동작함

## 13. CI/CD 및 배포

### Pull Request CI

PR이 `develop`, `release/*`, `main`을 대상으로 생성되면 다음 항목을 자동 검증합니다.

1. Frontend lint, test, build
2. Backend unit test 및 build
3. AI lint, pytest 및 Golden Query 기본 평가
4. Docker Compose 구성 검증
5. 애플리케이션 Secret 포함 여부 점검

### NCP 배포

- PoC 서버는 최소 필요 사양의 Linux Server 1대로 구성합니다.
- 외부에는 웹 서비스용 `80/443`만 공개합니다.
- SSH 포트는 참여자 또는 사내 공인 IP에만 허용합니다.
- PostgreSQL과 내부 API 포트는 외부에 공개하지 않습니다.
- `release/*`는 통합시험용으로 배포할 수 있습니다.
- `main` 태그 배포는 관리자의 승인 후 실행합니다.
- 배포 후 Health Check와 핵심 시나리오 Smoke Test를 수행합니다.

### 릴리스 절차

1. Day 2에 `develop`에서 `release/poc-v0.1`을 생성합니다.
2. 릴리스 브랜치에서 결함만 수정하고 기능을 추가하지 않습니다.
3. `release/poc-v0.1`을 NCP에 배포하여 UAT를 수행합니다.
4. 검증 완료 후 `release/poc-v0.1` → `main` PR을 병합합니다.
5. `poc-v0.1.0` 태그와 GitHub Release를 생성합니다.
6. 릴리스 변경사항을 `develop`에 재반영합니다.
7. 치명적 오류는 `hotfix/*`로 수정하고 `main`, `develop` 모두에 반영합니다.

## 14. 비용 관리

네이버클라우드 프로모션 크레딧은 계정별 사용기간과 적용 상품이 다를 수 있으므로 서버 생성 전 콘솔에서 적용조건을 확인합니다.

| 사용률 | 대응 |
| --- | --- |
| 70% | 사용량 재점검 및 불필요 리소스 중지 |
| 85% | 신규 유료 리소스 생성 금지 |
| 95% | 서버 중지 또는 PoC 조기 종료 판단 |

비용 통제 원칙은 다음과 같습니다.

- 서버 1대와 최소 스토리지로 시작
- PoC 단계에서 관리형 DB, Load Balancer 및 Kubernetes 제외
- 실험하지 않는 시간에는 서버 중지 검토
- 매일 오전·오후 실제 사용비용 확인
- Object Storage, Snapshot 및 Public IP의 별도 과금 여부 확인
- PoC 종료 후 보존 필요성을 확인하고 불필요 리소스 정리

## 15. 보안 및 데이터 원칙

- 실제 고객의 이름, 연락처, 주소, 주문정보를 PoC에 사용하지 않습니다.
- 테스트 데이터는 가상 또는 비식별 데이터로 구성합니다.
- 사용자가 열람할 수 없는 문서는 검색과 RAG 답변에서도 제외합니다.
- 승인된 지식만 RAG 검색대상에 포함합니다.
- 근거가 부족하면 추정 답변 대신 확인 불가 메시지를 반환합니다.
- 답변에는 근거 문서와 출처를 표시합니다.
- 로그에 비밀번호, Token, 개인정보 및 문서 원문 전체를 남기지 않습니다.
- AI가 생성한 답변과 코드는 사람이 검토한 후 업무에 활용합니다.

## 16. 주요 문서

| 문서 | 위치 | 목적 |
| --- | --- | --- |
| API 명세 | `docs/api/` | Frontend, Backend, AI 간 계약 |
| 기술 결정기록 | `docs/adr/` | DB, RAG, 인증 등 주요 의사결정 근거 |
| Golden Query | `docs/test/golden-query.*` | RAG 품질 평가 질문·기대근거 |
| UAT 결과 | `docs/test/uat-result.*` | 시연 시나리오와 검증결과 |
| 배포 절차 | `infra/README.md` | NCP 환경 구축·배포·복구 방법 |

## 17. 향후 로드맵

PoC 완료 후 다음 순서로 단계적 확장을 검토합니다.

1. **업무 운영 MVP**: 프로젝트·업무·상담·첨부·변경이력·기본 대시보드
2. **지식·AI MVP**: 지식 승인·버전·공개범위, Hybrid Search, 내부 챗봇
3. **데이터 분석 MVP**: ETL·DW·KPI 사전·셀프분석·데이터 품질
4. **통합 Pilot**: 내부 사용자 시험운영, 보안·정확성·운영부담 평가
5. **후속사업 검토**: 기존 시스템 연계, 사용자 확대 및 정식 운영구조 결정

## 18. 참고자료

- [GitHub flow](https://docs.github.com/en/get-started/using-github/github-flow)
- [GitHub 보호 브랜치 관리](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule)
- [GitHub Actions Secret 사용](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets)
- [NAVER Cloud Cost Explorer](https://guide.ncloud-docs.com/docs/costexplorer-overview)
- [NAVER Cloud 예산 설정](https://guide.ncloud-docs.com/docs/costexplorer-budget)

## 19. 이용 및 공개 범위

본 프로젝트는 남도장터 내부 PoC를 목적으로 합니다. 저장소의 코드, 데이터, 문서 및 화면은 사전 승인 없이 외부에 공개하거나 업무 외 목적으로 사용하지 않습니다.

---

**담당부서:** 쇼핑몰운영팀  
**프로젝트 상태:** 3일 PoC 준비  
**목표 릴리스:** `poc-v0.1.0`
