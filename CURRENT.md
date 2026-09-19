# MITCHELL Current

Updated: 2026-09-20 / Generation: 14 / Writer: MITCHELL
Expected previous generation: 13
Recovery base: b3a72ee5b4814f516b88308d6d65a4704a839a7c

## 목적·현재

Bootstrap macOS v0.2 최초 IVA 결과 원문을 exact Git commit/blob으로 수신하고 MAC-F001–F003을 기존 D027 실행 승인 범위에서 교정했다. 현재 상태는 작성자 교정·표적 검사·새 후보 고정 완료다. 원 후보753bc82의 FAIL과 MERGE/RELEASE HOLD를 보존한다. 새 후보의 별도 IVA affected-only 재검증은 NOT_RUN이며 작성자가 독립 PASS를 선언하지 않는다.

| 항목 | 현재 값 |
|---|---|
| Persona / writer | MITCHELL / 현재 채널 직접 수행 |
| Task | BOOTSTRAP-MAC-IVA001-CORRECTION-001 |
| 실행 근거 | 기존 Owner macOS 구현·계속 지시 D027; IVA 결과 자체는 새 권한 아님 |
| 원 IVA 결과 | docs/execution/BOOTSTRAP_MACOS_IVA_RESULT_001_20260920.md v1.0 |
| 결과 commit / blob | b3a72ee5b4814f516b88308d6d65a4704a839a7c / c0904f3302eb04ae4e4d7e9f6a66d49043b257ab |
| 원 검증 후보 | 753bc82f63f85722cd76ba78b186bcaf46253677 / FAIL |
| Finding | MAC-F001 P2 / MAC-F002 P2 / MAC-F003 P3 |
| 제품 / Branch / PR | AofSpds/bootstrap / work/bootstrap-macos-v0.2 / #2 OPEN·DRAFT·UNMERGED |
| 새 exact head | 30a06ea0807a2c9686f4dd15062e08fd56bdf891 |
| 새 exact tree | 4692aadb0637c4c3765c237431357ea4b34d1184 |
| 변경 범위 | 원 후보 대비5파일 / +326 / -18; 전체35파일 |
| macOS 작성자 CI | 35457443486 / Apple Silicon·Intel SUCCESS |
| Windows 회귀 CI | 35457443465 / PowerShell5.1·7 SUCCESS |
| 작성자 Linux 시험 | 표적35 + 기존58 + 정책8 = 101 PASS |
| 원 IVA / 새 후보 재검증 | COMPLETED·FAIL / NOT_RUN |
| Windows main | 7ede3b394032b3f1da60235cb0ef2dad410df565; 이번 변경 없음 |
| Windows 보호 파일 | 18개 blob 동일 |
| 실제 Mac·Windows 설치 수락 | NOT_RUN / INDETERMINATE |
| macOS 병합·릴리스·배포 | NOT_DONE / HOLD |
| PMO / 기타 Persona | NOT_DISPATCHED / NOT_INSTALLED |

## 교정과 증거

- F001: 확장 설치 시도 후 성공한 새 목록을 확보해야 다음 확장의 설치 여부를 판단한다. 실패·부분 조회 출력은 폐기하고 후속 확장 변경을 중단한다. 수동 재실행도 먼저 새 목록을 읽는다.
- F002: 경로 검사 false를 곧바로 미설치로 판단하지 않는다. 존재하는 부모의 탐색·직접 자식 열거가 성공하고 대상이 없을 때만 missing=2다. 불확실한 조회는5/APP_PATH_QUERY_FAILED로 설치를 차단한다. 두 표준 App 위치를 모두 확인하고 고장난 기존 앱도 보존한다.
- F003: lock 기반 디렉터리 mkdir 실패의 raw stderr를 억제하고 고정 exit2/INSTALL_LOCKED_OR_UNSAFE를 유지한다. 기존 lock·symlink 자동 삭제와 사용자 권한 변경은 없다.

원 후보의 세 대표 결함을 합성 시험으로 재현한 뒤 교정본의 표적35개가 통과했다. Linux 시험은 실제 Bash/격리 파일시스템과 합성 OS·brew·code 경계이며 실제 설치가 아니다. macOS arm64 로그는 Bash3.2.57, 기존57 PASS·1 SKIP, 정책8 PASS, 새35 PASS와 읽기 전용 Mobile Verify exit2를 확인했다. Intel과 Windows는 성공한 job/step을 확인했다. 이전 후보 CI를 재실행하지 않았으며 새 후보의 workflow만 실행됐다.

최종 artifact10588004720을 내려받아 CRC/경로/mode와35파일을 대조했다. Windows 텍스트9개의 export CRLF만 해시 계산에 정규화하고 bootstrap.bat은 원바이트로 유지한 exact tree가 일치한다. 받은 소스에서도 표적35개가 통과했다.

## 문서·다음

- 교정 완료보고: docs/execution/BOOTSTRAP_MACOS_IVA001_CORRECTION_COMPLETION_20260920.md
- 교정 Manifest: docs/execution/BOOTSTRAP_MACOS_IVA001_CORRECTED_MANIFEST_20260920.json
- IVA affected-only 입력: docs/execution/BOOTSTRAP_MACOS_IVA_AFFECTED_REREVIEW_PACKET_v0.2.md
- 기존 설계·최초 보고·원 IVA 결과는 당시 후보의 증거로 보존한다.

다음은 새 exact 후보의 MAC-F001–F003 및 실질적 공통 영향만 별도 IVA가 재검증하는 단계다. 원 MAC-V01/V05 PASS의 소스·합성 범위는 보존하고 macOS 전체 또는 실제 사용자 Mac 수락 PASS로 확대하지 않는다. Windows18개와 기존 정상 경로의 유효 증거는 재사용한다.

Homebrew 설치 지원선·catalogue·동의·CLT/Homebrew 최초 준비·Xcode/SDK/license/로그인 수동 경계는 변경하지 않았다. 실제 패키지 설치·Finder/Gatekeeper·TCC/ACL·계정/키·서명·Mobile build·Mac PR 병합·릴리스는 수행하지 않았다. web-starter와 sns-gateway는 변경하지 않았다.

EFFECT_STATE: macOS 교정 작업 브랜치·작성자 CI 및 MITCHELL 상태/완료 기록만 변경. 최초 IVA 보고서는 보존.
LAST_WORKLOG_EVENT: E018.
OWNER_ACTION_REQUIRED: 새 affected-only Git 패킷을 별도 IVA 채널에 전달. API 키·새 저장소는 필요 없음.

마일스톤: Windows 병합 → Mac 구현 → 최초 IVA FAIL → F001–F003 교정·작성자 검사·새 후보 고정 [현재] → 별도 IVA affected-only → 승인된 실제 Mac 수락·병합·릴리스.
