# MITCHELL → IVA affected-only 재검증 패킷

**PACKET_ID:** `MITCHELL-IVA-BW-001-REREVIEW`
**VERSION:** `0.2`
**DATE:** `2026-09-15`
**FROM:** `MITCHELL`
**TO:** `IVA`
**PROJECT:** `MITCHELL`
**STATUS:** `CORRECTED_CANDIDATES_FIXED / REREVIEW_REQUESTED / MERGE_HOLD`

## 1. 목적

최초 IVA 보고서 `MITCHELL-IVA-BW-001-RESULT`가 반환한 다음 네 finding의 교정 여부만 affected-only로 재검증한다.

- `IVA-B001`
- `IVA-B002`
- `IVA-W001`
- `IVA-W002`

전체 프로젝트를 처음부터 다시 검토하거나 기존에 유효한 성공 증거를 무정보 반복하지 않는다. 수정이 다른 경로에 실질적 영향을 준 경우에만 그 영향을 추가한다.

## 2. 원 검증 결과

| 필드 | 값 |
|---|---|
| Repository | `AofSpds/mitchell` |
| Path | `docs/execution/IVA_REVIEW_RESULT_20260915.md` |
| Commit | `d16166f8b390fb63f6332b77960171e803f5bd59` |
| Blob | `1b421cda20e9da660c1ce8bff6181805c011c85c` |
| Document SHA-256 | `2b9505d997c6243f81cf76072e99478b0052e0756749047c09fb758be026f672` |

최초 판정은 Bootstrap FAIL, Web Starter FAIL, 실환경 INDETERMINATE, merge HOLD였다. 이 판정을 삭제하지 않는다.

## 3. 재검증 대상

### Bootstrap

```text
Repository = AofSpds/bootstrap
Branch     = work/bootstrap-v0.1
PR         = #1
Head       = 33b1b7e6a788d04ba61acd6c689bb5f20245116c
Tree       = 2a28a91f8fdbbbc80bd37209206cdab9cf5e715a
Base main  = 351333d6b4e6db1635c8caeb0f7dd8c4e3aee72c
```

### Web Starter

```text
Repository = AofSpds/web-starter
Branch     = work/web-starter-v0.1
PR         = #1
Head       = a2c63cca6bdbf0667ba2d9e1c8d8e7af2e137a17
Tree       = 34716ab2e0f82f625f6f6ada1cf0c205d665da51
Base main  = 0d855cd9aaf1ee8ebf31a2a7af6d67c90367d0d6
```

작업 브랜치와 PR의 head를 직접 확인한다. `main`, PR test merge commit, 과거 IVA 대상 head와 혼동하지 않는다.

## 4. 작성자 교정 요약

### B001

- Authorization 전체 값과 standalone Bearer를 우선 정제한다.
- quoted JSON 민감 필드, 일반 password/key, GitHub/JWT 형태, query string, 한글·공백 사용자 경로 회귀 사례를 추가했다.
- `ConvertTo-SafeResultsJson`의 숫자 종료코드 보존을 유지했다.

최소 확인 경로:

```text
scripts/common.ps1
tests/run.ps1
tests/logs-and-catalogue.ps1
scripts/install-packages.ps1  # nextAction 유입 경로
```

### B002

- signed/unsigned Win32 종료코드 정규화 함수를 추가했다.
- `0x8A15010A`, `-1978334966`, `2316632330`을 `REBOOT_REQUIRED`로 분류한다.
- MSI 3010/1641, 전체 종료코드 2, 일반 실패·취소와의 구분, 자동 재부팅 금지를 유지했다.

최소 확인 경로:

```text
scripts/common.ps1
scripts/install-packages.ps1
tests/run.ps1
tests/logs-and-catalogue.ps1
```

### W001

- `demo`와 `invalid`를 분리하는 화면 컴포넌트를 추가했다.
- invalid는 DEMO를 표시하지 않고 설정 오류·복구 링크를 표시하며 CRUD 경로로 진입하지 않는다.
- URL-only, key-only, invalid URL, 금지 key 형식, 정상 DEMO와 ready 상태 회귀 검사를 추가했다.

최소 확인 경로:

```text
src/components/notes-configuration-state.tsx
src/app/notes/page.tsx
src/lib/env.ts
tests/unit/notes-configuration-state.test.ts
```

### W002

- logout scope를 `local`로 명시했다.
- SDK 성공, 반환 error, throw를 구분한다.
- 실패를 성공 화면으로 이동시키지 않고 `/notes?error=logout`에서 복구 안내한다.
- 비승인 사용자 cleanup 실패도 별도 경로로 처리한다.

최소 확인 경로:

```text
src/lib/auth-session.ts
src/app/login/actions.ts
src/app/login/page.tsx
src/app/notes/page.tsx
tests/unit/auth-session.test.ts
```

## 5. 작성자 검사·artifact 증거

| 대상 | CI run | 결과 | Artifact ID | 외부 SHA-256 | 내부 후보 ZIP SHA-256 |
|---|---:|---|---:|---|---|
| Bootstrap | `34970656993` | powershell-51 / powershell-7 success | `10396389180` | `021a1caf5717ae5e05ffa6800a940bb8035ec466d0c28dca17af1f113b8cd6c7` | `a65cb366a9ccc5ae1de0ea0878649a45c590c507634deb10a573c27191a919e6` |
| Web Starter | `34970348151` | web / windows-node / rls-fixture success | `10396488405` | `416622c3d094662098722ff882f7a57bbbababa062c61705a259d4864f6b8be0` | `a8f85ab275c718727099136b235cf5bd7fa57362f1be7304a7f52368ea34c4df` |

Bootstrap source는 Git text normalization과 raw CRLF `bootstrap.bat`을 구분해 exact tree를 재계산했다. Web source는 추출 원본 바이트로 exact tree를 재계산했다. 상세 inventory는 `docs/execution/CANDIDATE_MANIFEST_20260915_CORRECTED.json`과 `CORRECTION_COMPLETION_20260915.md`에 있다.

Bootstrap의 중간 run `34970081183` 실패는 재부팅 fixture 간 상태 초기화 누락이었다. 최종 head에서 테스트 격리를 수정하고 두 PowerShell job이 성공했다. 이 실패를 숨기거나 제품 PASS로 대체하지 않는다.

## 6. IVA 재검증 요청

### Bootstrap affected-only

1. `Protect-Text`와 `ConvertTo-SafeResultsJson`에서 가짜 값만 사용해 다음을 확인한다.
   - Authorization: Bearer
   - 일반 Bearer
   - quoted JSON authorization/password
   - 일반 password/key
   - GitHub/JWT 모양
   - query string
   - 한글·공백 사용자 경로
   - 숫자 종료코드 보존
2. WinGet `0x8A15010A` signed/unsigned, MSI 3010/1641, 일반 실패를 확인한다.
3. 최종 전체 종료코드와 수동 재부팅 안내, 자동 재부팅 금지를 확인한다.
4. 가능하면 PowerShell 5.1/7의 새 회귀 사례를 사용한다. 실제 인증정보를 사용하지 않는다.

### Web Starter affected-only

1. `/notes`에서 완전 무설정만 DEMO인지 확인한다.
2. URL-only, key-only, invalid URL, 금지 key 형식이 invalid 복구 화면이며 CRUD 호출이 차단되는지 확인한다.
3. sign-out 성공, SDK 반환 error, throw와 비승인 사용자 cleanup 실패가 성공과 구분되는지 확인한다.
4. local scope가 의도와 일치하고 승인 사용자·소유권/RLS 경계가 수정으로 약화되지 않았는지 확인한다.
5. 실제 Supabase 계정/쿠키/Storage 검사는 환경이 없으면 `NOT_RUN / INDETERMINATE`로 유지한다.

## 7. 기대 반환

finding별로 다음 중 하나를 반환한다.

```text
PASS
FAIL
INDETERMINATE
NOT_RUN
```

반환 문서에는 반드시 다음을 포함한다.

- 검증한 exact head/tree
- 재사용한 기존 증거와 새로 실행한 증거 구분
- B001/B002/W001/W002 각각의 판정과 근거
- 새 finding이 있다면 정확한 영향·재현·최소 교정 범위
- 실제 Windows/Supabase 등 NOT_RUN의 유지 여부
- `MERGE_RECOMMENDATION = PASS / HOLD`

## 8. 권한·금지 범위

이 패킷은 read-only affected-only 독립검증 요청이다. 다음을 승인하지 않는다.

- 제품 코드 수정
- PMO dispatch
- PR 병합·main 반영
- Template 활성화
- 릴리스·배포
- 사용자 PC 설치
- Supabase/외부 계정 변경
- 실제 SNS 게시
- 새로운 Persona·페어 검증자 생성

`PMO_RUNTIME = NOT_DISPATCHED`다. IVA는 본인 검증 결과만 Git에 기록하고, 작성자 산출물을 직접 고쳐 PASS로 만들지 않는다.

## 9. 운영 기록

- 작성자 교정 완료보고: `docs/execution/CORRECTION_COMPLETION_20260915.md`
- 교정 후보 manifest: `docs/execution/CANDIDATE_MANIFEST_20260915_CORRECTED.json`
- 본 패킷은 이를 포함하는 `AofSpds/mitchell` remote commit/readback으로 고정한다. 문서 안에서 자신의 미래 commit SHA를 예측하지 않는다.
