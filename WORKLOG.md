# MITCHELL Worklog

전체 대화 원문이 아니라 의미 있는 작업 사건의 정제 기록이다.

## E012 — SNS Gateway 재개와 로컬 게시함·알림 구현 / 2026-09-19

Actor: MITCHELL
Recovery base: mitchell@3e159bf790ab3cdd775add73b8aa7cde89bf5577 / sns-gateway@54cbdcbd77dd2fbcd46b28597e8e5863c7883f0b

- 사용자 계속 진행 지시에 따라 기존 제품 head, Draft PR #1과 성공 CI35422061756을 직접 복구했다. 성공한 합성 공유 후보를 다시 생성하지 않았다.
- 서버/API 키 없는 계약을 유지하면서 실제 로컬 사진 가져오기, JPEG 사본·해시·중복 등록 방지, SQLite v2 이관, 사진 순서/문구/이력과 iOS/Android 알림 코드를 추가했다.
- 로컬 단위/SQLite/정적 계약68개를 확인했다. 정확한 source head/tree, native CI 결과와 artifact는 아래 완료보고가 소유한다.
- 지정 폴더/앨범 지속 연동과 보존 관리·실기/SNS 시험은 미완료다. 전체 v0.1 완성으로 표시하지 않는다.
- API 키 질문은 SNS OAuth/서버 도입 승인으로 해석하지 않았다. PMO·IVA·기기·실게시·병합·배포는 실행하지 않았다. B/W는 변경하지 않았다.

Results: docs/execution/SNS_GATEWAY_EXECUTION_20260919.md, SNS_GATEWAY_COMPLETION_20260919.md, SNS_GATEWAY_IVA_PACKET_v0.1.md.

## E011 — SNS Gateway 최초 구현 후보 / 2026-09-19 (재개 시 remote 증거로 복구)

- 사용자가 생성한 AofSpds/sns-gateway에서 초기 main13d83595…와 work/sns-gateway-v0.1, Draft PR #1이 만들어졌다.
- SG-00/01 합성 JPEG 공유, Kotlin/Swift bridge, SQLite 공유 이력, 날짜 선정 기반과 CI를 구현했다.
- 최초 CI35421776429는 Expo/TypeScript 조합 검사에서 실패했다. SDK 요구에 맞게 TypeScript6.0.3으로 교정한21a9105…에서 CI35422061756의 checks/Android/iOS simulator가 성공했다.
- 문서 전용54cbdcbd…에서 API 키 불필요·기기 수락 범위를 정리했다. native build는 실제 기기 또는 SNS 호환성 PASS가 아니었다.

## E010 — 외부 서버·스토리지 없는 모바일 수동 공유 상세 설계 / 2026-09-19

Actor: MITCHELL
Recovery base: `a2ab0d75c5b72cdca7c39db1dea0c44422b7b1ec`

- 사용자가 외부 서버·외부 스토리지를 배제하고 최종 게시는 직접 누르는 방식을 선택해 상세 설계를 요청했다.
- 기존 운영 문서·기억·계획과 전달된 모바일 서버형 설계를 읽고, Apple/Android/Expo 공식 자료의 로컬 알림·공유·파일·백업 경계를 확인했다.
- `docs/mobile/LOCAL_SHARING_DESIGN_v1.0_20260919.md` 작성: 입력·날짜 의미, 9시 알림, local DB, 사진 사본, OS 공유, 문구 fallback, 공유 시도/사용자 확인 상태, 9단계 WBS와 22개 시험 조건.
- 잠금 상태 강제 화면 실행, 문자 그대로 한 번 터치, 공유 callback만으로 게시 성공, 특정 앨범 추가시각 자동 확정은 보장하지 않는다.
- 기존 B/W의 PR exact head와 Draft·미병합 상태를 재조회했다. 제품 코드·PR·CI를 변경하거나 검증을 반복하지 않았다.
- 문서의 소유권에 따라 CURRENT/DECISIONS/Persona 기억과 README 진입점을 현행화했다. 과거 계획·IVA 보고서는 수정하지 않았다.
- 모바일 구현·실기/SNS 공유·새 저장소·계정/키·설치·공개 게시·배포·PMO/IVA runtime은 실행하지 않았다.

Next: L00 실제 공유 호환성 spike. 기록 자체는 기기 시험 PASS가 아니다. 저장 완료는 이 문서를 포함하는 commit/readback으로 확인한다.

## E009 — IVA 최종 반환 패킷 규칙 반영 / 2026-09-15

- 사용자가 `MITCHELL-IVA-RETURN-PACKET-RULE-ACK` 패킷을 전달했다.
- IVA의 최종 반환은 항상 MITCHELL에 그대로 전달 가능한 인계 패킷 형식으로 하도록 운영 계약에 반영했다.
- 상세 검증 결과가 길면 Git에 원문을 기록하고, 채팅에는 Git 경로와 핵심 판정·exact 대상·finding별 결과·merge 권고·미실행·권한 경계·다음 조치를 담은 패킷만 반환한다.
- 원문 패킷은 `docs/execution/IVA_RETURN_PACKET_RULE_20260915.md`에 보존했다.
- 제품 코드, PR, CI, 사용자 PC, Cloud, SNS에는 변경하지 않았다.

## E008 — affected-only IVA 재검증 결과 기록 / 2026-09-15

- IVA가 `docs/execution/IVA_AFFECTED_ONLY_REREVIEW_RESULT_20260915.md`를 Git에 기록했다.
- exact corrected candidates의 `IVA-B001`, `IVA-B002`, `IVA-W001`, `IVA-W002`가 모두 PASS였고 새 finding은 없었다.
- `MERGE_RECOMMENDATION = PASS`이나 실제 Windows·Supabase 통합은 `NOT_RUN / INDETERMINATE`, release/deploy는 `HOLD`다.
- 결과 commit은 `8919c23eef0d8b845e5c8cd66e89f7be85209154`, 결과 blob은 `be514d035fde03370db0c6ad90bfa916c0ffbe79`다.
- 제품 PR은 계속 Draft·미병합이며 제품 main, 사용자 PC, Cloud, SNS에는 변경이 없다.

## E007 — Exact head 안정화와 UTF-8 복구 / 2026-09-15

- PR 설명 현행화 중 `NONEXISTENT` placeholder가 commit `fc52d761e7fcd97ffab29f42cce6f1e7c7fbfd41`에 생성됐고, 확인 즉시 `f880d0297e4d092c0f7d5025ff42cc7648fb66aa`에서 삭제했다.
- 실질 교정 head `33b1b7e6…` 대비 현재 head의 파일 diff는 0이며 tree는 동일한 `2a28a91f8fdbbbc80bd37209206cdab9cf5e715a`다. force-reset하지 않았다.
- 최신 Bootstrap CI run `34972848451`의 PowerShell 5.1/7 두 job이 성공했다. artifact `10397977572`의 outer SHA-256은 `63218f527893dc5a311151f9fe354dd2839b5ca9525a19798dd4b1e4bcadf74f`, inner ZIP SHA-256은 `7f9dcce619f0e403cb2b23dec7783a3f9c5a96a1b8cf9535a6e97d91f928735b`다.
- 이전 WORKLOG blob의 비 UTF-8 바이트를 확인해 UTF-8 문서로 재구성했다.
- 제품 main, 사용자 PC, Cloud, SNS에는 변경 없음. PR은 Draft·미병합으로 유지한다.

## E006 — 최초 IVA 결과와 affected-only 교정 / 2026-09-15

- IVA 결과 commit: `d16166f8b390fb63f6332b77960171e803f5bd59`.
- 판정: Bootstrap FAIL, Web Starter FAIL, 실환경 INDETERMINATE, merge HOLD.
- Finding: `IVA-B001`, `IVA-B002`, `IVA-W001`, `IVA-W002`.
- Bootstrap 교정 tree `2a28a91f…`, Web Starter 교정 head/tree `a2c63cca…` / `34716ab2…`에서 작성자 CI 성공.
- 교정 완료보고, corrected manifest, affected-only IVA 재검증 패킷 작성. PMO dispatch·제품 병합·배포·실환경 변경 없음.

## E005 — 중단 복구와 후보 완료 정리 / 2026-09-15

- 제품 브랜치, Draft PR 2개와 성공 CI를 remote 증거로 복구했다.
- 최초 후보: Bootstrap `b4cabc7…`, Web Starter `15efb21…`.
- 완료보고, 후보 manifest, 최초 IVA 입력 패킷을 작성했다.

## E004 — 최초 구현 후보와 작성자 CI / 2026-09-15

- Bootstrap/Web Starter 구현 후보와 Draft PR을 생성했다.
- 작성자 CI 성공. 실제 Windows 설치·Supabase 실계정·SNS·독립검증·배포 완료는 아님.

## E003 — 현재 채널 실행권한 기록 / 2026-09-15

- 사용자가 MITCHELL 채널에서 상세 계획에 따른 GitHub 구현을 지시했다.
- 구현·작성자 점검·브랜치·커밋·PR 권한을 적용하고 계정·실게시·독립검증 경계는 유지했다.

## E002 — 정책 및 구현 계획 문서화 / 2026-09-15

- HLOM 운영·현재성·기억 정책 중 필요한 부분을 읽고 MITCHELL/PMO/IVA 계약에 적용했다.
- Bootstrap → Web Starter → 첫 사진 앱의 구현 계획과 운영 문서를 작성했다.

## E001 — 저장소 진입점 생성 / 2026-09-15

- `AofSpds/mitchell`과 `AofSpds/bootstrap`을 확인하고 MITCHELL README를 초기 등록했다.
- Bootstrap 제품 코드는 이 사건에서 변경하지 않았다.
