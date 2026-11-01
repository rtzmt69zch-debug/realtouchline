# 2026-27 5대 리그 선수 데이터베이스 (초기 연동판)

## 포함 범위
- 프리미어리그, 라리가, 분데스리가, 세리에 A, 리그 1의 구단 목록 전체 (96개 구단)
- 프로젝트에 이미 등록된 구단별 대표 선수 데이터
- 선수별 `currentRating`, `potentialRating`, `position`, `role`, `league`, `club` 필드
- 기존 엔진 호환을 위해 `rating=currentRating`, `potential=potentialRating`도 유지

## 데이터 품질 안내
현재 파일은 **전체 등록 선수 명단 완성본이 아닙니다.** 각 구단에 프로젝트에서 제공된 대표 선수만 입력했으며, 그 외 선수 명단은 아직 확보되지 않았습니다. 포함된 능력치도 실제 경기 기록으로 검증된 공식 수치가 아니라 게임 초기 밸런스용 추정치이며 `verified:false`로 표시합니다. 나이·국적 등 확인되지 않은 정보는 임의로 채우지 않았습니다.

## 게임 연동
`src/player-data.js`가 `data/players/rosters.js`를 불러옵니다. 구단에 11명 이상 등록된 경우에만 `getImportedClubRoster()`가 실제 구단 선수단을 반환해 기존 `engine.js`의 `makeSquad()`가 사용합니다. 현재처럼 11명 미만인 구단은 기존 생성 선수단으로 안전하게 대체됩니다. 따라서 일부 대표 선수만 있는 데이터 때문에 게임 선수단이 한두 명으로 축소되는 문제를 방지합니다.

## FotMob JSON 가져오기
팀 단위로 확보한 FotMob JSON을 아래 명령으로 병합할 수 있습니다.

```bash
node scripts/import-fotmob-rosters.mjs <팀_JSON_파일> [구단명]
```

가져오기 결과는 `data/players/rosters.js`와 `data/players/rosters.json` 양쪽에 저장됩니다. 팀별 JSON의 `squad.squad` 구조를 사용합니다.

## 평가 등급
- 90–99 월드클래스
- 85–89 최상위권
- 80–84 상위권
- 75–79 준수
- 70–74 평균 이상
- 65–69 평균
- 55–64 성장형
- 54 이하 하위권
