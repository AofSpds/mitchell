# MITCHELL

초보 개발환경·GitHub 기반 AI 개발·모바일 사진 공유 제품의 운영·계획 저장소입니다. 메인 페르소나는 MITCHELL입니다.

## 현재 진입점

- 현재 상태·정확한 제품 후보·남은 단계: `CURRENT.md`
- SNS Gateway 구현 실행 범위: `docs/execution/SNS_GATEWAY_EXECUTION_20260919.md`
- SNS Gateway 후보 완료보고: `docs/execution/SNS_GATEWAY_COMPLETION_20260919.md`
- 모바일 로컬 설계: `docs/mobile/LOCAL_SHARING_DESIGN_v1.0_20260919.md`
- 역할·권한·검증·기억 계약: `AGENTS.md`
- 결정/지속 맥락/사건: `DECISIONS.md`, `memory/MITCHELL.md`, `WORKLOG.md`

현재 모바일 제품은 **AofSpds/sns-gateway**입니다. 서버·외부 사진 저장소·SNS OAuth/API 키 없이 기기 사진과 문구를 준비하고, 로컬 알림 후 사용자가 공식 SNS 앱으로 공유하여 최종 게시합니다. 제품 작업 브랜치에 로컬 게시함·알림·공유 코드가 존재하며 정확한 검사 결과와 미실행 항목은 CURRENT/완료보고가 소유합니다. 전체 앱 완성·실기 수락·배포 완료를 뜻하지 않습니다.

| 영역 | 위치 | 경계 |
|---|---|---|
| 운영/계획/검증 기록 | AofSpds/mitchell | 제품 실행 코드와 분리 |
| Windows 개발환경 | AofSpds/bootstrap PR #1 | 기존 후보 보존 |
| 웹 템플릿 | AofSpds/web-starter PR #1 | 기존 후보 보존 |
| 모바일 사진 공유 | AofSpds/sns-gateway PR #1 | 실제 코드·CI·작업계획 |

B/W의 과거 affected-only IVA PASS는 모바일 앱 검증이 아닙니다. 실제 Windows/Supabase 통합·제품 병합·Template·배포는 별도 상태로 보존합니다. 이번 모바일 작업에서 bootstrap/web-starter는 변경하지 않았습니다.

## 시작·복구

README → CURRENT → AGENTS → 관련 DECISIONS/계획/실행기록을 읽고, 필요할 때 memory/WORKLOG 최근 항목만 확장합니다. 과거 설계-only 상태는 후속 실행 지시와 Git 증거에 따라 현행화하되 원 계획·IVA 결과를 소급 변경하지 않습니다. 중단 후 성공한 작업을 무조건 재실행하지 않습니다.

MITCHELL은 현재 채널의 작성자입니다. PMO NOT_DISPATCHED, SNS Gateway 독립 IVA NOT_RUN. 다른 Persona·페어 검증자는 미설치입니다. 작성자 검사·native compile·실기·독립검증·병합·배포를 구분합니다. IVA 최종 반환은 Git 결과 위치와 정확한 대상·판정을 담은 복사 가능한 인계 패킷입니다.

공개 Git에는 개인 사진·키·비밀번호·개인 서명키·대화 원문·비공개 프로젝트 원문·원본 로그를 저장하지 않습니다. Git 문서 등록은 ChatGPT 플랫폼 설정 자동 변경이 아닙니다.
