# SNS Gateway — IVA-002 F001–F004 교정 완료보고

DOCUMENT_ID: SNSG-IVA002-CORRECTION-COMPLETION-001  
VERSION: 1.0 / DATE: 2026-09-19 / WRITER: MITCHELL  
STATUS: AUTHOR_CORRECTION_COMPLETED / EXACT_CANDIDATE_FIXED / IVA_REREVIEW_NOT_RUN / MERGE_HOLD

## 1. 수신·권한·기존 판정

별도 IVA가 작성한 `MITCHELL-SNSG-IVA-002-RESULT` v1.0.1을 `AofSpds/mitchell@5133d5fe9af59b8ed06e7bf7bedba29108c10c79`의 `docs/execution/SNS_GATEWAY_IVA_RESULT_002_20260919.md`에서 직접 읽었다. Git blob은 `575fbf734d47484a14a6d3fd8a1a79db736be60f`다. 해당 문서는 수정하지 않았다.

원 후보 `cf6967ce920793a72f88da746af4d0a317e61ed8`의 판정은 FAIL, SGV-F001–F004는 모두 MEDIUM/P2, MERGE/RELEASE_DEPLOY는 HOLD다. SGV-01/03/08 PASS, SGV-02/05/06/07 FAIL, SGV-04 INDETERMINATE라는 원 범위를 보존한다. PASS는 소스·fixture 한정이며 실제 기기/SNS 수락 PASS가 아니다.

사용자의 기존 구현·교정 진행 승인(D021) 안에서 MITCHELL이 직접 수정했다. IVA 결과 자체를 새 실행권한으로 해석하지 않았다. 수신 CURRENT Generation9는 `edd19651259193be0320197bda26cc8d448153ce`에 먼저 반영했다. PMO dispatch, 다른 Persona, 계정·키·기기 설치·SNS 전송/게시·병합·배포는 하지 않았다.

## 2. 고정 후보

| 항목 | 값 |
|---|---|
| Repository | AofSpds/sns-gateway |
| Branch / PR | work/sns-gateway-v0.1 / #1 OPEN·DRAFT·UNMERGED |
| Head | 4ab10f481f7da66c00591ebd6cf597de527b901c |
| Tree | caa83524a6118272952fd669d225d253dd477edb |
| Original reviewed head | cf6967ce920793a72f88da746af4d0a317e61ed8 |
| Base main (unchanged) | 13d83595e31867c24fb8154074a7ad76d5995f95 |
| Changed files | 19 / +648 / -100 |
| Source files | 75 |
| Author CI | 35439466823 |

19파일은 네 교정의 구현·UI·추가 이관·시험·설명에 한정한다. 의존성 잠금 파일, 제품 알림 설계, SNS 공유 방식은 변경하지 않았다. 기존 Bootstrap/Web Starter는 변경하지 않았다.

## 3. Finding별 작성자 교정

| Finding | 원 문제 | 교정과 보존한 조건 |
|---|---|---|
| SGV-F001 | 실패 선두10개가 뒤의 정상 후보를 계속 막음 | native I/O 전에 영속 순환 순서를 기록한다. 아직 미시도/가장 오래된 시도부터 최대10개 선택. STARTED/SKIPPED/ERROR도 다음 정상 후보를 막지 않으며 실제 성공만 IMPORTED. 시도 횟수·마지막 결과와 원 pending·baseline·FIRST_OBSERVED/null 등록일을 보존한다. |
| SGV-F002 | 최근100건 밖의 미확인 이력이 UI에서 보이지 않아 초기화를 계속 막음 | UNRESOLVED/ALL 필터, started_at+id 내림차순 keyset cursor, 이전100건 UI를 추가했다. 미확인205개 및 같은 시각 이력도 끝까지 접근한다. 삭제·초기화 보호는 유지하고 사용자 결과 확인 후에만 해제한다. |
| SGV-F003 | Android UUID.tmp 중단 사본이 관리 목록·용량·삭제에서 누락 | 엄격한 앱 소유 UUID.tmp를 IMPORT_TEMP로 집계한다. 쓰기·rename·삭제는 동기화된 경계와 active registry로 보호한다. 비활성 중단 temp는1시간 후 정리 대상, 명시적 초기화에는 포함한다. 공유 staging7일 유예·미확인 사진 보호는 바꾸지 않았다. |
| SGV-F004 | I/O 오류가 빈 목록/삭제 성공으로 변환 | Kotlin Os.lstat/remove 및 Swift throwing Foundation/POSIX 경계에서 확정된 ENOENT만 부재로 인정한다. 열거·metadata·remove/unlink 오류를 전파하고 receipt 중복을 거부한다. DB-known 경로도 journal에 포함하며 최종 완전 inventory가 비어야 reset_pending과 DB를 정리한다. |

MIGRATION_4는 source_retry_attempts와 이력 조회 index를 추가한다. 과거 v1/v2/v3 데이터, 사진 등록일, source identity, 날짜 revision과 공유 당시 snapshot은 유지한다. 무인 게시·외부 서버/API/OAuth는 추가하지 않았다.

Android 기존 minSdk24를 유지하기 위해 API26 NIO 대신 API21+ android.system.Os를 사용했다. 파일 내용이 있는 폴더를 재귀 삭제하지 않고 소유 경로와 leaf 파일명 전체를 검증한다. 미등록 JPEG를 tmp로 속여 제거하지 않는다. 예상 밖 파일/권한 오류는 무시하는 대신 복구가 필요하다고 처리한다.

## 4. 작성자 검사·증거 한계

| 검사 | 결과·범위 |
|---|---|
| F001–F004 표적 TS/SQLite + lifecycle | 42/42 PASS; 실제 제품 TS·SQLite, native 경계만 fixture |
| 확장된 Node 회귀군 | 110/110 PASS (기존94+새16); 새로운 교정 소스 대상 |
| Kotlin/JVM 파일 경계 | 23 PASS; 비특권 Linux 실제 파일 I/O, Context/Android Os shim 사용. Android Os 실기 실행 아님 |
| Swift 파일 경계 | 새16 + 기존6 PASS; 실제 Foundation·unlink, Linux 비특권 환경 및 macOS CI |
| CI checks | SUCCESS |
| Android unsigned Release / 인터넷 권한 제거 | SUCCESS |
| iOS unsigned simulator Release | SUCCESS |
| 실제 기기/SNS / 새 독립 IVA | NOT_RUN / NOT_RUN |

CI job: checks105887666099, android105887759446, ios-simulator105887759379. 기존 reviewed head의 성공 CI35433487023은 재실행하지 않았다. 새 소스의 공통 저장·파일 변경에 대한 회귀·빌드 검사이며 전체 독립검증을 반복한 것이 아니다.

표적 시험은 실패선두10+정상11번째, throw/복구/부분성공, 과거100/101·미확인205개·동일시각 cursor·v1이관, 중단 temp/active/용량, 열거·metadata·remove/unlink 오류, journal·reset_pending 유지와 접근 복구를 다룬다. 시험용 사본만 사용했고 raw logs/개인사진/키를 Public Git에 저장하지 않았다.

UI 시험은 조회 함수 실행과 UI 연결 정적 확인이다. 실제 폰의 화면 터치·PhotoKit/SAF·권한철회·iCloud-only·metadata·백업·알림 cold/warm/잠금/재부팅·SNS 수신은 NOT_RUN/INDETERMINATE다. unsigned APK는 설치 서명을 마친 APK가 아니며 simulator .app은 iPhone IPA가 아니다.

### 중간 Android 빌드 실패와 교정

첫 교정 커밋 `9847200c2448e98ebe0f9812cee813285dbbe865`의 CI `35438877502`에서 checks/iOS는 성공했으나 Android는 `LocalManagedFiles.kt:91 Unresolved reference unlink`로 실패했다. JVM Os shim이 실제 공개 SDK에 없는 이름까지 제공해 이 오류를 사전에 잡지 못했다. 공개 API21+ `Os.remove`로 교정하고 전후 lstat·일반파일 검사·ENOENT 구분을 유지했다. 동일 이름으로 fixture를 수정해 Kotlin23건을 다시 통과했고 후속 커밋 `4ab10f48…`에서 새 CI를 확인했다. 기존 실패 커밋/실행은 삭제하거나 성공으로 바꾸지 않았다. Swift POSIX unlink 및 앱 최소 SDK는 변경하지 않았다.

## 5. 소스 동일성

Source artifact10582954297: `SNS_GATEWAY_IVA002_FINAL_SOURCE_20260919.zip`.
외부 SHA-256: `1dae80fa852c79fa0cf502accd82477467109688a204ffdcb9d9905d9a59dce2`.
내부 source ZIP SHA-256: `556c4a9f180747a0d457191f9cd5f6ae113c06005aa8cc1a0c6efd6dda408606`.

ZIP CRC/경로 안전성을 확인했다. 추출75개 파일은 로컬 검사 파일과 바이트 단위 일치했고, 재계산 Git tree가 위 exact tree와 일치했다. 다운로드 소스의 표적42개 시험도 PASS다. 로컬 Git의 임시 작업용 commit을 remote parent로 사용하지 않았으며 원 cf6967ce… 이후 두 개의 fast-forward 교정 커밋으로 저장했다.

## 6. 다음 단계

최초 FAIL은 보존한다. F001–F004의 작성자 교정 및 표적 검사가 완료됐으나 독립 재검증 판정은 NOT_RUN이다. `docs/execution/SNS_GATEWAY_IVA_AFFECTED_REREVIEW_PACKET_v0.3.md`로 별도 IVA affected-only 재검증을 요청한다. 변경이 영향을 준 경로만 확장하고 기존 유효 증거를 무정보 반복하지 않는다.

MERGE_RECOMMENDATION=HOLD, RELEASE_DEPLOY_RECOMMENDATION=HOLD를 유지한다. 실제 기기·SNS·계정·개인 서명·제품main·릴리스·배포는 변경하지 않았다. 다음 사용자 행동은 새 재검증 패킷 전달 한 가지다. API 키와 새 저장소는 필요 없다.
