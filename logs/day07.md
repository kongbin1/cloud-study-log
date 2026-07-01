# 7일차 (2026-06-30)

## 오늘 한 것
- 3개월 학습 로드맵 작성 후, "클라우드 가려면 백엔드부터" → **백엔드 먼저** 가기로 결정
- 백엔드는 **우분투(서버 환경)** 에서 진행하기로
- 우분투에 파이썬 가상환경(venv) + FastAPI 설치
- **FastAPI 첫 서버**를 띄우고 curl로 JSON 응답 받기 성공

> 6일차에 curl로 *남의 서버*에 요청했다면, 오늘은 *내 서버*가 응답하는 쪽을 만들었다. 요청/응답 양쪽을 다 다뤄본 셈.

---

## 진행 과정

### 1. 개발 환경 준비
```bash
python3 --version          # 파이썬 확인
sudo apt install python3-pip python3-venv -y   # pip 설치

mkdir ~/fastapi-study
cd ~/fastapi-study
python3 -m venv venv       # 가상환경 생성
source venv/bin/activate   # 활성화 → 프롬프트에 (venv) 붙음

pip install fastapi uvicorn
```
- **venv = 이 프로젝트 전용 파이썬 상자.** 라이브러리가 다른 프로젝트랑 안 섞임.
- **fastapi** = 웹 서버를 쉽게 만드는 도구 / **uvicorn** = 그 서버를 실제로 돌리는 엔진

### 2. 첫 서버 코드 (main.py)
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def home():
    return {"message": "hello FastAPI"}
```
- `@app.get("/")` → **GET** 방식으로 `/` 주소에 요청 오면 (6일차 메서드)
- `return {...}` → 파이썬 딕셔너리가 자동으로 **JSON** 응답이 됨 (6일차 JSON)

### 3. 서버 실행 + 응답 확인
```bash
uvicorn main:app --reload      # main.py의 app 실행, 코드 바뀌면 자동 재시작
```
→ `Application startup complete.` + `Uvicorn running on http://127.0.0.1:8000`

서버가 터미널을 점유하므로, **tty2(두 번째 콘솔)** 로 전환해 로그인 후 요청:
```bash
curl http://127.0.0.1:8000/
# {"message":"hello FastAPI"}   ← 성공!
```

---

## 막혔던 문제 & 해결 (오늘의 핵심)

### 1. `pip insatll` 오타
`ERROR: unknown command "insatll" - maybe you meant "install"`
→ 철자 오타. 에러 메시지가 친절하게 알려줬다. **에러는 메시지부터 읽자.**

### 2. `SyntaxError: unterminated string literal`
서버 실행 시 5번째 줄 `@app.get("/")`에서 에러.
- 원인: 따옴표가 진짜 `"`가 아니라 한글 입력 상태에서 들어간 **다른 따옴표 문자**였음.
- 해결: 영문 상태에서 따옴표를 다시 타이핑 → 해결.
- 교훈: 눈에 똑같아 보여도 파이썬은 다른 글자로 본다.

### 3. 텍스트 전용 서버라 브라우저가 없음
GUI 없는 우분투 서버라 브라우저로 못 봄.
→ **tty2 콘솔**로 전환해 curl로 테스트. (실무 서버 환경과 동일한 방식)

---

## 배운 것
- **127.0.0.1(localhost) = 나 자신만 접속 허용.** 밖에서 접속하려면 `--host 0.0.0.0`으로 띄워야 한다. → 나중에 AWS EC2 배포할 때 똑같이 적용됨 (0.0.0.0 + 보안그룹에서 포트 열기).
- `uvicorn main:app` 에서 `main`=파일명, `app`=변수명.
- `--reload` 는 개발 중 자동 재시작용으로 편하다.
- 백그라운드 실행은 `&`, 종료는 `kill %1`.

## 다음에 할 것 (Day 8)
- Path / Query 파라미터 받기 (`/users/3`, `?sort=name`)
- 엔드포인트 여러 개 만들기
- 윈도우 브라우저에서 VM 서버 접속해보기 (`--host 0.0.0.0` + VM IP `192.168.142.128:8000/docs`)

## 한 줄 회고
> 드디어 "내가 만든 서버"가 응답을 뱉었다. 6일차에 curl로 보던 그 JSON을 내 손으로 만들어냈다. 백엔드의 진짜 출발점.
