# MITCHELL — Bootstrap · Web Starter · Daily Photo Publisher 작업계획서

- 문서 ID: `MITCHELL-PLAN-001`
- 버전: `1.0`
- 작성일: `2026-09-15` / 기준 시간대: `Asia/Seoul`
- 작성 페르소나: `MITCHELL`
- 상태: `IMPLEMENTATION_PLAN_CANDIDATE` — 상세 구현에 사용할 계획이며, 코드 구현 완료나 실행 승인 자체가 아니다.
- 현 요청으로 수행하는 범위: 운영정책·정제 기억·계획 문서 작성 및 `AofSpds/mitchell` Git 반영.
- 수행하지 않는 범위: 실제 PC 설치, 서비스 가입·결제, SNS 게시, PMO/IVA 세션 실행, 제품 배포.

## 1. 목적과 성공 장면

개발 경험이 없는 사용자가 Windows PC에 필요한 도구를 설치하고, 기존 AI 구독으로 코드를 수정하며, 개발자가 GitHub에서 구조와 변경을 쉽게 확인할 수 있게 한다. 첫 실전 과제는 사진과 문구를 준비하여 SNS에 게시하고 오전 9시 예약 실행까지 연결하는 것이다.

```text
친구: Bootstrap ZIP 다운로드 → 압축 해제 → bootstrap.bat 실행
                                   ↓
                       설치 계획 확인 및 최초 동의
                                   ↓
                 Git / GitHub Desktop / VS Code / Node 준비
                                   ↓
                    GitHub 및 선택한 AI 계정 직접 로그인
                                   ↓
                Web Starter로 자기 프로젝트 생성 → Clone
                                   ↓
                    VS Code → 실행 → AI에게 작은 기능 요청
                                   ↓
                     브라우저 확인 → Commit → Push
                                   ↓
개발자: 같은 저장소에서 변경 확인 · 문제 재현 · 복구 지원
```

설치 실행을 단순화하되 UAC, 로그인, 약관 동의, 보안 경고, 계정 권한까지 무조건 없애는 무인 설치를 약속하지 않는다. Git은 코드 이력이고, 사진·운영 DB·토큰의 백업 수단이 아니다.

## 2. 현재 확인한 것과 아직 없는 것

| 항목 | 2026-09-15 직접 확인 또는 현재 요청의 의미 |
|---|---|
| `AofSpds/mitchell` | 최초 확인 시 Public·main·빈 저장소. 이번 문서화 작업의 대상 |
| `AofSpds/bootstrap` | Public·main·빈 저장소. 이번 계획 작성 중 제품 코드를 변경하지 않음 |
| `AofSpds/web-starter` | 계획상의 대상. 이번 작업에서 생성·존재 확인하지 않음 |
| 실제 앱 저장소 | 예정. 친구 계정과 SNS 운영계정은 아직 연결하지 않음 |
| MITCHELL | 사용자 지정 메인 대화 페르소나. 현재 문서/Git 작업 수행자 |
| PMO | 사용자 지정 Codex WORK 작업자. 역할 정의만 적용; 실행 세션 미호출 |
| IVA | 사용자 지정 독립 검증자. 역할 정의만 적용; 검증 미실행 |
| 다른 페르소나/페어 검증 | 미설치, 미호출. 자동 추가하지 않음 |
| ChatGPT 프로젝트 설정 | 등록용 지침 파일 제공. 실제 설정 화면의 변경은 확인하지 못함 |

HLOM에서 선택적으로 참조한 정책의 기준 commit은 `e01a99560dd7145c6643dd0769e01f43610c1d41`이다. CURRENT 파일 안의 과거 active 표시를 최신 main 사실과 혼동하지 않는다. 본 프로젝트는 HLOM의 조직 전체나 글로벌 Bootstrap 배포를 설치한 것이 아니라, 사용자 요청으로 필요한 원칙을 현지화한다.

## 3. 유지할 상위 요구사항과 구현 기본값

사용자가 지정한 이름·목표·분리 원칙을 보존한다. 이전 대화의 권고를 모두 사용자의 명시적 승인이라고 바꾸지 않는다. 아래 설계 기본값은 계획 승인 시 함께 채택할 제안이며, 그 후 같은 질문을 반복하지 않는다.

| ID | 요구/기본값 | 처리 |
|---|---|---|
| R01 | 완전 초보도 시작 가능하고 개발자가 봐줄 수 있을 것 | 한국어 온보딩, 표준 파일 구조, GitHub Desktop 중심 |
| R02 | Windows 초기 설치를 한 번에 묶을 것 | PowerShell + WinGet Bootstrap; 재실행 가능 |
| R03 | 설치 도구와 실제 프로젝트를 분리할 것 | bootstrap / web-starter / 실제 앱 경계 유지 |
| R04 | GitHub·VS Code·Node.js 기반 | 기본 도구로 유지; 강제 Cursor 구독 없음 |
| R05 | 기존 ChatGPT 또는 Claude 구독 활용 | 공식 코딩 확장 선택 설치; 로그인·사용 가능 범위는 해당 계정에서 확인 |
| R06 | Next.js + TypeScript + Tailwind + Supabase | 별도 Spring/FastAPI 백엔드 없이 시작 |
| R07 | 처음 필요한 패키지를 준비할 것 | Core 8종 + 명시적 Optional 목록; 모두 무조건 설치하지 않음 |
| R08 | 오전 9시 사진 게시 | KST 기준, 실행기·날짜창·중복방지·지연 정책을 함께 구현 |
| R09 | SNS 연동 확장 | 공통 인터페이스와 mock부터; 첫 실계정 검증은 Instagram, 다음 Threads |
| R10 | MITCHELL / PMO / IVA만 사용 | 이름 정의와 실제 세션 실행을 분리; 다른 조직 이식 금지 |
| R11 | 반복 확인 최소화 | 기본값·허용범위·중단조건을 한 계획으로 승인 |
| R12 | 현재 채널에서 Git 작업 진행 | 현재 문서 단계는 MITCHELL 직접 완료. PMO로 자동 이관하지 않음 |

### 이전 상위안의 보완 사항

1. `AofSpds/mitchell`은 새 운영·계획 허브다. 기존 `bootstrap`을 이름만 바꾼 저장소가 아니다.
2. Next.js 프로젝트를 생성해도 Supabase의 Cloud 프로젝트·사용자·권한까지 자동 생성되지는 않는다. 공식 스타터의 공개 예제 RLS를 개인 사진 서비스에 그대로 적용하지 않는다.
3. 예약 실행은 화면/Server Action 자체가 아니라 별도 작업 실행기가 담당한다. 브라우저를 열어 놓을 필요가 없는 구조로 만든다.
4. 중복 게시 방지·RLS·비밀키 분리는 마지막 안정화가 아니라 첫 실제 게시 전 필수조건이다.
5. SNS별 결과를 따로 저장한다. Instagram 성공·Threads 실패를 전체 성공으로 표시하지 않는다.
6. 그날 오전 9시에는 그날 오후 사진이 존재하지 않는다. 기본 날짜창은 `당일 00:00 ≤ 촬영시각 < 09:00`으로 설계한다. `전일 전체`는 선택 옵션이며 사용자 요구를 몰래 전일로 바꾸지 않는다.
7. 초보자의 첫 실행은 Supabase·SNS 키가 없어도 가능한 DEMO 모드로 한다. DEMO를 실제 저장·게시 성공으로 표시하지 않는다.
8. 배포 무료 티어와 정확한 정시 실행을 동일시하지 않는다. Vercel Hobby Cron은 정시 호출을 보장하지 않아 초기 정시 실행기로 쓰지 않는다.

## 4. 저장소 및 실행 아키텍처

```text
                      운영·계획 계층
                 AofSpds/mitchell
          현재 / 정책 / 결정 / 기억 / 계획 / 결과
                         |
               제품 코드는 아래에 분리
                         |
     +-------------------+--------------------+
     |                                        |
AofSpds/bootstrap                      AofSpds/web-starter
Windows 도구 설치                     Next.js 표준 템플릿
     |                                        |
     +-------------- 개발 준비 ---------------+
                                              |
                           실제 앱: daily-photo-publisher
                                              |
             +--------------------------------+-------------------+
             |                                |                   |
       Next.js 관리화면                 공유 서버 로직         로컬 실행기
   React / TS / Tailwind                사진·예약·게시          Windows 09:00
             |                                |                   |
             +--------------------+-----------+-------------------+
                                  |
                            Supabase Cloud
                       Auth / PostgreSQL / Storage
                                  |
                        SNS adapter → 공식 API
```

초기 실제 앱 저장소는 비공개 권고다. 운영 hub·설치기·템플릿의 공개 여부와 사용자의 사진 서비스 공개 여부는 별개의 결정이다. MITCHELL hub를 친구에게 넘길 때 내부 운영정책 전체를 친구의 앱 Persona로 자동 복사하지 않는다.

### 개발 경로와 운영 경로 분리

```text
개발: 요구사항 → 코드 수정 → 테스트 → Commit/Push → 리뷰 → 승인된 반영
운영: 사진 준비 → 예약 → 실행기 → SNS 요청 → 게시 결과/이력
```

게시 성공 후 Git commit을 자동 생성하지 않는다. 운영 데이터는 DB·Storage에 남는다.

## 5. 조직·승인·검증 Flow

```text
사용자: 목적·경계 지정
          ↓
MITCHELL: 계획·기본값·완료기준·위험 정리
          ↓
계획/실행범위 승인 한 번
          ↓
현재 집행 경로: MITCHELL 직접 Git 작업
(명시적인 PMO 전환 지시가 있을 때만 PMO 실행 경로로 변경)
          ↓
승인된 작업 → 작성자 자체 점검 → 완료보고 → 대상 commit/tree 고정
          ↓
IVA: 별도 세션에서 해당 대상의 최초 독립검증
          ↓
MITCHELL: 보고서 원문 보존·결과 정리
          ↓
사용자: 필요한 병합·실사용·공개 배포 경계 승인
```

MITCHELL은 현재 작성한 계획을 IVA의 이름으로 자가 승인하지 않는다. IVA가 아직 가동되지 않으면 `NOT_RUN`이다. PMO/IVA 정의 문서나 프롬프트 생성만으로 설치·실행·PASS를 주장하지 않는다.

검증 실패 시 무한 수정 루프를 만들지 않는다. 작성자의 진행 중 자체 점검과 완료 후 별도 correction을 구분한다. 완료 후 correction·affected-only 재검증은 범위가 묶인 승인 대상이며, 계획 승인과 함께 구체적으로 사전 승인되지 않았다면 자동 실행하지 않는다. 같은 원인의 무정보 재시도는 두 번에서 멈추고 원인·효과·필요 입력을 정리한다. 이미 성공한 외부효과는 다시 만들지 않는다.

## 6. Bootstrap 제품 스펙

### 6.1 대상과 패키지

초기 검증 기준은 Windows 11 x64, 개인 PC, 온라인 설치다. ARM64·회사 관리 PC·오프라인 환경은 감지하고 지원 미검증을 표시한다. WSL·가상화 기능을 자동 활성화하지 않는다. Windows PowerShell 5.1을 진입점으로 사용하므로 PowerShell 7이 없는 PC에서도 시작할 수 있어야 한다.

| 그룹 | 도구 | WinGet ID 후보 / 처리 |
|---|---|---|
| Core | Git for Windows | `Git.Git` |
| Core | GitHub Desktop | `GitHub.GitHubDesktop` |
| Core | GitHub CLI | `GitHub.cli` |
| Core | VS Code | `Microsoft.VisualStudioCode` |
| Core | Node.js LTS | `OpenJS.NodeJS.LTS`; npm/npx 별도 중복 설치하지 않음 |
| Core | PowerShell 7 | `Microsoft.PowerShell` |
| Core | Windows Terminal | `Microsoft.WindowsTerminal` |
| Core | 7-Zip | `7zip.7zip` |
| Optional | Python / JDK / Docker Desktop / DBeaver | 기본 OFF; 지원되는 ID·라이선스·선행조건을 B01에서 확정 |
| Optional | Codex / Claude Code | 공식 VS Code 확장 중심; None / Codex / Claude / Both 선택 |

위 ID는 구현자가 공식 manifest 및 `winget show --id ... --exact --source winget`으로 검증할 설치 대상 목록이다. 이 계획 작성 환경에서 Windows 설치를 실제 확인한 것으로 보지 않는다. Python/JDK의 지원 버전과 AI 확장 publisher/ID도 B01에서 고정한다. Docker는 라이선스·가상화·재부팅 조건 설명 후 별도 선택이 있어야 한다.

버전 정책: 최초 구현 시 공식 지원 Node LTS major와 패치·Next.js 안정판의 호환성을 확인하여 기록한다. 프로젝트는 `package-lock.json`을 커밋하고 이후 `npm ci`를 사용한다. 설치기의 릴리스 시험에 사용한 버전은 검증 기록에 고정한다. 기존 설치 버전은 무조건 최신으로 덮지 않고 호환 확인 후 SKIP 또는 조치 필요를 반환한다.

### 6.2 사용자 실행 Flow

```text
ZIP 해제 → bootstrap.bat
  → OS/CPU/PowerShell/WinGet/네트워크 검사
  → 기존 설치 탐지 및 설치 예정 목록 표시
  → Core + AI + Optional 선택을 한 번 수집
  → 설치·약관·관리자 권한 필요사항 안내 및 동의
  → 패키지별 순차 설치
  → 현재 프로세스 PATH 갱신 / 실행파일 직접 탐지
  → 버전·실행 확인
  → 성공 / 일부 실패 / 사용자 조치 필요 요약
  → GitHub 및 AI 로그인 안내
```

WinGet이 없거나 조직 정책으로 차단되어 있으면 공식 App Installer 복구 안내에서 멈춘다. 임의 출처의 설치 관리자를 대신 내려받지 않는다. 관리자 권한은 필요한 설치기가 요청하도록 하고 Bootstrap 전체를 항상 관리자 모드로 실행하지 않는다.

### 6.3 파일과 책임

```text
bootstrap/
  bootstrap.bat                   최초 진입·종료코드 전달
  bootstrap.ps1                   모드·프로파일·전체 순서
  config/packages.psd1            패키지 ID·범위·탐지·검증 정의
  scripts/preflight.ps1           OS / WinGet / 권한 / 네트워크
  scripts/install-packages.ps1    탐지·설치·종료코드 처리
  scripts/install-ai.ps1          공식 확장 선택 설치
  scripts/verify.ps1              실행파일·버전·환경 검증
  scripts/common.ps1              로그 마스킹·PATH·결과 객체
  tests/                         파서·mock 설치·재실행 테스트
  docs/FIRST_RUN.md               한국어 첫 실행 안내
  docs/TROUBLESHOOTING.md         막힌 화면별 해결 안내
  docs/TESTED_VERSIONS.md         실제 검증 환경/버전
  README.md / AGENTS.md / .gitignore
```

작은 모듈 단위만 분리한다. 플러그인 프레임워크·자체 패키지 매니저·DSC 플랫폼은 만들지 않는다.

### 6.4 인터페이스 계약

계획용 인터페이스이며 구현 파일이 이미 있다는 뜻은 아니다.

```powershell
.\bootstrap.ps1 -Mode Plan
.\bootstrap.ps1 -Mode Install -Profile Core -AI Codex
.\bootstrap.ps1 -Mode Verify
```

`Plan`·`Verify`는 설치·업그레이드·시스템 설정 변경을 하지 않는다. `Install`은 선택 목록과 동의를 확정한 후 실행한다. 비대화 모드는 선택값과 동의 플래그가 모두 있는 경우만 허용한다. 모든 명령은 파일 경로의 공백·한글·특수문자를 처리해야 한다.

패키지 결과는 `PRESENT_COMPATIBLE`, `INSTALLED`, `SKIPPED`, `FAILED`, `ACTION_REQUIRED`, `REBOOT_REQUIRED`로 구분한다. 전체 종료코드는 0=완료, 1=설치/검증 실패, 2=사용자 조치 필요로 통일하고, 원래 설치기 종료코드는 별도 필드로 보존한다. 재부팅은 자동 실행하지 않는다.

권장 결과 객체: `packageId, selected, detectedVersion, requestedVersion, action, status, installerExitCode, verification, nextAction`. 로그는 `%LOCALAPPDATA%\MitchellBootstrap\logs` 등에 저장하고 공유본에서 사용자 경로·토큰을 제거한다. stdout의 특정 영문 문구 하나만으로 성공을 판정하지 않는다.

### 6.5 안전·복구 조건

- 공식 패키지 ID·source·publisher를 고정한다. 해시 검증 우회·SmartScreen/Defender 해제·영구 실행정책 완화를 하지 않는다.
- 원격 코드를 바로 `Invoke-Expression`으로 실행하는 방식은 기본 배포 방식에서 제외한다.
- 로컬 검토된 스크립트 실행에 필요한 경우에만 해당 PowerShell 프로세스 범위 옵션을 쓰고 의미를 안내한다. 기기 전체 정책은 바꾸지 않는다.
- `winget install`에는 지원되는 경우 `--no-upgrade`를 사용하고, 별도 탐지/호환 검사를 병행한다.
- Node가 오래됐거나 다른 배포판이 있으면 충돌 정보를 보여주고 자동 제거하지 않는다.
- 중간 실패 후 재실행하면 설치 완료 항목은 다시 변경하지 않는다. 이미 존재하던 소프트웨어를 복구 명목으로 제거하지 않는다.
- 자동 재부팅·글로벌 npm 도구 대량설치·사용자 PATH 전체 덮어쓰기·Git 전역 사용자명 변경을 하지 않는다.
- 프로젝트 복제·계정 로그인은 도구 설치와 구분한다. GitHub Desktop은 AI 실행기가 아니다.

## 7. Web Starter 제품 스펙

### 7.1 기본 선택

`Next.js App Router + React + TypeScript strict + Tailwind CSS + Supabase Cloud`를 사용한다. 패키지 매니저는 npm 한 종류다. shadcn/ui는 Button/Card/Input 등 실제 필요한 부품만 추가한다. 별도 API 서버, ORM, Redux, 로컬 DB, Docker는 기본 구성에 넣지 않는다.

공식 Supabase `with-supabase` 예제를 시작 근거로 삼되, upstream ref와 파일 변경을 기록하고 필요한 부분만 남긴다. 공식 Next.js 생성기가 포함하는 AGENTS/CLAUDE 지침이 있다면 검토하여 프로젝트 규칙과 병합한다. 한쪽을 무조건 덮어쓰지 않는다. `CLAUDE.md`는 AGENTS를 참조하는 연결 파일이며 새 Persona의 설치가 아니다.

### 7.2 첫 실행과 데이터 연결

```text
Template로 자기 저장소 생성 → Clone → npm ci → npm run dev
                                                   |
                      +----------------------------+------------------+
                      |                                               |
                 설정 없음                                        설정 완료
                 DEMO 화면                               Supabase Auth·데이터 연동
             실제 저장/게시 불가 표시                       RLS 적용 후 통합 테스트
```

Supabase 프로젝트 생성, 계정 로그인, 키 등록은 별도 온보딩 단계다. DEMO 모드는 가짜 성공 응답을 실서비스 응답으로 혼용하지 않는다. API 키가 없어서 첫 페이지나 CI build 전체가 즉시 실패하는 구조를 피한다.

### 7.3 파일 구성과 스크립트

```text
web-starter/
  src/app/                        페이지·레이아웃·서버 엔드포인트
  src/components/                 공통 UI
  src/lib/env.ts                  공개/서버 환경변수 구분과 검증
  src/lib/supabase/client.ts      브라우저 클라이언트
  src/lib/supabase/server.ts      사용자 세션을 검증하는 서버 클라이언트
  src/lib/supabase/                선택 버전의 세션 갱신 구현
  supabase/migrations/             최소 예제 테이블·권한·RLS
  tests/                          환경·인증·권한·UI smoke
  docs/                           시작·복구·요구사항 작성법
  .vscode/extensions.json         최소 권장 확장
  .env.example / package.json / package-lock.json
  AGENTS.md / CLAUDE.md / README.md / .gitignore
```

공통 명령은 `dev`, `build`, `start`, `lint`, `typecheck`, `test`로 고정한다. `check`는 lint → typecheck → test → build를 호출한다. 단위 테스트는 Vitest, 핵심 브라우저 smoke는 Playwright로 제안한다. 이 Playwright는 앱 테스트 도구이며 네이버 자동 게시 기능의 설치를 뜻하지 않는다.

첫 DB 예제는 사용자 소유 `notes` 같은 최소 CRUD다. 사용자 A가 B의 행·파일을 조회·수정·삭제하지 못해야 한다. 로그인 실패, 세션 만료, 로그아웃 상태도 검증한다. 초기 앱은 승인된 단일 사용자를 대상으로 하며 공개 회원가입 서비스를 기본 개방하지 않는다.

### 7.4 비밀정보와 권한

Supabase의 publishable key는 브라우저 사용을 위한 키이고 secret/service-role 계열은 서버 전용이다. key 하나로 로그인 사용자 권한을 대신하지 않는다. 서버 로직은 사용자 인증과 소유권을 직접 검증하며 RLS를 우회하는 키를 브라우저에 넣지 않는다.

`NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`와 서버 전용 비밀변수를 구분한다. Git에는 placeholder가 있는 `.env.example`만 둔다. 사진 Storage는 private을 기본으로 한다. 사용자 기기마다 본인의 계정·키를 설정하며 개발자의 서비스 키를 친구에게 배포하지 않는다.

## 8. 첫 앱: Daily Photo Publisher 스펙

### 8.1 v1 수직 기능 범위

첫 완성 단위는 한 사용자, 한 SNS 계정, 이미지 한 장을 가진 게시물 하나다. 사용자가 선택한 사진과 문구를 실제 게시하고 결과를 확인하는 흐름을 먼저 완성한 뒤 Threads를 붙인다. 동영상·릴스·캐러셀·여러 고객 계정·과금·대량 발행은 후속 범위다.

```text
사진 수동 업로드 / 승인된 전용 폴더에서 수집
           ↓
촬영시각·형식·크기 검증 / 원본 hash / 게시용 이미지 생성
           ↓
비공개 Storage 저장 → 화면에서 사진·문구·대상 계정 확인
           ↓
DRAFT → 사용자 확인 → SCHEDULED 또는 지금 게시
           ↓
공유 게시 서비스가 대상별 작업을 원자적으로 점유
           ↓
공식 SNS API에 전송 → remote ID 확인 → 대상별 상태 기록
           ↓
성공 / 부분 성공 / 실패 / 수동확인 필요를 구분하여 표시
```

폰 전체 사진을 자동으로 공개 대상으로 삼지 않는다. 자동수집은 사용자가 지정한 게시 전용 폴더만 읽고 원본을 이동·삭제하지 않는다. OneDrive 등 동기화 서비스는 선택 입력 경로이며 특정 유료 서비스 가입을 강제하지 않는다.

### 8.2 화면

`/` 대시보드, `/photos` 사진 선택, `/posts/new` 문구·대상·시간 설정, `/history` SNS별 결과, `/settings` 계정·시간대·자동모드 설정을 제공한다. 개발 초기에는 mock adapter로 전 과정을 시험하며 화면에 SIMULATION을 표시한다. 연결하지 않은 SNS는 비활성으로 표시하고 작동하는 척하는 빈 adapter를 만들지 않는다.

### 8.3 날짜·이미지 정책

- 시간대는 `Asia/Seoul`. DB 저장은 UTC `timestamptz`, 표시와 날짜창 계산은 KST다.
- 기본 날짜창은 당일 00:00 이상 09:00 미만. 전일 모드는 별도 설정값이다.
- EXIF 촬영시각을 우선한다. 시간대가 없으면 설정 시간대를 적용했다는 provenance를 기록한다. 촬영시각이 없거나 모호하면 사용자 확인 전 자동선정에서 제외한다. 파일 수정시각을 촬영시각이라고 조용히 대체하지 않는다.
- 원본 hash로 중복 유입을 식별한다. 선택 순서는 촬영시각·hash로 결정적으로 정한다. 일일 자동 게시 한도 기본값은 1건이며 화면에서 확인하고 설정할 수 있게 한다.
- 선택 사진이 없으면 `NO_PHOTOS`로 실행만 기록하고 빈 글을 발행하지 않는다.
- 원본은 변경하지 않고 게시용 사본에서 GPS 등 불필요한 EXIF를 제거한다. v1 입력은 JPEG/PNG, 게시용은 adapter가 요구하는 형식으로 검증·변환한다. HEIC 미지원은 명확히 안내한다.
- 이미지 크기·비율·파일 제한은 선택한 공식 API 버전에 맞춰 adapter capability에서 검증한다. 이 계획에 확인되지 않은 플랫폼 제한 숫자를 고정하지 않는다.

### 8.4 최소 데이터 모델

아래는 구현용 제안 스키마다. 실제 DB에 이미 존재한다는 뜻이 아니다.

| 엔터티 | 핵심 필드와 제약 |
|---|---|
| photos | id, owner_id, original_hash, storage_path, captured_at, capture_time_source, timezone_assumption, created_at; owner/hash unique |
| posts | id, owner_id, caption, source_date, scheduled_at, timezone, created_at; 본문과 예약 의도 |
| post_photos | post_id, photo_id, display_order; v1은 사진 1장, 소유자 일치 검증 |
| publication_targets | id, owner_id, provider, external_account_id, enabled; 토큰 원문은 저장하지 않음 |
| deliveries | id, post_id, target_id, status, attempt_count, remote_container_id, remote_post_id, last_error_code, lease_owner, lease_until; post/target unique |
| job_runs | id, schedule_key, started_at, completed_at, mode, outcome, summary; 중복 실행 추적 |
| schedule_settings | owner_id, timezone, local_time, photo_window, daily_limit, enabled; 기본 enabled=false |

게시물 전체의 요약 상태는 deliveries로 계산한다. 일부 성공 시 PARTIAL을 표시한다. 단일 `posts.status=SUCCESS`로 여러 SNS의 서로 다른 결과를 숨기지 않는다. 사진·게시물·대상 간 참조에서 다른 사용자의 ID를 조합할 수 없도록 서버 검증과 DB 제약을 함께 설계한다.

### 8.5 상태·재시도·중복 방지

```text
DRAFT → SCHEDULED → PUBLISHING → SUCCESS
                        |             
                        +→ FAILED
                        +→ MANUAL_REQUIRED
DRAFT/SCHEDULED → CANCELLED
```

`SUCCESS`는 provider가 돌려준 실제 식별자와 확인 가능한 결과가 있어야 한다. mock 성공과 다르다. 취소는 미발행 예약을 중단하는 것이며 이미 공개된 글의 자동 삭제를 뜻하지 않는다.

작업 점유는 DB transaction/RPC 등으로 원자적으로 수행한다. 수동 버튼·스케줄러·재실행이 동시에 와도 동일 delivery를 중복 실행하지 않는다. 네트워크 호출 전 attempt를 기록하고 provider container/post ID를 얻는 즉시 저장한다.

외부 게시와 DB 기록을 하나의 분산 트랜잭션으로 묶을 수 있다고 가정하지 않는다. 요청 전 실패처럼 게시효과가 없다고 확인된 경우에만 제한적으로 재시도한다. 기본 상한은 최초 1회 + 재시도 2회이며 provider Retry-After를 존중한다. 요청 후 timeout, 응답 손실, 게시 후 DB 기록 실패는 먼저 provider 조회로 조정한다. 확인할 수 없으면 MANUAL_REQUIRED로 보내고 무조건 다시 게시하지 않는다. exactly-once를 무근거로 보장하지 않는다.

### 8.6 Adapter 계약

`Publisher`는 최소 `checkConnection`, `validateMedia`, `publish`, `reconcile`를 제공한다. 공통 결과는 provider, accountId, remoteContainerId, remotePostId, outcome, retryable, errorCode를 포함한다. 플랫폼별 OAuth·권한·제한·이미지 제공방식은 adapter 내부 책임이다.

우선순위는 mock → Instagram → Threads다. Facebook Page, X, 네이버 블로그, 티스토리는 후속 후보로 보존한다. Meta 공식 자료에서 Instagram 프로페셔널 계정과 게시 API를 확인했지만 실제 계정의 이용 자격·권한·앱 심사는 별도로 검증한다. Instagram Login과 Facebook Login 방식을 혼합하지 않는다. Threads도 별도 앱 설정·권한·토큰을 확인하고 Instagram 토큰을 그대로 사용할 수 있다고 가정하지 않는다.

이미지 URL을 provider가 가져가는 방식이면 짧은 유효기간의 접근 가능한 HTTPS URL을 제공하고 실제 fetch를 검증한다. 전체 bucket을 public으로 전환하지 않는다. 서명 URL과 토큰을 일반 로그에 남기지 않는다.

티스토리 Open API 종료는 공식 공지로 재확인했다. 네이버 종료 공지는 이번 읽기에서 제목만 회수되어 상세 재확인을 후속 연동의 선행조건으로 남겼다. 비공개 endpoint 추측, CAPTCHA 우회, anti-detection 구현은 하지 않는다. 네이버/티스토리 UI 자동화는 이번 v1의 필수 완료범위가 아니다.

### 8.7 코드 구조

```text
daily-photo-publisher/
  src/app/                         관리 UI·사용자 요청 경계
  src/lib/photos/                  입력·EXIF·hash·게시용 이미지
  src/lib/publishers/               interface / mock / instagram / threads
  src/lib/publications/            검증·점유·게시·결과 조정
  src/lib/scheduling/              KST 날짜창·due 계산·지연 정책
  src/lib/supabase/                데이터 접근
  scripts/run-scheduled.ts         웹 UI 없는 독립 실행 진입점
  scripts/install-task.ps1         예약 작업 설치·설정 검증
  scripts/remove-task.ps1          이 프로젝트가 만든 예약만 제거
  supabase/migrations/              테이블·제약·RLS·작업 점유 함수
  tests/fixtures/                  합성 사진/가짜 토큰만
  docs/                           시작·SNS 연결·운영·사고 복구
```

SNS token은 client component나 NEXT_PUBLIC 변수로 가지 않는다. 초기에는 계정 소유자가 관리하는 서버 전용 설정으로 주입한다. 상시 실행기의 운영 비밀정보는 사용자 범위의 안전한 저장소 또는 암호화 저장을 사용한다. 멀티사용자 OAuth 토큰 보관 시스템은 v1 단일 사용자 범위를 넘어 별도 설계한다.

## 9. 오전 9시 Scheduler 스펙

### 9.1 초기 로컬 실행

```text
Windows 작업 스케줄러, KST 09:00
       ↓
고정된 프로젝트 경로 / Node 실행파일 / 실행 스크립트
       ↓
설정·자격증명·시간대·예약 활성화 확인
       ↓
필요 시 승인된 입력 폴더 수집 → 날짜창 계산
       ↓
오늘 실행 여부·중복 점유 확인
       ↓
게시 대상 준비 → 공유 publication 서비스
       ↓
provider 결과 조정 → DB 기록 → 실행 종료
```

Next.js 개발서버나 브라우저를 열어 놓아야 실행되는 방식으로 만들지 않는다. 테스트용 `run-scheduled`는 기본 dry-run이고, 실제 전송은 별도 명시적 실행 옵션과 활성화 설정이 모두 있을 때만 허용한다.

예약 설치 시 작업명·경로·사용자·시간대·동작·네트워크 필요·중복 실행 정책을 표시한다. Windows의 표시용 시간대 ID와 앱의 IANA 시간대를 구분한다. PC 시간대를 몰래 바꾸지 않는다. 예약 작업에 비밀번호를 코드로 저장하지 않고, 로그온 여부에 따른 실행 가능성을 실제 시험한다.

기본 지연 정책은 예정 시각 후 30분까지 같은 날짜의 누락 작업 1회 허용, 그 이후에는 MANUAL_REQUIRED다. 이는 계획상의 운영 기본값이다. 원본 09:00 cutoff는 지연 실행 시각으로 늘리지 않는다. PC가 꺼져 있으면 정확히 09:00에 실행할 수 없다는 한계를 문서와 화면에 표시한다. 다음날 전날 누락분을 자동으로 대량 몰아 올리지 않는다.

### 9.2 Cloud 전환

Vercel은 Next.js 웹 배포 후보이며 초기 예약 실행기의 필수조건이 아니다. 공식 문서상 Hobby Cron에는 시간 단위 정밀도 제한이 있으므로 정확한 오전 9시 용도로 단정하지 않는다. Cloud 실행이 필요해지면 가격·실행 정밀도·타임아웃·중복 호출·이미지 접근·secret 저장 조건을 재검증한다. 로컬과 Cloud를 동시에 active scheduler로 켜지 않는다.

정시 기준은 '작업 호출 시각'과 'SNS에 실제 공개된 시각'을 따로 측정한다. 네트워크와 provider 처리까지 포함하여 초 단위 게시 완료를 보장하지 않는다.

## 10. 구현 WBS와 선후관계

일정 추정치보다 산출물·의존성·완료기준을 먼저 고정한다. 표의 담당 실행은 현재 MITCHELL 경로이며, 사용자가 PMO로 전환하면 같은 패킷을 PMO에 넘긴다. 재계획이나 역할 추가를 기본 요구로 하지 않는다.

| WBS | 작업·주요 산출물 | 선행 | 완료기준 |
|---|---|---|---|
| F01 | 운영정책·현재·결정·기억·단일 계획서 Git 등록 | 현재 요청 | 파일 전체 및 remote head/tree readback, 코드 미구현 표시 |
| B01 | 공식 패키지 ID·버전·확장 publisher 조사; packages.psd1 정의 | 실행범위 승인 | 모든 선택 항목에 source/탐지/검증/권한/제약 명시 |
| B02 | bat/PS 진입점·Plan·Verify·preflight 구현 | B01 | Node/Git/PS7 없는 조건에서 파서·진입점 작동 |
| B03 | 설치·이미 설치 탐지·PATH·종료코드·재실행 구현 | B02 | mock 설치기 시험 및 무변경 재실행 확인 |
| B04 | AI/Optional 선택·공식 설치 경로·로그 마스킹 | B03 | None/단일/Both·취소·실패 처리, 암묵적 로그인/결제 없음 |
| B05 | 한국어 안내·문제해결·Windows 시험·릴리스 후보 ZIP | B04 | 깨끗한 Windows와 기존설치 PC의 실제 결과 구분·기록 |
| W01 | 공식 예제 기준 Next/TS/Tailwind/npm scaffold | B01 | upstream/버전 기록, lockfile, dev/build 동작 |
| W02 | 키 없는 DEMO·env 검증·기본 UI·AI 안내 | W01 | 계정 미연결 상태도 첫 화면, DEMO 표시 |
| W03 | Supabase 사용자 세션·예제 CRUD·RLS·private storage 패턴 | W02 + 테스트 계정 준비 | A/B/anon 권한 시험, secret 브라우저 미노출 |
| W04 | 테스트·최소 CI·템플릿 온보딩·복구 안내 | W03 | 새 clone의 npm ci/check 및 Git 복구 재현 |
| P01 | 사진 업로드·EXIF·hash·사본처리·사진 선택 화면 | W04 | 날짜/형식/소유자·민감 metadata 검사 |
| P02 | posts/deliveries/job_runs와 예약 모델·mock adapter | P01 | mock end-to-end, partial 결과·중복 점유·취소 시험 |
| P03 | Instagram 연결 점검·권한/이미지 계약·실제 adapter | P02 + 계정 자격 확인 | mock contract + 승인된 단일 실제 게시 증거 |
| P04 | Threads adapter·대상별 결과 | P03 또는 Instagram 외부 대기 중 독립 작업 | 해당 계정 별도 권한 시험, 한쪽 실패가 다른 쪽을 재발행하지 않음 |
| S01 | 독립 실행기·KST 날짜창·입력폴더·dry-run | P02 | 웹서버 없이 mock 예약 실행, cutoff·지연 시험 |
| S02 | Windows 예약 설치/해제·실행 중복·지연·상태 기록 | S01 + 첫 adapter | 설치 XML/설정 readback, 실제 계정 실행 전 dry-run 통과 |
| S03 | 단일 사진 실제 정시 pilot | S02 + 실사용 활성화 승인 | 승인된 대상만 1회 게시, remote ID/시각/로그 대조 |
| R01 | 완료보고·대상 freeze·IVA 검증 패킷 | 해당 묶음 구현 종료 | 대상 SHA/tree·테스트·미검증·한계가 고정됨 |
| R02 | IVA 최초 독립검증 → 결과 정리 | R01 + IVA 활성화 | 요구/권한/복구/주장범위 판단; NOT_RUN과 PASS 구분 |
| R03 | 승인된 병합·릴리스·친구 온보딩 | R02 + 해당 경계 승인 | 지정 ref 반영 readback, 설치 ZIP/checksum, 운영 한계 안내 |

실행 묶음은 ① B01–B05 Bootstrap, ② W01–W04 Web Starter, ③ P01–P02+S01 mock 앱, ④ P03–P04+S02–S03 실계정·예약이다. 각 묶음은 완료보고로 닫고 자기 검증을 독립검증이라고 부르지 않는다. 인접 묶음이 외부 계정을 기다릴 때 의존하지 않는 mock·문서·테스트 작업은 계속할 수 있다.

## 11. 검증 위치와 시험표

| 지점 | 자체 점검 | IVA가 볼 핵심 | 사용자 확인이 필요한 것 |
|---|---|---|---|
| Bootstrap 후보 | 파서·모듈·mock 설치·Windows 실행 증거 | 설치 안전, 재실행, 실패 복구, 버전 주장 | 본인 PC 설치 허용/UAC; 문항별 설계 재승인 아님 |
| Web Starter 후보 | lint/typecheck/test/build·인증 smoke | 초보 시작 가능, RLS, secret 경계 | 테스트 Cloud 계정/키 등록 |
| Mock 앱 후보 | 사진·상태·중복·시간대·실행기 | 원래 요구 유지, 실패·불명효과 정책 | 화면/사진 선정이 의도와 맞는지 1회 시연 |
| 실게시 후보 | 권한·형식·단일 승인 전송 | 계정/사진/공개 범위, 결과 증거 | 지정 테스트 사진·대상 계정의 실제 게시 |
| 예약 pilot | dry-run·작업설정 readback | 중복·지연·중단·운영한계 | 매일 자동 게시 활성화 범위 |

시험 케이스는 계획상의 요구이며 아직 실행 결과가 아니다.

| ID | 재현 조건 | 기대 결과 |
|---|---|---|
| T01 | Git·Node·PS7 없는 Windows | Bootstrap 진입 가능, 필요한 항목만 설치 예정 |
| T02 | 모든 패키지 호환 버전 설치됨 | 설치·업그레이드 없이 SKIP |
| T03 | 구버전 Node 또는 다른 설치경로 | 충돌/호환 정보, 자동 제거 없음 |
| T04 | WinGet 없음·UAC 거절·오프라인 | ACTION_REQUIRED/실패 원인과 재개 지점 |
| T05 | 경로 공백·한글·ZIP 미해제 | 정상 처리 또는 구체적 해제 안내 |
| T06 | 중간 설치 실패 후 재실행 | 완료 항목 유지, 실패 항목만 처리 |
| T07 | AI None / Codex / Claude / Both | 선택만 설치, 로그인·결제는 자동 수행 안 함 |
| T08 | 프로젝트 env 없음 | DEMO 페이지, 실제 저장/게시 버튼 비활성 |
| T09 | 사용자 A/B/비로그인 | 다른 사용자 행/사진 접근·ID 조합 차단 |
| T10 | 브라우저 번들·로그·Git diff 검사 | server secret/SNS token/개인 사진 원문 없음 |
| T11 | 00:00·08:59:59·09:00·전일 사진 | 날짜창 경계 정확, 09:00 이후 자동 제외 |
| T12 | EXIF 없음·시간대 모호·HEIC | 자동 공개하지 않고 확인/미지원 안내 |
| T13 | 같은 사진 재수집·버튼 중복 클릭 | hash 및 delivery 제약으로 중복 방지 |
| T14 | 스케줄러와 수동 게시 동시 호출 | 같은 delivery는 하나만 점유 |
| T15 | Instagram 성공·Threads 실패 | PARTIAL, 성공한 대상 재발행 없음 |
| T16 | 요청 후 timeout / DB 기록 실패 | 먼저 조정, 확인 불가 시 MANUAL_REQUIRED |
| T17 | 429·토큰 만료·권한 부족 | 제한된 안전 재시도 또는 재로그인 안내 |
| T18 | 사진 없음·파일 동기화 미완료 | 빈 글 발행 없음, 처리 이유 기록 |
| T19 | 09:10 재개 / 11:00 재개 / 다음날 재개 | 허용 지연만 1회; 늦거나 지난 작업은 수동 확인 |
| T20 | Next 개발서버/브라우저 종료 | 독립 실행기는 정상 작동 |
| T21 | 실제 SNS 단일 게시 | remote ID와 실제 계정의 게시물 대조 |
| T22 | 예약 삭제·자동모드 OFF | 해당 앱의 미래 작업만 중단, 기존 공개 글은 삭제 안 함 |

정적 검사·mock PASS, Windows 실기 PASS, Cloud 통합 PASS, 실게시 PASS는 각각 독립 증거다. Linux 환경의 코드 검사만으로 Windows 설치 성공을 선언하지 않는다. IVA가 실행할 수 없는 환경은 `INDETERMINATE` 또는 해당 항목 `NOT_RUN`으로 보존한다.

## 12. Git·문서·기억 반영 규칙

현재 foundation 문서는 사용자 요청에 따라 MITCHELL이 빈 `mitchell/main`에 초기 등록한다. 이는 계획 문서를 보관하는 작업이며 구현 승인·IVA PASS·제품 release가 아니다. 이후 제품 코드 변경은 작업 브랜치와 PR을 기본으로 하며 공유 이력을 force-push하거나 reset --hard로 복구하지 않는다.

작업 시작 시 target repo·base ref·현재 head·dirty 여부·허용 경로를 읽는다. 작업 종료 시 변경 파일, 테스트 결과, 미검증 항목, commit/tree, 외부효과를 기록한다. remote head가 예상 parent와 다르면 먼저 조정한다. 서로 다른 저장소의 commit을 하나의 원자적 commit인 것처럼 취급하지 않는다.

기억은 `CURRENT.md`의 현재 상태, `DECISIONS.md`의 결정, `memory/MITCHELL.md`의 지속 맥락, `WORKLOG.md`의 사건을 분리한다. 매 대화마다 모든 문서를 수정하지 않고 의미 있는 작업 묶음 종료·승계·결정 변경 때 반영한다. 변경된 판단은 superseded 관계로 남긴다. 공개 저장소에는 정제된 요약만 올린다.

새 채널은 README → CURRENT → AGENTS → 필요한 결정/계획 → 기억·WORKLOG 최근 항목 순서로 읽는다. 충돌이 없으면 전 역사 재독을 요구하지 않는다. 현재 채널을 PMO/IVA로 바꿔 호출하지 않는다. 다른 페르소나는 별도 명시적 활성화 경로를 사용한다.

## 13. 승인과 사용자 개입을 최소화하는 경계

### 현재 승인된 행위

이 요청은 MITCHELL의 운영정책·정제 기억·작업계획서 작성 및 `AofSpds/mitchell` 문서 Git 반영을 허용한다. PC 설치·Cloud 환경 변경·제품 코드 실행으로 자동 확대하지 않는다.

### 계획 승인 시 한 번에 묶을 작업

실행자는 선택한 묶음의 파일 생성·일반 의존성 구성·로컬/mock 테스트·작업 브랜치와 PR 작성까지 같은 범위에서 진행한다. 파일명, 함수명, 테스트 케이스별로 사용자에게 다시 묻지 않는다. 새 저장소가 필요하면 이름·소유자·공개범위를 실행 승인에 포함한다. 생성 도구가 없을 때만 수동 생성 1건으로 묶어 요청한다.

### 사용자가 직접 해야 하는 최소 개입

1. 현재 ChatGPT 프로젝트 설정에 제공된 지침을 한 번 등록한다. Git 문서 반영과 별개다.
2. 초기 온보딩에서 GitHub·AI·Supabase·SNS 계정 로그인과 필요한 동의를 묶어서 수행한다. 비밀번호/토큰을 대화창에 붙여 넣도록 요구하지 않는다.
3. 실제 공개될 계정·사진·문구와 예약 활성화 범위를 한 번 확인한다. 활성화 후 동일 범위의 매일 게시마다 재승인을 요구하지 않는다.

새 유료 결제, 공개/비공개 변경, 파괴적 데이터 변경, 배포·예약 활성화, 범위 확대, 실패효과 불명확, 새 Persona 추가는 그대로 사용자 경계다. 권한 부족은 기술적 권한 요구이고 개발 판단 재확인과는 다르다.

## 14. 완료 정의와 후속 범위

| 완료 단계 | 반드시 있어야 하는 증거 |
|---|---|
| 계획 준비 | 단일 계획서·현재/결정/기억·지침·Git readback |
| Bootstrap 후보 | 반복 실행 가능한 설치기·검증·정제 로그·Windows 실기 결과 |
| Starter 후보 | 새 clone에서 DEMO·개발 실행·build·권한 시험·시작 안내 |
| 첫 앱 mock 후보 | 사진→예약→mock 게시→대상별 이력과 실패 처리 |
| 실연동 후보 | 승인된 실제 사진의 provider ID·게시물·계정 대조 |
| 예약 pilot | KST 09:00 실행 증거·중복/지연/중단 시험·운영 한계 |
| 배포 가능 | IVA 결과·사용자 disposition·승인 ref readback·온보딩 묶음 |

후속 범위는 Facebook Page, X, 네이버/티스토리 반자동 게시 검토, AI caption API, HEIC/동영상/캐러셀, Cloud scheduler, 여러 사용자/계정, 유료 운영이다. v1 완료를 위해 전부 선제 설치하지 않는다. 친구의 기존 AI 구독이 프로그램의 무제한 API 호출·호스팅 비용까지 포함한다고 가정하지 않는다.

## 15. 다음 작업자의 시작 지시

```text
PROJECT = MITCHELL
ENTRY = AofSpds/mitchell / README.md
PLAN = docs/IMPLEMENTATION_PLAN_v1.0.md

CURRENT와 AGENTS, DECISIONS를 먼저 읽고 현재 실행권한을 확인한다.
현재 단계는 계획 문서 준비다. 계획이 있다는 이유로 코드를 실행하지 않는다.
실행이 승인되면 지정 묶음의 WBS부터 시작하고 같은 설계 질문을 반복하지 않는다.
현재 Git 실행 주체는 MITCHELL이다. 명시적 PMO 전환이 있을 때만 PMO로 수행한다.
다른 Persona/페어 검증자를 추가하지 않는다.
완료 시 결과·고정 target·테스트·한계·외부효과·기억 delta를 한 번에 반환한다.
자체 점검을 IVA 검증으로 보고하지 않는다.
```

## 16. 근거와 검증 한계

공통 운영 원칙은 HLOM의 README, CURRENT, OWNER_INTERFACE, COMMON_KERNEL, PROJECT_PROFILE, CONTINUITY_CONTRACT에서 필요한 부분을 읽어 현지화했다. HLOM 전 규격의 적합성 인증이나 글로벌 배포 설치를 주장하지 않는다. 상세 pin·조회 범위와 기술 근거는 `docs/SOURCES.md`에 둔다.

주요 공식 근거: Microsoft WinGet install 문서, Next.js Installation 문서, Supabase Next.js Quickstart 및 API keys 문서, OpenAI Projects/Codex IDE 문서, Anthropic Claude Code VS Code 문서, Vercel Cron usage/pricing 문서, Meta의 공식 Postman Instagram/Threads 컬렉션, 티스토리 종료 공지다. Meta developer 본문 일부는 조회 실패하여 계정 연결 단계에서 해당 버전·권한·제한을 다시 확인한다.

이번 문서 작성은 구현·설치·독립검증 결과가 아니다. 사용자의 실제 계정·PC·사진을 사용한 검증은 각 WBS에서 별도로 증명한다.
