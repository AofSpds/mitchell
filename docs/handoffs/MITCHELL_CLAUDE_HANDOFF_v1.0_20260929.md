# MITCHELL → Claude 프로젝트 이관서

DOCUMENT_ID = MITCHELL-CLAUDE-HANDOFF-20260929-001
VERSION = 1.0
DATE = 2026-09-29
FROM = MITCHELL
TO = Claude / Owner가 전달하여 시작하는 후속 작성자
PROJECT = MITCHELL
PRIMARY_PRODUCT = AofSpds/sns-gateway
STATUS = HANDOFF_PREPARED / CLAUDE_NOT_DISPATCHED
SOURCE_ANCHOR = AofSpds/mitchell@ceecfc922193b0c2d2e4f99117fb3edfb0c047ee

## 0. 가장 먼저 알아야 할 정정

이 문서는 과거 대화의 마지막 상태를 복제하지 않고 2026-09-29 GitHub connector readback을 기준으로 작성했다. 기록 원문은 2026-09-19~20의 사건이며, 오늘 새로 구현·검증한 것으로 바꾸지 않는다.

**SNS Gateway의 지정 폴더/앨범, 날짜별 묶음/revision, 오래된 알림 날짜 처리, 보존·삭제·용량 관리 코드는 이미 구현됐고, IVA 지적 네 건의 교정과 affected-only PASS 후 PR #1이 main에 병합됐다.** 네 영역을 미구현으로 다시 만들지 않는다. 남은 것은 실제 기기·SNS 수락, SGV-04 알림/재개 검증, 승인된 서명·설치·릴리스 절차다. 새 시험에서 발견하는 결함만 해당 범위로 교정한다. [S1–S4]

Windows Bootstrap PR #1도 병합됐고, macOS Bootstrap은 최초 IVA FAIL 후 교정 후보 재검증 대기다. Web Starter는 여전히 Draft·미병합이다. 현재 운영 CURRENT의 주 작업은 Mac 교정 재검증 대기이며, 이번 이관은 이를 SNS 미구현 상태로 되돌리지 않는다. [S5–S7]

일부 제품 README/AGENTS/옛 계획에는 `main 미병합`, `IVA NOT_RUN`, 초기 후보 설명이 남아 있다. 이는 문서 상태 불일치다. 현재 PR/ref 및 후속 exact 결과를 우선하되 과거 보고서 자체는 역사적 증거로 보존한다. 특히 과거 68개 시험/f5dfbe9 후보의 ZIP을 최신 릴리스로 쓰지 않는다.

## 1. 사용자 의도와 유지할 제품 계약

사용자는 개발자이며 비개발자 친구가 쉽게 사용하고, 개발자는 GitHub에서 구조·변경을 점검할 수 있는 제품을 원한다. 같은 설계 질문이나 작은 구현 선택을 반복 확인시키지 않는다. 제품 이름은 `sns-gateway`이며 사진 공유는 SNS 접속 도우미의 첫 기능이다.

확정된 현 범위는 다음과 같다. 원래의 서버형/Meta HTTP API 제안은 현 모바일 경로에서 비채택이다. 사용자에게 API 키를 물어본 과거 질문은 서버 추가 승인으로 해석하지 않는다. [S1, S8]

```text
휴대폰 게시함 / 지정 폴더·앨범
  → 로컬 사진 사본·문구·날짜 묶음
  → 09:00 OS 로컬 알림
  → 사용자가 알림 선택·잠금 해제·미리보기
  → iOS/Android OS 공유 API
  → 공식 SNS 앱에서 사용자가 계정·내용·공개 범위 확인 후 게시
```

- iPhone와 Galaxy 공통 지원을 목표로 하되 실제 OS/SNS 조합별 지원은 시험 결과로만 표시한다.
- 우리 앱의 외부 서버, 임시 이미지 호스팅, Supabase, SNS HTTP API/OAuth, API 키, 자체 로그인, 원격 Push, AI Caption API는 없다.
- SNS 앱 자체의 로그인과 인터넷 업로드는 사용자의 SNS 앱이 담당한다. 우리 앱의 로컬 전용과 인터넷 없는 SNS 공개를 혼동하지 않는다.
- 09:00은 알림 기준이다. 정시 게시 완료, 잠금 상태의 강제 SNS 화면 실행, 상시 폴더 감시, 완전 무인 게시를 보장하지 않는다.
- 공유 callback은 원격 게시 영수증이 아니다. 관측한 OS 결과와 사용자 완료 진술을 구분하고 remote post ID를 만들지 않는다.
- 실제 키·개인 사진·서명키·대화 원문·원본 로그·비공개 프로젝트 원문을 공개 Git/CI에 저장하지 않는다.

## 2. 역할·수신·실행 권한

현재 ChatGPT 채널의 페르소나는 계속 MITCHELL이다. PMO는 기존 Codex WORK 작업자 이름이며 `NOT_DISPATCHED`다. IVA는 별도 독립검증자다. 다른 페어 검증자나 페르소나는 설치하지 않는다. **Claude는 이 문서를 Owner가 전달하는 외부 후속 작성자이며, PMO/IVA를 자동 재명명하거나 새 상설 Persona를 등록하는 것이 아니다.**

직전 사용자 지시는 남은 구현 진행, 이번 지시는 Claude 이관용 문서·패킷 작성이다. 이번 작성 세션에서 Claude를 실제 실행하지 않았다. Owner가 패킷을 전달하여 시작하면 기존 SNS Gateway 범위의 후속 작성 작업을 이어간다. 수신자가 도구/저장소 접근을 확보한 사실은 직접 확인해야 한다.

| 처리 | 범위 |
|---|---|
| 즉시 착수 가능한 후속 작업 | exact Git 복구, 현재/과거 문서 불일치 정리, 기기 수락 계획과 실행 가능한 하네스/기존 절차 정리, 격리 작업 사본의 자체 검사·빌드, 관측된 결함의 국소 교정, 작업 브랜치·Draft PR·완료보고 |
| 별도 허용이 필요한 실제 효과 | 사용자 기기에 설치, OS 권한 동의, SNS로 합성/실사진 전달, 공개 게시, Apple/Meta 계정·결제·개인 서명키 등록, 제품 main 추가 병합, 공개 릴리스·스토어 제출 |
| 이번 기본 실행 대상 아님 | Bootstrap/Web Starter 제품 변경, Mac 독립 IVA를 Claude가 자동 수행, 새로운 서버/API/의존 플랫폼 도입 |

과거 SNS PR #1과 Windows PR #1 병합 승인은 해당 exact 후보에 대해 이미 사용된 처분이다. 이를 미래 PR의 포괄 병합권한으로 확장하지 않는다. 제품 CURRENT/기억 lineage는 MITCHELL이 소유한다. Claude는 결과와 정제 memory delta를 반환하고, 운영 원장을 임의로 덮어쓰지 않는다.

## 3. 저장소 지도와 정확한 출발점

| 저장소·경로 | 확인한 ref | 현재 상태·역할 |
|---|---|---|
| AofSpds/mitchell main | ceecfc922193b0c2d2e4f99117fb3edfb0c047ee | 이관 준비 전 운영 anchor; CURRENT generation 14. 정책·기억·보고서 저장소 |
| AofSpds/sns-gateway main | 1c7cd117d18929cdf40abc0e4f73fec4c87b2c65 | PR #1 MERGED/CLOSED. 후속 SNS 작업의 출발점 |
| SNS 검증된 작성자 head | 4ab10f481f7da66c00591ebd6cf597de527b901c | 과거 work/sns-gateway-v0.1. main과 source tree 동일 |
| SNS source tree | caa83524a6118272952fd669d225d253dd477edb | 75개 소스 파일. 재구현/구후보 재병합 금지 |
| AofSpds/bootstrap main | 7ede3b394032b3f1da60235cb0ef2dad410df565 | Windows PR #1 MERGED/CLOSED; 실기 설치 수락은 별도 |
| Bootstrap Mac PR #2 | work/bootstrap-macos-v0.2 @ 30a06ea0807a2c9686f4dd15062e08fd56bdf891 | OPEN/DRAFT/UNMERGED; tree 4692aadb0637c4c3765c237431357ea4b34d1184; 재검증 대기 |
| AofSpds/web-starter main | 0d855cd9aaf1ee8ebf31a2a7af6d67c90367d0d6 | 초기 진입점 |
| Web Starter PR #1 | work/web-starter-v0.1 @ a2c63cca6bdbf0667ba2d9e1c8d8e7af2e137a17 | OPEN/DRAFT/UNMERGED; tree 34716ab2e0f82f625f6f6ada1cf0c205d665da51 |

`mitchell`의 이관 문서 저장 커밋은 위 SOURCE_ANCHOR 뒤에 생긴다. 정확한 저장 commit/blob은 최종 전달 패킷과 영수증에서 확인하며, 이 문서가 자신의 미래 commit을 예측하지 않는다. [S1, S3, S5–S7]

## 4. Claude의 최소 읽기 순서

1. 이 이관서와 전달 패킷. 다음으로 MITCHELL의 README/CURRENT/AGENTS/관련 DECISIONS를 현재 ref로 읽어 변경 유무를 비교한다.
2. SNS Gateway의 main exact ref와 PR #1 MERGED 상태를 확인한다. main README의 오래된 상태 문구만으로 결론 내리지 않는다.
3. SNS 소스의 `docs/WORK_PLAN_v0.1.md`, `docs/LIFECYCLE.md`, `docs/IVA002_CORRECTIONS.md`, `docs/BUILD_MATRIX.md`, `docs/DEVICE_COMPATIBILITY.md`, `docs/PRIVACY.md`, `docs/RECOVERY.md`를 읽는다.
4. MITCHELL anchor의 `SNS_GATEWAY_LIFECYCLE_COMPLETION_20260919.md` → `SNS_GATEWAY_IVA_RESULT_002_20260919.md` → `SNS_GATEWAY_IVA002_CORRECTION_COMPLETION_20260919.md` → `SNS_GATEWAY_IVA002_AFFECTED_REREVIEW_RESULT_20260919.md`를 읽는다. 모두 `docs/execution/` 아래다.
5. 작업할 경로의 실제 코드와 기존 시험만 확장한다. 전 역사를 다시 읽거나 Bootstrap/Web 전체를 전역 재검증하지 않는다.

Git 접근이 안 되면 첨부한 검증 소스로 읽기·격리 자체 검사를 할 수는 있지만, 현재성을 확인하지 못한 원격 mutation은 보류한다. 첨부에 `.git`이 없으면 새 저장소로 초기화해 원격을 덮어쓰지 않는다.

## 5. SNS Gateway 구현 현황 — 다시 만들지 않을 것

| 영역 | 존재하는 구현과 유지할 불변식 |
|---|---|
| 입력·게시함 | 직접 사진 가져오기, 방향 반영 JPEG 사본, 기존 등록시각·해시 보존. 휴대폰 원본 읽기만 수행 |
| 지속 소스 | Android 로컬 SAF 폴더/iOS PhotoKit 앨범 1개 연결. 앱 재개·새로고침 시 스캔. 완전한 목록 최대 2,000개, 회차당 신규 10장. iOS 앨범 선택 목록 최대 100개 |
| 날짜 의미 | 최초 소스 목록은 baseline만 생성. 처음 발견한 시각을 실제 폴더 추가시각으로 바꾸지 않음. FIRST_OBSERVED의 등록시각 null 보존·날짜 확인. 직접 등록분만 KST 00:00≤t<09:00 자동 후보 |
| 날짜 묶음 | service_date별 사진 순서·문구, immutable revision, expected-revision 충돌 거부, 공유 당시 snapshot 연결, 닫기·다시 열기 |
| 알림 | iOS 반복 calendar/Android inexact 알람. 시간 변경·ON/OFF·시험 알림. Android 원 예정일/iOS 전달일을 구분하고 오래된 알림 날짜 확인 |
| 이력 | OS 공유 요청·미확인·관측 취소·사용자 진술 분리. 미확인/전체 필터와 keyset cursor. double tap 단일 실행·중단 후 미확인 복구 |
| 보존·삭제 | 원본 보존. 열린 날짜/미확인 공유 보호. 닫힌 날짜와 해결된 공유의 7일 유예. journal·tombstone·reset_pending 및 재개. 관리 사본/staging 500MiB 한도 |
| 이관·실패 처리 | SQLite v1→v2→v3 및 부가 migration4. 이전 이력 보존·실패 rollback. 권한/I/O 오류를 부재나 삭제 성공으로 위장하지 않음 |

500MiB는 DB·OS 전체 사용량이 아니라 관리 사진·staging 범위다. 사본 한 장 10MiB와 최대 변 2048px도 현재 제품 계약에 있다. 여기 적은 값은 소스에 구현된 기존 기준이지 이관 과정에서 새로 정한 요구가 아니다. [S2, S8]

### 반드시 보존할 IVA 교정

- **SGV-F001:** 실패 선두 항목이 정상 후속 사진을 영구 차단하지 않도록 영속 시도 순서·횟수를 native 호출 전에 저장하고 순환한다. STARTED 중단도 다음 후보를 막지 않는다.
- **SGV-F002:** 오래된 미확인 기록도 UNRESOLVED/ALL 및 `(started_at,id)` cursor로 조회·해결할 수 있다. 100건 목록 절단으로 초기화가 막히지 않는다.
- **SGV-F003:** Android 앱 소유 UUID.tmp만 IMPORT_TEMP로 식별·용량 집계한다. active writer 보호와 비활성 1시간 유예를 지킨다. 이는 공유 staging 7일 유예와 다른 규칙이다.
- **SGV-F004:** 열거·metadata·삭제 오류는 fail-closed. 확인된 부재만 삭제 완료로 취급하고 DB-known 경로도 journal에 남긴다. 최종 완전 inventory가 비어야 reset을 끝낸다.

최초 cf6967ce 후보의 FAIL은 보존한다. 후속 4ab10f48의 네 finding만 PASS이며, 실제 폰 전 영역 PASS나 향후 수정본 PASS가 아니다. [S4]

## 6. 코드·빌드 탐색 지도

```text
app/GatewayApp.tsx                 화면·날짜/이력/소스/정리 조작
src/gateway/providers.ts           SNS 희망 대상·호환성 표시
src/domain/                       planning / inbox / lifecycle / sharing
src/services/                     sources / reminders / share / maintenance
src/storage/                      schema / migration2·3·4 / inbox / lifecycle / history
modules/local-platform/index.ts   TypeScript↔native 계약
modules/local-platform/ios/        LocalSources / LocalPhotos / LocalReminders
                                 ReminderLifecycle / LocalManagedFiles / LocalPlatformModule
modules/local-platform/android/src/main/java/expo/modules/localplatform/
                                 같은 역할의 Kotlin 코드
plugins/                          native 설정 생성
.github/workflows/ci.yml           exact source checkout·자체검사·native 빌드
```

기존 잠금 조합은 Expo 57.0.24, React Native 0.86.3, React 19.2.3, expo-dev-client 57.0.19, expo-sqlite 57.0.3, TypeScript 6.0.3이다. `.nvmrc`는 Node 22.16.0, CI Android는 JDK17이다. **소스/기존 빌드에 기록된 버전일 뿐 최신 또는 현재 공식 권장 조합이라는 주장이 아니다.** 패키지를 일괄 최신화하지 않는다. 변경이 필요한 경우에만 공식 문서와 실제 SDK 호환성을 확인하고 lockfile 변경을 별도 diff로 남긴다. [S8]

새 격리 checkout에서 필요한 경우 사용할 기존 명령:

```sh
npm ci --ignore-scripts
npm run check
npx expo install --check
npm run bundle
```

기존 성공 CI를 실행했다는 증거로 위 명령을 무정보 반복하지 않는다. 새 환경의 실행성·변경 영향 또는 빌드 산출물 만료 때문에 필요하면 목적을 명시하고 새 결과로 기록한다. native 절차는 CI/BUILD_MATRIX를 기준으로 한다. 개발 `npm run android`/`ios`는 기기 설치까지 이어질 수 있으므로 대상 기기 승인 전 무조건 실행하지 않는다.

현재 CI의 push 트리거는 과거 work/sns-gateway-v0.1에 제한된다. 새 브랜치에서는 main 대상 Draft PR의 pull_request 트리거로 exact head CI가 실행되는지 확인한다. 성공 이력이 있다는 이유로 새 브랜치도 자동 검사된 것으로 표시하지 않는다.

## 7. 검증 증거와 아직 모르는 것

### 재사용 가능한 증거

- 작성자 CI `35439466823`: checks/android/ios-simulator 성공 기록. 이관 작성 중 제품 CI를 새로 실행하지 않았다.
- 작성자 보고 수치: Node 확장 110, 표적 TS/SQLite 42, Kotlin/JVM 23, Swift 신규16+기존6. 서로 겹칠 수 있으므로 합쳐 고유 시험 수로 발표하지 않는다.
- 별도 IVA: TS/SQLite 21, Kotlin/JVM 14, Swift Foundation 11개 합성 시나리오, 총46 PASS/0 FAIL. 작성자 시험 수에 더하지 않는다.
- 검증된 작성자 head4ab10f48과 병합main1c7cd117은 tree caa83524가 같다. commit identity는 다르다.

### 이번 이관에서 새로 확인한 것

Git refs/PR/운영 보고서 readback, source artifact10582954297 다운로드, ZIP CRC·경로 확인, 75개 원본 파일로 Git tree 재계산을 수행했다. 결과는 caa83524와 일치한다. 제품 시험·native 빌드·독립 IVA·기기시험을 새로 수행한 것이 아니다.

```text
source artifact ID = 10582954297
source workflow   = 35439466823
outer SHA-256     = 1dae80fa852c79fa0cf502accd82477467109688a204ffdcb9d9905d9a59dce2
inner source SHA  = 556c4a9f180747a0d457191f9cd5f6ae113c06005aa8cc1a0c6efd6dda408606
source files      = 75
```

이 source/evidence는 인계 ZIP에 포함한다. 최신 run artifact 목록에서 현재 source만 반환됐고 과거 Android/iOS binary는 목록에 없었다. 과거 다운로드 링크의 생존을 가정하지 않는다. 별도 새 빌드가 필요할 수 있으며, 출처가 다른 과거 첨부 바이너리를 최신 제품으로 전달하지 않는다.

### 남은 실제 gate

`SGV-04 = INDETERMINATE`: 알림 날짜/재개 소스는 있지만 cold/warm tap·재부팅·지연·시간대 변경의 실제 OS 수락은 미확정이다.

`DEVICE_SNS_ACCEPTANCE = NOT_RUN / INDETERMINATE`: iPhone/Galaxy PhotoKit/SAF·권한 철회·iCloud-only·실제 decoder/방향/색상/메타데이터·파일 보호·저장공간 부족·백업/통신·Instagram/Threads 사진/순서/문구/취소/최종 게시를 실제 조합에서 확인해야 한다.

`SIGNED_DEVICE_INSTALL = NOT_DONE`, `RELEASE_DEPLOY = NOT_DONE / HOLD`. unsigned APK를 설치 완료본이라고 부르지 않고 simulator .app을 iPhone IPA라고 부르지 않는다. [S3–S4]

## 8. Claude의 첫 실행 묶음 — 재개 절차 제안

아래는 이번 인계에서 정리한 **후속 작업 순서**다. 새로운 구현·시험이 완료됐다는 기록은 아니다.

| 순서 | 작업 | 산출물/종료 기준 |
|---|---|---|
| C0 | refs/정책/결과를 읽고 현재 기준을 수신 | 현재 main/tree, 기존 PASS·미실행, 사용 가능한 실행환경을 한 표로 기록. 이미 완료한 네 기능 재구현 금지 |
| C1 | SNS 현재 문서의 stale status 정리 | README의 미병합/IVA NOT_RUN 등 현재 안내만 새 branch에서 정리. 당시 FAIL/완료보고·artifact는 수정하지 않음 |
| C2 | 기기시험 준비 묶음 | 기존 합성사진과 DEVICE_COMPATIBILITY를 활용해 설치·권한·알림·공유의 한 번에 실행 가능한 체크리스트와 증거 양식 작성 |
| C3 | 필요한 새 자체검사/unsigned 빌드 | 기존 lock 유지, source commit/tree·OS/SDK·빌드 결과·체크섬. 실행 안 된 경로는 NOT_RUN |
| C4 | 승인된 실기 수락 | 아래 그룹 E2E를 기기별 한 묶음으로 수행. 기기/SNS 전송·서명 경계의 허용이 없으면 준비까지만 |
| C5 | 관측 결함의 국소 교정 | 정확한 실패 관측→변경→영향 시험. 변경된 candidate만 별도 IVA로 전달, 전체 과거 PASS 반복 금지 |
| C6 | 반환 | 완료보고·manifest·독립검증 입력/남은 gate·MITCHELL 반환 패킷. main 자동 병합·스토어 제출 금지 |

새 작업 브랜치 제안: `work/sns-gateway-device-readiness-20260929`를 최신 확인된 main에서 만든다. 이름은 제안이며 아직 생성하지 않았다. 완료된 PR #1을 다시 열거나 과거 branch를 force-reset하지 않는다. ref가 이 문서와 다르면 먼저 변경 delta를 읽고 합치며, 사용자에게 이미 답한 설계 질문부터 다시 하지 않는다.

실기·서명 환경이 없으면 막힌 부분만 남기고 C1/C2 및 허용된 격리 검사·문서 작업을 끝낸다. 사용자 PC 경로, 연결된 기기, Apple 서명 계정이 있다고 추정하지 않는다. 준비가 필요하면 기기 설치·합성사진 전송 범위·서명 경로를 한 번의 묶음 안내로 요청하며 비밀키를 채팅에 붙여 넣게 하지 않는다.

## 9. 실기 그룹 E2E 제안과 증거 형식

코드를 다시 전역 검증하는 대신 실제 플랫폼 차이가 있는 경로를 기기별로 한 묶음씩 확인한다. 실제 SNS로 넘기는 행위도 상대 앱이 업로드할 수 있으므로 사전에 허용된 합성 사진만 사용한다. 공개 게시 허용이 없으면 SNS 작성 화면에서 중단하고 PUBLISHED를 기록하지 않는다.

| 그룹 | 입력·상황 | 기대 관측 |
|---|---|---|
| G1 게시함·소스 | 최초 baseline, 신규 11개 중 선두 실패10, 재개, 권한 철회, iCloud-only | 과거 사진 일괄 유입0; 정상11번째 진행; 오류≠사진없음; 임의 클라우드 다운로드0 |
| G2 날짜·revision | 당일00:00/08:59:59/09:00, 소스 unknown, 사진/문구 수정 | cutoff 유지; unknown 소급 확정0; 이전 공유 snapshot 불변 |
| G3 알림 | Android/iOS cold·warm, 전날알림 다음날 탭, OFF, 재부팅/시간대/지연 | 원 예정일/전달일 구분; 날짜 확인; 무인 화면전환/공유0; 실제 알림 미수신을 성공으로 표시하지 않음 |
| G4 공유 | 1장·3장·순서·문구, 취소·대상없음·double tap·중단 | 수신 앱별 관측 기록; callback≠게시성공; 자동재전송0 |
| G5 보존·복구 | 미확인 이력101건 이상, 임시파일1h/공유7d, quota, 선택삭제/reset 중단·I/O 실패 | 미확인 조회가능; 원본불변; active/recent 보호; journal 유지·접근회복 후 재개 |

시험 전용 복제 DB/합성 파일로 경계를 만드는 것이 원칙이며 실제 사용자 자료에 날짜조작·권한오류·초기화를 유발하지 않는다. 위 G1/G5의 서비스 로직은 기존 합성 PASS를 재사용하고, 기기 고유 차이 또는 변경 영향이 없는 조건을 모두 다시 실행할 필요는 없다.

제안 artifact: `DEVICE_ACCEPTANCE_<platform>_<date>.json`과 같은 stem의 정제 Markdown. 필드: sourceCommit/sourceTree, buildId/binarySha256, device/OS/SNS app version, permission profile, timezone, scenarios[{id,status,expected,observed,evidenceLocator}], testFixtureHash, externalEffectAuthorization, notRunReasons. 파일명·사진내용·본문·토큰·실제 사용자 경로는 정제한다. 예상 결과를 실제 observed로 채우지 않는다.

## 10. 다른 저장소의 대기 상태 — SNS 실행 범위에 자동 포함하지 않음

### Bootstrap

Windows는 Core8종과 선택 설치, Plan/Install/Verify, 기존 설치 보존, 로그 마스킹·재부팅 필요 처리 후보가 main에 병합됐다. 실제 Windows 설치·로그인은 별도다.

Mac은 .command/Bash3.2·Homebrew Core/Mobile/Optional·AI 선택 코드와 교정 후보가 PR #2에 있다. 지원선은 기존 D028 기준 Apple Silicon macOS15+ 설치,14 진단 전용, Intel 명시적 opt-in 미수락 경로다. 현재 버전/정책을 이번 이관이 새로 공식 검증한 것은 아니다.

MAC-F001 확장 사후조회 실패, F002 앱 부재/조회오류, F003 lock stderr 교정 후 작성자 CI는 성공 기록이 있으나 **새 후보 IVA affected-only NOT_RUN**이다. 재검증 패킷은 `docs/execution/BOOTSTRAP_MACOS_IVA_AFFECTED_REREVIEW_PACKET_v0.2.md`에 이미 있다. 최초 FAIL·Mac 병합/릴리스 HOLD를 유지한다. SNS 수락을 진행하려고 Mac 전체 재구현·검증자 자가호출을 하지 않는다. [S5]

### Web Starter

Next.js/TypeScript/Tailwind/Supabase의 DEMO, 설정오류 분리, 로그인·메모 CRUD·RLS·private storage 정책 후보다. 웹 실행은 로컬로 가능하지만 실제 데이터 기능은 Supabase 연결이 필요하며, 사진/SNS 앱 자체가 아니다. 이번 모바일의 필수 의존성이 아니다.

PR #1은 Draft·미병합이다. 운영의 B/W affected-only 결과는 네 finding PASS이나 PR 본문은 오래된 재검증 NOT_RUN 상태가 남아 있다. 실제 Supabase 통합·Template·릴리스와 병합 처분을 따로 보존한다. 외부 서버 없는 SNS 경로에 Supabase를 재도입하지 않는다. [S6–S7]

## 11. Claude → MITCHELL 반환 계약

상세 결과는 SNS 작업 브랜치의 `docs/execution/`에 기록하고, 채팅에는 아래 구조의 복사 가능한 패킷을 반환한다. MITCHELL CURRENT/DECISIONS/memory를 직접 덮어쓰지 말고 운영 반영용 정제 delta와 결과 위치를 준다.

```text
PACKET_ID = MITCHELL-CLAUDE-SNSG-RETURN-<date>-001
FROM = Claude / 후속 작성자
TO = OWNER / MITCHELL
PROJECT = MITCHELL
PRODUCT = sns-gateway
STATUS = 실제 완료 상태
BASE_COMMIT / BASE_TREE = ...
BRANCH / PR = ...
TARGET_COMMIT / TARGET_TREE = ...
CHANGED_SCOPE = ...
AUTHOR_CHECKS = 실행/재사용 구분, CI 및 범위
DEVICE_ACCEPTANCE = PASS / FAIL / INDETERMINATE / NOT_RUN
SNS_ACCEPTANCE = 실제 OS·앱별 결과
SGV_04 = 근거를 동반한 상태; 이전 INDETERMINATE 소급삭제 금지
IVA = 새 후보는 NOT_RUN, 기존4건 PASS는 역사적 범위
REPORT_PATH / REPORT_COMMIT / REPORT_BLOB / SHA256 = ...
ARTIFACTS = source/binary별 commit·checksum·설치 가능 여부
OPEN_FINDINGS / NOT_RUN = ...
MERGE_RECOMMENDATION = 권고와 실행 구분
RELEASE_DEPLOY = HOLD 또는 별도 승인·실제 증거
EFFECTS = 코드/Git/빌드/기기/계정/SNS/병합/배포 각각
MEMORY_DELTA = MITCHELL 반영 후보; 직접 lineage 변경 아님
NEXT_ACTION = 정확한 다음 한 가지 또는 행동 불필요
```

Git 저장 실패 또는 도구 미연결이면 결과를 채팅/파일로 반환하고 `GIT_RECORD=NOT_DONE`이라고 쓴다. 출처 없는 commit/blob/체크섬, 플랫폼 설정 자동등록, 이미 실행한 Claude/PMO/IVA, 백그라운드 후속 작업을 만들어내지 않는다.

## 12. 근거 목록과 인계의 한계

이 문서는 코드 재검증 보고서가 아니라 출처를 고정한 이관서다. 외부 웹 연구를 새로 수행하지 않았고, 새 SDK/API 정책을 정한 문서도 아니다. 구현 변경 시 관련 공식 자료 확인은 후속 작성자의 작업이다.

- **S1** MITCHELL anchor `ceecfc922193b0c2d2e4f99117fb3edfb0c047ee`: README, CURRENT, AGENTS, DECISIONS 및 WORKLOG 최근 E015–E018. CURRENT blob `5e9dbecb0025c38c9444af4a0fd135541657ba5b`.
- **S2** 같은 anchor: `docs/execution/SNS_GATEWAY_LIFECYCLE_COMPLETION_20260919.md`, blob `5213244b275cec44dbe2c9d27cd13f707613dbbe`.
- **S3** GitHub SNS PR #1·main readback: `1c7cd117d18929cdf40abc0e4f73fec4c87b2c65`, tree `caa83524a6118272952fd669d225d253dd477edb`.
- **S4** 같은 운영 anchor: 최초 `SNS_GATEWAY_IVA_RESULT_002_20260919.md` v1.0.1 blob `575fbf734d47484a14a6d3fd8a1a79db736be60f`; 후속 `SNS_GATEWAY_IVA002_AFFECTED_REREVIEW_RESULT_20260919.md` blob `b61e82a87a534193ab50c420152df145319e7558`. 최초 기록 commit `5133d5fe9af59b8ed06e7bf7bedba29108c10c79`, 후속 기록 commit `13d5befecca9c1dfb27c67b2572c729b601f62bc`.
- **S5** GitHub Bootstrap main·PR #1/2와 S1의 Mac current. 교정 head `30a06ea0807a2c9686f4dd15062e08fd56bdf891`.
- **S6** GitHub Web Starter main·PR #1 readback. head `a2c63cca6bdbf0667ba2d9e1c8d8e7af2e137a17`.
- **S7** S1의 DECISIONS D017–D028 및 README가 보존한 B/W 검증 계보. 독립 결과 원문 `docs/execution/IVA_AFFECTED_ONLY_REREVIEW_RESULT_20260915.md`는 후속 관련 작업 시 읽는다.
- **S8** exact SNS tree의 소스 artifact10582954297. package.json, .nvmrc, CI, WORK_PLAN/LIFECYCLE/IVA002_CORRECTIONS/BUILD_MATRIX 및 소스 목록을 읽고 원본75파일의 tree를 확인했다. 전체 코드에 대한 신규 품질감사는 수행하지 않았다.
- **S9** 이 대화에 첨부된 `SNS_GATEWAY_WORK_PLAN_DRAFT_v0.1_20260919.md` 전체를 로컬에서 읽었다. 이는 실행 전 원계획이며 현재 상태의 근거로 쓰지 않았다. 첨부 검색은 결과가 없었고 이미 마운트된 정확한 경로를 사용했다. 오래된 서버형 모바일 제안과 f5dfbe9 후보 첨부는 최신 실행 기준에서 제외했다.

검증 보고서의 당시 NOT_DONE과 이후 main 병합 기록은 충돌이 아니라 시점 차이다. 반면 현재 사용자 안내에 남아 있는 미병합 문구는 후속 C1에서 교정할 대상이다. 원 계획의 미구현 표를 조용히 삭제하는 대신 역사적 표임을 유지한다.

**이관 준비 완료는 Claude 실행 완료가 아니다. 현재 외부 작성자 실행은 NOT_DISPATCHED이며, Owner가 전달 패킷을 Claude에 보내는 것이 다음 행동이다.**
