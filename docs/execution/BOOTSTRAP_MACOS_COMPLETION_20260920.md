# Bootstrap macOS v0.2 — 작성자 완료보고

DOCUMENT_ID = BOOTSTRAP-MAC-COMPLETION-001
VERSION = 1.0
DATE = 2026-09-20
WRITER = MITCHELL
STATUS = AUTHOR_IMPLEMENTATION_COMPLETED / EXACT_CANDIDATE_FIXED / IVA_NOT_RUN / MACOS_MERGE_HOLD

## 1. 재개·실제 완료

사용자는 Windows 후보를 병합하고 macOS 확장을 설계·구현하는 순서에 동의한 뒤 중단 후 계속을 지시했다. 현재 MITCHELL 채널이 직접 수행했고 PMO로 이관하지 않았다.

재개 시 GitHub connector로 운영 README/CURRENT/AGENTS/관련 결정과 제품 refs/PR/계획을 읽었다. 운영 main은 d4abb038…/CURRENT generation12였고, 제품에는 이미 Windows merge7ede3b…와 macOS 초기 후보78a919e…/Draft PR#2가 존재했다. Windows 병합과 초기 구현을 다시 생성하지 않았다. 초기 macOS CI35449122339와 Windows CI35449122295 성공도 복구했다. 이 보고서는 중단 전 실행과 재개 후 실행을 합쳐 정리하지만 이전 일을 이번 턴에 새로 수행했다고 주장하지 않는다.

Windows merge commit의 parent는 초기 main351333d6…와 검증 head f880d029…이며 tree2a28a91f…는 IVA-B001/B002 PASS 대상과 동일하다. 실제 Windows 설치 수락·릴리스는 별도다.

## 2. 정확한 최종 대상

| 필드 | 값 |
|---|---|
| Repository | AofSpds/bootstrap |
| Branch / PR | work/bootstrap-macos-v0.2 / #2 OPEN·DRAFT·UNMERGED |
| Exact head | 753bc82f63f85722cd76ba78b186bcaf46253677 |
| Exact tree | 0b5ff0331af7db7f83d23c308370a148acd4ad74 |
| Parent / recovered candidate | 78a919e9300bbb8de8fbc8843d7a8dc6b59044a1 |
| Parent tree | 92153d14f9821b0e891bd09fa00b22a1c1fbc070 |
| Base main / Windows merge | 7ede3b394032b3f1da60235cb0ef2dad410df565 |
| Base tree | 2a28a91f8fdbbbc80bd37209206cdab9cf5e715a |
| Source files | 33 |
| Windows protected blobs | 18 unchanged; README 안내만 추가 |

## 3. 기능과 안전 경계

- bootstrap.command 메뉴 또는 CLI → 기본 Bash3.2 엔진. Plan/Install/Verify, Core/Mobile/Optional, None/Codex/Claude/Both.
- Core: Git, GitHub Desktop, GitHub CLI, VS Code, Node. 기존22.16+ 22.x/24.x를 재사용하며 미설치 시 node@24. 호환되지 않는 기존 도구는 보존한다.
- Optional: Python3.13, PowerShell, SevenZip, Temurin21, Docker Desktop, DBeaver Community.
- Mobile: Core+Temurin17/CocoaPods/Android Studio, full Xcode/iOS SDK 및 Android SDK 파일 탐지. SDK 설치·license·first-launch·실제 build 성공은 별도다.
- CLT/Homebrew 최초 준비는 사용자 수동이다. Bootstrap이 원격 설치 스크립트를 받아 실행하거나 관리자 비밀번호를 수집하지 않는다.
- 표준 Homebrew prefix만 사용하고 Rosetta/root를 거부한다. 기존 도구·고장난 설치·receipt 불일치를 구분해 강제 재설치하지 않는다.
- Install만 명시적 동의를 받고 lock으로 직렬화한다. 고정 공식 package ID/extension만 요청한다. 성공 exit만으로 완료하지 않고 receipt와 실제 CLI/App 구조를 다시 확인한다.
- package 실패는 숫자 exit를 보존하고 독립 항목을 계속 처리한다. inventory 자체 오류는 빈 목록으로 처리하지 않고 후속 package 설치를 막는다.
- 직접 upgrade/reinstall/uninstall/link/보안 해제/자동 재부팅/dotfile·Git identity 변경 없음. Homebrew의 필요한 의존성 변경 가능성은 동의 전에 알린다. 전체 dependency version lock은 구현하지 않았다.
- 자체 출력은 고정 ID·상태·이유·숫자 exit다. vendor raw log/환경/개인 경로·키를 Git에 기록하지 않는다. Plan/Verify의 도구 내부 cache까지 전혀 안 바뀐다고 보장하지 않는다.

## 4. 재개 후 지원 정책 교정

초기 후보는 Homebrew의 과거14+ 표기를 사용했다. 2026-09-20 확인 시 검색 결과와 일부 열람 cache가 서로 달라 Homebrew/brew의 docs/Installation.md를 GitHub connector로 직접 읽었다. last_review_date2026-09-17, blob d7b95f04a6cd1cbe473a0ab2462e587f68348530의 원문은 Apple Silicon/macOS15+ 및 Intel Tier3를 명시한다.

최종 후보는 macOS15+를 Install 최소선으로 적용한다. macOS14에서는 Plan/Verify 진단을 조치 필요 상태로 제공하고, Install은 Homebrew·AI 호출/lock 전에 차단한다. Intel opt-in도 OS 차단을 우회하지 않는다. macOS13 이하는 기존처럼 진입 거부다. 이 정책은 OS 자동 업그레이드나 모든15+ 환경 수락 선언이 아니다.

변경은 지원 검사·fixture 기본값·8개 표적 시험·macOS CI runner·관련 문서8파일에 한정한다. catalogue·package 설치 경로·Windows18파일은 바꾸지 않았다. 새 CI는 macos-15 arm64와macos-15-intel을 사용한다. 초기14 CI 성공은 당시 후보의 기록으로 보존한다.

## 5. 작성자 검사

| 검사 | 결과와 실제 경계 |
|---|---|
| Linux 실제 Bash 엔진+격리 명령 경계 | 58개 PASS + 지원 정책8개 PASS; 실제 brew/code/CLT 설치 호출 없음 |
| 최종 macOS CI35452044776 | macos-15 job105920780633, macos-15-intel job105920780732 모두 SUCCESS |
| macOS CI 내용 | stock Bash syntax·동일 fixture군·Windows blob guard·실제 runner 읽기 전용 Mobile Verify; clean-Mac 설치 시험 아님 |
| Windows CI35452044793 | powershell-51 job105920780622, powershell-7 job105920780843 SUCCESS |
| 최종 source 동일성 | outer ZIP/inner tar SHA, CRC·경로·mode·33파일과 tree 일치 |
| macOS IVA / 실제 Mac 설치 | NOT_RUN / NOT_RUN |

Mac suite에는 Linux 전용 production-wrapper 음성 시험의 skip 분기가 있다. 초기 macOS 로그에서도58개 중1개 skip을 확인했다. 따라서 Linux58+8을 그대로 macOS의66개 실행 PASS라고 부르지 않는다. 최종 Mac CI는 각각 suite/읽기전용 검사가 성공한 것으로 기록한다. Native Verify는 미설치 도구가 있으면 exit2가 정상적인 조치 필요 결과이며, 모든 개발환경 준비가 끝났음을 의미하지 않는다.

초기 후보의 유효 CI는 재실행하지 않았다. 새 지원 정책을 변경한 후 저장소에 이미 설정된 PR CI가 자동 실행한 결과를 확인했다. 작성자 시험을 독립 IVA 증거로 표시하지 않는다.

## 6. 산출물 동일성

Source artifact10587226850 / bootstrap-macos-source:
- outer SHA-256: 2b9c6815ef19d0ca1a6369c58575799ded32150d9e20e26ea2381ed030db95ed
- inner bootstrap-macos-candidate.tar.gz SHA-256: d5b8379b25ad057afc5264080bde599f78dc5ada982f068e2bb7b91e153235d6
- COMMIT.txt/TREE.txt: 위 exact head/tree와 일치.
- ZIP CRC, tar/ZIP 절대경로·상위탈출·link entry 배제를 확인했다. 파일33개는 로컬 검사 작업본과 바이트 단위로 일치한다.
- git archive가 .gitattributes에 따라 Windows 텍스트9개를 CRLF로 출력하므로, 기존 blob과 대조해 LF로 계산한9개와 나머지 원바이트로 tree를 재구성했다. bootstrap.bat의 기존 CRLF는 그대로 유지한다.
- 처음 raw 전체로 계산한 tree 불일치는 이9개 EOL 변환 때문임을 확인했다. 정규화된 각각의 blob이 기존 Windows expected blob과 정확히 일치하며, 최종 tree0b5ff033…도 일치한다. 내용을 임의 교정하여 맞춘 것이 아니다.

## 7. 미실행과 다음

실제 clean/existing Mac 설치, Finder/Gatekeeper·권한·관리자 승인, 패키지 다운로드/설치·실패·재부팅·nvm/asdf/volta 충돌, Xcode/SDK/Simulator 실제 build, AI/GitHub 로그인은 NOT_RUN이다. Intel CI 성공을 Intel 실사용 수락으로 확대하지 않는다. 설치 스크립트 소스 후보이며 서명된 PKG·앱 릴리스가 아니다.

별도 IVA에 docs/execution/BOOTSTRAP_MACOS_IVA_PACKET_v0.1.md를 전달한다. macOS는 새 검증 대상이며 기존 B/W와 SNS Gateway PASS를 재사용하지 않는다. 유효 CI와 변경 없는 Windows blob 증거를 재사용하고 신설 macOS 계약과 실제 영향만 검증한다. 제품 PR#2 병합·사용자 Mac 설치·release/deploy는 별도 승인/증거까지 HOLD다.

PMO NOT_DISPATCHED. web-starter/sns-gateway 변경 없음. API 키·새 저장소·계정·결제·실사진·SNS·사용자 기기 변경 없음. 운영 기록 완료는 이 문서를 포함하는 원격 commit/readback으로 확인한다.
