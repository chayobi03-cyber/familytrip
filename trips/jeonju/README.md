# 전주 여행

전주 여행에 관한 일정, 방문지, 인수 메모를 모으는 영역입니다.

## 앱

전주 가족여행 플래너는 [`app/index.html`](app/index.html)에서 시작합니다. 저장소 루트에는 공통 명세와 다른 여행지 자료가 있고, 앱 HTML/CSS/JS 및 테스트는 이 하위 디렉터리에 함께 있습니다.

앱 실행:

```bash
cd trips/jeonju/app
python3 -m http.server 8000
```

테스트:

```bash
node tests/engine.test.js
```

## 관련 문서

- 제품·기능 명세: [`docs/SPECIFICATION.md`](../../docs/SPECIFICATION.md)
- 확장 가이드: [`docs/EXTENDING.md`](../../docs/EXTENDING.md)
- 인수·회귀 테스트 기준: [`docs/ACCEPTANCE_TESTS.md`](../../docs/ACCEPTANCE_TESTS.md)
- 공통 모듈 규약: [`modules/README.md`](../../modules/README.md)

## 운영 규칙

- 필수 방문지, 방문 순서, 운영시간 등은 계획 데이터와 현장 확인값을 구분합니다.
- 영업/입장 여부, 공연 일정, 주차, 예약 가능 여부는 출발 전에 공식 채널에서 확인합니다.
- 여러 여행에서 재사용할 기능은 먼저 `modules/`의 작성 규약과 비교해 공통화 여부를 판단합니다.
