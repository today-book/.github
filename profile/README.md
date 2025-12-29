# 📚 오늘 뭐 읽지?

> **Embedding 기반 AI 도서 추천 서비스**  
> 기분, 상황, 감정에 맞춰 AI가 오늘 읽을 책을 직접 골라주는 서비스

---

## 📌 프로젝트 소개

“오늘 뭐 읽지?”는  
**카카오톡 메시지처럼 자연스럽게 입력한 문장 + 선택지 응답**을 기반으로,  
사용자와 **의미적으로 가장 가까운 책**을 추천해주는 서비스입니다.

사용자는 프롬프트를 대화하듯 입력하거나,  
제공된 감정·상황 선택지를 조합해 책을 추천받을 수 있습니다.  
추천 결과는 카카오톡으로 공유하거나 내 책장에 저장할 수 있습니다.

---

## ✨ 핵심 기능

| 기능 구분             | 상세 내용                         |
|-------------------|-------------------------------|
| 📖 AI 기반 책 추천     | 사용자 프롬프트 + 선택지 기반 추천          |
| 🔐 카카오 로그인 기반 개인화 | 추천 도서 저장                      |
| 🧠 추천 이유 제공       | RAG 기반 **추천 설명(이유 문장) 자동 생성** |
| ❤️ 소셜 기능          | 카카오 친구에게 결과 공유                |

---

## 🏛️ 시스템 주요 구성

| 도메인/서버                        | 주요 역할                            |
|-------------------------------|----------------------------------|
| **Book-Collecting-Worker**    | 책 정보 수집(API 기반)                  |
| **Book-Preprocessing-Worker** | 책 정보를 저장하기 전 전처리 작업              |
| **Book-Embedding-Worker**     | Gemini embedding 활용, 텍스트 → 벡터 변환 |
| **Book-Service**              | 전처리된 책 데이터 저장                    |
| **Search-Service**            | 감정 분석·사용자 의도 파악·추천 설명 생성         |
| **User-Service**              | 사용자 계정·개인화 정보 관리                 |
| **Gateway-Server**            | 인증 처리 및 서비스 라우팅                  |
| **PostgreSQL**                | 책·사용자·추천 관련 데이터 영속 저장소           |
| **OpenSearch**                | 임베딩 백터 데이터 저장, 유사도 검색(k-NN)      |
| **Kafka**                     | 서비스 간 이벤트 기반 비동기 메시징             |
| **Redis**                     | 인증 토큰 및 개인화 데이터 캐시               |
| **Docker Compose**            | 서비스 환경 표준화 및 구성 관리               |

---

## 🧩 기술 스택

| 분야        | 기술                                               |
|-----------|--------------------------------------------------|
| Backend   | Spring Boot, Java                                |
| Data & AI | Vertex AI Text Embedding, Vertex AI Gemini (LLM) |
| Infra     | AWS, Kafka, Docker                               |
| Auth      | 카카오 OAuth2 로그인                                   |
| API Docs  | Swagger                                          |
| DB        | PostgreSQL, OpenSearch, Redis                    |

---

## 🧠 FlowChart

### Offline Flow (책 데이터 수집)

![offline_flow](/images/offline_flow.jpg)

### Online Flow (책 데이터 검색)

![online_flow](/images/online_flow.jpg)
---

## 🗄️ 데이터 구조

### RDB(PostgreSQL)

![postgre](/images/todaybook_postgresql.jpg)

### VectorDB(OpenSearch)

![opensearch](/images/todaybook_opensearch.jpg)

---

## ⚙️ 인프라 구조

![infra](/images/infra.jpg)

- VPC, Subnet, Security Group 분리 구성으로 운영 환경 네트워크 설계
- Public/Private Subnet 분리 기반 ALB–서비스 구조로 내부·외부 트래픽 분리
---

## 👥 팀

## 👥 팀 구성 및 개인별 기여

| 팀원명     | 포지션 | 담당 (개인별 기여)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------|-----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **임희호** | 팀장  | **Kafka 인프라 구성 및 운영**<br>- MSA 구조에 맞는 Kafka 클러스터 아키텍처 설계<br>- 브로커 서버와 애플리케이션 서버 분리 구성으로 장애 전파 최소화<br>- NAT 기반 네트워크 구성으로 Kafka 브로커 비공개 운영<br>- Kafka UI 연동을 통한 토픽·파티션·컨슈머 그룹·Lag 모니터링<br>- PLAINTEXT → SASL(SCRAM) 인증 방식 전환 및 Spring Boot 연동<br>- 토픽별 retention 정책 설정 및 성능 튜닝<br><br>**대용량 데이터 비동기 전처리 파이프라인 구축**<br>- 대량 이벤트 안정적 수집·전달·소비 구조 설계<br>- 전처리 로직 설계(유효성 검증, 데이터 정제, 포맷 정규화)<br>- 비즈니스 로직과 메시지 처리 로직 분리로 확장성·유지보수성 확보                                                                                        |
| **김지원** | 팀원  | **검색 서비스**<br>- LLM 기반 감정 분석 + OpenSearch k-NN 벡터 검색 결합 파이프라인 설계·구현<br>- 감정 점수와 벡터 유사도를 결합한 재랭킹 스코어 모델(α·β 가중치) 설계<br><br>**게이트웨이 & 인증 서비스**<br>- Rate Limiting, CORS 등 공통 보안·운영 정책 적용<br>- OAuth 로그인 이후 일회성 authCode 기반 로그인 구조 적용<br>- Redis + Lua Script 기반 Refresh Token Rotation으로 레이스 컨디션 방지<br><br>**배포 스크립트 & 운영 인프라**<br>- GitHub Actions + AWS SSM 기반 롤링 배포 자동화<br>- VPC·Subnet·Security Group 분리 기반 네트워크 설계<br>- Public/Private Subnet 분리 + ALB 구조로 트래픽 분리                                                   |
| **박성준** | 팀원  | **데이터 수집**<br>- 대용량 CSV(18개) 스트리밍 처리 후 raw 데이터 Kafka 발행<br>- 초기 데이터 동기 수집 + Kafka 비동기 발행 구조 설계<br>- Naver API 기반 매일 새벽 1시 정기 수집<br>- 숫자/알파벳/한글 초성 기반 전체 쿼리 패턴 생성 및 페이지네이션 수집<br>- HTTP 429 대응 선형 백오프 재시도 전략 적용<br>- 수집 단계와 Kafka 발행 단계 분리로 처리량 안정성 확보<br><br>**모니터링**<br>- Prometheus + Grafana 기반 서비스 메트릭 시각화<br>- CPU 사용률 임계치 기반 Slack 알림 설정<br>- Loki 중앙 로그 수집 및 Grafana 연동<br>- Alloy 기반 로그 수집 설정 중앙 관리                                                                                                            |
| **이서현** | 팀원  | **도서 서비스**<br>- ISBN 기반 도서 중복 검사 및 신규/기존 도서 구분 처리<br>- 도서 필드별 업데이트 전략 수립으로 데이터 품질·일관성 확보<br>- 배치 처리 시 트랜잭션 경계 및 DB 부하 고려한 저장 전략 설계<br><br>**도서 데이터 임베딩**<br>- Spring Batch 기반 대규모 도서 데이터 처리 파이프라인 구축<br>- Execution Context 활용 Keyset Pagination Reader 구현<br>- Spring AI + Vertex AI Embedding 모델 연동<br><br>**사용자 서비스**<br>- ‘내 책장’ 도메인 및 비즈니스 로직 구현<br>- Redis 기반 추천 결과 공유 기능 구현<br><br>**프론트엔드**<br>- Antigravity 기반 UI 생성 및 편집<br>- Access Token 자동 Refresh API 클라이언트 구현<br>- GitHub Secrets 기반 GitHub Pages 자동 배포 |
---

## 📌 사용 방법

1. https://todaybook.life 접속
2. 감정/상황 선택 또는 자유 입력
3. AI 추천 결과 확인
4. 결과 저장/공유

---

## 📄 라이선스

MIT License  
Copyright (c) 2025

---

## 🤝 기여 가이드

Issue 또는 Pull request 환영합니다 :)  
Question? → Discussions!
