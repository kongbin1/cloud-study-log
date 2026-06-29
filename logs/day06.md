# 6일차 (2026-06-29)

## 오늘 한 것
- 웹/HTTP 기본 개념 정리 (클라이언트–서버, 메서드, 상태 코드, URL, REST, JSON)
- `curl`로 실제 API에 요청을 보내며 배운 개념을 눈으로 확인

> 흐름: **개념 먼저 잡고 → curl로 실습**. 백엔드(FastAPI) 들어가기 전 꼭 필요한 기초.

---

## 개념 정리 (오늘의 이론)

### 1. 웹의 기본 형태
- 클라이언트 — 요청 → ← 응답 — 서버
- **클라이언트** = 요청하는 쪽 (손님, 크롬, 앱)
- **서버** = 응답하는 쪽 (답을 가져다주는 쪽)
- **HTTP** = 둘이 주고받는 약속(언어)

### 2. HTTP 메서드 — "무엇을 하고 싶은지"
| 메서드 | 하는 일 | 비유 |
| --- | --- | --- |
| GET | 조회(가져오기) | 메뉴판 보여주세요 |
| POST | 생성(새로 만들기) | 주문할게요 |
| PUT | 수정 | 주문 바꿀게요 |
| DELETE | 삭제 | 주문 취소할게요 |

### 3. 상태 코드 — "결과가 어땠는지"
| 코드 | 뜻 |
| --- | --- |
| 200 | 성공 |
| 404 | 페이지 없음 |
| 500 | 서버 에러 |

### 4. URL — "어디에 요청할지" 주소
`https://api.example.com/users/3?sort=name`
→ `프로토콜://도메인(서버)/경로(무엇을)/쿼리(옵션)`

| 부분 | 뜻 | 비유 |
| --- | --- | --- |
| 도메인 | 어느 서버 | 어느 건물 |
| 경로(Path) | 그 서버 안 무엇 | 몇 층 몇 호 |
| 쿼리(query) | 추가 조건 | "이름 순" |

### 5. REST — API를 만드는 "약속/스타일"
> 자원(데이터)은 **URL**로 표현하고, 그 자원에 뭘 할지는 **메서드**로 정한다.

| 하고 싶은 것 | 메서드 + URL |
| --- | --- |
| 사용자 목록 보기 | GET /users |
| 3번 사용자 보기 | GET /users/3 |
| 새 사용자 만들기 | POST /users |
| 3번 사용자 수정 | PUT /users/3 |
| 3번 사용자 삭제 | DELETE /users/3 |

### 6. JSON — 데이터를 주고받는 "형식"
사람도 읽을 수 있는 글자 형식.
```json
{
  "id": 3,
  "name": "오자빈",
  "isStudent": true,
  "skills": ["Python", "Java"]
}
```
규칙:
- `{ }` ⇒ 하나의 묶음(객체)
- `"키" : 값` 형태 (키는 항상 큰따옴표)
- 값 종류: 문자(`"오자빈"`), 숫자(`3`), 참/거짓(`true`), 목록(`[ ]`)

---

## 실습 — curl로 직접 확인

### 요청
```bash
curl -i https://jsonplaceholder.typicode.com/users/99999 | less
```
- `-i` : 응답 **헤더까지** 같이 출력
- `| less` : 긴 출력을 페이지 단위로 보기 (5일차 파이프 응용)

### 결과
```
HTTP/2 404
content-type: application/json; charset=utf-8
content-length: 2
server: cloudflare
via: 2.0 heroku-router
x-ratelimit-limit: 1000
x-ratelimit-remaining: 999
cf-cache-status: HIT
```
→ 응답 본문은 `{}` (빈 객체)

### 개념이 실제로 보인 것
- `GET /users/99999` → REST 스타일 URL (자원=users, 99999번)
- 없는 유저라서 **404** (배운 상태 코드 그대로)
- `content-type: application/json` → 배운 **JSON 형식**으로 응답
- `x-ratelimit-limit / remaining` → **API 요청 제한(rate limit)** 헤더
- `cf-cache-status: HIT` → Cloudflare **캐시 적중**
- `server: cloudflare`, `via: heroku-router` → 어떤 인프라를 거쳐오는지

---

## 배운 것 / 헷갈렸던 것
- **이론 → 실습 연결**이 핵심. 404·JSON·URL 구조가 curl 응답에 그대로 보였다.
- `curl -i`는 헤더를 봐야 할 때 필수. 상태 코드와 content-type을 먼저 확인하는 습관.
- 헤더에는 본문 말고도 **rate limit, 캐시, 서버 정보** 등 많은 정보가 담겨 있다.

## 다음에 할 것
- POST로 데이터 보내보기 (`curl -X POST -d ...`)
- FastAPI로 직접 간단한 REST API 만들어서 이 개념들을 서버 쪽에서 구현해보기

## 한 줄 회고
> "HTTP가 약속이다"라는 말이 curl 응답을 직접 보니 비로소 와닿았다. 이제 백엔드로 넘어갈 준비가 됐다.
