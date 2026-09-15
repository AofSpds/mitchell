# MITCHELL 결정 기록

Version 1.1 / Generation 2 / 2026-09-15 / Writer: MITCHELL
Expected previous generation: 1

사용자 지정, 상위 방향, 계획 채택에 따른 구현 기본값을 구분한다. 원문을 복제하지 않는 정제 요약이다. 계획 당시의 후보 상태와 후속 실행 승인 사이의 변경 관계를 보존한다.

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
| D014 | AUTHOR_DISPOSITION | 두 B/W 후보와 성공 CI를 고정하고 R01 완료보고·별도 IVA 입력을 보존. 이는 사용자의 검증 승인·IVA PASS·제품 병합이 아님 |

## 권한의 변경·보존

초기 D003/D004의 문서-only Git 반영 범위는 역사적 상태다. D012와 실행 영수증이 구현·작성자 점검·제품 브랜치·커밋·PR 작성 범위에서 이를 대체한다. 승인된 범위의 파일명·함수·일반 구현·기초 시험을 매번 되묻지 않는다.

계정 생성·결제·키 발급·사용자 PC 설치·실제 SNS 게시·자동 게시 활성화·visibility 변경·파괴적 작업·새 Persona는 묵시적으로 포함하지 않는다. 제품 병합·정식 릴리스는 계획의 독립검증/채택 경계를 유지한다. PMO/IVA를 실제 호출한 증거가 없으므로 NOT_DISPATCHED/NOT_RUN이다.

## 변경 이력

Generation 1은 계획/정책 문서화 시점이다. Generation 2는 후속 실행 승인과 실제 후보 생성, 중단 복구를 반영한다. D007–D011의 후보 의미를 삭제하지 않고 계획 채택이라는 후속 근거로 연결했다. 원래 계획 blob은 변경하지 않는다.
