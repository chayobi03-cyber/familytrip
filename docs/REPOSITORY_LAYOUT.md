# 가족여행 플래너 — 저장소 구조 및 마이그레이션 명세

- 문서 버전: 1.1
- 기준일: 2026-10-16
- 목표 저장소 이름: `familytrip`
- 방식: 단일 모노레포

## 1. 목적

전주·태안 등 여행별 계획과 앱을 하나의 저장소에서 관리합니다. 공통 규약은 `docs/`, 재사용 모듈은 `modules/`, 여행지별 자료는 `trips/<destination>/`에 배치합니다.

## 2. 저장소 구조

| 경로 | 책임 |
|---|---|
| `trips/jeonju/README.md` | 전주 여행별 문서, 운영 원칙, 앱 진입 링크 |
| `trips/jeonju/app/` | 전주 여행 플래너의 HTML/CSS/JS와 테스트 |
| `trips/taean/` | 태안 여행 계획, 체크리스트, 장소 후보, 출처 확인 기록 |
| `modules/` | 다른 여행에서도 재사용할 장소/활동/체크리스트 모듈 |
| `docs/` | 제품 명세, 데이터 규약, 아키텍처, 운영·배포 문서 |
| `.github/workflows/pages.yml` | 전주 앱 테스트와 Pages 배포 |

## 3. 전환 원칙

1. 기존 기능과 소스 코드를 보존하면서 이동합니다.
2. 앱 소스 변경 시 Pages workflow와 테스트 경로를 같은 변경에서 수정합니다.
3. 여행별 계획 문서와 앱 런타임 데이터 파일을 구분합니다.
4. 실시간 운영정보에는 출처와 확인 시각을 기록하며, 확인되지 않은 정보는 확인 완료로 표시하지 않습니다.
5. 공개 저장소에 숙소 예약번호, 연락처, 실시간 이동 경로 등 민감한 정보를 올리지 않습니다.

## 4. 전환 상태

### Phase 1 — 모노레포 스캐폴딩
- [x] 루트 README를 가족여행 모노레포 개요로 정리
- [x] `trips/jeonju/` 및 `trips/taean/`을 생성
- [x] `modules/` 공통 모듈 작성 규약 추가

### Phase 2 — GitHub 저장소 이름
- [x] 사용자가 저장소 이름 변경 완료를 보고함
- [ ] GitHub 연결에서 canonical 저장소 이름과 URL을 재확인

현재 연결된 GitHub 도구는 여전히 이전 저장소 이름을 반환하므로, 연결 메타데이터 동기화 이후 canonical URL을 재확인해야 합니다.

### Phase 3 — 전주 앱 이동 및 배포 경로
- [x] 앱의 HTML/CSS/JS와 테스트를 `trips/jeonju/app/` 아래로 이동
- [x] workflow 테스트 작업 디렉터리를 이동한 앱 경로로 변경
- [x] Pages artifact 경로를 `trips/jeonju/app`으로 변경
- [x] 문서의 앱/테스트 경로 갱신
- [ ] GitHub Actions 테스트 및 Pages 배포 최종 상태 확인
- [ ] 브라우저에서 실제 Pages 배포 URL 인수 테스트

## 5. 테스트

저장소 루트에서 실행:

```bash
cd trips/jeonju/app
node tests/engine.test.js
```

Playwright 스모크 테스트는 별도 환경 변수 `PW_MODULE`과 `CHROME`를 지정해 같은 디렉터리에서 실행합니다.

## 6. 완료 기준

- 여행지별 자료가 `trips/<destination>/`로 분리되어 있습니다.
- 전주 앱과 테스트가 `trips/jeonju/app/` 아래에 함께 위치합니다.
- Pages workflow가 앱 디렉터리만 배포하고 엔진 테스트를 먼저 수행합니다.
- 저장소 이름과 실제 Pages URL은 GitHub에서 직접 확인한 값으로 기록합니다.
