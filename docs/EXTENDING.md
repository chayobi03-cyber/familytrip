# 확장 가이드

현재 앱 루트는 `trips/jeonju/app/`입니다. 앱 내부는 **여행 데이터(`js/trips/*.js`)**, **일정 로직(`js/engine.js`)**, **저장소(`js/storage.js`)**, **화면(`js/app.js`)** 으로 분리되어 있습니다. 빌드 도구 없이 정적 파일만으로 동작합니다.

## 새 여행지 앱 데이터 추가 (코드)

1. 앱 루트 기준으로 `js/trips/jeonju.js`를 복사해 `js/trips/<새이름>.js`를 만듭니다.
2. `id`(고유), `title`, `dest`, `days`, `places`, `plans`를 수정합니다.
3. `index.html`에 새 파일의 스크립트 태그를 기존 여행 데이터 스크립트와 함께 등록합니다.
4. 저장소 루트에서 `cd trips/jeonju/app && node tests/engine.test.js`를 실행해 필수 방문지와 타임라인 규칙을 확인합니다.

여행별 문서와 장기 계획은 앱 내부 코드와 별개로 `trips/<destination>/` 아래에 둡니다. 새 목적지를 모노레포에 추가할 때는 먼저 해당 디렉터리의 README 및 계획/확인 문서를 작성합니다.

## 데이터 스키마

| 필드 | 설명 |
|---|---|
| `days[]` | `{n, label, start}` 일차와 시작 시각(HH:MM) |
| `mandatory[]` | 필수 방문지 place id. 모든 대안에 포함·복구되며 필수끼리 순서 교환은 거부됨 |
| `places[]` | `{id,name,cat,lat,lon,dur,desc,hours,tips[],checks[],food[],approx}` — `checks`는 방문 전 확인 체크리스트, `approx:true`는 좌표 근사 표시 |
| `plans{}` | 대안(균형/체험/여유 등 개수 제한 없음). `days[n]`은 `{id,p,dur?,included?}` 배열 (`p`=place id, 장소 없는 일정은 `rest:"이름"`) |

## 앱 안에서 만들기 (코드 없이)

상단 「＋ 새 여행」으로 새 여행을 만들고, 「장소」 탭에서 내 장소를 등록해 일정에 추가합니다. 「설정」에서 일차 추가, 백업/복원이 가능합니다.

## 확장 지점

- **장소 카테고리**: `js/app.js`의 `CATS`에 추가합니다.
- **이동 추정**: `js/engine.js`의 `travel()`을 수정합니다 (실시간 교통은 범위 밖).
- **저장소 교체**: `Storage`의 `load/save/photos` 인터페이스를 유지합니다.
- **가족 간 공유**: 현재는 JSON 내보내기/가져오기가 공유 수단입니다.
