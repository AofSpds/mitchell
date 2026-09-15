# MITCHELL 결정 기록

Version 1.0 / 2026-09-15

현재 사용자가 지정한 사항과 설계자가 제안한 기본값을 구분한다. 공개 파일이므로 대화 원문을 복제하지 않고 의미를 정제해 기록한다. 원문 메시지의 영구 ID는 제공되지 않았다.

| ID | 상태 | 내용 | 근거 |
|---|---|---|---|
| D001 | OWNER_DIRECTED | 메인 대화 Persona는 MITCHELL, Codex WORK 작업자는 PMO, 검증자는 IVA | 2026-09-15 현재 사용자 요청 |
| D002 | OWNER_DIRECTED | 다른 페어 검증자와 Persona는 미설치; 선제 생성하지 않음 | 같은 요청 |
| D003 | OWNER_DIRECTED | 우선 현재 MITCHELL 채널에서 Git 업무를 진행하고 첫 산출물은 상세 구현용 계획서 | 같은 요청 |
| D004 | OWNER_DIRECTED | AAA 또는 HLOM을 확인하여 필요한 운영·메모 정책을 현지 적용 | 같은 요청; HLOM을 공통 원천으로 선택해 조회 |
| D005 | USER_DIRECTION_CARRIED | Windows 초보 온보딩, Git 기반, 개발자가 점검 가능한 구조, 반복 확인 최소화 | 현재 대화의 사용자 요구 정제 요약 |
| D006 | BASELINE_CARRIED | PC Bootstrap / Web Starter / 실제 앱 분리, Next.js·TypeScript·Tailwind·Supabase 방향 | 앞선 상위 플랜과 사용자 수용 맥락; 개별 추가 설계까지 승인한 것은 아님 |
| D007 | DESIGN_DEFAULT_CANDIDATE | mitchell은 운영·계획 허브, bootstrap은 설치 코드 저장소 | 새 저장소를 기존 3계층에 연결한 현지 설계 |
| D008 | DESIGN_DEFAULT_CANDIDATE | Core 8종·Optional 분리, DEMO first, npm, 최소 테스트 구조 | MITCHELL-PLAN-001 |
| D009 | DESIGN_DEFAULT_CANDIDATE | 당일 00:00~09:00 촬영창, 1건 기본 한도, 30분 지연 허용 | 원래 당일 사진 요구를 보존하기 위한 초기 설정 제안 |
| D010 | DESIGN_DEFAULT_CANDIDATE | mock→Instagram→Threads, RLS·중복방지를 실제 게시 전 구현 | 공식 자료 확인 후 상위안 보완 |
| D011 | DESIGN_DEFAULT_CANDIDATE | 새 실제 앱 저장소 private 기본 권고; 공개·서비스 계정 작업은 별도 경계 | 공개 정책/템플릿과 사용자 데이터를 분리 |

## 현재 문서 작성·Git 권한

D003/D004에 따라 현재 MITCHELL 정책·기억·계획 문서를 AofSpds/mitchell에 보존한다. 빈 저장소 초기 문서 main 등록을 포함한다. 이 범위는 제품 구현, 계정 가입, 결제, 실제 SNS 게시, native ChatGPT 설정 변경 완료 또는 IVA PASS를 포함하지 않는다.

## 승인 결합 규칙

후속 실행 승인에서는 계획 버전·blob/ref와 실행 묶음·저장소·외부효과 허용범위를 함께 기록한다. 승인된 범위의 구현 방법은 작업자가 결정하고 반복 승인받지 않는다. 단순 수신 확인을 새 실행권한으로 확대하지 않는다.

## 변경 기록

Generation 1. 기존 결정을 대체한 기록은 아직 없다. 이후 변경 시 같은 ID의 의미를 조용히 덮지 말고 새 revision 또는 SUPERSEDED 관계로 연결한다.
