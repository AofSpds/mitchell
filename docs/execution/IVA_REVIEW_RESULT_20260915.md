# MITCHELL → IVA 최초 독립검증 결과

- 문서 ID: `MITCHELL-IVA-BW-001-RESULT`
- 버전: `1.0`
- 작성 시각: `2026-09-15T18:00:13+09:00` / Asia/Seoul
- 입력: `MITCHELL-IVA-BW-001`, IVA_REVIEW_PACKET v0.1, 2026-09-15.
- 검증자: **IVA — 사용자가 `iva`로 지정한 이 검증 대화**.
- 작성자: MITCHELL. 본 검증자는 대상 제품 소스를 작성·수정하지 않았다.
- 실제 실행 환경: ChatGPT의 GitHub connector 읽기 + 격리 Linux 컨테이너. 별도 Codex WORK/다른 검증자 프로세스를 실행한 것은 아니다.
- 상태: `READ_ONLY_REVIEW_COMPLETED / CORRECTION_REQUIRED`.
- 반환 대상: MITCHELL 작성자 채널. **이 결과는 자동 수정 명령·병합 승인·배포 승인이 아니다.**

## 1. 결론과 현재 조치

| 대상 | 판정 | 의미 |
|---|---|---|
| Windows Bootstrap 고정 후보 | **FAIL** | 로그 마스킹과 WinGet 재부팅 상태 분류에 교정 필요 |
| Web Starter 고정 후보 | **FAIL** | 잘못된 설정의 DEMO 오표시와 로그아웃 오류 처리에 교정 필요 |
| Windows 실기·실제 Supabase 통합 적합성 | **INDETERMINATE** | 필요한 실제 환경 시험이 NOT_RUN이며, 소스 검토로 대체하지 않음 |
| 두 후보의 현 상태 병합·실사용 릴리스 | **HOLD / 권고하지 않음** | 아래 4건 교정과 남은 실환경 수락 조건 확인 필요. 병합 권한도 별도 |

기존 CI 성공은 유효하다. 그러나 그 CI가 다루지 않는 오류 경로에서 결함을 확인했으므로 사양 충족 PASS로 승격하지 않는다. 이번 FAIL은 전체 설계를 다시 만들라는 뜻이 아니라, **정확히 지정한 4개 교정 범위를 반환한다는 뜻**이다.

Severity는 `MEDIUM / P2` 4건이다. 기능·안전·복구 계약을 깨는 결함으로 현 후보의 채택 전에 교정할 것을 권고한다. 실제 침해 사고 또는 재현된 계정 간 데이터 탈취를 주장하는 HIGH/CRITICAL 판정은 아니다.

## 2. 고정 대상과 동일성

| 항목 | Bootstrap | Web Starter |
|---|---|---|
| Repository | `AofSpds/bootstrap` | `AofSpds/web-starter` |
| PR / branch | `#1 / work/bootstrap-v0.1` | `#1 / work/web-starter-v0.1` |
| Head | `b4cabc7acb558c556a5b59a7826e43757d67eb27` | `15efb21e9cf3c4ba60c34af95f928ba38221a224` |
| Tree | `4d57b63278c33fa9213c1fa5ba82e19c1eefb168` | `b657dbbb7177a2e9e70ba8c922c4cec628bbd3ff` |
| Base main | `351333d6b4e6db1635c8caeb0f7dd8c4e3aee72c` | `0d855cd9aaf1ee8ebf31a2a7af6d67c90367d0d6` |
| Author CI run | `34939006314` | `34939036421` |
| 직접 읽은 최종 PR CI artifact ID | `10384850456` | `10384613044` |
| 소스 파일 수 | 19 | 53 |
| PR 상태 재확인 | OPEN / DRAFT / UNMERGED | OPEN / DRAFT / UNMERGED |

검증 시작·종료 전 PR 조회에서 head가 입력 패킷과 같았다. 원격 commit 객체의 tree를 직접 읽었으며 최신 main을 구현 후보와 혼동하지 않았다. `merge_commit_sha`가 존재한다는 이유로 병합 완료로 판정하지 않았다.

운영 기준은 `AofSpds/mitchell@ca07ffa3ec9e162583293e3a9e33317220464d77`이다. README, CURRENT, AGENTS, DECISIONS, 실행 승인 기록, 완료보고, 후보 manifest와 계획을 읽었다. 계획 파일 `docs/IMPLEMENTATION_PLAN_v1.0.md`의 blob은 입력과 같은 `3f4e6910d191f263baa08a1a4e4d1ce7bb4e7635`다. 초기 문서-only 범위와 후속 구현 승인의 대체 관계를 보존했으며, 후속 구현 승인을 본 검증의 제품 변경 권한으로 해석하지 않았다.

### 실제 소스 동일성 검사

CI artifact를 내려받아 안전하게 해제한 소스에서 Git blob/tree를 다시 계산했다. **Web Starter 53개 파일은 원본 바이트로 exact tree가 일치**한다. **Bootstrap은 19개 중 11개 파일의 CRLF→LF 정규화를 계산에만 적용한 뒤 exact tree가 일치**한다. `bootstrap.bat`의 CRLF는 그대로 유지했다. 원본 파일은 바꾸지 않았다.

Bootstrap raw tree는 `43032e0a797e78f4a064ea80423a1e38115d634a`이며, 정규화 후 tree가 고정 Git tree와 같다. 따라서 Bootstrap ZIP의 모든 바이트가 Git blob과 같다고 주장하지 않는다. 검토 후에도 동일성 검사를 통과했다. 파일별 SHA256·Git blob·정규화 여부는 `source_file_inventory.json`에 있다.

## 3. 교정 finding

### IVA-B001 — 로그 마스킹의 Bearer 헤더·따옴표 값 누락

- **Severity:** MEDIUM / P2
- **파일:** `scripts/common.ps1:9–14`, `:66–77`; 출력 유입 경로 `scripts/install-packages.ps1:27–28`.
- **요구 근거:** 계획 §6.4의 토큰·사용자 경로 제거, §6.5 안전 조건; 패킷의 로그 비밀정보·경로 마스킹 체크 포인트.
- **증거 수준:** exact source 정적 확인 + 해당 정규식을 그대로 추출한 Python 순차 치환 모델. PowerShell 5.1/7에서 이번 검증자가 실제 실행한 결과는 아니다.

`authorization`에 대한 일반 `key=value` 치환이 `Bearer`만 먼저 소비한다. 그 결과 다음 Bearer 치환은 인증 방식 문자열을 찾지 못하고 뒤의 불투명 토큰 값을 남긴다. 따옴표로 감싼 JSON key/value도 일반 치환식이 처리하지 못한다.

```text
입력: Authorization: Bearer fixture_access_value
출력: Authorization=<redacted> fixture_access_value

입력: {"password":"fixture_password_value"}
출력: {"password":"fixture_password_value"}
```

모두 검증용 가짜 값이다. 실제 키·비밀번호·사용자 로그를 사용하지 않았고 실제 유출 사고를 확인한 것은 아니다. 그러나 이런 문자열이 설치기 오류·예외에서 `nextAction`으로 유입되면 정제 로그에도 값이 남을 수 있다. 알려진 GitHub/JWT 모양에 우연히 맞아 추가 치환되는 값만으로 일반 Bearer 보호를 보장할 수 없다.

**권고:** Authorization 전체 값의 처리를 일반 key/value 처리보다 먼저 하거나 구조별로 처리한다. JSON 형태는 민감 필드를 구조적으로 지우는 방식 등을 적용하고, 임의 원본 출력을 공유 로그에 그대로 합치지 않는다. 값의 일부·토큰 prefix에 의존하지 않는 회귀 사례를 추가한다. 문자열 마스킹 후 JSON을 직렬화하고 숫자 종료코드는 숫자로 보존하는 기존 방식은 유지한다.

**영향 범위 재검증:** `Protect-Text`, `ConvertTo-SafeResultsJson`, 오류 요약 경로. Bearer 헤더·일반 Bearer·따옴표 JSON·일반 password·GitHub/JWT 형식·query string·한글/공백 사용자 경로를 PowerShell 5.1/7에서 확인한다. 실제 인증정보는 사용하지 않는다.

### IVA-B002 — WinGet 재부팅 필요 HRESULT를 일반 실패로 처리

- **Severity:** MEDIUM / P2
- **파일:** `scripts/install-packages.ps1:20–28`, `scripts/common.ps1:59–63`; 관련 mock은 `tests/run.ps1`.
- **요구 근거:** 계획 §6.4의 `REBOOT_REQUIRED`, 전체 종료코드 2, 원래 종료코드 보존과 §6.5의 수동 재부팅·복구 경계.
- **증거 수준:** exact source + Microsoft WinGet 공식 return-code 정의·실행 흐름 + 입력값에 대한 분기 모델. 실제 WinGet/설치기 실행은 NOT_RUN.

실제로 호출하는 프로세스는 `winget.exe`인데 재부팅 분기는 원시 MSI 값 `3010`, `1641`만 처리한다. Microsoft의 공식 WinGet 값 중 설치 전 재부팅이 필요한 경우는 `APPINSTALLER_CLI_ERROR_INSTALL_REBOOT_REQUIRED_FOR_INSTALL`, `0x8A15010A` / signed `-1978334966`이다. 이 값을 넣으면 현재 코드의 결과는 다음과 같다.

```text
winget ExitCode = -1978334966
현재 결과       = FAILED / 전체 종료코드 1
요구 상태       = 재부팅 필요 안내 / 전체 종료코드 2
```

이 결과는 오류 안내를 일반 재시도로 돌리고 재부팅 필요 상태를 기계적으로 구분하지 못하게 한다. 기존의 `3010` mock 통과만으로 실제 WinGet 경계가 검증된 것은 아니다.

**중요한 구분:** 조사한 공식 WinGet 소스는 `RebootRequiredToFinish`를 내부 성공으로 취급하는 별도 경로도 갖는다. 모든 재부팅 상황이 항상 `0x8A150109`로 끝난다고 단정하지 않는다. 본 finding의 구체적인 반례는 `0x8A15010A`이며, 지원할 WinGet 버전의 실제 동작에 맞춰 분류해야 한다.

**권고:** 지원 WinGet 버전의 HRESULT·signed/unsigned 표현을 정규화해 재부팅·사용자 조치·일반 실패를 분류한다. 자동 재부팅은 계속 금지한다. WinGet 프로세스 종료코드와 하위 설치기 종료코드는 가능한 경우 구별하고, 모르는 하위 코드를 추정해 채우지 않는다.

**영향 범위 재검증:** 해당 함수와 전체 종료코드 집계, `0x8A15010A`, 지원 버전에서 관측되는 완료 후 재부팅 경로, 취소·일반 실패·후속 탐지 실패. 먼저 PowerShell mock에서 확인하고 실제 설치 확인은 승인된 Windows 실환경 시험에서 수행한다.

### IVA-W001 — 부분 설정·금지 키 설정을 `/notes`에서 DEMO로 오표시

- **Severity:** MEDIUM / P2
- **파일:** `src/app/notes/page.tsx:9–10`; 관련 `src/lib/env.ts`.
- **요구 근거:** 계획 §7.2의 설정 없는 DEMO와 실제 연결 구분, 패킷의 부분 설정 fail-closed와 기본 화면·오류 처리.
- **증거 수준:** exact TS/TSX를 바꾸지 않고 메모리에서 transpile하여 실행한 소스 수준 재현. Next.js 서버·브라우저 실행은 아니다.

환경 파서는 설정을 `demo`, `invalid`, `ready`로 구분하지만 메모 화면은 `config.kind !== 'ready'`인 모든 경우를 DEMO 화면으로 반환한다.

| 재현 입력 | 환경 파서 | `/notes` 표시 | DB 호출 |
|---|---|---|---|
| URL·키 모두 없음 | demo | DEMO | 0 |
| URL만 있고 키 없음 | invalid | DEMO, 오류 원인 없음 | 0 |
| 공개 키 자리에 `sb_secret_…` 형태의 가짜 값 | invalid | DEMO, 오류 원인 없음 | 0 |

**데이터 접근은 차단된다. 이 finding은 인증 우회가 아니라, 잘못된 설정을 정상 체험 모드처럼 보이게 하는 상태·복구 UX 결함이다.** 초기 화면의 별도 오류 처리나 순수 환경 파서 단위 검사만으로 `/notes`의 오표시를 잡지 못한다.

**권고:** `demo`일 때만 DEMO를 보여 주고 `invalid`는 비밀 값 없이 설정 오류와 복구 경로를 표시하거나 설정 페이지로 이동시킨다. 설정 없음의 DEMO와 오류 상태를 각 페이지에서 동일한 계약으로 처리한다.

**영향 범위 재검증:** `/notes`, 공통 설정 상태 처리, 관련 환경/화면 테스트. URL만 있음·키만 있음·잘못된 URL·허용하지 않는 키 형식·정상 무설정 DEMO를 확인하고, invalid 상태의 CRUD 호출 차단은 계속 유지한다.

### IVA-W002 — 로그아웃 API의 반환 오류를 무시

- **Severity:** MEDIUM / P2
- **파일:** `src/app/login/actions.ts:22–24`; 같은 오류 반환을 고려할 관련 경로 `:16–18`.
- **요구 근거:** 계획 §7.3의 로그인 실패·세션 만료·로그아웃 상태 검증, 패킷의 서버 인증과 오류 처리.
- **증거 수준:** exact Server Action 코드를 실행하되 Supabase client는 오류를 반환하는 mock으로 대체. SDK의 반환 계약은 Supabase 공식 문서에서 확인했다.

`await client.auth.signOut()`의 반환값을 읽지 않고 항상 `/login`으로 이동한다. mock client가 `{error: {name: 'AuthApiError', status: 500, ...}}`를 반환해도 같은 이동을 수행하고 실패를 알리지 않는 것을 재현했다. 공식 문서의 호출 예도 반환값에서 `error`를 받도록 되어 있다.

사용자는 로그인 화면으로 이동한 사실만으로 로그아웃이 완료됐다고 오해할 수 있다. **실제 쿠키가 남는지, 실제 Supabase HTTP 오류에서 세션이 어떻게 정리되는지는 이번에 실행하지 않았으므로 확정하지 않는다.** 확인한 결함은 실패 결과를 버리는 분기다. JWT 자체의 만료/폐기 의미와 UI 이동은 구분해야 한다.

**권고:** 반환 오류와 예외를 처리하고, 실패·복구 안내 및 로컬 세션 정리의 정책을 명시한다. SDK logout scope도 의도와 일치하게 정한다. 비승인 사용자 로그인 후 정리 경로에서도 같은 실패 처리를 점검하되, `requireOwner`의 승인 사용자 검사는 유지한다.

**영향 범위 재검증:** sign-out 성공, SDK 반환 error, throw, 비승인 사용자 정리 실패를 소스/Server Action 수준에서 확인한다. 실제 계정 환경에서는 로그아웃 후 쿠키 상태와 보호된 `/notes` 재접근을 별도로 검사한다.

## 4. 확인한 정상 경계와 잔여 범위

| 구간 | 확인 결과 | 증거의 한계 |
|---|---|---|
| Plan / Verify | 소스에서 설치 호출 이전에 반환; Install에 동의 경로 존재 | 실제 Windows 시스템 변경 전후 비교는 미실행 |
| 기존 설치 보존 | 호환 설치 SKIP, 비호환 설치 KEEP_EXISTING, `--no-upgrade` 확인 | 레지스트리·실행파일 탐지와 기존 PC 재실행은 실기 미확인 |
| 보안 설정 | 프로세스 범위 BAT 옵션, 영구 정책 변경·자동 재부팅을 수행하는 경로는 검토 범위에서 확인하지 못함 | 설치기 자체 동작까지 증명하지 않음 |
| Keyless DEMO | 소스 재현에서 DB 호출 0, 저장 불가 표시 | invalid의 DEMO 오표시는 W001 |
| 승인 사용자 | 허용 ID 없음, claims 오류, 다른 subject에서 각 차단 분기 재현 | JWT 암호학적 검증 자체는 이번 mock의 범위 밖 |
| CRUD 소유권 | 클라이언트가 보낸 owner_id 무시, 서버 사용자 고정; invalid ID 차단, zero-row 삭제 오류 처리 확인 | 실계정 HTTP CRUD는 미실행 |
| RLS / Storage | 소유자별 정책·private bucket·경로 소유권 제한 소스 검토; 작성자 SQL fixture 성공 재사용 | 실제 Supabase Auth/Storage HTTP 통합은 미실행 |
| 키 경계·가입 | 공개 키 검증과 서버 허용 사용자 값 분리, 공개 가입 UI 없음; 운영 문서에서 signup 제한 절차 안내 | 실제 프로젝트 설정·키·signup 차단 상태는 미확인 |
| 재현성 | package-lock과 `npm ci` 사용, 해당 후보 CI 성공 확인 | 모든 OS/Node patch의 지원 또는 현재 보안 무결성을 인증하지 않음 |
| 구현 주장 | 작성자 완료보고가 Windows runner/mock, SQL fixture, 실제 환경과 미구현 사진 앱을 구분 | 원래 handoff ZIP 자체의 동일성은 아래 별도 한계 |

DB RLS가 사용자 A/B를 각자 자기 데이터로 나누는 것과 애플리케이션의 `APP_ALLOWED_USER_ID` 단일 사용자 제한은 서로 다른 경계다. 문서에 이 구분이 있으므로, RLS가 모든 authenticated 사용자의 자기 데이터 작업을 허용한다는 사실만으로 애플리케이션 승인 우회 결함이라고 판정하지 않았다.

### 보조 관찰 — 교정 4건과 구분

- **설치 전 요약 UX:** `bootstrap.ps1`은 Install 동의 전에 선택 패키지 이름을 보여 주지만, 개별 설치 상태 탐지는 뒤의 `Invoke-PackageStep`에서 수행한다. 계획 §6.2의 탐지 결과를 포함한 사전 설치 계획과는 차이가 있다. 별도 Plan 실행은 가능하다. 기본 더블클릭 경로에서 `유지/설치 예정/조치 필요`까지 한 번에 보여 주는 개선을 권고한다. 이것을 추가 보안 사고로 확대하지 않는다.
- **Windows 첫 시작 회귀:** 문서의 `npm ci` 경로를 실제 Windows PowerShell 실행정책·한글/공백 경로에서 확인해야 한다. `npm.ps1`이 정책으로 차단되는 환경이라면 영구 정책 완화 대신 `npm.cmd` 또는 문서화한 명령 프롬프트 경로를 제공하는 방법을 검토한다. 해당 사용자 PC에서 차단된다는 진단이나 실기 재현 주장은 아니다.
- **공급 버전:** Node 설치 pin 24.19.0과 CI에서 사용한 patch가 다른 사실은 작성자 문서에 명시돼 있다. 이번 검증에서 모든 WinGet manifest·확장 publisher·보안 patch의 현재성을 다시 검증한 것은 아니다. 배포 전 지원 조합·공급 패키지의 현행성 확인이 남는다.

## 5. 재사용한 증거와 이번 실행

### 5.1 재사용 — 작성자 CI이며 IVA 신규 실행이 아님

| 저장소 / run | Job | 직접 조회한 결과 |
|---|---|---|
| bootstrap / 34939006314 | powershell-51 | success, parser/mock |
| bootstrap / 34939006314 | powershell-7 | success, parser/mock/패키징 |
| web-starter / 34939036421 | web | success, npm ci/check/build/DEMO browser/의존성 audit/패키징 |
| web-starter / 34939036421 | windows-node | success, npm ci/check |
| web-starter / 34939036421 | rls-fixture | success, PostgreSQL A/B/anonymous SQL fixture |

작업별 step과 head를 GitHub API에서 읽었다. 전체 CI 재실행·새 workflow dispatch·재검증 루프는 수행하지 않았다. Windows runner는 깨끗한 Windows 11 사용자 환경이 아니며, SQL fixture는 Supabase HTTP 통합이 아니다. 과거 audit 성공이 모든 보안 문제 부재를 뜻하지 않는다.

### 5.2 이번 IVA가 새로 실행

| 실행 | 결과 | 정확한 범위 |
|---|---|---|
| 두 CI artifact SHA256와 소스 Git tree 계산 | exact tree 일치 | ZIP 다운로드·정적 추출·해시 계산 |
| Web 소스 수준 하네스 10개 시나리오 | 정상 방어 경로 7개 확인, 결함 경로 3개 재현 | unchanged TS/TSX + mocked Next/React/Supabase 경계; 3개 결함 경로는 W001 두 사례와 W002 한 사례 |
| Bootstrap 모델 10개 시나리오 | 대조 사례 7개 일치, 결함 사례 3개 재현 | 소스 정규식·재부팅 코드 목록을 추출한 Python 모델; 3개 결함 사례는 B001 두 사례와 B002 한 사례 |
| 검토 후 소스 tree 재계산 | 변경 없음 | 원본 72개 파일의 동일성 재확인 |

환경은 Linux, Node `22.16.0`, Python `3.13.5`, 하네스의 TypeScript `5.8.3`이다. 프로젝트 lockfile의 TypeScript 버전으로 새 build를 수행한 것이 아니다. 실제 프로젝트의 설치·빌드 증거는 작성자 CI에서 재사용했다. 컨테이너에 Windows PowerShell/WinGet/실제 Supabase 실행환경이 없으며 새로운 계정·키·시스템 패키지를 설치하지 않았다.

**하네스의 종료코드 0은 제품 PASS가 아니다.** 이 파일들은 현재 후보에서 결함이 재현되는지 확인하는 reproducer다. 수정 후 회귀 검사로 편입할 때는 정상 기대값으로 assertion을 전환해야 한다. 일반 테스트를 더 돌려 PASS를 얻는 대신 각 finding의 원인이 실제로 제거됐는지를 검사한다.

### 5.3 ZIP byte identity 주의

아래는 이번에 다운로드한 **최종 PR CI** artifact의 값이다.

| 항목 | Bootstrap SHA256 | Web Starter SHA256 |
|---|---|---|
| 외부 artifact ZIP | `6dda21c24ea6f1f33a6513237723246cb040bb30cadaa487e7efd8661daf6bc2` | `547d0f2a9f860477e340edd3cde01f38a0a09f0909c381a2338a2ea792c7b2cc` |
| 내부 candidate ZIP | `4c7aa5c307e01f82a9641b3aae5d0bc487746d447c09693a38f3d1b30eb090d4` | `46e86086eda57f4189996b339fde33445100733b226a0e1a5e440fe3fc61416c` |

운영 manifest에 적힌 handoff ZIP SHA256은 각각 `94737a28c6ea41b359e277af21d4038ba8c860d915440016171d141e78ea4b95`, `580f088d78b4571b931186dc57234f12cf62b4b0444d6b6e6957c970eb0a55d9`이며 위 값과 다르다. 그 별도 handoff ZIP 바이트는 이번 입력에 첨부되지 않았고 직접 확보하지 않았으므로 **그 ZIP 자체의 독립 checksum 검증은 NOT_RUN**이다. 어느 선행 run의 ZIP이라고 추측해 채우지 않는다.

검토한 CI 후보의 evidence commit은 PR 테스트용 merge commit `51aa63693a2833217163bad59328a8efc5bf754b` / `14696ef50e8f39966fb8c94ea61c5f58f49b884e`로 기록되어 있다. 하지만 추출한 실제 소스 tree는 두 고정 head의 tree와 일치한다. 따라서 이 차이를 소스 바꿔치기나 제품 병합 증거로 해석하지 않는다. 향후 handoff에는 실제 전달 ZIP의 artifact/run 식별자와 SHA256을 함께 고정하는 것을 권고한다.

## 6. NOT_RUN / INDETERMINATE

| 항목 | 이번 상태 | 필요한 증거 |
|---|---|---|
| 깨끗한 Windows 11 x64 ZIP 다운로드·해제·BAT 실행·UAC·WinGet 설치 | NOT_RUN | 승인된 실기/VM에서의 결과 |
| 기존 사용자 PC의 보존·충돌·PATH·한글/공백 경로·재실행 | NOT_RUN | 설치 전후 비교와 Verify 결과 |
| 새 Bootstrap regression의 실제 PowerShell 5.1/7 실행 | NOT_RUN | B001/B002 수정 범위의 target-runtime 결과 |
| 실제 AI 계정 로그인 | NOT_RUN | 사용자가 직접 수행하는 계정 확인; 비밀번호/토큰 제출 불필요 |
| 실제 Supabase A/B Auth·HTTP CRUD·Storage·세션 만료·로그아웃 | NOT_RUN | 테스트용 승인 환경의 실제 계정/HTTP 결과 |
| 실제 Supabase 공개 가입 차단·private bucket 운영 상태 | NOT_RUN | 해당 프로젝트 설정 readback/시험 |
| 이번 IVA의 전체 npm ci/build/브라우저 재실행 | NOT_RUN | 기존 성공 CI 재사용; 불필요한 전역 재실행 요구 없음 |
| 모든 공식 설치 패키지/확장과 Node 보안 patch 현행성 | 이번 검증에서 전수 확인하지 않음 | 배포 시 지원 버전·공급 manifest 확인 |
| 작성자가 별도 전달한 원 handoff ZIP 바이트 | NOT_RUN | 정확한 ZIP 또는 artifact/run 식별자 |
| 사진 게시 앱·실제 SNS·예약 실행 | 이번 B/W 검증 대상 밖 | 미구현/미실행 상태 유지, 별도 제품 단계 |

NOT_RUN은 실패를 감추기 위한 PASS가 아니며, 환경이 없다는 이유만으로 재현되지 않은 결함을 FAIL이라고 만들지도 않는다. 해당 실사용 적합성은 INDETERMINATE다.

## 7. MITCHELL에 반환할 교정 묶음

이 보고서 수신만으로 아래 작업이 실행 승인된 것은 아니다. Owner가 교정 묶음을 승인한 경우 작성자가 수행하고, 새로운 head/tree를 고정한 뒤 영향을 받은 부분만 재검증한다.

| 묶음 | 최소 교정 대상 | 작성자 완료 시 필요한 결과 | IVA 재검증 경계 |
|---|---|---|---|
| B-CORRECTION | B001 마스킹, B002 WinGet 상태 분류, 관련 tests/복구 안내 | PowerShell 5.1/7의 표적 사례; 원본/공유 로그 값 제거; 전체 종료코드 일치 | common/install-packages와 수정에 연결된 결과 집계·출력 경로 |
| W-CORRECTION | W001 invalid 화면, W002 sign-out 오류·정리 경로, 관련 tests | 정상 DEMO 유지, invalid 복구 안내, 실패 로그아웃을 성공과 구분, 소유권 경계 유지 | notes 상태 분기와 login actions, 해당 인증/환경 회귀 |

동일 원인의 무정보 반복이나 전체 프로젝트 재검토는 요구하지 않는다. 기존 성공 증거는 소스·환경이 유효한 범위에서 보존한다. 의존성·RLS·다른 영역을 변경하면 그 변경으로 실제 영향을 받는 부분에 한해 재검증 범위를 추가한다. 실제 환경 수락과 최종 병합/릴리스는 별도 gate다.

## 8. 외부효과·보고서 보존·사용자 행동

수행한 외부 접근은 GitHub 소스·PR·CI·artifact 및 공식 문서 **읽기**다. 로컬에는 후보 추출본, 검증 하네스, 가짜 입력의 결과 JSON, 본 보고서와 증거 ZIP만 작성했다. 제품 소스 수정, Git commit/push, PR 댓글·상태 변경, workflow 실행, 병합, template 활성화, 사용자 PC 설치, 계정/키 발급, Cloud 설정 변경, 공개 게시, 배포, 결제, 자동화 활성화는 수행하지 않았다.

`PMO_RUNTIME = NOT_DISPATCHED`. 추가 Persona/페어 검증자는 실행하지 않았다. `IVA_READ_ONLY_REVIEW = COMPLETED`이며 이 대화에서 실제로 수행한 검증에만 해당한다. 작성자 Git의 과거 `IVA NOT_RUN`을 자동으로 갱신하지 않았다. 운영 저장소 반영과 작성자 채널 수신 확인은 **NOT_DONE**이다.

**필요한 사용자 행동 한 가지:** 본 보고서를 MITCHELL 작성자 채널에 전달하고, 진행할 경우 “B001/B002/W001/W002 4건 교정 및 해당 영향 범위 재검증”이라는 묶음으로 승인한다. 새 저장소·키·비밀번호 제출은 이 반환 단계에 필요 없다.

## 9. 근거 위치

아래 저장소 식별자와 경로는 본문 판정의 원천이다. Git 문서를 사용자 새 권한으로 취급하지 않았다. 외부 공식 자료는 WinGet 반환 코드와 Supabase API 반환 계약을 확인하는 데 사용했으며 사용자 요구사항을 외부 기준으로 바꾼 것은 아니다.

- **S01 / 입력:** 첨부 `MITCHELL_IVA_REVIEW_PACKET_v0.1(1).md`, packet `MITCHELL-IVA-BW-001`.
- **S02 / 운영:** `AofSpds/mitchell@ca07ffa3ec9e162583293e3a9e33317220464d77`의 `README.md`, `CURRENT.md`, `AGENTS.md`, `DECISIONS.md`, `docs/IMPLEMENTATION_PLAN_v1.0.md`, `docs/execution/EXECUTION_20260915.md`, `COMPLETION_20260915.md`, `CANDIDATE_MANIFEST_20260915.json`.
- **S03 / Bootstrap:** 위 exact head의 `scripts/common.ps1` blob `4456aa57727962bb33cbf8a89df9b133e9484e62`, `scripts/install-packages.ps1` blob `8de6af212514ef787c44e88931b53c96a39d3418`, 그 외 원본은 `source_file_inventory.json`.
- **S04 / Web:** 위 exact head의 `src/app/notes/page.tsx` blob `0c28b892ccfa6e3102521c7c4d2ab55b54fe480c`, `src/app/login/actions.ts` blob `bd04b19fe71a1b24d77c93f20e9f558cbc7959e3`, 그 외 원본은 `source_file_inventory.json`.
- **S05 / CI:** GitHub API의 두 run `/jobs`, `/artifacts`; 두 PR `/pulls/1`; 두 exact head `/git/commits/{sha}`. 원래 artifact ZIP도 증거 묶음에 포함했다.
- **S06 / WinGet 공식:** `microsoft/winget-cli@5b62860167520b1503b3880d5a026809eb07c6f4`, `src/AppInstallerCLICore/Workflows/InstallFlow.cpp`의 ExpectedReturnCode 매핑 및 약 493–570행 ReportInstallerResult, `src/AppInstallerSharedLib/Public/AppInstallerErrors.h`의 `0x8A15010A` 정의, `doc/windows/package-manager/winget/returnCodes.md`. 이는 source/API 계약 자료이며 실제 설치 증거는 아니다.
- **S07 / Supabase 공식:** `https://supabase.com/docs/reference/javascript/auth-signout` — 반환 `error` 및 scope 설명. 접근일 2026-09-15.
- **S08 / Microsoft 공식 참고:** `https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies` — 정책 범위와 스크립트 차단 의미. 특정 사용자 PC의 현재 정책 값은 미확인.
- **S09 / IVA 자체 증거:** `source_tree_verification.json`, `source_file_inventory.json`, `artifact_integrity.json`, `web_reproduction_results.json`, `bootstrap_model_results.json`, 실행 코드 `review_harness.cjs`, `bootstrap_model_checks.py`, `verify_sources.py`.
