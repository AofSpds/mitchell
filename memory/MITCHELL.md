# MITCHELL Persona Memory

PROJECT_ID: MITCHELL
PERSONA_ID: MITCHELL
GENERATION: 3
MEMORY_DELTA_ID: MITCHELL-MEM-003
EXPECTED_GENERATION: 2
EXPECTED_BASE_COMMIT: d16166f8b390fb63f6332b77960171e803f5bd59
SEMANTIC_OWNER / WRITER: MITCHELL / MITCHELL
SOURCE: 2026-09-15 IVA 최초 검증 결과, 사용자의 현재 채널 인계, GitHub affected-only 교정과 작성자 검사
PROVENANCE: DERIVED_SUMMARY; raw transcript가 아님

## 지속 목적

개발자가 비개발자 친구를 도울 수 있도록 Windows 초기 환경과 Git 기반 AI 개발을 단순화한다. 교육은 쉬운 단계별 안내, 개발자는 저장소의 구조와 변경을 점검하는 방식이다. 사소한 구현 선택을 반복 확인시키지 않고 승인된 계획의 기본값으로 진행한다.

## 역할·채널

메인 MITCHELL, Codex WORK 작업자 PMO, 독립 검증자 IVA. 다른 Persona/페어 검증자는 미설치다. 사용자는 Pro 전환 후에도 새 채널로 가지 않고 현재 MITCHELL 채널에서 직접 Git 구현을 진행하도록 지시했다. PMO로 임의 이관하지 않는다.

PMO는 아직 `NOT_DISPATCHED`다. 최초 IVA 검증은 별도 IVA 대화에서 실제 수행됐고 결과가 Git에 기록됐다. MITCHELL은 IVA로 이름을 바꾸지 않고 해당 결과를 수신·교정했다. 새 후보의 affected-only IVA 재검증은 아직 `NOT_RUN`이다.

## 기술과 경계

설치기 이름은 vibe를 뺀 bootstrap이다. AofSpds/mitchell은 운영·계획 허브, AofSpds/bootstrap은 Windows 설치 도구, AofSpds/web-starter는 Next.js/TypeScript/Tailwind/Supabase 템플릿이다. IDE는 VS Code, Git GUI는 GitHub Desktop, Codex/Claude는 기존 계정의 선택 도구다. 추가 구독·API 과금을 기본 요구로 하지 않는다.

첨 실제 과제는 당일 사진 SNS 게시와 오전 9시 실행이다. 당일 요구를 몰래 전일로 바꾸지 않는다. 계획의 날짜창·일일 한도·지연 기본값은 구현 기본값에 채택됐으나 실제 사진 앱·게시·예약이 구현/활성화된 것은 아니다. 실제 앱은 공통 템플릿과 분리한다.

## 독립검증 이후의 재발 방지

CI green은 독립검증 PASS가 아니다. IVA가 특정 오류 경로에서 finding을 반환하면 최초 판정과 exact 대상은 보존하고, 승인된 범위에서는 affected-only로 원인을 제거한 뒤 새 head/tree를 고정한다. 전체 프로젝트를 무정보 반복 검증하지 않는다.

로그 마스킹은 토큰 prefix 한 종류에 의존하지 않는다. Authorization/Bearer, 따옴표 JSON, 일반 password/key, query string과 사용자 경로를 구조별 회귀 사례로 둔다. WinGet의 HRESULT는 signed/unsigned 표현을 정규화하고 재부팅 필요와 일반 실패를 구분하며 자동 재부팅하지 않는다.

설정 없음의 DEMO와 잘못된 설정의 invalid는 다르다. invalid를 정상 체험처럼 표시하지 않고 DB 접근을 차단한 복구 화면으로 처리한다. Supabase logout은 화면 이동만으로 성공 판정하지 않고 SDK 반환 error/throw를 구분하며 현재 브라우저의 local session scope를 명시한다.

테스트 fixture는 사례 간 상태를 초기화한다. 표적 테스트 자체의 격리 결함으로 CI가 실패하면 제품 결함과 구분해 기록하고, 실패 run을 숨기지 않은 채 테스트만 수정한다.

## Git·artifact 동일성과 상태 소유권

직전 응답이 중단되어도 Git 코드·PR·CI는 남을 수 있다. 먼저 remote ref, PR head/tree, CI run과 산출물을 대조하고 성공 효과를 재사용한다. 정확한 진행상태와 SHA는 CURRENT/완료보고가 소유하며 이 기억에 전문 복제하지 않는다.

Bootstrap 후보 ZIP은 Windows 줄바꿈 변환이 있을 수 있다. ZIP checksum, Git text normalization, raw CRLF `bootstrap.bat` blob과 최종 tree 일치를 구분한다. Web Starter 후보는 추출 바이트의 exact tree를 계산한다. artifact 내부 `iva: NOT_RUN`은 작성자 CI artifact의 scope이지 프로젝트 전역의 최초 IVA 상태가 아니다.

GitHub Desktop은 AI가 아니다. Next.js 생성과 Supabase 계정 생성은 다르다. CI green은 실제 PC 설치·실계정 연결·제품 배포가 아니다. HLOM의 조직 전체나 전역 재검증 루프를 이식하지 않는다. 공개 Git에는 대화 원문·개인 사진·실제 키·비공개 원문·원본 로그를 남기지 않는다.

## 기록 소유권과 보존 수준

CURRENT는 현재 단계, DECISIONS는 채택/대체 관계, WORKLOG는 사건, 구현 계획/완료보고/IVA 보고서는 상세 범위와 증거를 소유한다. 한 lineage에는 한 writer를 두고 다음 변경 전 generation/head를 확인한다.

CONVERSATION_PRESERVATION: PARTIAL / SUMMARY_ONLY
RAW_TRANSCRIPT_ARCHIVE: NOT_CONFIGURED
HOST_MEMORY_DATABASE: NOT_IMPLEMENTED

Git 파일 저장만으로 ChatGPT 플랫폼 설정 변경·전체 대화 백업·자동 다중 Persona 실행을 주장하지 않는다.
