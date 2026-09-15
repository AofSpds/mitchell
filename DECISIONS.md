# MITCHELL 결정 기록

Version 1.3 / Generation 4 / 2026-09-15 / Writer: MITCHELL
Expected previous generation: 3

사용자 지정, 상위 방향, 계획 채택, 작성자 처분, 검증 결과와 운영 반환 규칙을 구분한다. 원문을 복제하지 않는 정제 요약이며 과거 후보와 후속 교정의 대체 관계를 보존한다.

| ID | 현재 상태 | 내용 / 근거 |
|---|---|---|
| D001 | OWNER_DIRECTED | 메인 MITCHELL, Codex WORK 작업자 PMO, 검증자 IVA — 사용자 지정 |
| D002 | OWNER_DIRECTED | 다른 Persona·페어 검증자는 미설치; 선제 생성하지 않음 |
| D003 | OWNER_DIRECTED | 현재 MITCHELL 채널이 Git 업무를 수행. 첫 산출물은 상세 계획서였으며 이후 구현 지시가 추가됨 |
| D004 | OWNER_DIRECTED | AAA 또는 HLOM의 필요한 정책을 현지 적용. HLOM을 선택적으로 참조했으며 조직 전체 이식 아님 |
| D005 | USER_DIRECTION_CARRIED | Windows 초보 온보딩, Git 기반, 개발자 점검 가능, 반복 질문 최소화 |
| D006 | BASELINE_CARRIED | Bootstrap / Web Starter / 실제 앱 분리, Next.js·TypeScript·Tailwind·Supabase |
| D007 | PLAN_ADOPTED_FOR_IMPLEMENTATION | mitchell 운영 허브 / bootstrap 설치 제품 경계. 이전 DESIGN_DEFAULT_CANDIDATE 상태는 D012로 대체 |
| D008 | PLAN_ADOPTED_FOR_IMPLEMENTATION | Core 8종·Optional 분리, DEMO first, npm, 최소 테스트. D012에 의한 계획 채택 |
| D009 | PLAN_ADOPTED_FOR_IMPLEMENTATION | 당일 00:00~09:00 촬영창, 1건 기본 한도, 30분 지연. D012에 의한 설계 기본값 채택이며 아직 사진 앱 구현은 아님 |
| D010 | PLAN_ADOPTED_FOR_IMPLEMENTATION | mock→Instagram→Threads, RLS·중복방지 선행. 실제 계정·게시 권한은 별도 |
| D011 | PLAN_ADOPTED_FOR_IMPLEMENTATION | 실제 앱 private 기본 권고와 계정 경계 유지. 새 저장소의 소유자·visibility 설정을 실제 변경한 것은 아님 |
| D012 | OWNER_EXECUTION_DIRECTED | 같은 MITCHELL 채널에서 상세 계획에 따른 GitHub 구현을 끝까지 진행하라는 후속 지시. 실행 영수증은 docs/execution/EXECUTION_20260915.md |
| D013 | OWNER_RESUME_DIRECTED | 중단 후 계속하라는 사용자 지시. 기존 refs·CI를 읽고 성공 효과는 재실행하지 않는 복구를 수행 |
| D014 | SUPERSEDED_BY_D016 | 최초 B/W 후보와 작성자 CI를 고정하고 IVA 입력을 보존한 작성자 처분. 이후 IVA 최초 검증과 affected-only 교정으로 후보 상태가 변경됨 |
| D015 | OWNER_CONTINUATION_APPLIED | D012/D013의 중단 없는 GitHub 실행 범위에서 사용자가 IVA 결과를 현재 MITCHELL 채널에 전달했다. 결과 자체를 새 권한으로 보지 않고, 이미 승인된 실행의 exact 4건 교정에만 적용했다. 병합·배포 권한은 포함하지 않음 |
| D016 | AUTHOR_CORRECTION_DISPOSITION | IVA-B001/B002/W001/W002만 교정하고 새 head/tree·작성자 CI·artifact를 고정했다. invalid 설정은 DEMO가 아닌 fail-closed 복구 화면, logout은 local-session 결과 확인, WinGet 재부팅 HRESULT는 REBOOT_REQUIRED, 공유 로그는 구조별 마스킹을 기본으로 한다. IVA 재검증 전 채택/PASS로 승격하지 않음 |
| D017 | IVA_REREVIEW_RESULT | exact corrected candidates에 대한 affected-only IVA 재검증에서 B001/B002/W001/W002가 모두 PASS, 새 finding NONE, `MERGE_RECOMMENDATION = PASS`로 판정됐다. 실제 Windows·Supabase 통합은 `NOT_RUN / INDETERMINATE`, `RELEASE_DEPLOY_RECOMMENDATION = HOLD`로 유지 |
| D018 | OWNER_OPERATION_RULE | IVA 최종 반환은 항상 MITCHELL에 바로 전달 가능한 인계 패킷 형식으로 한다. 상세 결과가 길면 Git에 원문을 기록하고 채팅에는 Git 경로·exact 대상·finding별 판정·merge 권고·미실행·권한 경계·다음 조치를 담은 패킷만 반환한다. 즉시 적용하며 별도 사용자 행동은 없음 |

## 권한의 변경·보존

초기 D003/D004의 문서-only Git 반영 범위는 역사적 상태다. D012와 실행 영수증이 구현·작성자 점검·제품 브랜치·커밋·PR 작성 범위에서 이를 대체한다. 승인된 범위의 파일명·함수·일반 구현·기초 시험을 매번 되묻지 않는다.

계정 생성·결제·키 발급·사용자 PC 설치·실제 SNS 게시·자동 게시 활성화·visibility 변경·파괴적 작업·새 Persona는 묵시적으로 포함하지 않는다. 제품 병합·정식 릴리스는 독립검증/채택 경계를 유지한다. PMO는 실제 dispatch하지 않았으므로 `NOT_DISPATCHED`다.

D017의 `MERGE_RECOMMENDATION = PASS`는 exact corrected candidates의 네 finding gate에 대한 독립 권고다. 제품 main 병합을 실제 수행한 것이 아니며, Template 활성화·릴리스·배포 승인도 아니다. D018은 반환 형식과 기록 위치를 정하는 운영 규칙으로 IVA의 제품 수정·병합·배포 권한을 확대하지 않는다.

## 검증 결과의 해석

최초 IVA FAIL은 당시 고정 후보의 유효 판정이다. affected-only 재검증 PASS는 교정 후보에 대한 후속 판정이며 최초 기록을 삭제하지 않는다. 깨끗한 Windows와 실제 Supabase Auth·쿠키·Storage 통합 수락은 별도 gate다.

## 변경 이력

Generation 1은 계획/정책 문서화 시점이다. Generation 2는 후속 실행 승인과 실제 후보 생성, 중단 복구를 반영했다. Generation 3은 최초 IVA 결과 수신, exact four-finding 교정, 새 후보·작성자 증거와 재검증 gate를 반영한다. Generation 4는 affected-only IVA PASS 결과와 IVA 최종 반환 패킷 의무화 규칙을 반영한다. 과거 FAIL/후보 의미를 삭제하지 않고 superseded 관계로 연결한다. 원래 계획 blob과 최초 IVA 보고서는 변경하지 않는다.
