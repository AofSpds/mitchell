# MITCHELL → IVA SNS Gateway affected-only 재검증 패킷

PACKET_ID = MITCHELL-SNSG-IVA-002-REREVIEW  
VERSION = 0.3  
DATE = 2026-09-19  
FROM = MITCHELL  
TO = IVA  
PROJECT = MITCHELL  
PRODUCT = SNS Gateway  
STATUS = AUTHOR_CORRECTION_COMPLETED / EXACT_CANDIDATE_FIXED / REREVIEW_REQUESTED / MERGE_HOLD

## 1. 요청과 원 판정

최초 독립검증 결과의 **SGV-F001–SGV-F004만** affected-only로 재검증한다. 원 보고서와 결과 인계를 검증 처음부터 반복하라는 요청으로 해석하지 않는다. 공통 파일 변경의 실질적 영향만 추가한다.

원 결과: AofSpds/mitchell@5133d5fe9af59b8ed06e7bf7bedba29108c10c79
경로: docs/execution/SNS_GATEWAY_IVA_RESULT_002_20260919.md
Blob: 575fbf734d47484a14a6d3fd8a1a79db736be60f
Document: MITCHELL-SNSG-IVA-002-RESULT v1.0.1

원 candidate FAIL, F001–F004 MEDIUM/P2, MERGE/RELEASE_DEPLOY HOLD를 보존한다. 원 SGV-01/03/08 PASS는 소스·fixture 범위, SGV-02/05/06/07 FAIL, SGV-04 INDETERMINATE다. 새 작성자 교정은 독립 PASS가 아니다.

## 2. 정확한 재검증 대상

```text
Repository = AofSpds/sns-gateway
Branch = work/sns-gateway-v0.1
PR = #1 (OPEN / DRAFT / UNMERGED)
Exact head = 4ab10f481f7da66c00591ebd6cf597de527b901c
Exact tree = caa83524a6118272952fd669d225d253dd477edb
Original reviewed head = cf6967ce920793a72f88da746af4d0a317e61ed8
Base main = 13d83595e31867c24fb8154074a7ad76d5995f95
Diff = 19 files / +648 / -100
Source files = 75
Author CI = 35439466823
```

시작 시 PR/branch head를 읽고 차이가 있으면 이 frozen candidate와 구분한다. 제품 main이나 merge-test ref를 검증 대상으로 바꾸지 않는다. 패킷 자체의 운영 commit은 패킷이 포함된 Git readback 영수증을 사용한다.

## 3. 읽을 결과와 코드

운영 저장소: `docs/execution/SNS_GATEWAY_IVA002_CORRECTION_COMPLETION_20260919.md`, `SNS_GATEWAY_IVA002_CORRECTED_MANIFEST_20260919.json`, 원 IVA 결과 §5. 기존 기준 설계와 과거 보고는 필요한 변경 영향 범위만 읽는다.
제품: `docs/IVA002_CORRECTIONS.md`, 변경된 소스와 `tests/iva002-corrections.test.mjs`, `tests/native/ManagedFileRegressions.swift`, `tests/native/kotlin/*`.

| Finding | 교정 경로 | 재검증 조건 |
|---|---|---|
| F001 | services/sources.ts, storage/lifecycle.ts, migration4.ts | 선두10개 영구 실패와 정상11번째, partial/throw/재시작·STARTED 순환, 실패 이력·PENDING 유지, 정상화 후회수, baseline/재등장·FIRST_OBSERVED/null·기존등록일 보존 |
| F002 | storage/history.ts, app/GatewayApp.tsx | 최신100 밖 미확인 접근, 전체/미확인 필터와 진짜 keyset 이전페이지, 동일시각 안정성,205개 미확인, v1이관, 사용자 해결 후 reset허용. 보호 제거·자동 재공유 금지 |
| F003 | Android LocalManagedFiles.kt/LocalPhotos.kt, services/maintenance.ts | UUID.tmp 엄격식별,0/부분쓰기/rename후DB미등록, active writer·최근temp 보호,1시간 유예 후회수·용량·초기화, 임의경로/원본/미확인공유 보호 |
| F004 | Kotlin/Swift LocalManagedFiles, services/maintenance.ts | 목록·child목록·metadata·remove/unlink 오류와 확정부재 구분, 미완전 inventory 성공금지, DB-known URI journal, 최종 empty증명 전reset보존, receipt중복/부분실패, 접근복구 후재개 |

F003/F004는 함께 검토하되 개별 finding 판정을 반환한다. tmp1시간은 staging7일과 다르다. Android minSdk24는 유지하며 Kotlin Os(API21+)를 사용한다. 앱 내 공유결과가 remote PUBLISHED로 바뀌지 않았는지 확인한다.

## 4. 작성자 증거와 재사용

- 표적 TS/SQLite/lifecycle42개(확장 Node110개에 포함), 확장 Node110개 PASS. 실제 제품 TS 함수와 SQLite를 실행하고 OS 경계만 fixture다.
- Kotlin23개 PASS는 비특권 Linux/JVM filesystem+Context/Os shim이다. 실제 Android Os 런타임 시험이 아니다.
- Swift 신규16개와 기존6개는 실제 Foundation/unlink 시험이며 Linux 비특권 및 macOS CI에서 확인했다.
- CI35439466823: checks / android / ios-simulator 모두 SUCCESS. checks105887666099 / android105887759446 / ios-simulator105887759379.
- Source artifact10582954297 outer SHA-256 1dae80fa852c79fa0cf502accd82477467109688a204ffdcb9d9905d9a59dce2.
- Inner source ZIP SHA-256 556c4a9f180747a0d457191f9cd5f6ae113c06005aa8cc1a0c6efd6dda408606.
- 소스75개 byte equality·tree·ZIP CRC를 확인했고 다운로드소스의 표적42개도 PASS.

중간 교정9847200c의 CI35438877502는 Android Os.unlink 심볼 오류로 실패했다. 공개 Os.remove로 수정한 후속4ab10f48가 최종 대상이다. 첫 성공 iOS/JS를 Android 성공으로 확대하지 않았다.

원 후보 CI35433487023은 재실행하지 않았다. 이미 유효한 작성자 자료는 재사용하고 IVA가 새로 실행한 fixture와 구분한다. 전체 독립검증 또는 전체 앱 재구현 루프를 만들지 않는다.

## 5. 미실행·권한·반환

실기/SNS는 NOT_RUN/INDETERMINATE다. PhotoKit/SAF 권한철회·iCloud-only·metadata·백업·실기통신, 알림 cold/warm·잠금·재부팅·시간대, 실제 SNS수신·공개게시를 source/native compile로 PASS 처리하지 않는다. unsigned APK/simulator .app은 서명된 실기 배포본이 아니다.

허용: 읽기 중심 독립검증, 격리 합성fixture, IVA 자신의 정제 결과 Git 보존.
금지: 제품 수정, PR ready/merge, 제품main, 릴리스/배포, 사용자기기·계정·키·실사진·SNS전송/게시, PMO/다른Persona dispatch.

상세 결과는 Git에 기록하고 채팅은 MITCHELL에 복사 가능한 반환패킷으로 한다. 반드시 exact head/tree, F001–F004 각각 PASS/FAIL/INDETERMINATE/NOT_RUN, 새finding 유무, 재사용/신규증거, 문서path/commit/blob, MERGE_RECOMMENDATION 및 RELEASE_DEPLOY_RECOMMENDATION, 미실행·권한·다음조치를 포함한다.

PMO_RUNTIME=NOT_DISPATCHED / IVA_AFFECTED_REREVIEW=NOT_RUN / MERGE=HOLD / RELEASE_DEPLOY=HOLD.
