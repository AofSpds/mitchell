# Bootstrap macOS v0.2 — IVA 독립검증 결과

DOCUMENT_ID = MITCHELL-BOOTSTRAP-MAC-001-RESULT
VERSION = 1.0
DATE = 2026-09-20
FROM = IVA
TO = MITCHELL
PROJECT = MITCHELL
PRODUCT = Bootstrap macOS v0.2
INPUT = MITCHELL-BOOTSTRAP-MAC-001-REVIEW v0.1
STATUS = REVIEW_COMPLETED / CORRECTION_REQUIRED / MERGE_HOLD

검증자는 이 별도 IVA 대화이며 제품 작성자는 MITCHELL이다. 실제 실행 환경은 GitHub connector 조회와 격리 Linux의 Bash 5.2.37·Python·합성 파일시스템이다. macOS 실행 증거는 작성자 CI에서 재사용했다. 제품 코드를 수정하거나 실제 패키지를 설치하지 않았다. 별도 PMO·Codex WORK·페어 검증자 프로세스는 실행하지 않았다.

## 1. 결론

```text
MACOS_CANDIDATE = FAIL
MACOS_IVA = COMPLETED
FINDINGS = MAC-F001 / MAC-F002 / MAC-F003
SEVERITY = MEDIUM_P2:2 / LOW_P3:1
MERGE_RECOMMENDATION = HOLD
RELEASE_DEPLOY_RECOMMENDATION = HOLD
REAL_MAC_INSTALL = NOT_RUN / INDETERMINATE
```

기존 설치 보존과 조회 오류 후 변경 차단 계약에 해당하는 P2 두 건 때문에 현 후보의 병합을 권고하지 않는다. P3 한 건은 lock 생성 실패의 로컬 출력 정제 누락이다. 실제 확장 업데이트·기존 앱 덮어쓰기·인증정보 외부 유출을 관측했다는 판정은 아니다. 세 건의 정확한 재현과 최소 교정 범위는 §5에 있다.

Windows B001/B002의 기존 PASS를 새 macOS 코드에 적용하지 않았다. Windows 파일 보존과 관련 유효 증거만 재사용했다. 본 결과는 macOS 최초 독립검증이며 제품 전체 재구현이나 다른 제품의 전역 재검증을 요구하지 않는다.

### 영역별 판정

PASS는 아래에서 확인한 소스·합성 시험 및 재사용 증거 범위이며 실제 Mac 설치 수락 PASS가 아니다.

| 영역 | 판정 | 근거와 한계 |
|---|---|---|
| MAC-V01 진입·지원 | PASS | 인수·root·Rosetta·CPU·OS·prefix 거부와 macOS14 Install 사전 차단을 확인했다. Bash3.2는 작성자 macOS CI 증거다. Finder/Gatekeeper 실기는 NOT_RUN. |
| MAC-V02 설치·기존 보존 | FAIL | 정상 기존 도구·비호환 Node·receipt-only 보존과 성공+receipt+사후 탐지는 통과했다. 앱 경로 접근 오류를 미설치로 처리하는 F002가 남는다. |
| MAC-V03 동의·오류·재개 | FAIL | Plan/Verify 무설치, 명시적 동의, lock 직렬화, 숫자 실패 코드, package inventory 실패 차단은 통과했다. 확장 조회 실패 F001, 앱 접근 오류 F002, raw 경로 출력 F003은 교정 필요. |
| MAC-V04 AI·Mobile | FAIL | 고정 확장 ID·수동 로그인·CLT/Homebrew/Xcode/SDK/license 경계와 build 미수락 표시는 유지됐다. 조회 실패 후 기존 확장에도 설치 명령을 보내는 F001이 남는다. |
| MAC-V05 증거·기존 보호 | PASS | source33개, EOL9개 정규화 계산 후 exact tree, Windows18개 보존을 확인했다. 작성자 fixture·native read-only Verify·실제 설치·독립검증·릴리스의 구분은 적절하다. |

## 2. 고정 대상과 원천

| 필드 | 값 |
|---|---|
| Repository | AofSpds/bootstrap |
| Branch / PR | work/bootstrap-macos-v0.2 / #2 |
| Exact head | 753bc82f63f85722cd76ba78b186bcaf46253677 |
| Exact tree | 0b5ff0331af7db7f83d23c308370a148acd4ad74 |
| Parent / 이전 Mac 후보 | 78a919e9300bbb8de8fbc8843d7a8dc6b59044a1 |
| Base main | 7ede3b394032b3f1da60235cb0ef2dad410df565 |
| Windows base tree | 2a28a91f8fdbbbc80bd37209206cdab9cf5e715a |
| 시작 branch/PR 및 종료 전 PR | 위 head 일치 / OPEN / DRAFT / UNMERGED |
| 관측한 PR merge-test ref | add32bbea12d501879fdf3b383c2acb13b95ce88; 검증 대상으로 사용하지 않음 |
| 운영 입력 anchor / 기록 전 main | AofSpds/mitchell@f6664bd18325cf4cb18492744e46974fc140c538 |
| 입력 path | docs/execution/BOOTSTRAP_MACOS_IVA_PACKET_v0.1.md |
| 입력 blob | 2fc37f3b389a3ab33bec80d7780f51960830d76d |

운영 anchor의 README, CURRENT, AGENTS, DECISIONS D027/D028, BOOTSTRAP_MACOS_COMPLETION_20260920.md와 BOOTSTRAP_MACOS_MANIFEST_20260920.json을 GitHub connector로 직접 읽었다. CURRENT는 Generation13 / Writer MITCHELL이며 이 검증자가 해당 lineage를 변경하지 않는다.

제품 후보의 docs/macos/WORK_PLAN_v0.2.md, FIRST_RUN.md, ACCEPTANCE.md, SOURCES.md, scripts/macos/AGENTS.md, bootstrap.command, catalogue·engine·platform·CI·관련 시험을 대조했다. 작업계획과 패킷의 MAC-V01–V05가 판단 기준이며 외부 자료로 제품 요구사항을 새로 만들지 않았다.

Base main 대비 변경은 15파일, +976/-1, 2 commits ahead다. README 플랫폼 안내 외 기존 Windows 파일의 변경은 없고 Mac 파일 14개가 추가됐다. Windows PR#1의 과거 병합을 다시 수행하거나 그 PASS를 Mac으로 확장하지 않는다.

## 3. Artifact 및 소스 동일성

| 항목 | 독립 계산·대조 결과 |
|---|---|
| Source artifact | 10587226850 / bootstrap-macos-source |
| Outer ZIP bytes | 37567 |
| Outer SHA-256 | 2b9c6815ef19d0ca1a6369c58575799ded32150d9e20e26ea2381ed030db95ed |
| Inner tar | bootstrap-macos-candidate.tar.gz |
| Inner SHA-256 | d5b8379b25ad057afc5264080bde599f78dc5ada982f068e2bb7b91e153235d6 |
| COMMIT.txt / TREE.txt | exact head/tree와 일치 |
| 파일 수 | 33 |
| ZIP CRC / archive 경로·종류 | 정상; 절대경로·상위탈출·link entry 없음 |
| 원바이트로 계산한 tree | c619872cbc583b8095fdfb33969306ee7b370303 |
| EOL 대조 후 Git tree | 0b5ff0331af7db7f83d23c308370a148acd4ad74 / 일치 |
| Windows 보호 파일 | 18개 기존 blob 일치 |
| 시험 후 원본 파일·mode | 시험 전과 동일 |

Git archive가 attributes에 따라 CRLF로 내보낸 Windows 텍스트9개만 기존 expected blob과 대조하여 LF로 계산했다. 대상은 bootstrap.ps1, config/packages.psd1, scripts의 common/install-ai/install-packages/preflight/verify.ps1, tests의 logs-and-catalogue/run.ps1이다. bootstrap.bat의 원래 CRLF와 나머지 파일은 원바이트를 유지했다. 파일 mode를 포함해 tree를 재구성했으며 정규화는 해시 계산에만 적용했다. 따라서 ZIP의 모든 바이트가 Git blob과 같다고 주장하지 않는다.

시험 후에도 33개 파일의 SHA-256·Git blob·mode inventory가 최초와 완전히 같았다. 제품 원본, 기존 시험, Windows 보호 파일을 변경하지 않았다.

## 4. 재사용 증거와 IVA 신규 실행

### 4.1 작성자 증거 재사용

| Run / Job | 직접 확인한 증거 | 재사용 범위 |
|---|---|---|
| macOS35452044776 / arm64 105920780633 | job/step SUCCESS 및 전체 로그 | exact753bc82 checkout, Bash3.2.57, Windows18 blob guard, fixture58 중57 PASS·1 SKIP와 정책8 PASS, native read-only Mobile Verify exit2 |
| macOS35452044776 / Intel 105920780732 | job/step metadata SUCCESS | stock Bash fixture 및 native read-only Verify step 성공. 이번에 Intel 전체 로그는 다시 수집하지 않았고 개별 실행 수를 새로 확정하지 않음 |
| Windows35452044793 / PS5.1 105920780622 | job/step metadata SUCCESS | 변경 없는 Windows parser/mock 회귀 |
| Windows35452044793 / PS7 105920780843 | job/step metadata SUCCESS | 변경 없는 Windows parser/mock 회귀 |

arm64 로그에서 실제 host는 macOS15.7.9이며 stock Bash3.2.57이었다. Linux 전용 음성 시험 1개는 SKIP다. 이를 macOS 실행 PASS로 합산하지 않았다. native Verify는 일부 도구가 미설치여서 exit2를 반환했고 Xcode/AndroidSDK는 도구·파일 발견과 build 수락을 다른 라벨로 표시했다.

작성자가 보고한 Linux58+8=66 PASS는 작성자 증거다. 위 CI와 기존 전체 시험군을 IVA가 새로 실행했다고 표현하지 않는다. 이번에 CI 재실행, 실제 Mac 패키지 설치, native 앱 빌드·기기 수락은 수행하지 않았다.

### 4.2 IVA 신규 시험

exact Bash engine을 source하여 실제 Bash로 실행했다. OS/CPU/CLT/Homebrew/VS Code 경계는 합성 함수와 파일 기반 상태로 대체했다. lock·동시 실행과 접근 거부는 격리된 실제 Linux 파일시스템에서 시험했다. 실제 brew/code 설치 명령을 실행하지 않았다.

| ID | 독립 시나리오 그룹 | 판정 |
|---|---|---|
| IVM-01 | OS/root/Rosetta/CPU/prefix/선행조건 및 macOS14 차단 10조건 | PASS |
| IVM-02 | 사용 가능한 Bash5.2의 4개 shell 파일 syntax | PASS |
| IVM-03 | production wrapper가 fixture 환경변수로 Linux를 Mac으로 우회하지 않음 | PASS |
| IVM-04 | 잘못된 인수·주입 형태 6조건에서 platform 호출 전 차단 | PASS |
| IVM-05 | 전체 선택 조합의 Plan/Verify에서 설치·lock 없음 | PASS |
| IVM-06 | 비대화 무동의 Install 거부 | PASS |
| IVM-07 | 호환·구형·고장·PATH/도구 누락 등 기존 Node/CLI 9조건 보존 | PASS |
| IVM-08 | receipt-only cask를 미설치로 덮어쓰지 않음 | PASS |
| IVM-09 | 고정14 package·2 extension 요청, receipt/사후탐지, 반복 실행 추가 요청0 | PASS |
| IVM-10 | 실제 brew wrapper의 안전 환경 설정·상속 override 해제·exit17 보존 | PASS |
| IVM-11 | 최초 inventory 실패·비정상 목록 차단, raw vendor 출력 미노출 | PASS |
| IVM-12 | 성공 installer 후 inventory 실패가 후속 package 설치를 차단 | PASS |
| IVM-13 | 실패 exit42 보존, 독립 package 계속 처리, lock 해제 | PASS |
| IVM-14 | 설치 exit0·receipt 존재만으로 사후 탐지 실패를 성공 처리하지 않음 | PASS |
| IVM-15 | 최초 extension 목록 조회 오류 시 확장 설치0 | PASS |
| IVM-16 | 첫 확장 설치 후 목록 오류 시 기존 두 번째 확장 보존 | FAIL / F001 |
| IVM-17 | 기존 lock 거부 및 자동 stale 제거 없음 | PASS |
| IVM-18 | 동시 Bash 프로세스2개에서 두 번째 차단, 중복 설치 요청 없음 | PASS |
| IVM-19 | symlink lock root 거부, 대상 불변 | PASS |
| IVM-20 | 접근 가능한 실제 합성 App 구조 탐지 | PASS |
| IVM-21 | 접근 불가 App 부모를 미설치로 오인하지 않음 | FAIL / F002 |
| IVM-22 | lock 생성 실패의 stderr에서 HOME 경로를 노출하지 않음 | FAIL / F003 |
| 합계 | 고유 시나리오 그룹22개 | 19 PASS / 3 FAIL |

22는 내부 assertion 수나 Mac 기기 시험 수가 아니다. IVM-20/21은 exact bm_find_app 함수와 실제 파일 검사식을 사용했고 Apple plutil만 합성했다. 접근 거부 사례는 plutil 호출 이전에 발생한다. 통합 Install 경로에서는 탐지 함수만 이 exact 함수로 바꿔 넣고 brew는 계속 가짜였다.

비특권 시험 wrapper가 HOME을 초기화하는 것을 확인해 격리 HOME을 보존하도록 하네스를 교정했다. 관련 두 사례만 재실행했으며 초회 관측을 최종 판정에 중복 사용하지 않았다. 하네스 교정은 제품 수정이 아니다. 최종 결과에는 HARNESS_ERROR가 없다.

검증 작업공간의 정제 증거 식별: final_review_results.json SHA-256=0e77590683ba9bda16e52582c31ca9d10b3a0e7104a5981cdf51700f563ab445, source identity.json SHA-256=414e3127e41ceda109c25758d8d797eb2d708adfdde82f7193d94f1530b17dc3. 이 해시는 작업공간 증거의 식별자이며 해당 JSON이나 raw 로그를 Git에 업로드했다는 뜻은 아니다. 재현 조건과 정제 관측은 아래에 보존한다.

## 5. 교정 Finding

파일·줄 번호는 모두 exact753bc82 후보 기준이다. 세 건 모두 현 소스의 실제 경로를 검토·실행했으며 실제 Mac 설치의 결과로 확대하지 않는다.

### MAC-F001 — 확장 목록 재조회 실패 후 기존 확장에도 설치 요청

Severity = MEDIUM / P2. Verdict = FAIL. 영역 = MAC-V03 / MAC-V04.

위치: scripts/macos/bootstrap.sh:167–193, 특히186의 실패 후 continue와 다음 반복175–180. 요구: 패킷 MAC-V03의 inventory 오류 차단, MAC-V04의 기존 확장 유지, scripts/macos/AGENTS.md:5의 CLI 실패 구분.

원인: 최초에는 기존 확장 목록을 성공적으로 읽었지만, 첫 확장 설치 후 재조회가 실패하면 extensions가 빈 문자열 또는 실패 출력으로 바뀐다. 실패 분기의 continue는 전체 확장 처리를 중단하지 않고 다음 ID의 설치 판단으로 진행한다.

재현: Core 도구는 모두 존재하고 최초 확장 목록에는 anthropic.claude-code만 존재하도록 둔다. Install --accept --ai Both에서 openai.chatgpt 합성 설치는 exit0, 이후 목록 조회는 stdout 없이 exit74를 반환한다. 실제 engine에서 다음 순서가 관측됐다.

```text
code --list-extensions                         => anthropic.claude-code
code --install-extension openai.chatgpt        => 0
code --list-extensions                         => 74 / 빈 출력
code --install-extension anthropic.claude-code => 호출됨: 기존 설치인데 보존되지 않음
code --list-extensions                         => 74 / 빈 출력
최종 exit=1, 두 항목 EXTENSION_QUERY_FAILED
```

영향: 전체 실패를 보고하기는 하지만 그 전에 이미 존재한다고 확인한 확장에 불필요한 설치 요청을 보낸다. VS Code 공식 CLI는 --install-extension을 설치 또는 업데이트 명령으로 설명하므로 --force가 없다는 사실만으로 이 요청이 읽기 전용이라고 볼 수 없다. 이번에는 가짜 code를 사용했으므로 실제 업데이트·재설치·네트워크 다운로드는 관측하지 않았다.

최소 교정: 목록 조회 실패 시 확장 inventory를 무효로 취급하고 후속 확장 변경을 중단한다. 다시 변경하려면 성공한 새 목록을 확보해야 한다. 빈/부분 실패 출력을 미설치 증거로 사용하지 않는다. 고정 ID와 기존 확장 보존, 실제 로그인 수동 경계를 유지한다.

Affected-only 재검증: 최초 목록 실패, 첫 설치 후 실패+기존 다른 확장, 부분 stdout+nonzero, 두 확장 모두 미설치, 정상 재조회 후 처리, 실패 뒤 사용자 재실행. 불확실한 외부효과를 무조건 다시 실행하지 않아야 한다.

### MAC-F002 — 앱 부모 경로 접근 오류를 확정된 미설치로 처리

Severity = MEDIUM / P2. Verdict = FAIL. 영역 = MAC-V02 / MAC-V03.

위치: scripts/macos/platform.sh:50–64, 특히54 및63; scripts/macos/bootstrap.sh:114–117,132–154. 요구: 기존 App/고장난 설치 보존, 탐지·inventory 실패를 누락으로 오인하지 않음, WORK_PLAN_v0.2.md:42–49.

원인: bm_find_app은 [ -e "$path" ] 또는 [ -L "$path" ]가 참일 때만 기존 App을 검사하고, 두 검사 모두 거짓이면 다음 경로로 넘어간 뒤 missing=2를 반환한다. 부모에 접근할 수 없는 경우도 이 검사 결과는 거짓이므로 확정된 부재와 구분되지 않는다.

재현: 격리 HOME/Applications 아래 GitHub Desktop.app의 Info.plist 및 실행 파일 구조를 만든다. 접근 가능한 대조군은 탐지된다. 비특권 프로세스에서 Applications 부모의 접근을 차단하면 실제 stat은 Permission denied를 반환한다. exact bm_find_app은 그 상황에서 exit2를 반환했고 합성 brew에 연결한 Install 경로는 아래 요청을 보냈다.

```text
실제 stat = 접근 거부(EACCES), ENOENT 아님
bm_find_app = 2 (MISSING)
brew install --cask --require-sha homebrew/cask/github = 호출됨
접근 복구 후 기존 합성 App = 여전히 존재
통합 engine 최종 exit = 1 (사후 탐지 실패)
```

영향: 기존 앱이 있는지 확인할 수 없는데 설치를 요청한다. 나중에 Homebrew가 충돌을 거부할 가능성은 Bootstrap의 기존 설치 보존 판단을 대신하지 않는다. 이 재현에서 실제 Homebrew를 호출하거나 기존 앱을 덮어쓰지는 않았다. Mac의 ACL/TCC를 실기에서 재현했다는 주장도 아니다.

최소 교정: 탐지 대상 및 부모의 접근·조회 실패를 별도 오류로 반환하여 설치 분기로 보내지 않는다. 확정된 부재와 불확실한 상태를 구분하고, 보호된 경로에 대해 chmod·보안 우회·강제 설치를 자동 수행하지 않는다. 정상적으로 없는 App 또는 없는 사용자 Applications 디렉터리까지 모두 고장으로 처리하지 않도록 부재 조건도 명확히 한다.

Affected-only 재검증: 접근 가능한 기존/없는 앱, 없는 사용자 Applications, 접근 불가 부모, 고장난 symlink·plist·실행 파일, receipt-only, 표준 두 App 위치 중 하나에서 오류 발생. 탐지 오류 이후 brew 설치 요청0을 확인한다. 실제 Mac 권한 수락은 별도다.

### MAC-F003 — lock 디렉터리 생성 실패에서 HOME 경로가 raw stderr로 노출

Severity = LOW / P3. Verdict = FAIL. 영역 = MAC-V03.

위치: scripts/macos/platform.sh:85–95, 특히92. 요구: WORK_PLAN_v0.2.md:50과 작성자 완료보고 §3의 고정 상태·이유·숫자 출력, 개인 경로·vendor 원본 출력 제외.

원인: base 디렉터리를 새로 만드는 mkdir에는 stderr 정제가 없지만, 이어지는 install.lock mkdir에는 2>/dev/null이 있다. 첫 mkdir 실패는 bm_emit의 고정 코드 경로를 거치지 않고 원래 오류 문자열을 출력한다.

재현: 비특권 격리 HOME을 읽기·탐색만 가능한 상태로 두고 .mitchell-bootstrap이 없는 상태에서 Install --accept를 실행한다. OS/도구는 합성 경계다. 설치 요청은0이고 engine은 exit2/INSTALL_LOCKED_OR_UNSAFE로 올바르게 차단했지만 stderr에는 전체 합성 HOME 경로가 포함됐다. 아래 경로는 보고서에서 정규화한 것이다.

```text
mkdir: cannot create directory '$HOME/.mitchell-bootstrap': Permission denied
최종 exit=2 / INSTALL_LOCKED_OR_UNSAFE
설치 호출=0 / stderr에 정제 전 HOME 경로 포함
```

영향: 진단 출력을 공유하면 사용자 홈 경로가 함께 포함될 수 있다. 실제 비밀번호·토큰 노출 또는 외부 전송은 관측하지 않았다. 단독으로 치명적 취약점이라는 판정은 아니며, 병합 HOLD의 핵심 원인은 F001/F002다.

최소 교정: base mkdir 오류도 직접 출력하지 않고 고정 실패 이유로 처리한다. 숫자 실패 상태와 사용자 복구 안내는 보존한다. stderr를 숨기는 대신 성공으로 바꾸거나 기존 lock을 자동 삭제해서는 안 된다.

Affected-only 재검증: HOME 쓰기 거부, 기존 안전/불안전 디렉터리, symlink, 이미 존재하는 lock, 생성·해제 성공. stdout과 stderr를 함께 확인하며 실제 사용자 경로·인증정보는 사용하지 않는다.

## 6. 외부 공식 자료의 보조 확인

제품 요구는 §2의 Git 계약을 기준으로 했다. 외부 공식 자료는 지원 정책·식별자·명령 의미의 보조 확인이며 실제 설치나 라이선스 수락을 대신하지 않는다.

- Homebrew/brew docs/Installation.md를 connector로 직접 읽어 last_review_date2026-09-17과 blob d7b95f04a6cd1cbe473a0ab2462e587f68348530을 확인했다. 작성자의 macOS15+/Apple Silicon 및 Intel Tier3 설명과 일치한다. 웹 캐시의14+ 표기로 제품 계약을 조용히 되돌리지 않았다. 원문: `https://github.com/Homebrew/brew/blob/main/docs/Installation.md`.
- 공식 formula/cask 페이지로 catalogue의14개 ID와 종류를 대조했다. formula: git, gh, node@24, python@3.13, powershell, sevenzip, cocoapods. cask: github, visual-studio-code, temurin@17, temurin@21, android-studio, docker-desktop, dbeaver-community. 조회 형식: `https://formulae.brew.sh/formula/<id>`, `https://formulae.brew.sh/cask/<id>`. 최신 patch나 모든 OS의 설치 가능성을 보장한 것은 아니다.
- Homebrew manpage의 install/list, --require-sha 및 환경 옵션을 보조 확인했다: `https://docs.brew.sh/Manpage`.
- VS Code CLI의 --install-extension은 설치/업데이트, --list-extensions는 설치 목록 조회다. 이는 F001의 명령 의미 근거이며 가짜 code의 호출을 실제 업데이트 성공으로 확대하지 않았다: `https://code.visualstudio.com/docs/configure/command-line#_working-with-extensions`.

## 7. 미실행과 최소 다음 범위

```text
CLEAN_EXISTING_MAC_INSTALL = NOT_RUN / INDETERMINATE
FINDER_GATEKEEPER_DEVICE_ACCEPTANCE = NOT_RUN
REAL_PACKAGE_INSTALL_ADMIN_PROMPTS = NOT_RUN
REAL_AI_GITHUB_LOGIN = NOT_RUN
XCODE_ANDROID_SDK_LICENSE_SIGNING_BUILD = NOT_RUN
WINDOWS_REAL_INSTALL = NOT_RUN / INDETERMINATE (기존 상태 유지)
MACOS_PR_READY_OR_MERGE = NOT_DONE
RELEASE_DEPLOY = NOT_DONE
PMO_RUNTIME = NOT_DISPATCHED
OTHER_PERSONA_DISPATCH = NOT_DONE
```

실제 Mac의 관리자 승인·패키지 다운로드/설치·의존성 변경·권한/저장공간 오류·nvm/asdf/volta 충돌·계정 로그인·Xcode/SDK/Simulator build를 수행하지 않았다. Intel runner 성공도 Intel 실사용 수락으로 확대하지 않는다. 이 스크립트 후보 검토가 서명된 PKG 릴리스 완료를 뜻하지 않는다.

MITCHELL은 이 결과를 수신하고 승인된 범위 안에서 MAC-F001–F003을 한 묶음으로 교정한다. 작성자 표적 검사 → 완료보고 → 새 exact head/tree 고정 → IVA affected-only 재검증 순서를 적용한다. 기존 유효한 Windows 보호 증거·macOS 정상 경로는 재사용하고 변경 영향만 다시 확인한다. 동일 후보 전체를 반복 검증하거나 PMO로 임의 이관하지 않는다.

이 검증 결과 자체는 제품 수정·기기 설치·병합·릴리스·배포의 새 실행권한이 아니다. macOS 최초 FAIL 기록은 해당 후보의 결과로 보존하고 후속 교정 결과는 별도 후보에 기록한다.

## 8. Git 기록 및 효과 경계

영속 쓰기 대상은 AofSpds/mitchell의 본 IVA 정제 보고서 한 개다. 제품 코드·제품 PR/branch/main·기존 B/W 및 SNS 결과·MITCHELL CURRENT/DECISIONS/WORKLOG/memory는 변경하지 않는다. 개인사진·인증정보·대화 원문·raw CI/기기 로그를 공개 Git에 저장하지 않는다.

본 문서의 기록 완료는 생성 API 응답과 exact commit의 readback으로 확정하며 자신의 미래 commit SHA를 본문에 예상해 넣지 않는다. 반환 패킷이 실제 결과 path/commit/blob과 기록 영수증을 소유한다.

마일스톤: Windows 병합 → Mac 구현·작성자 검사·후보 고정 → IVA 최초 검증 완료/교정 필요 [현재] → MITCHELL 수신·표적 교정 → 새 후보 affected-only 재검증 → 별도 승인된 실제 Mac 수락·병합·릴리스.
OWNER_ACTION = 반환 패킷을 MITCHELL 작성자 채널에 전달.
