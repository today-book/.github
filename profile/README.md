아래는 요청하신 내용을 기반으로 깔끔하고 구조적인 README.md 초안입니다.
필요 시 배지, 아키텍처 이미지, 상세 기술 문단 등을 추가해드릴 수 있어요!

⸻


# 📚 오늘 뭐 읽지?

> **Embedding 기반 AI 도서 추천 서비스**  
> 기분, 상황, 감정에 맞춰 AI가 오늘 읽을 책을 직접 골라주는 서비스

---

## 📌 프로젝트 소개

“오늘 뭐 읽지?”는  
**카카오톡 메시지처럼 자연스럽게 입력한 문장 + 선택지 응답**을 기반으로,  
사용자와 **의미적으로 가장 가까운 책**을 추천해주는 서비스입니다.

사용자는 프롬프트를 대화하듯 입력하거나  
제공된 감정/상황 선택지를 조합하여 책을 추천받을 수 있습니다.  
또한 결과는 카카오톡을 통해 저장·공유하거나 친구에게 퀴즈로 보낼 수 있습니다.

---

## ✨ 핵심 기능

| 기능 구분 | 상세 내용 |
|---------|---------|
| 📖 AI 기반 책 추천 | 사용자 프롬프트 + 선택지 기반 추천 |
| 🔐 카카오 로그인 기반 개인화 | 사용자 이력 기반 맞춤형 추천 |
| 🧠 추천 이유 제공 | RAG 기반 **추천 설명(이유 문장) 자동 생성** |
| ❤️ 소셜 기능 | 카카오 친구에게 결과 공유 / 퀴즈 기능 |
| 🛒 도서 구매 연동 | 추천 결과 → 구매 페이지 이동 |

---

## 🏛️ 시스템 주요 구성

| 도메인/서버 | 주요 역할 |
|------------|---------|
| **Crawler-Server** | 책 정보 수집(API 기반) |
| **Embedding-Server** | Gemini embedding 활용, 텍스트 → 벡터 변환 |
| **BOOK Server** | PGVector 저장소 사용, 벡터 기반 유사도 검색 |
| **RAG-Server** | 감정 분석 + 문맥 추론 + 추천 설명 생성 |
| **Kafka** | 서비스 간 이벤트 기반 비동기 전달 |
| **Redis** | 추천 결과 캐싱 |
| **Docker Compose** | MSA 통합 배포 |

---

## 🧩 기술 스택

| 분야 | 기술 |
|------|------|
| Backend | Spring Boot, Kotlin/Java |
| Data & AI | **Google Gemini Embedding**, PGVector, RAG |
| Infra | Kafka, Redis, Docker Compose |
| Auth | 카카오톡 OAuth 로그인 |
| API Docs | Swagger |
| DB | PostgreSQL + PGVector |

---

## 🧠 Embedding & RAG 흐름

사용자 입력(문장/선택지)
↓
임베딩 변환 (Gemini)
↓
PGVector에서 책 벡터와 유사도 비교
↓
Top-K 도서 후보
↓
RAG 설명 생성(추천 이유 제공)
↓
추천 결과 제공 + 공유/저장 기능

---

## 🗄️ 데이터 구조 (예시)

| 필드 | 설명 |
|------|------|
| title | 책 제목 |
| author | 저자 |
| description | 줄거리/소개 |
| genre | 장르 |
| embedding | 벡터 데이터(PGVector) |

---

## ⚙️ MSA 배포 구조

Docker Compose 기반으로 아래 서비스들을 구동합니다:

- crawler-server
- embedding-server
- book-server
- rag-server
- kafka / zookeeper (or KRaft)
- redis
- postgresql(pgvector)

> 환경 변수 예시, compose 파일은 추후 상세 버전으로 제공 예정

---

## 🚧 개발 로드맵

- [ ] 카카오 로그인 + 사용자 정보 기반 추천 고도화
- [ ] 검색 이력 기반 재추천 기능
- [ ] 추천 정확도 향상(Gemini + Fine-tuning embedding)
- [ ] 친한 친구 취향 기반 간접 추천 강화
- [ ] 이메일/문자 공유 기능 확장

---

## 👥 팀

| 역할 | 이름 | 담당 |
|------|------|------|
| Backend | - | 데이터 수집, 임베딩 서버, RAG 서버 |
| Product/UX | - | 서비스 기획 및 사용자 여정 설계 |
| Platform | - | Docker/Kafka/인프라 운영 |

(추가 예정)

---

## 📌 사용 방법

1. 카카오톡 또는 Web UI 접속
2. 감정/상황 선택 또는 자유 입력
3. AI 추천 결과 확인
4. 결과 저장/공유/구매 연결

---

## 📄 라이선스

MIT License  
Copyright (c) 2025

---

## 🤝 기여 가이드

Issue 또는 Pull request 환영합니다 :)  
Question? → Discussions!

---

> **오늘 하루에 어울리는 책을 AI에게 맡겨보세요!**  
> _What should I read today?_


⸻

원하신다면 다음도 바로 만들어 드릴 수 있습니다👇
	•	🎨 아키텍처 다이어그램 (백엔드/데이터 파이프라인 포함)
	•	🧱 Docker Compose 예시 파일
	•	📊 ERD & Kafka Topic 설계
	•	🔐 카카오 로그인 Flow 문서
	•	🎯 RAG 파이프라인 상세 문서

어떤 부분부터 더 디테일하게 확장해볼까요?
