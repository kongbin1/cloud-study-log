# 📘 3개월 학습 로드맵 (7일차~)

> 목표: **FastAPI 백엔드 + AWS 클라우드 + IoT(캡스톤: 스마트 창문)** / 자격증: AWS Cloud Practitioner → SAA
> 전제: 주 5회 · 회당 1~2시간 (1주 ≈ 4~5일차). 날짜보다 **순서**가 중요하며, 페이스는 조절 가능.

## ✅ 지금까지 (1~6일차)
- 리눅스 기본 명령어 / 네트워크 트러블슈팅 / apt
- 파일 권한(chmod) / grep·파이프
- 웹·HTTP 기초(메서드·상태코드·URL·REST·JSON) + curl 실습

---

## 🗺️ 전체 그림

| 월 | 테마 | 끝나면 할 수 있는 것 |
| --- | --- | --- |
| 1개월차 | 파이썬 백엔드 (FastAPI + DB) | 내 손으로 REST API 서버 만들기 |
| 2개월차 | AWS 클라우드 + 배포 | 그 서버를 인터넷에 띄우기 + 자격증 |
| 3개월차 | 캡스톤(스마트 창문) IoT 연동 | 센서 → 클라우드 → API 완성 |

---

## 🟢 1개월차 — 백엔드 기초 (FastAPI + DB)

### Week 1: FastAPI 첫 서버 (Day 7~10)
- [ ] Day 7: FastAPI 첫 서버 + uvicorn + `/docs` (curl로 내 서버에 요청)
- [ ] Day 8: Path / Query 파라미터, 엔드포인트 여러 개
- [ ] Day 9: POST + Pydantic 모델 (요청 body 받기)
- [ ] Day 10: PUT / DELETE → 메모리 기반 CRUD 완성

### Week 2: 데이터베이스 (Day 11~14)
- [ ] SQL 기초 (SELECT / INSERT / UPDATE / DELETE) + SQLite
- [ ] SQLAlchemy(ORM)로 FastAPI에 DB 연결
- [ ] CRUD를 메모리 → DB로 전환
- [ ] 정리 + 복습

### Week 3: 백엔드 실전 요소
- [ ] 에러 핸들링 · 상태코드 제대로 쓰기
- [ ] 환경변수, CORS
- [ ] 간단한 인증 (API key / 토큰 맛보기)
- [ ] pytest 기초 (테스트)

### Week 4: 도커 + 1차 미니 프로젝트
- [ ] Docker 개념 + Dockerfile로 FastAPI 컨테이너 실행
- [ ] docker-compose (앱 + DB)
- [ ] 미니 프로젝트 "할 일 관리 API" 마무리
- [ ] README / 문서 정리

---

## 🟡 2개월차 — AWS 클라우드 + 배포

### Week 5: AWS 입문
- [ ] 계정 생성, IAM, 프리티어, 리전 / AZ
- [ ] 핵심 서비스 개요 (EC2 · S3 · RDS · VPC)
- [ ] Cloud Practitioner 이론 시작

### Week 6: EC2 실배포
- [ ] EC2 띄우기 + SSH 접속 (1~5일차 리눅스 실력 발휘)
- [ ] FastAPI 앱 EC2에 올리기
- [ ] 보안그룹, nginx

### Week 7: 관리형 서비스
- [ ] RDS (관리형 DB)
- [ ] S3 (파일 저장)
- [ ] 도메인 + HTTPS 맛보기

### Week 8: 🎯 AWS Cloud Practitioner 시험
- [ ] 모의고사 풀이
- [ ] 약점 복습
- [ ] 시험 응시

---

## 🔵 3개월차 — 캡스톤(스마트 창문) IoT 연동 + 마무리

### Week 9: IoT 설계
- [ ] 센서 데이터 흐름 설계
- [ ] MQTT 개념
- [ ] AWS IoT Core 개요

### Week 10: 디바이스 ↔ 백엔드
- [ ] 센서 데이터 받는 API
- [ ] 데이터 저장 / 실시간성

### Week 11: 통합 / 자동화
- [ ] GitHub Actions (CI/CD) 맛보기
- [ ] 로그 / 모니터링
- [ ] 캡스톤 전체 통합 테스트

### Week 12: 마무리 + 다음 단계
- [ ] 프로젝트 문서화 / 포트폴리오 정리
- [ ] AWS SAA 학습 로드맵 시작

---

## 🏁 3개월 후 남는 것
**FastAPI 백엔드 + AWS 배포 + IoT 연동 + 자격증(Cloud Practitioner)**
