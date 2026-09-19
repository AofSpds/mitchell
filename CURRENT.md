# MITCHELL Current

Updated: 2026-09-20 / Generation: 13 / Writer: MITCHELL
Expected previous generation: 12
Recovery base: d4abb0380a2bbef36db7e7ee225e5dda204c479e

## 목적·현재

사용자가 승인한 Windows Bootstrap 기준선 병합과 macOS 확장 작업을 중단 지점부터 복구했다. Windows PR #1은 이미 2026-09-19에 병합돼 있었으므로 반복하지 않았다. macOS 초기 후보·성공 CI도 복구하고 현재 Homebrew 지원 정책에 맞춘 후속 코드를 추가했다. 최종 macOS 후보의 작성자 검사·Git 고정은 완료했으며 별도 IVA와 실제 Mac 설치 수락은 미실행이다.

| 항목 | 현재 값 |
|---|---|
| Persona / Git executor | MITCHELL / 현재 채널 직접 수행 |
| Task | BOOTSTRAP-MAC-001 / RESUME-COMPLETE |
| 실행 근거 | 사용자의 Windows 병합 후 macOS 설계·구현 진행 및 중단 후 계속 지시; D027/D028 |
| 제품 | AofSpds/bootstrap |
| Windows PR #1 | MERGED / CLOSED |
| Windows main | 7ede3b394032b3f1da60235cb0ef2dad410df565 |
| Windows merge tree | 2a28a91f8fdbbbc80bd37209206cdab9cf5e715a; B001/B002 교정 검증 tree와 동일 |
| macOS Branch / PR | work/bootstrap-macos-v0.2 / #2 OPEN·DRAFT·UNMERGED |
| macOS exact head | 753bc82f63f85722cd76ba78b186bcaf46253677 |
| macOS exact tree | 0b5ff0331af7db7f83d23c308370a148acd4ad74 |
| 복구한 초기 Mac 후보 | 78a919e9300bbb8de8fbc8843d7a8dc6b59044a1 |
| macOS 작성자 CI | 35452044776 / macos-15 arm64·macos-15-intel SUCCESS |
| Windows 회귀 CI | 35452044793 / PowerShell 5.1·7 SUCCESS |
| Linux 작성자 시험 | 기존58 + 지원 정책8 = 66 PASS |
| Windows 기존 파일 보존 | 18개 blob 동일; README 플랫폼 안내만 추가 |
| macOS 독립 IVA | NOT_RUN / 최초 검증 패킷 준비 |
| 실제 clean/existing Mac·Windows 설치 수락 | NOT_RUN / INDETERMINATE |
| macOS 제품 병합·릴리스·배포 | NOT_DONE / HOLD |
| PMO / 기타 Persona | NOT_DISPATCHED / NOT_INSTALLED |

## 구현 범위

Mac bootstrap.command와 Bash3.2 엔진에 Plan/Install/Verify, Core/Mobile/Optional·AI 선택, OS/CPU/Rosetta/CLT/stock Homebrew 검사, 사용자 동의·단일실행 lock, 기존 도구 보존, 설치 receipt+사후 탐지, 숫자 실패 코드·재실행 처리를 구현했다.

기본 Core는 Git/GitHub Desktop/GitHub CLI/VS Code/Node다. Mobile은 Temurin17/CocoaPods/Android Studio와 Xcode·SDK 탐지이며, Xcode·SDK 설치·라이선스·서명·실제 앱 빌드까지 완료한다는 뜻이 아니다. CLT와 Homebrew 최초 설치는 공식 경로에서 사용자가 직접 수행한다. API 키·추가 가입·결제는 자동 처리하지 않는다.

현재 설치 기준은 Apple Silicon macOS15+이며 macOS14는 Plan/Verify 진단만 허용하고 Install을 차단한다. Intel은 Homebrew Tier3로 명시하고 macOS15+에서 별도 opt-in이 필요한 미수락 경로다. 이전14+ 제안은 Homebrew 공식 Git 원문(last_review_date 2026-09-17, blob d7b95f04a6cd1cbe473a0ab2462e587f68348530) 재확인으로 대체했다. 과거 후보와 당시 CI 기록은 삭제하지 않는다.

## 증거·다음

- 완료보고: docs/execution/BOOTSTRAP_MACOS_COMPLETION_20260920.md
- Manifest: docs/execution/BOOTSTRAP_MACOS_MANIFEST_20260920.json
- 별도 IVA 입력: docs/execution/BOOTSTRAP_MACOS_IVA_PACKET_v0.1.md
- 제품 설계: bootstrap/docs/macos/WORK_PLAN_v0.2.md
- 사용자 안내·미실행: bootstrap/docs/macos/FIRST_RUN.md, ACCEPTANCE.md

Artifact10587226850의 outer/inner SHA-256과33파일을 확인했다. archive에서 .gitattributes에 의해 CRLF가 된 Windows 텍스트9개만 기존 blob에 맞는 LF로 계산하고 bootstrap.bat은 원바이트를 유지하여 exact tree를 재구성했다. 모든 archive 파일은 작성자 검사 작업본과 바이트 단위로 일치했다. macOS runner 검사는 패키지 설치가 아니라 Bash fixture·읽기 전용 Verify다.

이제 macOS 신설 경로를 별도 IVA가 검증한다. 기존 Windows B001/B002 PASS나 SNS Gateway PASS를 macOS로 확대하지 않는다. 유효 CI는 재사용하며 사용자 Mac 설치·관리자 승인·계정·로그인·서명·릴리스는 별도 권한과 증거가 필요하다.

## 다른 제품·효과 보존

SNS Gateway PR #1은 앞선 승인으로 main 1c7cd117d18929cdf40abc0e4f73fec4c87b2c65에 병합됐으며 실제 기기/SNS·서명·릴리스는 여전히 미완료로 보존한다. 이번 작업에서 sns-gateway/web-starter의 코드·refs·PR은 변경하지 않았다. Web Starter의 과거 IVA PASS와 미병합/실Supabase 미실행 기록도 이번 macOS 작업과 구분한다.

EFFECT_STATE: 중단 전 Windows 병합·Mac 초기 구현을 복구하고 Mac 작업 브랜치·검사·운영 기록을 갱신했다. 사용자 PC/Mac·계정·키·실사진·SNS·릴리스에는 변경 없음.
LAST_WORKLOG_EVENT: E017.
OWNER_ACTION_REQUIRED: 별도 IVA 채널에 BOOTSTRAP_MACOS_IVA_PACKET_v0.1.md의 exact Git 경로를 전달한다. API 키·새 저장소는 불필요하다.

마일스톤: Windows 기준선 병합 → Mac 설계·구현 → 작성자 검사·후보 고정 [현재] → 별도 IVA → 승인된 실제 Mac 수락·별도 병합·릴리스.
