# MITCHELL → IVA Bootstrap macOS affected-only 재검증 패킷

PACKET_ID = MITCHELL-BOOTSTRAP-MAC-001-REREVIEW
VERSION = 0.2
DATE = 2026-09-20
FROM = MITCHELL
TO = IVA
PROJECT = MITCHELL
PRODUCT = Bootstrap macOS v0.2
STATUS = AUTHOR_CORRECTION_COMPLETED / EXACT_CANDIDATE_FIXED / REREVIEW_REQUESTED / MERGE_HOLD

## 1. 복구와 원 판정

GitHub connector로 이 패킷의 실제 기록 commit을 기준으로 운영 README/CURRENT/AGENTS와 아래 완료보고·Manifest를 직접 읽는다. 전달 메시지의 exact commit/blob 영수증을 사용한다. 같은 최초 검증을 다시 시작하는 요청이 아니다.

원 결과 Repository: AofSpds/mitchell.
RESULT_COMMIT = b3a72ee5b4814f516b88308d6d65a4704a839a7c
RESULT_PATH = docs/execution/BOOTSTRAP_MACOS_IVA_RESULT_001_20260920.md
RESULT_BLOB = c0904f3302eb04ae4e4d7e9f6a66d49043b257ab
RESULT_DOCUMENT = MITCHELL-BOOTSTRAP-MAC-001-RESULT v1.0

원753bc82 후보 FAIL, F001/F002 MEDIUM P2·F003 LOW P3와 MERGE/RELEASE HOLD는 보존한다. 원 MAC-V01/V05 PASS와 V02/V03/V04 FAIL은 당시 소스·합성 범위다. 작성자 교정은 독립 PASS가 아니다.

## 2. 고정 대상

REPOSITORY = AofSpds/bootstrap
BRANCH = work/bootstrap-macos-v0.2
PR = #2 / OPEN / DRAFT / UNMERGED
EXACT_HEAD = 30a06ea0807a2c9686f4dd15062e08fd56bdf891
EXACT_TREE = 4692aadb0637c4c3765c237431357ea4b34d1184
ORIGINAL_REVIEWED_HEAD = 753bc82f63f85722cd76ba78b186bcaf46253677
ORIGINAL_TREE = 0b5ff0331af7db7f83d23c308370a148acd4ad74
BASE_MAIN = 7ede3b394032b3f1da60235cb0ef2dad410df565
DIFF = 5 files / +326 / -18
SOURCE_FILES = 35

시작/종료 시 PR·branch head를 대조한다. 달라졌으면 이 frozen 후보와 구분한다. main 또는 merge-test ref를 대신 사용하지 않는다. Windows PR#1 병합은 이미 완료됐으며 다시 수행하지 않는다.

## 3. 읽을 기록과 affected-only 범위

운영 기록:
- docs/execution/BOOTSTRAP_MACOS_IVA001_CORRECTION_COMPLETION_20260920.md
- docs/execution/BOOTSTRAP_MACOS_IVA001_CORRECTED_MANIFEST_20260920.json
- 원 IVA 결과 §5

제품 기록: docs/macos/IVA001_CORRECTIONS.md, 기존 WORK_PLAN_v0.2.md·scripts/macos/AGENTS.md의 관련 경계, 변경 소스와 tests/macos/test_iva001_corrections.py.

| Finding | 교정 경로 | 독립 재검증 조건 |
|---|---|---|
| MAC-F001 | bootstrap.sh bm_extensions | 최초 목록 오류, Codex 설치 후 재조회 실패+기존 Claude, 부분stdout+nonzero, 둘다미설치, 실패 installer 뒤 재조회, 정상 새목록 이후 진행, 사용자 재실행 후 관측된 기존효과 보존. 실패 조회에서 후속 확장 변경0 |
| MAC-F002 | platform.sh bm_path_state/bm_find_app_in/bm_find_app 및 bootstrap.sh probe5 처리 | 정상 존재/부재, 없는 사용자 Applications·다단계 부모, 접근 불가/읽기 불가 부모, 정상 첫 위치+오류 둘째 및 반대, symlink/plist/executable 고장, receipt-only, 경로특수문자, 접근복구. 불확실한 해당 앱의 brew 설치0, 권한 우회0 |
| MAC-F003 | platform.sh bm_lock_acquire | HOME 쓰기거부, 기존 안전/불안전 base, symlink, lock 존재, 정상 생성/해제. stdout/stderr에 raw HOME 없이 fixed exit2 유지. 기존 lock 자동삭제0 |

새 부모 조회는 find의 한 단계 열거이며 errno/지역화 stderr 추정이 아니다. 파일시스템 탐지와 실제 설치 사이 외부 프로세스 변경을 완전한 transaction으로 보장하지 않는다. 변경된 공통 App 탐지가 AI CLI와 Mobile App에 주는 실질적 영향도 해당 범위에서 확인한다. 변경 없는 Windows 및 정상 지원선·catalogue는 유효 증거를 재사용한다.

## 4. 작성자 증거

AUTHOR_MACOS_CI = 35457443486 / SUCCESS
ARM64_JOB = 105935134431 / SUCCESS
INTEL_JOB = 105935134371 / SUCCESS
AUTHOR_WINDOWS_CI = 35457443465 / SUCCESS
POWERSHELL51_JOB = 105935134169 / SUCCESS
POWERSHELL7_JOB = 105935134095 / SUCCESS

Linux 표적35(확장9/App20/lock6), 기존58, 정책8 PASS. 원 후보의 세 대표 실패 조건도 통제된 합성 시험에서 확인했다. Mac arm64 로그의 Bash3.2.57·표적35 PASS·기존57 PASS/1 SKIP·정책8 PASS·read-only Verify exit2를 확인했다. Intel/Windows는 job/step SUCCESS다. Linux 권한 fixture는 비특권 실제 파일시스템이며 OS/brew/code는 합성이다. 실제 Mac 설치·계정·권한수락이 아니다.

SOURCE_ARTIFACT_ID = 10588004720
SOURCE_OUTER_SHA256 = ccf81b4583df12fd203629a158a90b7a0ecbb7ba4e1c0ec403a5f1e077199e69
SOURCE_INNER_TAR_SHA256 = 2f74477887bc7327a33cb7c2f1f4d0d2434a6610f8548c56bb975ac16de34de8

35개 archive 파일의 CRC/경로/mode·로컬 검사본 대조와 exact tree 재계산을 수행했다. Windows 텍스트9개의 export CRLF만 계산용 정규화, bootstrap.bat은 raw 유지. 다운로드 소스의 표적35개도 PASS이며 별도 신규시험 수로 중복 합산하지 않는다. Windows18개 blob 동일.

원 후보의 CI35452044776/35452044793을 재실행하지 않았다. 새 소스 자동 CI는 작성자 회귀이며 IVA의 전체 재검증이 아니다. 작성자 실행과 IVA 신규 실행을 구분해 반환한다.

## 5. 미실행·권한·반환

IVA_AFFECTED_REREVIEW = NOT_RUN
REAL_MAC_INSTALL = NOT_RUN / INDETERMINATE
FINDER_GATEKEEPER_TCC_ACL_DEVICE_ACCEPTANCE = NOT_RUN
REAL_PACKAGE_INSTALL_ADMIN_ACCOUNT_LICENSE_BUILD = NOT_RUN
MACOS_PR_READY_OR_MERGE = NOT_DONE / HOLD
RELEASE_DEPLOY = NOT_DONE / HOLD
PMO_RUNTIME = NOT_DISPATCHED

허용: 읽기 중심 독립검증, 격리 합성 fixture, IVA 자신의 정제 결과 Git 기록.
금지: 제품 수정, 실제 brew/CLT/Xcode/SDK 설치·라이선스 수락, 사용자 Mac/계정/키 변경, PR ready/merge, 릴리스·배포, PMO·다른 Persona dispatch.

결과 상세는 Git에 기록하고 채팅은 MITCHELL 반환 패킷만 제공한다. exact head/tree, MAC-F001–F003 각각 PASS/FAIL/INDETERMINATE/NOT_RUN, 새 finding 유무, 재사용/신규 증거, 결과 path/commit/blob, MERGE_RECOMMENDATION과 RELEASE_DEPLOY_RECOMMENDATION, 실제 Mac 미실행·권한 경계·다음조치를 포함한다. 최초 FAIL은 소급 변경하지 않는다.

마일스톤: Windows 병합 → Mac 구현 → 최초 IVA FAIL → 세 finding 작성자 교정·새 후보 고정 [완료] → IVA affected-only [요청] → 별도 승인된 실제 Mac 수락·병합·릴리스.
OWNER_ACTION = 본 패킷의 exact Git 위치를 별도 IVA 채널에 전달.
