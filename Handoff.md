# Handoff — Familytrip Monorepo

- 작성일: 2026-10-17 (KST)
- 저장소: https://github.com/chayobi03-cyber/familytrip
- 기본 브랜치: `main`
- 목적: 가족여행 관련 앱·여행별 계획·재사용 모듈을 한 저장소에서 지속 관리
- 상태: **모노레포 구조 반영 완료 / Pages 배포 확인 대기**

## 1. 현재 상태 요약

| 영역 | 상태 | 확인 내용 |
|---|---|---|
| 저장소 이름 | 완료 | GitHub 메타데이터에서 `chayobi03-cyber/familytrip` 확인 |
| 모노레포 뼈대 | 완료 | `trips/jeonju/`, `trips/taean/`, `modules/`, `docs/` |
| 전주 앱 이동 | 완료 | 앱 HTML/CSS/JS와 테스트를 `trips/jeonju/app/`로 이동 |
| 엔진 자동 테스트 | 통과 확인 | Actions의 `test` job이 통과한 실행 확인 |
| GitHub Pages 배포 | 미완료 | `configure-pages`가 Pages 사이트 조회 시 404 반환 |
| 실제 웹 URL 인수 테스트 | 대기 | Pages 활성화 및 배포 성공 후 진행 |

## 2. 저장소 구조

```text
familytrip/
├── .github/workflows/pages.yml  # 엔진 테스트 + GitHub Pages 배포
├── docs/
│   ├── REPOSITORY_LAYOUT.md     # 모노레포 구조와 마이그레이션 상태
│   ├── SPECIFICATION.md         # 앱 기능/제품 명세
│   ├── EXTENDING.md             # 확장 가이드
│   ├── ACCEPTANCE_TESTS.md      # 인수·회귀 테스트
│   └── DEPLOYMENT.md            # 배포·운영 가이드
├── modules/                     # 재사용 가능한 여행/장소/체크리스트 모듈
└── trips/
    ├── jeonju/
    │   ├── README.md
    │   └── app/
    │       ├── index.html
    │       ├── css/
    │       ├── js/
    │       └── tests/
    └── taean/                   # 태안 여행 계획 및 확인 기록
```

## 3. GitHub Pages 배포 차단 원인

현재 워크플로 파일 `.github/workflows/pages.yml`은 아래 순서로 실행됩니다.

1. `trips/jeonju/app`에서 `node tests/engine.test.js` 실행
2. `actions/configure-pages@v5`
3. `trips/jeonju/app`을 Pages artifact로 업로드
4. `actions/deploy-pages@v4`로 배포

최근 실행은 `configure-pages` 단계에서 Pages 사이트를 조회할 때 404가 반환되어 실패했습니다. 앞서 `enablement: true`를 사용한 실행도 연결된 GitHub 토큰의 관리자 권한 부족으로 실패했습니다. 현재 워크플로에서는 해당 옵션을 제거했습니다.

### 사용자가 직접 확인할 설정

1. [GitHub Pages 설정](https://github.com/chayobi03-cyber/familytrip/settings/pages)을 엽니다.
2. **Build and deployment → Source**를 `GitHub Actions`로 선택합니다.
3. 변경사항을 저장합니다.
4. [GitHub Actions](https://github.com/chayobi03-cyber/familytrip/actions)에서 새 실행을 확인합니다.
5. `test`와 `deploy`가 모두 성공한 뒤 GitHub가 제공하는 Pages URL을 열어 인수 테스트합니다.

**배포 URL은 성공한 Pages 설정/실행 결과에서 확인하기 전에는 완료된 것으로 간주하지 않습니다.**

## 4. 검증 명령

저장소 루트에서 실행:

```bash
cd trips/jeonju/app
node tests/engine.test.js
```

Playwright 스모크 테스트는 기존 실행 환경에 `PW_MODULE` 및 `CHROME` 환경 변수가 구성된 경우 같은 디렉터리에서 실행합니다. 환경이 없으면 테스트를 실행한 것처럼 보고하지 않습니다.

## 5. 운영 원칙

- 여행별 계획은 `trips/<destination>/` 아래에 둡니다.
- 앱 코드/테스트는 해당 앱의 `app/` 하위에 둡니다.
- 여러 여행에서 재사용할 수 있는 기능은 `modules/`의 규약을 먼저 검토합니다.
- 가격, 운영시간, 예약 가능 여부, 조석, 날씨처럼 변동되는 정보에는 출처와 확인 시각을 기록합니다.
- 확인하지 않은 값은 확정 사실로 표현하지 않습니다.
- 공개 저장소에는 예약번호, 개인 연락처, 실시간 위치와 같은 민감정보를 올리지 않습니다.
- 앱의 브라우저 로컬 저장 데이터는 자동으로 기기 간 동기화된다고 가정하지 않습니다.

## 6. 다음 작업 순서

- [ ] GitHub Settings → Pages에서 Source를 `GitHub Actions`로 설정·저장
- [ ] 새 Actions 실행에서 엔진 테스트와 배포가 모두 통과하는지 확인
- [ ] 실제 Pages URL에서 데스크톱·모바일 화면, 지도 로딩, 일정 편집 및 새로고침 복원 확인
- [ ] 검증 완료 후 `docs/REPOSITORY_LAYOUT.md`와 `docs/DEPLOYMENT.md`의 상태를 갱신
- [ ] 태안 여행 자료를 실제 계획·출처·확인 시각 기준으로 채우고 공통 모듈 후보를 분리

## 7. 관련 링크

- [저장소](https://github.com/chayobi03-cyber/familytrip)
- [Actions](https://github.com/chayobi03-cyber/familytrip/actions)
- [Pages 설정](https://github.com/chayobi03-cyber/familytrip/settings/pages)
- [저장소 구조 명세](https://github.com/chayobi03-cyber/familytrip/blob/main/docs/REPOSITORY_LAYOUT.md)
- [배포 가이드](https://github.com/chayobi03-cyber/familytrip/blob/main/docs/DEPLOYMENT.md)
