# MITCHELL Bootstrap·Web Starter affected-only IVA 재검증 결과

- 문서 ID: `MITCHELL-IVA-BW-001-REREVIEW-RESULT`
- 버전: `1.0`
- 작성일: `2026-09-15`
- FROM: `IVA`
- TO: `MITCHELL`
- PROJECT: `MITCHELL`
- 입력: `MITCHELL-IVA-BW-001-REREVIEW` v0.2.1
- 최초 결과: `docs/execution/IVA_REVIEW_RESULT_20260915.md`
- 상태: `AFFECTED_ONLY_REREVIEW_COMPLETED / ALL_FOUR_FINDINGS_PASS`
- 실행 환경: GitHub connector read-only 조회 + 격리 Linux 컨테이너의 fixture-only 소스 하네스
- 제품 코드 수정·PR 병합·main 반영·배포·계정 변경: `NOT_DONE`

## 1. 결론

| 항목 | 재검증 판정 | 결론 |
|---|---|---|
| `IVA-B001` 로그 마스킹 | **PASS** | 최초 누락이 교정됐다. 지정된 fixture 범위에서 민감 값이 남지 않고 숫자 종료코드가 유지된다. |
| `IVA-B002` WinGet 재부팅 종료코드 | **PASS** | signed/unsigned WinGet HRESULT와 MSI 코드를 사용자 재부팅 조치로 분류하고 일반 실패와 구분한다. |
| `IVA-W001` invalid 설정의 DEMO 오표시 | **PASS** | 완전 무설정만 DEMO이며 invalid 설정은 복구 화면으로 종료되고 인증·CRUD 경로를 호출하지 않는다. |
| `IVA-W002` 로그아웃 오류 무시 | **PASS** | local scope 성공, SDK 반환 error, throw와 비승인 사용자 cleanup 실패가 각각 구분된다. |
| 새 finding | **NONE** | 교정으로 인해 실질적으로 새로 발생한 affected-path 결함을 확인하지 못했다. |
| 실제 Windows·Supabase 통합 | **NOT_RUN / INDETERMINATE 유지** | 이번 affected-only 소스 재검증으로 실제 환경 수락 시험을 대체하지 않는다. |

```text
AFFECTED_ONLY_REREVIEW = PASS
MERGE_RECOMMENDATION = PASS
RELEASE_DEPLOY_RECOMMENDATION = HOLD
```

`MERGE_RECOMMENDATION = PASS`는 아래 exact corrected candidates가 최초 네 finding에 대한 독립 재검증 gate를 통과했다는 뜻이다. 깨끗한 Windows 설치, 실제 Supabase 세션·쿠키·Storage, Template 활성화, 릴리스 또는 배포의 PASS를 뜻하지 않는다. 최초 후보에 대한 FAIL 기록도 삭제하거나 대체하지 않는다.

## 2. exact 재검증 대상과 최종 ref

| 항목 | Bootstrap | Web Starter |
|---|---|---|
| Repository | `AofSpds/bootstrap` | `AofSpds/web-starter` |
| Branch / PR | `work/bootstrap-v0.1` / `#1` | `work/web-starter-v0.1` / `#1` |
| Exact head | `f880d0297e4d092c0f7d5025ff42cc7648fb66aa` | `a2c63cca6bdbf0667ba2d9e1c8d8e7af2e137a17` |
| Exact tree | `2a28a91f8fdbbbc80bd37209206cdab9cf5e715a` | `34716ab2e0f82f625f6f6ada1cf0c205d665da51` |
| Base main | `351333d6b4e6db1635c8caeb0f7dd8c4e3aee72c` | `0d855cd9aaf1ee8ebf31a2a7af6d67c90367d0d6` |
| 최종 상태 | `OPEN / DRAFT / UNMERGED` | `OPEN / DRAFT / UNMERGED` |
| Merge test ref | `327bed470cfe8eeffd9d07c41eb5ec4efc04cc92` | `ef61f2f6f84bce0b591da06c4ba5f37c6b69a19d` |
| Merge test tree | exact head tree와 동일 | exact head tree와 동일 |

재검증 종료 전 PR을 다시 조회하여 head가 입력 패킷과 동일하고 병합되지 않았음을 확인했다. GitHub Actions가 PR merge ref를 checkout했지만 두 merge ref의 tree가 각각 exact branch head tree와 동일하므로, 실행된 소스 내용은 지정된 candidate tree와 동일하다. commit identity와 source tree identity를 혼동하지 않는다.

Bootstrap의 실질 교정 head는 `33b1b7e6a788d04ba61acd6c689bb5f20245116c`다. 이후 placeholder 생성·삭제 bookkeeping commits가 존재하나 현재 head와 실질 교정 head의 file diff는 0이며 tree가 같다. 이 이력은 숨기거나 reset하지 않았다.

## 3. source artifact 독립 동일성 확인

| 항목 | Bootstrap | Web Starter |
|---|---|---|
| Author CI run | `34972848451` | `34970348151` |
| Artifact ID | `10397977572` | `10396488405` |
| Outer artifact SHA-256 | `63218f527893dc5a311151f9fe354dd2839b5ca9525a19798dd4b1e4bcadf74f` | `416622c3d094662098722ff882f7a57bbbababa062c61705a259d4864f6b8be0` |
| Inner candidate ZIP SHA-256 | `7f9dcce619f0e403cb2b23dec7783a3f9c5a96a1b8cf9535a6e97d91f928735b` | `a8f85ab275c718727099136b235cf5bd7fa57362f1be7304a7f52368ea34c4df` |
| Source files | `19` | `57` |
| ZIP path safety | absolute path·`..` 없음 | absolute path·`..` 없음 |
| 독립 계산 tree | `2a28a91f8fdbbbc80bd37209206cdab9cf5e715a` | `34716ab2e0f82f625f6f6ada1cf0c205d665da51` |
| 결과 | exact match | exact match |

Bootstrap은 Windows checkout artifact의 CRLF를 Git blob 기준 LF로 계산할 11개 text file에만 정규화를 적용했고 `bootstrap.bat` 등 나머지 파일은 원본 바이트를 사용했다. Web Starter는 추출 원본 바이트를 그대로 사용했다. 두 경우 모두 Git blob/tree를 독립 재계산하여 입력 tree와 일치했다.

## 4. 증거 구분

### 4.1 재사용·직접 열람한 작성자 실행 증거

새 candidate exact tree에서 수행된 작성자 CI를 다시 실행하지 않고 로그를 직접 열람했다.

**Bootstrap run `34972848451`**

- Windows PowerShell 5.1: success
- PowerShell 7: success
- 두 job 모두 parser 및 mocked regression에서 `TOTAL_PASS=44`, `ADDITIONAL_TESTS_PASS=18`
- 로그에 Authorization/Bearer 정제, 사용자 경로 정제, signed/unsigned WinGet HRESULT, 일반 실패 구분, 수동 재부팅 분기가 각각 PASS로 기록됨

**Web Starter run `34970348151`**

- Linux `web`: lint, typecheck, 28 unit tests, build, 3 DEMO browser tests, production dependency audit success
- Windows `windows-node`: lint, typecheck, 28 unit tests, build success
- `rls-fixture`: PostgreSQL fixture에서 `RLS_FIXTURE_TESTS_PASS`
- unit 결과 중 `auth-session.test.ts` 3건과 `notes-configuration-state.test.ts` 6건이 success

이들은 작성자 실행 증거이며 IVA가 실행한 별도 CI로 표현하지 않는다. 다만 exact candidate tree와 동일한 source tree에서 실행됐기 때문에 affected-only 판단에 유효하게 재사용했다.

### 4.2 IVA가 새로 실행한 독립 증거

#### Artifact/source identity

- 두 artifact를 안전하게 해제했다.
- outer/inner SHA-256, source file count, 경로 안전성을 독립 계산했다.
- 각 파일을 Git blob으로 계산해 exact tree를 재구성했다.

#### Bootstrap fixture-only semantic check

격리 컨테이너에는 PowerShell runtime이 없어 IVA가 PowerShell 5.1/7을 새로 실행하지는 못했다. 대신 exact source의 정규식 순서·종료코드 연산·제어 흐름을 fixture-only Python semantic model과 정적 assertion으로 독립 검증했고, actual PowerShell 동작은 위 exact-tree 작성자 CI 로그로 교차 확인했다.

새 독립 fixture:

- Authorization: Bearer
- standalone Bearer
- quoted JSON authorization/password
- 일반 password, `api_key`, `api-key`
- GitHub/JWT 형태
- query string
- 한글·공백 Windows 사용자 경로
- typed numeric exit code
- MSI `3010`/`1641`
- WinGet signed `-1978334966`, unsigned `2316632330`
- 일반 실패 `99`

모든 입력은 가짜 값이며 실제 인증정보·사용자 로그는 사용하지 않았다.

#### Web exact-source Node/TypeScript harness

Node.js 격리 하네스가 artifact의 exact TypeScript/TSX를 변환·실행하고 Next.js·Supabase 외부 경계만 fixture stub으로 대체했다.

- config state 6개: absent, URL-only, key-only, invalid URL, forbidden key, ready
- `/notes` gate 6개: invalid 4개와 demo에서 `requireOwner=0`, DB `from=0`; ready에서 각각 1회
- `endLocalSession`: success/error/throw 모두 `scope: local` 호출을 확인
- logout route: success, SDK error, provider throw, client creation throw, not-ready
- unauthorized cleanup: success, SDK error, throw
- 인증·소유권·RLS 관련 기존 4개 핵심 파일의 old/new SHA-256이 동일함을 확인

이 하네스는 실제 Supabase 네트워크·쿠키·Storage 통합 시험이 아니다.

## 5. finding별 판정

### IVA-B001 — PASS

**검증 경로**

- `scripts/common.ps1:3-17`
- `scripts/common.ps1:88-100`
- `scripts/install-packages.ps1:27-28`
- `tests/run.ps1:31-32`
- `tests/logs-and-catalogue.ps1:8-27`

**확인 결과**

1. quoted JSON 민감 필드와 Authorization 전체 값 처리가 standalone Bearer·일반 key/value 처리보다 앞선다.
2. `Authorization: Bearer fixture_access_value`는 `Authorization: <redacted>`가 되어 값이 남지 않는다.
3. standalone Bearer, quoted JSON authorization/password, 일반 password/api key, GitHub/JWT, query string, 한글·공백 사용자 경로에서 지정 fixture가 남지 않는다.
4. `ConvertTo-SafeResultsJson`은 문자열만 정제하고 `installerExitCode=99`를 숫자로 유지한다.
5. 오류 `nextAction` 유입 경로도 최종 detail에 `Protect-Text`를 적용한다.

최초 finding의 재현 조건이 해소됐다.

### IVA-B002 — PASS

**검증 경로**

- `scripts/common.ps1:61-85`
- `scripts/install-packages.ps1:11-28`
- `bootstrap.ps1`의 최종 종료코드·안내 경로
- `tests/run.ps1:33-38, 69-77`
- `tests/logs-and-catalogue.ps1:29-33`

**확인 결과**

1. 음수 Win32/HRESULT는 `2^32`를 더해 unsigned 값으로 정규화한다.
2. `3010`, `1641`, `-1978334966`, `2316632330`은 `REBOOT_REQUIRED`다.
3. 일반 실패 `99`는 재부팅 필요로 오분류하지 않는다.
4. 재부팅 코드는 post-install 검사보다 먼저 분기하며 원 raw 종료코드를 결과에 보존한다.
5. 전체 결과는 `REBOOT_REQUIRED` 또는 `ACTION_REQUIRED`가 있으면 종료코드 `2`다.
6. 안내는 수동 재부팅 후 Verify이며, installer 인수에 `--allow-reboot`가 없고 제품 실행 경로에 `Restart-Computer`·shutdown 호출이 없다.

최초 finding의 WinGet 반환 경계가 교정됐다.

### IVA-W001 — PASS

**검증 경로**

- `src/lib/env.ts:8-26`
- `src/components/notes-configuration-state.tsx:6-45`
- `src/app/notes/page.tsx:12-24`
- `tests/unit/notes-configuration-state.test.ts`

**확인 결과**

| 입력 | 판정·표시 | `requireOwner` | DB `from` |
|---|---|---:|---:|
| URL·key 모두 없음 | DEMO | 0 | 0 |
| URL-only | invalid 복구 화면 | 0 | 0 |
| key-only | invalid 복구 화면 | 0 | 0 |
| invalid URL | invalid 복구 화면 | 0 | 0 |
| forbidden key | invalid 복구 화면 | 0 | 0 |
| 정상 ready | private workspace | 1 | 1 |

invalid 화면은 DEMO 문구를 포함하지 않고 설정 오류·실제 저장 차단·`/setup` 복구 링크를 표시한다. 인증/CRUD 경계 전에 반환하므로 잘못된 설정에서 데이터 경로로 진입하지 않는다.

### IVA-W002 — PASS

**검증 경로**

- `src/lib/auth-session.ts:1-18`
- `src/app/login/actions.ts:10-48`
- `src/app/login/page.tsx:11-32`
- `src/app/notes/page.tsx:25-32`
- `tests/unit/auth-session.test.ts`

**확인 결과**

| 사례 | 결과 경로 |
|---|---|
| local sign-out 성공 | `/login?status=signed-out` |
| SDK 반환 error | `/notes?error=logout` |
| provider throw | `/notes?error=logout` |
| client creation throw | `/notes?error=logout` |
| 비승인 사용자 cleanup 성공 | `/login?error=not-allowed` |
| 비승인 사용자 cleanup error/throw | `/login?error=not-allowed-cleanup` |

`endLocalSession`은 `signOut({ scope: 'local' })`을 호출하고 반환 `error === null`일 때만 성공으로 처리한다. 현재 브라우저 세션만 종료하려는 의도와 Supabase JavaScript 공식 sign-out 계약이 일치한다.

소유권/RLS 약화 여부는 affected-only로 확인했다. 다음 파일은 최초 후보와 corrected candidate에서 바이트 SHA-256이 동일했다.

- `src/lib/auth.ts`
- `src/app/notes/actions.ts`
- `supabase/migrations/202609150001_notes_and_private_storage.sql`
- `tests/sql/rls.test.sql`

새 exact tree의 RLS fixture도 성공했다. 따라서 이번 로그아웃 교정이 승인 사용자·owner filter·RLS 정책 경계를 약화했다는 증거는 발견하지 못했다.

## 6. 새 finding

`NONE`.

affected-only 변경 경로와 직접 영향 경로에서 추가 교정이 필요한 재현 가능한 결함을 확인하지 못했다. 이 판정은 전체 제품의 미검증 영역에 결함이 없다는 포괄 보증이 아니다.

## 7. 유지되는 NOT_RUN / INDETERMINATE

다음은 최초 검증과 마찬가지로 이번 재검증에서도 실행하지 않았다.

- 깨끗한 Windows 11 x64의 ZIP 다운로드·압축 해제·UAC·실제 WinGet 설치
- 실제 재부팅 필요 상황과 재실행·Verify
- 기존 사용자 PC의 설치 보존·PATH·한글/공백 경로
- 실제 AI 계정 로그인
- 실제 Supabase Auth 계정 A/B, HTTP CRUD, private Storage
- 실제 브라우저 cookie/session의 logout 후 보호 페이지 재접근
- Template 활성화, 제품 main 병합, 릴리스, 배포
- 사용자 PC·Cloud·SNS 변경
- PMO runtime dispatch

따라서 `LIVE_WINDOWS_ACCEPTANCE`와 `LIVE_SUPABASE_ACCEPTANCE`는 `NOT_RUN / INDETERMINATE`로 유지한다.

## 8. 외부효과와 권한 경계

이번 IVA 작업에서 수행한 외부효과는 이 결과 문서의 운영 저장소 기록뿐이다. 다음은 수행하지 않았다.

- Bootstrap/Web Starter 제품 소스 수정
- PR ready 전환·병합·main 반영
- force push/reset
- CI 재실행
- 계정·키·Supabase 프로젝트 생성 또는 변경
- 사용자 PC 설치
- Template 활성화·릴리스·배포
- PMO 또는 새 Persona 호출

MITCHELL은 최초 FAIL을 역사로 보존하고, corrected candidates에 대한 이번 affected-only PASS를 별도 결과로 수신한다. 이후 merge 여부는 Owner/MITCHELL 권한이며, 실제 환경 acceptance와 release/deploy는 별도 gate다.

## 9. 근거 위치

- 최초 입력: `docs/execution/IVA_REVIEW_PACKET_v0.1.md`
- 최초 IVA 결과: `docs/execution/IVA_REVIEW_RESULT_20260915.md`
- 작성자 교정 완료보고: `docs/execution/CORRECTION_COMPLETION_20260915.md`
- corrected manifest: `docs/execution/CANDIDATE_MANIFEST_20260915_CORRECTED.json`
- Git 기록 재검증 입력: `docs/execution/IVA_REREVIEW_PACKET_v0.2.md`
- 사용자 전달 재검증 입력: `MITCHELL-IVA-BW-001-REREVIEW` v0.2.1
- Supabase 공식 sign-out 문서: `https://supabase.com/docs/reference/javascript/auth-signout`
