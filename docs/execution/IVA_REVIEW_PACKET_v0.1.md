# MITCHELL → IVA / Bootstrap·Web Starter 최초 독립검증 입력

Packet: MITCHELL-IVA-BW-001 / Version: 0.1 / 2026-09-15
FROM: MITCHELL
TO: 별도로 활성화한 IVA
PROJECT: MITCHELL
STATUS: READY_NOT_DISPATCHED
AUTHOR: MITCHELL; PMO NOT_DISPATCHED; IVA NOT_RUN

이 문서는 실행 입력이다. 현재 대화 채널의 MITCHELL binding을 IVA로 바꾸거나 별도 검증 runtime을 자동 실행하지 않는다. 다른 페어 검증자를 설치하지 않는다.

## 1. 먼저 읽을 것

1. AofSpds/mitchell / README.md → CURRENT.md → AGENTS.md.
2. docs/IMPLEMENTATION_PLAN_v1.0.md, blob 3f4e6910d191f263baa08a1a4e4d1ce7bb4e7635.
3. docs/execution/EXECUTION_20260915.md와 COMPLETION_20260915.md.
4. docs/execution/CANDIDATE_MANIFEST_20260915.json.

계획의 과거 문서-only 범위는 이후 실행 승인 기록으로 대체되었다. 작성자의 구현 승인과 IVA의 별도 검증 권한은 구분한다.

## 2. 정확한 대상

| 항목 | Bootstrap | Web Starter |
|---|---|---|
| Repo | AofSpds/bootstrap | AofSpds/web-starter |
| PR | #1 | #1 |
| Branch | work/bootstrap-v0.1 | work/web-starter-v0.1 |
| Head | b4cabc7acb558c556a5b59a7826e43757d67eb27 | 15efb21e9cf3c4ba60c34af95f928ba38221a224 |
| Tree | 4d57b63278c33fa9213c1fa5ba82e19c1eefb168 | b657dbbb7177a2e9e70ba8c922c4cec628bbd3ff |
| Author CI run | 34939006314 | 34939036421 |

main의 초기 README나 이후 움직인 head를 위 대상과 동일시하지 않는다. 먼저 exact commit을 조회한다. PR ref가 변경됐으면 새 대상 재고정이 필요하다는 사실만 보고하고 검증 범위를 임의 확대하지 않는다.

## 3. 검증 목적과 체크 포인트

**Bootstrap:** 초보자 진입, PS5.1 호환성, Plan/Verify 무설치, 선택/동의, 기존 버전 보존, 공식 패키지/확장 식별, 호환성/탐지, 실패 종료코드, 재실행, 로그 비밀정보·경로 마스킹, PATH/UAC/재부팅/보안 설정 경계, README와 실제 동작의 일치.

**Web Starter:** 키 없는 DEMO와 실저장 구분, 부분 설정 fail-closed, 서버 인증/승인 사용자/소유권 검증, notes CRUD와 RLS, private Storage 정책, public/server key 경계, 공개 가입 금지 안내, lockfile 재현성, 한국어 시작/복구, 기본 화면과 오류 처리.

**주장 범위:** Windows runner/mock를 깨끗한 Windows 설치로 주장했는가; SQL fixture를 실제 Supabase Auth/Storage 통합으로 주장했는가; CI를 독립검증으로 주장했는가; 제품 main/배포 상태·사진 앱 구현 범위를 정확히 구분했는가.

계획과 산출물의 의미·안전·복구·사용성을 검토한다. Test Green만으로 사양 충족 PASS를 주지 않는다. 해당 source와 환경이 같은 성공 검사 증거는 재사용하고 정보 없는 전역 재실행을 요구하지 않는다.

## 4. 별도로 남아 있는 시험

깨끗한 Windows 11 x64에서 다운로드/압축해제/실행/UAC/WinGet 설치, 기존 사용자 PC 재실행, 실제 AI 계정 로그인, Supabase 실제 계정 A/B 및 HTTP/Storage 통합은 현재 NOT_RUN이다.

IVA에게 해당 환경과 별도 승인이 있으면 정확한 범위에서 수행하고 증거를 반환한다. 없으면 NOT_RUN 또는 INDETERMINATE로 남긴다. 계획상의 시험을 실행했다고 채우거나, 사용자 비밀번호/토큰을 대화창으로 요구하지 않는다. 실제 설치·Cloud 변경·SNS 게시·추가 결제·릴리스는 이 검증 패킷만으로 허용하지 않는다.

## 5. 권한과 금지

허용된 별도 검증은 exact source read, 사양 대조, 격리 환경의 안전한 자체 재현, 구조화된 findings 반환이다. 제품 소스 수정, main 병합, force push, 새 계정/키, 사용자 PC 설치, 실제 사진 게시, 자동화 활성화, 역할 추가를 임의 수행하지 않는다. 검증자는 작성자의 파일을 고치지 않고 교정 범위를 제안한다.

## 6. 반환 형식

- 검증자 이름/실제 실행 환경과 시각, exact repo/head/tree.
- Bootstrap과 Web Starter 각각 PASS / FAIL / INDETERMINATE 및 의미. 전체 릴리스 가능 여부는 별도 판정.
- finding별 ID, severity, 파일:줄/요구사항 근거, 재현/영향, 권고.
- 재사용한 증거와 실제 새로 실행한 시험을 구분.
- NOT_RUN 목록, 외부효과, 남은 사용자/환경 의존성.
- 수정·병합·배포를 수행하지 않았는지 명시.

독립검증 결과를 MITCHELL에 반환한다. 결과는 자동 구현 명령이나 병합 승인이 아니다. 필요한 correction 및 affected-only 재검증 범위를 한 번에 묶고 무한 검증 루프를 만들지 않는다.
