# MITCHELL → IVA Bootstrap macOS v0.2 검증 전달 패킷

PACKET_ID = MITCHELL-BOOTSTRAP-MAC-001-REVIEW
VERSION = 0.1
DATE = 2026-09-20
FROM = MITCHELL
TO = IVA
PROJECT = MITCHELL
PRODUCT = Bootstrap macOS v0.2
STATUS = AUTHOR_COMPLETED / EXACT_CANDIDATE_FIXED / IVA_NOT_RUN / MACOS_MERGE_HOLD

## 1. 복구와 정확한 대상

현재 채널은 MITCHELL 작성자 채널이다. 별도 IVA가 신설 macOS 코드 후보를 독립검증한다. 기존 Windows B001/B002 또는 SNS Gateway의 독립 PASS를 macOS에 적용하지 않는다.

```text
REPOSITORY = AofSpds/bootstrap
BRANCH = work/bootstrap-macos-v0.2
PR = #2 / OPEN / DRAFT / UNMERGED
EXACT_HEAD = 753bc82f63f85722cd76ba78b186bcaf46253677
EXACT_TREE = 0b5ff0331af7db7f83d23c308370a148acd4ad74
PREVIOUS_MAC_HEAD = 78a919e9300bbb8de8fbc8843d7a8dc6b59044a1
BASE_MAIN = 7ede3b394032b3f1da60235cb0ef2dad410df565
WINDOWS_BASE_TREE = 2a28a91f8fdbbbc80bd37209206cdab9cf5e715a
WINDOWS_PR_1 = MERGED / CLOSED
```

검증 전에 remote PR/branch head를 읽는다. 다르면 frozen 대상과 구분하며 main 또는 PR merge-test ref로 몰래 바꾸지 않는다. Windows PR#1은 이전 사용자 승인으로 이미 병합됐으므로 이 요청으로 재병합하지 않는다.

## 2. 읽을 Git 문서

운영 저장소 AofSpds/mitchell에서 이 패킷이 포함된 exact commit을 기준으로 읽는다. 전달 메시지의 실제 Git 영수증을 사용하고 미래 commit을 예상하지 않는다.

- README.md, CURRENT.md, AGENTS.md, DECISIONS D027/D028.
- docs/execution/BOOTSTRAP_MACOS_COMPLETION_20260920.md.
- docs/execution/BOOTSTRAP_MACOS_MANIFEST_20260920.json.

제품 exact 후보의 docs/macos/WORK_PLAN_v0.2.md, FIRST_RUN.md, ACCEPTANCE.md, SOURCES.md 및 scripts/macos/AGENTS.md를 읽고 구현 주장과 코드를 대조한다. 최초 B/W 자료는 Windows 보호 경계에 필요한 범위만 재사용한다.

## 3. 검증 범위

| 영역 | 독립 확인할 계약 |
|---|---|
| MAC-V01 진입·지원 | .command/Bash3.2, 잘못된 인수·root·Rosetta·OS/CPU·Homebrew prefix 오류 거부. macOS14는 진단만, Install은 brew/AI/lock 전에 차단. Intel opt-in이 OS 기준을 우회하지 않음 |
| MAC-V02 설치·기존 보존 | Core/Mobile/Optional 고정 공식 IDs, 기존 PATH/App/Java/Node 재사용, 비호환·receipt-only를 누락으로 오인하지 않음. 호출 성공+receipt+사후 탐지 필요 |
| MAC-V03 동의·오류·재개 | Plan/Verify 설치 없음, 비대화 무동의 거부, lock·실패 코드·재실행·부분실패. inventory 실패를 빈 목록으로 바꾸지 않음. vendor raw log 노출과 shell injection 경계 |
| MAC-V04 AI·Mobile | 공식 VS Code extension만 선택 설치, 기존 확장 유지. CLT/Homebrew/Xcode/SDK·license·계정·서명 수동 경계. SDK 파일 발견을 build 성공이라 하지 않음 |
| MAC-V05 증거·기존 보호 | Windows18개 blob 동일, Mac33개 source/tree/attributes EOL 구분, 작성자 fixture/native read-only/device/IVA/release 완료 구분 |

새 macOS 최초 독립검증이며, 기존 프로젝트 전체를 처음부터 재검증하는 요청은 아니다. 유효 작성자 증거는 재사용하고 신규 위험·검증 공백을 중심으로 독립 fixture를 구성한다. Windows 파일이 동일하면 관련 기존 검증을 반복할 필요가 없다.

## 4. 작성자 증거

- Linux 실제 Bash 엔진+경계 fixture58개 및 지원 정책8개 PASS. OS/brew/code를 대체한 격리 시험이며 실제 설치가 아니다.
- macOS CI35452044776: arm64 job105920780633, Intel job105920780732 SUCCESS. stock Bash·fixture·Windows blob guard·native read-only Mobile Verify. Linux 전용1개 skip 분기를 실행 PASS로 합산하지 않는다.
- Windows CI35452044793: PowerShell5.1 job105920780622, PowerShell7 job105920780843 SUCCESS.
- Source artifact10587226850, source33파일.
- Outer SHA256=2b9c6815ef19d0ca1a6369c58575799ded32150d9e20e26ea2381ed030db95ed.
- Inner tar SHA256=d5b8379b25ad057afc5264080bde599f78dc5ada982f068e2bb7b91e153235d6.
- archive33파일은 로컬 검사본과 byte equality. .gitattributes CRLF9개만 기존 blob과 대조하여 정규화하고 .bat은 raw 유지한 Git tree가 exact target과 일치.

원 macOS78a919e/CI35449122339는 유효 과거 증거로 보존한다. 후속753bc82는 Homebrew 공식 Git 원문2026-09-17 정책에 맞춰 설치 최소선을15로 바꾼 후보다. Sources에 이전 web cache와 원문 차이를 기록했다.

## 5. 미실행·권한·반환

MACOS_IVA = NOT_RUN
CLEAN_EXISTING_MAC_INSTALL = NOT_RUN / INDETERMINATE
WINDOWS_REAL_INSTALL = NOT_RUN / INDETERMINATE
MACOS_PR_MERGE = NOT_DONE / HOLD
RELEASE_DEPLOY = NOT_DONE / HOLD
PMO_RUNTIME = NOT_DISPATCHED
OTHER_PERSONAS = NOT_INSTALLED

허용: Git/코드 읽기, 격리 synthetic fixture, IVA 자신의 정제 결과 Git 기록.
금지: 제품 코드 수정, 실제 brew/CLT/Xcode/SDK 설치·license 수락, 사용자 Mac/PC 변경, 실제 계정·키·결제·로그인, macOS PR ready/merge·릴리스·배포·PMO/다른 Persona dispatch.

실제 Finder/Gatekeeper·클린/기존 Mac·관리자 승인·의존성 변경·실계정·Mobile build는 미실행으로 남긴다. 정상 동작이 가능한 특정 호스트 구성의 CI를 전체 실기 PASS로 확대하지 않는다.

상세 결과는 Git에 기록하고 채팅 최종 반환은 MITCHELL에 복사 가능한 인계 패킷으로 한다. exact head/tree, MAC-V01–V05 판정, finding별 severity/재현/영향/최소교정, 신규·재사용 증거 구분, 결과 path/commit/blob, MERGE_RECOMMENDATION과 RELEASE_DEPLOY_RECOMMENDATION, 미실행·권한·다음조치를 포함한다.

마일스톤: Windows 병합 → Mac 구현 → 작성자 검사·고정 [완료] → 별도 IVA [다음] → 승인된 실제 Mac 수락·병합·릴리스.
