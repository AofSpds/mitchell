# MITCHELL IVA 교정 완료보고

- 문서 ID: `MITCHELL-BW-001-CORRECTION-COMPLETION`
- 버전: `1.0`
- 작성일: `2026-09-15`
- 작성자: `MITCHELL`
- 상태: `AUTHOR_CORRECTION_COMPLETED / AUTHOR_CHECKS_SUCCESS / IVA_REREVIEW_NOT_RUN`
- 원 검증 결과: `docs/execution/IVA_REVIEW_RESULT_20260915.md`

## 1. 목적과 범위

최초 IVA 검증이 반환한 `MEDIUM / P2` 네 finding만 교정했다. 전체 설계 재작성, 제품 main 병합, 배포, 실제 Windows 설치, 실제 Supabase 계정 변경은 수행하지 않았다.

| Finding | 최초 판정 | 작성자 교정 상태 | 독립 재검증 |
|---|---|---|---|
| IVA-B001 | FAIL 원인 | CORRECTED_BY_AUTHOR | NOT_RUN |
| IVA-B002 | FAIL 원인 | CORRECTED_BY_AUTHOR | NOT_RUN |
| IVA-W001 | FAIL 원인 | CORRECTED_BY_AUTHOR | NOT_RUN |
| IVA-W002 | FAIL 원인 | CORRECTED_BY_AUTHOR | NOT_RUN |

최초 FAIL은 당시 후보에 대한 유효 기록으로 유지한다. 아래 새 후보는 IVA PASS가 아니라 affected-only 재검증 입력이다.

## 2. 정확한 새 후보

| 대상 | Branch / PR | 최초 IVA 대상 | 새 Head | 새 Tree |
|---|---|---|---|---|
| Bootstrap | `work/bootstrap-v0.1` / #1 | `b4cabc7acb558c556a5b59a7826e43757d67eb27` | `33b1b7e6a788d04ba61acd6c689bb5f20245116c` | `2a28a91f8fdbbbc80bd37209206cdab9cf5e715a` |
| Web Starter | `work/web-starter-v0.1` / #1 | `15efb21e9cf3c4ba60c34af95f928ba38221a224` | `a2c63cca6bdbf0667ba2d9e1c8d8e7af2e137a17` | `34716ab2e0f82f625f6f6ada1cf0c205d665da51` |

두 PR은 `OPEN / DRAFT / UNMERGED`다.

## 3. Bootstrap 교정

### IVA-B001 — 로그 마스킹

수정 경로:

- `scripts/common.ps1`
- `tests/run.ps1`
- `tests/logs-and-catalogue.ps1`
- `docs/TROUBLESHOOTING.md`

변경:

- Authorization 전체 값과 standalone Bearer를 일반 key/value보다 먼저 처리한다.
- 따옴표 JSON의 `authorization`, `password`, token/key 필드를 정제한다.
- 일반 password/key, GitHub/JWT 형태, query string, 한글·공백 Windows 사용자 경로를 회귀 사례로 추가했다.
- `ConvertTo-SafeResultsJson`의 숫자 종료코드 보존과 마스킹 후 직렬화 계약은 유지했다.

### IVA-B002 — WinGet 재부팅 HRESULT

수정 경로:

- `scripts/common.ps1`
- `scripts/install-packages.ps1`
- `tests/run.ps1`
- `tests/logs-and-catalogue.ps1`
- `docs/TROUBLESHOOTING.md`

변경:

- 종료코드를 unsigned Win32 값으로 정규화하는 함수를 추가했다.
- `0x8A15010A`, signed `-1978334966`, unsigned `2316632330`을 `REBOOT_REQUIRED`로 분류한다.
- MSI `3010`/`1641` 처리, 전체 종료코드 `2`, 수동 재부팅·자동 재부팅 금지 계약을 유지했다.
- 일반 실패 `99`는 재부팅 필요로 오분류하지 않는다.

### 투명하게 보존한 중간 실패

첫 교정 head `4b5791ae38c6625a0955b78d7c30a6ae254380a8`의 PR run `34970081183`은 실패했다. signed-HRESULT 제품 로직은 독립 helper 검사에서 통과했으나, 직전 `3010` fixture가 `present=true`를 남겨 다음 설치 경로를 건너뛰는 테스트 격리 결함이었다. 각 재부팅 사례 전에 상태를 초기화한 테스트 전용 commit `33b1b7e6a788d04ba61acd6c689bb5f20245116c`을 추가했다. 제품 분기를 우회하거나 기대값을 약화하지 않았다.

## 4. Web Starter 교정

### IVA-W001 — invalid 설정의 DEMO 오표시

수정/추가 경로:

- `src/components/notes-configuration-state.tsx`
- `src/app/notes/page.tsx`
- `tests/unit/notes-configuration-state.test.ts`
- `docs/RECOVERY.md`

변경:

- `demo`일 때만 DEMO를 표시한다.
- `invalid`는 설정 오류, 안전한 reason, `/setup` 복구 링크를 표시하고 CRUD를 호출하지 않는다.
- URL-only, key-only, 잘못된 URL, 금지 key 형식, 정상 무설정 DEMO, ready 상태를 회귀 검사한다.

### IVA-W002 — 로그아웃 결과 처리

수정/추가 경로:

- `src/lib/auth-session.ts`
- `src/app/login/actions.ts`
- `src/app/login/page.tsx`
- `src/app/notes/page.tsx`
- `tests/unit/auth-session.test.ts`
- `docs/RECOVERY.md`

변경:

- logout scope를 현재 브라우저 세션의 `local`로 명시했다.
- SDK 성공, 반환 error, throw를 구분한다.
- 실패 시 `/notes?error=logout`에서 세션 잔존 가능성과 복구 행동을 알리고 성공 메시지를 표시하지 않는다.
- 비승인 사용자 로그인 후 cleanup 실패도 별도 오류로 처리한다.

## 5. 작성자 검사 결과

| 대상 | Run | Job | 결과 |
|---|---|---|---|
| Bootstrap | `34970656993` | powershell-51 | success |
| Bootstrap | `34970656993` | powershell-7 | success |
| Web Starter | `34970348151` | web | success |
| Web Starter | `34970348151` | windows-node | success |
| Web Starter | `34970348151` | rls-fixture | success |

Bootstrap 검사는 PowerShell parser/mock과 교정 회귀 사례다. Web 검사는 npm ci, lint/type/unit/build, DEMO browser, dependency audit, Windows Node check, PostgreSQL 정책 fixture를 포함한다. 기존 성공 증거는 유지했지만 새 head에서 해당 workflow 전체가 다시 성공했다.

## 6. Artifact와 source identity

| 항목 | Bootstrap | Web Starter |
|---|---|---|
| Artifact ID | `10396389180` | `10396488405` |
| 외부 artifact SHA-256 | `021a1caf5717ae5e05ffa6800a940bb8035ec466d0c28dca17af1f113b8cd6c7` | `416622c3d094662098722ff882f7a57bbbababa062c61705a259d4864f6b8be0` |
| 내부 candidate ZIP SHA-256 | `a65cb366a9ccc5ae1de0ea0878649a45c590c507634deb10a573c27191a919e6` | `a8f85ab275c718727099136b235cf5bd7fa57362f1be7304a7f52368ea34c4df` |
| Source file count | 19 | 57 |
| Tree 대조 | Windows text normalization + raw `bootstrap.bat` 적용 후 exact `2a28a91f…` | 추출 원본 바이트로 exact `34716ab2…` |

외부 ZIP의 계산 SHA-256은 GitHub artifact digest와 일치한다. Bootstrap은 CI의 Windows checkout 때문에 11개 text 파일의 CRLF를 Git blob LF로 정규화했고, 저장소 blob 자체가 CRLF인 `bootstrap.bat`은 raw blob으로 유지한 뒤 exact tree를 얻었다. 이 차이를 전체 바이트 동일성으로 과장하지 않는다.

Artifact `evidence.json`의 `iva: NOT_RUN`은 해당 작성자 CI artifact 범위의 표시다. 최초 IVA 독립검증이 수행된 프로젝트 현재 상태를 부정하지 않으며, 새 후보의 IVA 재검증은 실제로 아직 NOT_RUN이다.

## 7. 남은 gate

- IVA affected-only 재검증: `NOT_RUN`
- 깨끗하 Windows 11 x64 실제 설치/UAC/WinGet: `NOT_RUN`
- 기존 사용자 PC 보존·PATH·한글/공백 경로·재실행: `NOT_RUN`
- 실제 Supabase Auth/HTTP CRUD/Storage/logout/cookie: `NOT_RUN`
- 제품 main 병합·Template 활성화·릴리스·배포: `NOT_DONE`
- PMO runtime: `NOT_DISPATCHED`

다음 입력은 `docs/execution/IVA_REREVIEW_PACKET_v0.2.md`다. 첫 IVA 보고서가 요구한 affected-only 범위를 넘는 전역 재검증은 요청하지 않는다.
