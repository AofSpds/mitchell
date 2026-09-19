# MITCHELL 결정 기록

Version 1.9 / Generation 10 / 2026-09-20 / Writer: MITCHELL
Expected previous generation: 9
Expected base: d4abb0380a2bbef36db7e7ee225e5dda204c479e

사용자 지정, 상위 방향, 계획 채택, 작성자 처분, 검증 결과와 운영 반환 규칙을 구분한다. 원문을 복제하지 않는 정제 요약이며 과거 후보와 후속 교정의 대체 관계를 보존한다.

| ID | 현재 상태 | 내용 / 근거 |
|---|---|---|
| D001 | OWNER_DIRECTED | 메인 MITCHELL, Codex WORK 작업자 PMO, 검증자 IVA — 사용자 지정 |
| D002 | OWNER_DIRECTED | 다른 Persona·페어 검증자는 미설치; 선제 생성하지 않음 |
| D003 | OWNER_DIRECTED | 현재 MITCHELL 채널이 Git 업무를 수행. 첫 산출물은 상세 계획서였으며 이후 구현 지시가 추가됨 |
| D004 | OWNER_DIRECTED | AAA 또는 HLOM의 필요한 정책을 현지 적용. HLOM을 선택적으로 참조했으며 조직 전체 이식 아님 |
| D005 | USER_DIRECTION_CARRIED | Windows 초보 온보딩, Git 기반, 개발자 점검 가능, 반복 질문 최소화 |
| D006 | BASELINE_CARRIED | Bootstrap / Web Starter / 실제 앱 분리, Next.js·TypeScript·Tailwind·Supabase. 모바일 로컬 경로의 필수 스택은 D019/D020으로 별도 구분 |
| D007 | PLAN_ADOPTED_FOR_IMPLEMENTATION | mitchell 운영 허브 / bootstrap 설치 제품 경계. 이전 DESIGN_DEFAULT_CANDIDATE 상태는 D012로 대체 |
| D008 | PLAN_ADOPTED_FOR_IMPLEMENTATION | Core 8종·Optional 분리, DEMO first, npm, 최소 테스트. D012에 의한 계획 채택 |
| D009 | PLAN_ADOPTED_FOR_IMPLEMENTATION | 기존 무인 게시 계획은 당일 00:00~09:00 촬영창, 1건 기본 한도, 30분 지연. 모바일 수동 경로는 D019/D020으로 등록일·알림·수동 공유를 별도 설계; 과거 의미 보존 |
| D010 | PLAN_ADOPTED_FOR_IMPLEMENTATION | 기존 API 계획은 mock→Instagram→Threads, RLS·중복방지 선행. 모바일 로컬 경로에서는 공식 앱 공유 호환성 확인으로 대체하며 실제 계정·게시 권한은 별도 |
| D011 | PLAN_ADOPTED_FOR_IMPLEMENTATION | 실제 앱 private 기본 권고와 계정 경계 유지. 새 저장소의 소유자·visibility 설정을 실제 변경한 것은 아님 |
| D012 | OWNER_EXECUTION_DIRECTED | 같은 MITCHELL 채널에서 상세 계획에 따른 GitHub 구현을 끝까지 진행하라는 후속 지시. 실행 영수증은 docs/execution/EXECUTION_20260915.md |
| D013 | OWNER_RESUME_DIRECTED | 중단 후 계속하라는 사용자 지시. 기존 refs·CI를 읽고 성공 효과는 재실행하지 않는 복구를 수행 |
| D014 | SUPERSEDED_BY_D016 | 최초 B/W 후보와 작성자 CI를 고정하고 IVA 입력을 보존한 작성자 처분. 이후 IVA 최초 검증과 affected-only 교정으로 후보 상태가 변경됨 |
| D015 | OWNER_CONTINUATION_APPLIED | D012/D013의 중단 없는 GitHub 실행 범위에서 사용자가 IVA 결과를 현재 MITCHELL 채널에 전달했다. 결과 자체를 새 권한으로 보지 않고, 이미 승인된 실행의 exact 4건 교정에만 적용했다. 병합·배포 권한은 포함하지 않음 |
| D016 | AUTHOR_CORRECTION_DISPOSITION | IVA-B001/B002/W001/W002만 교정하고 새 head/tree·작성자 CI·artifact를 고정했다. invalid 설정은 fail-closed 복구 화면, logout은 local-session 결과 확인, WinGet 재부팅 HRESULT는 REBOOT_REQUIRED, 공유 로그는 구조별 마스킹. D017이 후속 독립검증 결과 |
| D017 | IVA_REREVIEW_RESULT | exact corrected candidates의 B001/B002/W001/W002 모두 PASS, 새 finding NONE, MERGE_RECOMMENDATION PASS. 실제 Windows·Supabase는 NOT_RUN/INDETERMINATE, RELEASE_DEPLOY_RECOMMENDATION HOLD |
| D018 | OWNER_OPERATION_RULE | IVA 최종 반환은 항상 MITCHELL 인계 패킷. 상세 결과는 Git, 채팅에는 exact 대상·판정·기록 위치·권고·미실행·권한·다음 조치. 즉시 적용 |
| D019 | OWNER_MOBILE_SCOPE_DIRECTED | 후속 모바일 요구에서 외부 서버와 외부 스토리지를 금지하고 최종 SNS 게시를 직접 누르는 방식에 동의했다. 모바일은 사진 준비·9시 알림·공식 앱 공유 경로로 상세 설계한다. 서버 동기화/임시 외부 저장소/무인 게시 제안은 이 경로에서 비채택으로 대체. 기존 B/W는 폐기하지 않음 |
| D020 | DESIGN_PROPOSAL_ADOPTED_BY_D021 | 최초 제안은 iPhone/Galaxy 공통 로컬 앱, RN/Expo/TS·SQLite·native bridge, 앱 게시함과 지정 소스, 등록일 기준 날짜창, 기본 최대10장, 공유 시도와 사용자 완료 분리였다. 이후 작업계획 실행 지시로 구현 기본값에 사용하며 실제 기기/SNS 호환성 PASS는 아님 |
| D021 | OWNER_EXECUTION_CONTINUATION | 사용자가 만든 sns-gateway에서 작업계획 초안에 따라 현재 MITCHELL 채널 직접 구현을 지시했고, 남은 폴더/앨범·날짜 묶음·지난 알림·보존 관리를 충분히 생각해 구현하라고 후속 지시했다. 작은 구현 선택을 다시 묻지 않으며 PMO로 이관하지 않음 |
| D022 | AUTHOR_LIFECYCLE_DISPOSITION | SNSG-LIFECYCLE-001에서 네 남은 코드 영역을 구현한다. 소스 baseline/완전 스캔/unknown 날짜, immutable revision, 알림 날짜 확인, 미확인 공유 보호와7일 유예·삭제 journal·500MiB 관리 사본 제한을 적용한다. 상세 한계·시험·exact 대상은 완료보고가 소유하며 독립검증/실기 PASS로 승격하지 않음 |
| D023 | IVA_RESULT_INGESTED | SNS Gateway cf6967ce…의 최초 IVA-002 v1.0.1을 exact5133d5fe…/blob575fbf73…로 수신. candidate FAIL, F001–F004 MEDIUM/P2, MERGE/RELEASE HOLD. SGV-01/03/08 PASS는 소스·fixture 범위, SGV-04 INDETERMINATE. 최초 판정을 보존 |
| D024 | AUTHOR_AFFECTED_CORRECTION | D021의 기존 실행 범위에서 F001 영속 순환 재시도, F002 미확인/전체 이력 cursor, F003 엄격한 앱 소유 임시 사본, F004 I/O 오류 fail-closed와 삭제/reset journal만 교정. 임시 import 파일1시간 유예는 공유 staging7일 유예와 구분. 작성자 검사 후 새 후보 고정·별도 IVA affected-only 재검증. 권한 확대·독립 PASS 선언 아님 |
| D025 | IVA_AFFECTED_REREVIEW_RESULT | exact 교정 후보 4ab10f48…/tree caa83524…의 SGV-F001–F004 모두 PASS, affected scope 신규 finding NONE, MERGE_RECOMMENDATION PASS. 최초 cf6967ce 후보 FAIL은 보존. SGV-04 INDETERMINATE 및 실제 기기/SNS NOT_RUN 유지, RELEASE_DEPLOY_RECOMMENDATION HOLD. 실제 merge는 별도 Owner 처분 |
| D026 | OWNER_MERGE_DISPOSITION | 사용자가 후속 ‘진행하세요’로 SNS Gateway PR #1 병합 진행을 승인했다. 검증 exact head 4ab10f48…를 expected head로 고정하고 merge commit 방식으로 병합하여 product main=1c7cd117…이 됐다. PR #1은 MERGED/CLOSED. release/deploy와 실제 기기/SNS·서명 설치 권한은 포함하지 않음 |
| D027 | OWNER_BOOTSTRAP_MACOS_EXECUTION | 사용자가 Windows Bootstrap 검증 후보 병합 후 macOS v0.2 설계·구현을 진행하도록 승인하고 중단 후 계속 지시했다. Windows PR#1 merge7ede3b…와 Mac 초기78a919e…/PR#2를 원격에서 복구했으며 성공 효과를 반복하지 않았다. macOS 코드·자체 검사·작업 브랜치/PR·운영 Git 기록 범위이며 실제 사용자 Mac 설치·macOS 병합·릴리스·계정 변경은 별도 |
| D028 | AUTHOR_MACOS_SUPPORT_DISPOSITION | Homebrew 공식 Git 원문(last_review_date2026-09-17, blob d7b95f04…) 재확인으로 초기14+ 설치안을15+ native Apple Silicon 설치선으로 대체한다. macOS14는 Plan/Verify 진단 전용, Intel은 Tier3·macOS15+ 명시적 opt-in 미수락 경로다. CLT/Homebrew 최초 준비·Xcode/SDK/license/서명은 사용자 수동. Windows18개 blob 보존, Mac 신설 경로는 별도 IVA 대상 |

## 권한의 변경·보존

초기 문서-only 범위는 D012와 실행 영수증이 B/W 구현·작성자 점검·제품 브랜치·커밋·PR 작성 범위에서 대체했다. 모바일 설계-only 상태는 D021과 docs/execution/SNS_GATEWAY_EXECUTION_20260919.md의 실행 승인으로 sns-gateway 코드·자체 점검·Git 후보 보존 범위에서 대체됐다. 기기 설치·실사진 공개·계정 생성·결제·개인 서명키·공개범위 변경·파괴적 변경·새 Persona·병합·배포는 여전히 별도 경계다.

D017의 merge PASS는 B/W 독립 권고이며 실제 병합이나 release/deploy 승인이 아니다. D018은 IVA의 제품 수정·병합·배포 권한을 확대하지 않는다. D019/D021은 모바일 무인 게시 권한을 부여하지 않는다. API 키 등록 질문은 외부 서버나 SNS HTTP API를 새로 추가하라는 승인으로 해석하지 않았다. D023 결과 자체도 새 실행권한이 아니며 D024는 기존 승인 범위의 국소 교정이다. D025의 MERGE_RECOMMENDATION=PASS 자체는 실행권한이 아니었고, D026의 별도 Owner 처분으로 실제 merge가 수행됐다. D026은 release·deploy·실기 설치·SNS 공개 권한까지 확대하지 않는다.

D027은 Windows PR#1의 별도 승인 병합과 macOS v0.2 구현에 적용한다. Windows의 과거 PASS를 Mac으로 확대하지 않으며 Mac PR#2의 독립검증·실기 수락·병합·릴리스 권한을 구분한다. D028은 지원 정책을 보수적으로 갱신한 작성자 구현 선택이며 OS 업그레이드나 사용자 보안 설정 변경을 승인한 것이 아니다.

## 검증 결과의 해석

최초 B/W IVA FAIL과 그 교정 후보의 affected-only PASS는 해당 범위에 보존한다. SNS Gateway 최초 IVA-002의 cf6967ce 후보는 FAIL로 보존한다. 후속 exact 교정 후보의 F001–F004 affected-only 재검증은 PASS이며 신규 finding은 없다. 교정 후보는 D026으로 main에 병합됐지만 실제 기기/SNS 수락은 NOT_RUN/INDETERMINATE이고 SGV-04도 INDETERMINATE를 유지한다. 완료한 코드 묶음과 전체 제품 수락을 같은 이름으로 표시하지 않는다.

Windows Bootstrap PR#1은 D027에 따라 병합됐고 실제 Windows 수락은 미실행으로 남는다. Mac v0.2의 작성자 시험·runner Verify는 실제 사용자 Mac 설치나 독립 IVA PASS가 아니다. 초기78a919e 후보의14 환경 CI와 후속753bc82 후보의15 환경 CI는 서로 다른 소스·정책의 증거로 보존한다.

## 변경 이력

Generation 1: 계획/정책 문서화. Generation 2: 후속 실행 승인과 최초 후보·복구. Generation 3: 최초 B/W IVA 결과·4건 교정. Generation 4: B/W affected-only IVA PASS·최종 반환 패킷 의무. Generation 5: 외부 저장소 없는 모바일 수동 공유 요구와 설계 후보. Generation 6: SNS Gateway 실행/재개와 네 남은 영역의 작성자 구현 처분. Generation 7: SNS Gateway IVA-002 FAIL 수신과 F001–F004 교정 처분. Generation 8: 교정 후보의 affected-only IVA PASS·merge recommendation PASS 수신, release/deploy HOLD와 실기 미실행 보존. Generation 9: Owner merge 승인에 따라 검증 후보를 sns-gateway main에 병합, release/deploy 및 실기 gate는 미실행으로 보존. Generation 10: Windows Bootstrap 승인 병합 복구와 Mac 확장·지원 정책 교정·작성자 후보 고정. 과거 계획·검증 보고서 원문은 변경하지 않는다.
