# MITCHELL — Bootstrap / Web Starter 작성자 완료 및 중단 복구 보고

ID: MITCHELL-BW-001 / Date: 2026-09-15 / Author: MITCHELL
Scope: 구현 후보 2개, 작성자 점검, Git/CI 산출물 복구, 검증 대상 고정.
Status: AUTHOR_CHECKED_CANDIDATES; 전체 계획·실사용·독립검증 완료가 아님.

## 1. 결론

중단 전에 Bootstrap과 Web Starter의 코드, 작업 브랜치, Draft PR, 성공한 CI가 이미 만들어졌다. 재개 시 remote refs와 PR 상태, 최신 CI jobs, 남아 있는 후보 ZIP을 대조했다. 제품 코드를 처음부터 만들거나 통과한 CI를 다시 실행하지 않았다. 중단의 플랫폼 내부 원인은 확인할 수 없으며, 운영 문서가 이전 상태에 남아 있었던 사실과는 구분한다.

이번 종료 범위는 B/W 구현 후보의 결과 정리와 R01 검증 인계다. Windows 실제 설치·Supabase 실계정 통합·사진 게시 앱·예약 게시·IVA·병합·정식 릴리스가 남아 있다.

## 2. 고정된 Git 대상

| 항목 | Bootstrap | Web Starter |
|---|---|---|
| Repository | AofSpds/bootstrap | AofSpds/web-starter |
| PR | https://github.com/AofSpds/bootstrap/pull/1 | https://github.com/AofSpds/web-starter/pull/1 |
| Branch | work/bootstrap-v0.1 | work/web-starter-v0.1 |
| Head | b4cabc7acb558c556a5b59a7826e43757d67eb27 | 15efb21e9cf3c4ba60c34af95f928ba38221a224 |
| Tree | 4d57b63278c33fa9213c1fa5ba82e19c1eefb168 | b657dbbb7177a2e9e70ba8c922c4cec628bbd3ff |
| Base main | 351333d6b4e6db1635c8caeb0f7dd8c4e3aee72c | 0d855cd9aaf1ee8ebf31a2a7af6d67c90367d0d6 |
| Source files | 19 | 53 |
| State | OPEN / DRAFT / UNMERGED | OPEN / DRAFT / UNMERGED |

PR snapshot의 `merge_commit_sha`는 병합 완료 증거가 아니다. 실제 `merged=false`, base/head와 현재 main을 함께 확인했다. main은 초기 README 기준이므로 기본 브랜치 ZIP을 구현 완성본으로 안내하지 않는다.

계획 원문은 `docs/IMPLEMENTATION_PLAN_v1.0.md`, blob `3f4e6910d191f263baa08a1a4e4d1ce7bb4e7635`이다. 실행권한은 `EXECUTION_20260915.md`가 기록하며, 기존 계획을 사후에 사용자 지시였던 것처럼 고치지 않았다.

## 3. 구현한 것

### Bootstrap

Windows PowerShell 5.1 진입점, Core 8종과 선택 패키지 목록, Plan/Install/Verify, 설치 전 동의, 기존 호환 설치 보존, PATH 재탐지, 설치기 종료코드 보존, 실패 후 재실행, 정제 로그, AI 확장 선택 설치, 한국어 시작/오류 안내가 들어 있다. 프로그램 설치·로그인·결제·보안 경고 승인을 같은 것으로 취급하지 않는다.

Core는 Git, GitHub Desktop, GitHub CLI, VS Code, Node.js, PowerShell 7, Windows Terminal, 7-Zip이다. npm/npx는 Node에 포함된다. Python/JDK/Docker/DBeaver와 Codex/Claude 확장은 선택이며 로그인·키 발급은 자동 수행하지 않는다.

### Web Starter

Next.js 16.3.5 / TypeScript / Tailwind / Supabase 기반이다. 계정 없는 DEMO, 부분 설정 오류 구분, 쿠키 세션, 승인 사용자 제한, 메모 CRUD, 사용자 소유권 RLS, private Storage 정책, 환경변수 분리, npm lockfile, 한국어 시작/복구 문서, AGENTS/CLAUDE 연결 지침이 들어 있다.

DEMO는 실제 저장/게시 성공을 흉내 내지 않는다. Storage 정책을 제공하는 것과 실제 사진 업로드 앱을 구현한 것은 다르다. 이 템플릿에는 SNS 앱·사진·예약 실행기를 넣지 않았다.

## 4. 실행 증거

다음은 GitHub Actions의 작성자 점검이며 IVA 검증이 아니다. 재개 작업에서 아래 성공 결과를 다시 조회하고 재사용했다.

| Run | Job | 실제 확인한 결과/범위 |
|---|---|---|
| bootstrap / 34939006314 | powershell-51 | success; Windows PowerShell 5.1 parser 및 설치기 mock |
| bootstrap / 34939006314 | powershell-7 | success; PowerShell 7 parser/mock, 소스 후보 패키징 |
| web-starter / 34939036421 | web | success; npm ci, lint/types/unit/build, DEMO Chromium, production 의존성 high/critical audit, 패키징 |
| web-starter / 34939036421 | windows-node | success; Windows runner의 npm ci/check |
| web-starter / 34939036421 | rls-fixture | success; PostgreSQL 17의 A/B/anonymous SQL 정책 평가 |

- https://github.com/AofSpds/bootstrap/actions/runs/34939006314
- https://github.com/AofSpds/web-starter/actions/runs/34939036421

동일한 head의 선행 push 검사도 성공했다: bootstrap `34938618661`, web-starter `34938721030`. 최종 판정은 위 PR run과 exact source tree를 함께 사용한다. 다른 head에서 실패했던 과거 CI가 최종 head의 결과를 대신하지 않으며, 과거 실패 기록을 삭제하지 않는다.

Windows runner는 도구가 사전 설치된 환경이다. 깨끗한 Windows 11 설치/UAC 검증이 아니다. SQL fixture는 실제 SQL 정책 평가지만 Supabase Auth/Storage HTTP 통합 시험은 아니다. audit 성공은 해당 시점·검사 범위의 결과이며 모든 보안 위험이 없다는 인증이 아니다.

## 5. 후보 ZIP 대조와 배포 한계

| 파일 | SHA256 |
|---|---|
| bootstrap-v0.1-candidate.zip | 94737a28c6ea41b359e277af21d4038ba8c860d915440016171d141e78ea4b95 |
| web-starter-v0.1-candidate.zip | 580f088d78b4571b931186dc57234f12cf62b4b0444d6b6e6957c970eb0a55d9 |

전달 ZIP은 기존 CI의 inner candidate ZIP을 바꾸지 않고 이름만 구분해 제공한다. 별도의 `.git` 디렉터리·node_modules·실제 계정정보는 포함하지 않는다. GitHub의 자동 생성 ZIP은 압축 바이트가 달라 이 checksum과 같을 필요가 없다.

Web Starter는 53개 파일의 바이트로 계산한 tree가 원격 tree와 일치했다. Bootstrap은 19개 중 8개가 바이트 일치하고 11개는 Windows 아카이브의 CRLF를 LF로 환원했을 때 해당 Git blob과 일치했다. 원래 Git의 bootstrap.bat은 CRLF 그대로 비교했다. 이 변환을 명시한 뒤 복원한 canonical tree가 원격 tree와 일치한다. Bootstrap ZIP 전체를 Git blob과 바이트 동일하다고 주장하지 않는다. 파일별 결과는 `CANDIDATE_MANIFEST_20260915.json`에 있다.

ZIP은 검토 후보이며 서명된 설치 프로그램이나 정식 릴리스가 아니다. CI artifact 보존기간이 끝나면 exact commit에서 새로 패키징하고 새 ZIP checksum을 기록한다. 기존 checksum을 새 ZIP에 그대로 적용하지 않는다.

## 6. WBS별 완료 수준

| WBS | 상태 |
|---|---|
| F01 | 정책·계획 보존 완료; 이번에 실제 구현 상태로 현행화 |
| B01–B04 | 코드 후보 및 작성자 parser/mock 검사 완료. 모든 실제 설치 경로 통과를 뜻하지 않음 |
| B05 | 안내/문제해결/후보 ZIP 작성. 깨끗한 PC와 기존 사용자 PC의 실제 설치 미실행 |
| W01–W02 | scaffold/lockfile/DEMO 및 작성자 CI 완료 |
| W03 | 인증/CRUD/RLS/private Storage 코드와 SQL fixture 완료. 실제 Supabase HTTP·계정 통합 미실행 |
| W04 | CI/온보딩/복구 문서 완료. 사용자 PC Bootstrap→Clone 통합, 실계정 연결은 미실행 |
| P01–P04 / S01–S03 | 아직 미구현. 실제 앱 저장소·계정 경계를 준비한 뒤 별도 제품으로 진행 |
| R01 | 두 구현 후보의 완료보고·대상 고정·IVA 입력 패킷 작성 |
| R02 / R03 | IVA NOT_RUN; 제품 병합·릴리스·템플릿 활성화·배포 NOT_DONE |

P/S가 미구현인 사실을 계정 없는 DEMO의 성공으로 가리지 않는다. mock 앱 개발 자체는 실계정 없이 가능하지만 실제 앱 대상 저장소의 소유자/공개범위는 이번 세 저장소의 실행 대상으로 확정되지 않았다. 공개 템플릿을 실제 앱으로 임의 전환하지 않는다.

## 7. 알려진 제한과 후속 순서

Bootstrap은 Node 공급자 최신판과 WinGet 제공판 차이를 문서화하고 카탈로그에 존재한 24.19.0을 신규 설치판으로 고정했다. 실사용 배포 전 공급판·보안 패치 현행성을 다시 확인해야 한다. 현재 성공한 mock 테스트는 이 버전의 사용자 PC 설치 증거가 아니다.

다음 순서는 별도 IVA의 고정 후보 검증 → 영향을 받는 범위의 조치 결정 → Windows 실제 설치 및 Supabase 실제 연결 확인 → 승인된 병합/Template 채택이다. 실제 앱은 별도 저장소에 P01/P02/S01 mock부터 개발하고, 계정 준비·실게시·자동 게시 활성화는 별도 경계를 지킨다.

현재 MITCHELL이 IVA로 이름만 바꿔 검증하지 않았다. PMO NOT_DISPATCHED, IVA NOT_RUN, 다른 Persona NOT_INSTALLED. 사용자 PC, Cloud, SNS, 과금, 저장소 visibility는 변경하지 않았다.

## 8. 재개와 사용자 행동

현재 두 PR의 Git 복구·완료보고 정리에 추가 저장소 생성이나 키 전달은 필요 없다. 다음 독립검증은 `IVA_REVIEW_PACKET_v0.1.md`를 별도 IVA 실행에 사용한다. 패킷 작성만으로 실행 예약·dispatch·검증 완료를 주장하지 않는다. 실제 앱 단계에서는 별도 저장소 생성/접근 준비를 한 번에 요청하고 기존 설계 질문은 반복하지 않는다.
