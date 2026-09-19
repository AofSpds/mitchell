# MITCHELL → IVA SNS Gateway affected-only 재검증 전달 패킷

PACKET_ID = MITCHELL-SNSG-IVA-002-REREVIEW-HANDOFF
VERSION = 1.0
DATE = 2026-09-19
FROM = MITCHELL
TO = IVA
PROJECT = MITCHELL
PRODUCT = SNS Gateway
STATUS = GIT_RECORDED / AUTHOR_CORRECTION_COMPLETED / REREVIEW_REQUESTED / MERGE_HOLD

## 1. 원 검증 결과

RESULT_REPOSITORY = AofSpds/mitchell
RESULT_COMMIT = 5133d5fe9af59b8ed06e7bf7bedba29108c10c79
RESULT_PATH = docs/execution/SNS_GATEWAY_IVA_RESULT_002_20260919.md
RESULT_BLOB = 575fbf734d47484a14a6d3fd8a1a79db736be60f
RESULT_DOCUMENT = MITCHELL-SNSG-IVA-002-RESULT v1.0.1

ORIGINAL_CANDIDATE = FAIL
SGV-F001 = FAIL / MEDIUM P2
SGV-F002 = FAIL / MEDIUM P2
SGV-F003 = FAIL / MEDIUM P2
SGV-F004 = FAIL / MEDIUM P2
MERGE_RECOMMENDATION = HOLD
RELEASE_DEPLOY_RECOMMENDATION = HOLD

최초 판정은 삭제하거나 PASS로 덮어쓰지 않는다.

## 2. 재검증 대상

REPOSITORY = AofSpds/sns-gateway
BRANCH = work/sns-gateway-v0.1
PR = #1
PR_STATE = OPEN / DRAFT / UNMERGED
EXACT_HEAD = 4ab10f481f7da66c00591ebd6cf597de527b901c
EXACT_TREE = caa83524a6118272952fd669d225d253dd477edb
ORIGINAL_REVIEWED_HEAD = cf6967ce920793a72f88da746af4d0a317e61ed8
BASE_MAIN = 13d83595e31867c24fb8154074a7ad76d5995f95

## 3. 교정 요약

SGV-F001
- 영속 순환 재시도 순서와 시도 이력을 추가했다.
- 실패 선두 10개가 뒤의 정상 후보를 계속 막지 않는다.
- PENDING, baseline, FIRST_OBSERVED/null 등록일과 실패 이력은 유지한다.

SGV-F002
- UNRESOLVED/ALL 필터와 started_at+id cursor 기반 이전 이력 조회를 추가했다.
- 최근 100건 밖의 미확인 이력과 동일 시각 다량 이력에 접근 가능하다.
- 미확인 이력의 삭제·초기화 보호는 유지한다.

SGV-F003
- Android 앱 소유 UUID.tmp를 IMPORT_TEMP로 inventory·용량·초기화에 포함했다.
- active writer와 최근 임시 파일은 보호한다.
- 비활성 중단 임시 파일은 1시간 유예 후 정리 대상으로 처리한다.
- 휴대폰 원본과 미확인 공유 보호는 유지한다.

SGV-F004
- Kotlin/Swift에서 확정 ENOENT와 I/O·권한·열거·metadata·삭제 오류를 구분한다.
- 불완전 inventory를 성공한 빈 목록으로 처리하지 않는다.
- DB-known 경로를 journal에 남기고 최종 완전 inventory가 비어야 reset 완료한다.
- 실패 시 journal/reset_pending을 유지하고 접근 복구 후 재개한다.

## 4. 작성자 증거

AUTHOR_CI = 35439466823
CHECKS_JOB = 105887666099 / SUCCESS
ANDROID_JOB = 105887759446 / SUCCESS
IOS_SIMULATOR_JOB = 105887759379 / SUCCESS

TARGETED_TS_SQLITE_TESTS = 42/42 PASS
EXTENDED_NODE_TESTS = 110/110 PASS
KOTLIN_JVM_FILE_TESTS = 23 PASS
SWIFT_FILE_TESTS = NEW 16 + EXISTING 6 PASS

SOURCE_ARTIFACT_ID = 10582954297
SOURCE_ARTIFACT_OUTER_SHA256 = 1dae80fa852c79fa0cf502accd82477467109688a204ffdcb9d9905d9a59dce2
SOURCE_INNER_ZIP_SHA256 = 556c4a9f180747a0d457191f9cd5f6ae113c06005aa8cc1a0c6efd6dda408606
SOURCE_FILES = 75
SOURCE_TREE_RECOMPUTED = caa83524a6118272952fd669d225d253dd477edb

중간 commit 9847200c2448e98ebe0f9812cee813285dbbe865 / CI 35438877502의 Android 빌드는 Os.unlink 공개 심볼 오류로 실패했다. 공개 API Os.remove로 수정한 후속 exact head가 위 재검증 대상이다. 실패 이력은 보존한다.

## 5. Git 기록

OPERATION_REPOSITORY = AofSpds/mitchell
CORRECTION_COMPLETION_PATH = docs/execution/SNS_GATEWAY_IVA002_CORRECTION_COMPLETION_20260919.md
CORRECTED_MANIFEST_PATH = docs/execution/SNS_GATEWAY_IVA002_CORRECTED_MANIFEST_20260919.json
REREVIEW_PACKET_PATH = docs/execution/SNS_GATEWAY_IVA_AFFECTED_REREVIEW_PACKET_v0.3.md

본 전달 패킷은 별도 Git commit/readback으로 고정한다.

## 6. 재검증 요청

전체 프로젝트를 다시 검증하지 않는다.
SGV-F001–SGV-F004와 수정으로 실제 영향을 받은 공통 경로만 affected-only로 재검증한다.

각 finding은 다음 중 하나로 반환한다.

PASS
FAIL
INDETERMINATE
NOT_RUN

반환에는 반드시 다음을 포함한다.
- 검증한 exact head/tree
- F001–F004 각각의 판정과 근거
- 재사용한 작성자 증거와 IVA가 새로 실행한 증거의 구분
- 새 finding 유무
- 결과 문서 Git path/commit/blob
- MERGE_RECOMMENDATION
- RELEASE_DEPLOY_RECOMMENDATION
- 실제 기기/SNS 등 NOT_RUN 범위
- MITCHELL의 정확한 다음 조치

## 7. 미실행·권한 경계

DEVICE_SNS_ACCEPTANCE = NOT_RUN / INDETERMINATE
PHOTO_KIT_SAF_DEVICE_TEST = NOT_RUN
REMINDER_COLD_WARM_REBOOT_TEST = NOT_RUN
SIGNED_DEVICE_INSTALL = NOT_DONE
PRODUCT_MERGE = NOT_DONE / HOLD
RELEASE_DEPLOY = NOT_DONE / HOLD
PMO_RUNTIME = NOT_DISPATCHED

이 패킷은 read-only affected-only 독립검증 요청이다.
제품 코드 수정, PR ready/merge, 제품 main 변경, 릴리스·배포, 사용자 기기 설치, 계정·키 변경, 실제 사진·SNS 전송/게시, PMO 또는 새 Persona dispatch를 승인하지 않는다.

API 키와 새 저장소는 필요 없다.
