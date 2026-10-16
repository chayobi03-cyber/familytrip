# 가족여행 플래너

전주·태안 등 여러 여행지를 하나의 모노레포에서 관리하는 가족여행 플래너입니다. 공통 규약은 `docs/`, 재사용 모듈은 `modules/`, 여행지별 계획과 앱은 `trips/<destination>/` 아래에 둡니다.

## 여행지

- [전주 여행 및 앱](trips/jeonju/README.md) — 기존 여행 플래너 앱은 `trips/jeonju/app/`에서 유지합니다.
- [태안·몽산포 여행](trips/taean/README.md) — 태안 여행 계획, 체크리스트, 출처 확인 기록을 관리합니다.

## 저장소 구조

```text
familytrip/
├── trips/
│   ├── jeonju/
│   │   ├── README.md
│   │   └── app/        # 전주 가족여행 플래너 웹 앱
│   │       ├── index.html
│   │       ├── css/
│   │       ├── js/
│   │       └── tests/
│   └── taean/          # 태안 여행별 계획과 현장 확인
├── modules/            # 재사용 가능한 여행/장소/체크리스트 모듈
├── docs/               # 제품 명세, 데이터 규약, 배포 및 인수 문서
└── .github/workflows/  # 테스트 후 전주 앱 디렉터리를 Pages로 배포
```

저장소 이름과 연결 상태는 GitHub에서 확인합니다. 현재 연동 커넥터가 보고하는 저장소 이름이 이전 이름과 달라질 경우, GitHub 앱 연결을 새 저장소와 동기화한 후 사용하세요.

## 전주 앱 실행

저장소 루트에서 다음 명령을 실행합니다.

```bash
cd trips/jeonju/app
python3 -m http.server 8000
```

브라우저에서 `http://localhost:8000`을 엽니다.

## 문서

- [저장소 구조·마이그레이션 명세](docs/REPOSITORY_LAYOUT.md)
- [앱 확장 가이드](docs/EXTENDING.md)
- [제품·기능 명세](docs/SPECIFICATION.md)
- [인수·회귀 테스트 기준](docs/ACCEPTANCE_TESTS.md)
- [배포 및 운영 가이드](docs/DEPLOYMENT.md)
- [공통 여행 모듈 규약](modules/README.md)

## 공통 원칙

- 계획된 정보와 공식 출처로 확인된 정보를 구분합니다.
- 운영시간, 가격, 예약, 조석, 날씨 등 변동 정보에는 출처와 확인 시각을 남깁니다.
- 공개 저장소에는 숙소 예약번호, 연락처, 가족의 실시간 위치·이동 일정 등 민감한 정보를 기록하지 않습니다.
- 앱의 일정과 사진은 현재 브라우저 로컬 저장소에 보관되며 자동으로 기기 간 동기화되지 않습니다.
