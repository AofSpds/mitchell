# Bootstrap macOS v0.2 — IVA-001 교정 완료보고

DOCUMENT_ID = MITCHELL-BOOTSTRAP-MAC-001-CORRECTION-COMPLETION
VERSION = 1.0 / DATE = 2026-09-20 / WRITER = MITCHELL
STATUS = AUTHOR_CORRECTION_COMPLETED / EXACT_CANDIDATE_FIXED / IVA_REREVIEW_NOT_RUN / MERGE_HOLD

## 1. 원문 수신과 실행 경계

별도 IVA 결과 MITCHELL-BOOTSTRAP-MAC-001-RESULT v1.0을 GitHub connector로 직접 읽었다.
Repository: AofSpds/mitchell.
Commit: b3a72ee5b4814f516b88308d6d65a4704a839a7c.
Path: docs/execution/BOOTSTRAP_MACOS_IVA_RESULT_001_20260920.md.
Blob: c0904f3302eb04ae4e4d7e9f6a66d49043b257ab.

원753bc82 후보 FAIL, MAC-F001/F002 MEDIUM P2와 F003 LOW P3, MERGE/RELEASE HOLD를 보존한다. MAC-V01/V05 PASS와 V02/V03/V04 FAIL은 원 후보의 소스·합성 범위다. 보고서를 수정하거나 작성자 교정으로 소급 PASS하지 않았다.

기존 Owner의 macOS 구현·계속 지시(D027) 안에서 세 finding만 교정했다. 결과 자체를 새로운 실행권한으로 보지 않았다. MITCHELL이 직접 수행했으며 PMO NOT_DISPATCHED, 새 IVA NOT_RUN이다. 실제 사용자 Mac·패키지·계정·키·권한·제품main·배포를 변경하지 않았다.

## 2. 새 exact 후보

| 항목 | 값 |
|---|---|
| Repository | AofSpds/bootstrap |
| Branch / PR | work/bootstrap-macos-v0.2 / #2 OPEN·DRAFT·UNMERGED |
| Head | 30a06ea0807a2c9686f4dd15062e08fd56bdf891 |
| Tree | 4692aadb0637c4c3765c237431357ea4b34d1184 |
| Parent / original reviewed head | 753bc82f63f85722cd76ba78b186bcaf46253677 |
| Original tree | 0b5ff0331af7db7f83d23c308370a148acd4ad74 |
| Windows main / base | 7ede3b394032b3f1da60235cb0ef2dad410df565 |
| Diff | 5 files / +326 / -18 |
| Source file count | 35 |

변경 대상은 다음5개 파일이다: scripts/macos/bootstrap.sh, scripts/macos/platform.sh, tests/macos/test_iva001_corrections.py, .github/workflows/macos.yml, docs/macos/IVA001_CORRECTIONS.md.

## 3. Finding별 교정

| Finding | 실제 변경 | 보존·한계 |
|---|---|---|
| MAC-F001 | 확장 설치 성공 또는 nonzero 시도 후 목록을 새로 조회한다. 실패·부분 출력은 새 inventory로 반영하지 않고 후속 확장 처리를 종료한다. 성공한 목록만 다음 판단에 사용한다. | 기존 확장·고정 ID·실패 숫자·수동 로그인 유지. 수동 재실행도 먼저 조회해 관측된 기존 효과를 재설치하지 않는다. |
| MAC-F002 | bm_path_state는0=존재 관측,2=부재 확인,5=조회 불확실이다. -e/-L false 후 부모 경로를 확인하고 탐색·한 단계 열거 성공과 대상 부재를 요구한다. 두 App 위치를 모두 대조한다. | 정상 미설치·없는 사용자 Applications는 허용. 고장난 symlink/plist/executable은3으로 보호. 부모 접근 오류는5/APP_PATH_QUERY_FAILED로 해당 설치 차단. 권한 변경·보안 우회 없음. |
| MAC-F003 | lock 기반 mkdir의 stderr도 직접 출력하지 않도록 억제한다. | exit2/INSTALL_LOCKED_OR_UNSAFE·기존 lock·symlink 거부 유지. 실패를 성공으로 바꾸거나 stale lock을 삭제하지 않는다. |

F002는 지역화된 오류문자열이나 errno를 추측하는 방식이 아니다. find는 parent/.의 직접 자식만 열거하고 하위 디렉터리는 prune한다. HOME 등의 특수문자를 literal로 취급하고 탐지 경로를 로그로 출력하지 않는다. 건강한 첫 App이 불확실한 두 번째 App 경로를 숨기지 않도록 보수적으로 처리한다. 외부 프로세스가 탐지 이후 파일을 바꾸는 모든 경쟁 조건까지 원자적으로 통제한 것은 아니다.

제품 실행에 Python/Node 의존성을 추가하지 않았다. Python은 시험용이다. Homebrew 지원선·패키지 식별자·모바일 범위·동의·Windows 코드·알림/SNS 앱은 변경하지 않았다.

## 4. 작성자 검사

| 검사 | 결과 / 실행 범위 |
|---|---|
| 원 후보의 대표 F001/F002/F003 조건 | 세 조건 모두 기대한 결함 재현. 원 제품 bytes 변경 없음 |
| 새 표적 Linux 시험 | 35 PASS / 0 FAIL: 확장9, App20, lock6 |
| 새 후보의 기존 Linux 시험 | 58 PASS + 지원 정책8 PASS |
| Linux 합계 | 101개 고유 시험 PASS; 반복 실행 수를 더하지 않음 |
| macOS arm64 | Bash3.2.57, 기존57 PASS/1 SKIP, 정책8 PASS, 표적35 PASS |
| macOS Intel | fixture·Windows guard·native Verify job/step SUCCESS 확인; 개별 로그 수치는 별도 재수집하지 않음 |
| Windows PowerShell5.1/7 | 자동 실행된 회귀 job/step SUCCESS; Windows18개 blob 동일 |
| 받은 최종 artifact | 동일 소스의 표적35개 재실행 PASS; 별도 신규 시험 수로 더하지 않음 |

Linux 표적 시험은 비특권 사용자와 실제 Bash·격리 파일시스템을 사용한다. OS/brew/code는 합성 경계이며 실제 패키지·확장을 설치하지 않았다. Linux에서 plist field만 시험용 plistlib adapter로 대체하며 Mac CI에서는 native Apple plutil을 사용한다. root 권한으로 접근 거부를 시험한 것으로 취급하지 않는다. MAC_TEST_PRODUCT_ROOT는 원 후보 재현을 위한 시험 하네스 옵션이고 제품 우회 설정이 아니다.

macOS CI: 35457443486. arm64 job105935134431 / Intel job105935134371 모두 SUCCESS.
Windows CI: 35457443465. PS5.1 job105935134169 / PS7 job105935134095 모두 SUCCESS.

arm64 로그의 실제 OS는 macOS15.7.9, native Mobile Verify는 exit2였다. 일부 도구 누락 안내이며 모든 도구 준비 또는 설치 성공을 뜻하지 않는다. Verify에서는 설치를 요청하지 않았다. 이전753bc82의 CI35452044776/35452044793은 재실행하지 않고 과거 증거로 보존했다. 새 commit에 자동 실행된 회귀 CI와 독립 IVA를 구분한다.

## 5. 산출물 동일성

Artifact10588004720 / bootstrap-macos-source / 43127 bytes.
Outer SHA-256: ccf81b4583df12fd203629a158a90b7a0ecbb7ba4e1c0ec403a5f1e077199e69.
Inner tar SHA-256: 2f74477887bc7327a33cb7c2f1f4d0d2434a6610f8548c56bb975ac16de34de8.

다운로드 ZIP의 CRC와 압축 내 절대경로·상위 경로 이탈·link entry 부재를 확인했다. COMMIT.txt/TREE.txt가 exact 후보와 일치한다.35개 source 파일과 실행 mode를 대조했다. export된 Windows 텍스트9개의 CRLF만 hash 계산에 LF로 정규화하고 bootstrap.bat은 원바이트를 유지해 재계산한 tree가4692aadb…로 일치했다. 같은 정규화 기준에서 로컬 검사 파일과 바이트가 일치한다. archive 원본 전체가 Git blob bytes와 같다는 뜻은 아니다.

Windows 보호18개는 기존 exact blob과 동일하다. 원 .bat 줄바꿈과 executable .command mode도 유지한다. 로컬 임시 commit은 remote parent로 사용하지 않았으며 실제753bc82를 parent로 fast-forward했다. 합성 회귀시험 소스는 제품 Git에 포함하지만, 원본 실행 로그·개인자료·인증정보는 공개 Git에 기록하지 않았다.

## 6. 다음 단계

세 finding의 작성자 교정·검사 완료이며 새 독립 판정은 NOT_RUN이다. docs/execution/BOOTSTRAP_MACOS_IVA_AFFECTED_REREVIEW_PACKET_v0.2.md로 MAC-F001–F003과 실질적 공통 영향만 재검증한다. 변경 없는 Windows 및 유효 정상 경로는 재사용한다.

Finder/Gatekeeper, 실제 clean/existing Mac 패키지 설치·관리자 승인·TCC/ACL·의존성 변경·실계정 로그인·Xcode/SDK/license/서명/Mobile build는 NOT_RUN/INDETERMINATE다. MERGE_RECOMMENDATION=HOLD, RELEASE_DEPLOY_RECOMMENDATION=HOLD를 유지한다. PR ready/merge·릴리스·배포는 하지 않았다.
