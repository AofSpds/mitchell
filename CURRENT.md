# MITCHELL Current

Updated: 2026-09-15 / Generation: 4 / Writer: MITCHELL
Recovery base: `99fd6ebd59dcdaf810a2d103379123cd302c0d98`

## 현재 목적

최초 IVA 독립검증의 P2 4건을 affected-only로 교정했고, 새 후보의 작성자 CI까지 성공했다. 최초 FAIL은 당시 후보에 대한 유효 기록으로 보존한다. 새 후보의 IVA 재검증은 아직 `NOT_RUN`이며 두 제품 PR은 Draft·미병합이다.

| 항목 | 현재 값 |
|---|---|
| Persona | `MITCHELL` |
| Task | `MITCHELL-BW-001-CORRECTION-REREVIEW-HANDOFF` |
| 최초 IVA 결과 | Bootstrap FAIL / Web Starter FAIL / 실환경 INDETERMINATE / merge HOLD |
| 교정 범위 | `IVA-B001`, `IVA-B002`, `IVA-W001`, `IVA-W002` |
| 교정 상태 | `AUTHOR_CORRECTION_COMPLETED / AUTHOR_CI_SUCCESS` |
| IVA 재검증 | `NOT_RUN / PENDING_HANDOFF` |
| PMO runtime | `NOT_DISPATCHED` |
| 다른 Persona | `NOT_INSTALLED` |
| 제품 merge / template / release / deploy | `NOT_DONE / NOT_DONE / NOT_DONE / NOT_DONE` |
| 사용자 PC / 실제 Supabase / 실제 SNS | `NOT_RUN / NOT_RUN / NOT_RUN` |

## Exact candidates

| 저장소 | Branch / PR | Head | Tree | 작성자 CI |
|---|---|---|---|---|
| `AofSpds/bootstrap` | `work/bootstrap-v0.1` / #1 | `f880d0297e4d092c0f7d5025ff42cc7648fb66aa` | `2a28a91f8fdbbbc80bd37209206cdab9cf5e715a` | `34972848451` success |
| `AofSpds/web-starter` | `work/web-starter-v0.1` / #1 | `a2c63cca6bdbf0667ba2d9e1c8d8e7af2e137a17` | `34716ab2e0f82f625f6f6ada1cf0c205d665da51` | `34970348151` success |

Bootstrap의 실질 교정 head는 `33b1b7e6a788d04ba61acd6c689bb5f20245116c`다. 이후 placeholder 생성·삭제의 두 bookkeeping commit이 추가됐지만, 현재 head와 실질 교정 head의 파일 diff는 0이고 tree는 동일하다. 이력은 force-reset하지 않았고 현재 exact head에서 PowerShell 5.1/7 CI가 다시 성공했다.

## 교정 요약

- B001: Authorization/Bearer, quoted JSON, password/key/token/query string, 한글·공백 경로 마스킹과 회귀검사 추가.
- B002: WinGet `0x8A15010A`, signed `-1978334966`, unsigned `2316632330`을 `REBOOT_REQUIRED`로 분류.
- W001: `/notes`의 `demo`와 `invalid`를 분리하고 invalid 설정에서 CRUD를 차단.
- W002: sign-out 성공·반환 error·throw와 비승인 사용자 cleanup 실패를 구분하고 local scope 명시.

## 현재 gate

- 최초 IVA FAIL을 PASS로 덮어쓰지 않는다.
- affected-only IVA 재검증 전에는 PR ready 전환, main 병합, Template 활성화, 릴리스, 배포를 하지 않는다.
- 깨끗한 Windows 11 실기와 실제 Supabase Auth/Storage/세션 통합은 별도 수락 gate이며 `NOT_RUN / INDETERMINATE`다.
- 재검증 입력은 `docs/execution/IVA_REREVIEW_PACKET_v0.2.md`다.

EFFECT_STATE: 제품 작업 브랜치와 CI artifact만 변경됐다. 사용자 PC, Cloud, SNS, 제품 main에는 변경 없음.
LAST_WORKLOG_EVENT: `WORKLOG.md / E007`.
OWNER_ACTION_REQUIRED: 별도 IVA 채널에 재검증 패킷을 전달한다. 새 저장소·키·비밀번호는 필요 없다.
