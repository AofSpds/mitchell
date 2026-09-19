# MITCHELL

초보 개발환경·GitHub 기반 AI 개발·사진 공유 앱을 위한 운영·계획 저장소입니다. 메인 페르소나는 MITCHELL입니다.

## 현재 진입점

**현재 설계는 외부 서버·외부 사진 저장소 없이 동작하는 모바일 사진 공유 도우미입니다.** 사진과 문구는 기기에서 준비하고, 오전 9시 로컬 알림을 받은 사용자가 공식 SNS 앱에서 최종 게시합니다. 상세 설계는 문서이며 모바일 구현·실기 검증·배포 완료가 아닙니다.

- 현재 상태: `CURRENT.md`
- 새 모바일 상세 설계: `docs/mobile/LOCAL_SHARING_DESIGN_v1.0_20260919.md`
- 역할·권한·기억: `AGENTS.md`
- 결정과 대체 관계: `DECISIONS.md`
- 기존 전체 계획: `docs/IMPLEMENTATION_PLAN_v1.0.md`
- 지속 맥락: `memory/MITCHELL.md`
- 사건 기록: `WORKLOG.md`

| 계층 | 위치 | 상태 |
|---|---|---|
| 운영·계획 | AofSpds/mitchell | 현재/정책/결정/기억/계획/결과 |
| Windows 설치 도구 | AofSpds/bootstrap, PR #1 | 구현 후보, Draft·미병합 |
| 웹 템플릿 | AofSpds/web-starter, PR #1 | 구현 후보, Draft·미병합 |
| 모바일 사진 공유 앱 | 실제 앱 저장소는 별도 | 상세 설계, 구현 미시작 |

Bootstrap/Web Starter는 최초 IVA 지적 4건의 affected-only 재검증을 통과했습니다. 이는 실제 Windows/Supabase 수락·병합·배포 또는 새 모바일 앱의 검증 PASS가 아닙니다. 결과는 `docs/execution/IVA_AFFECTED_ONLY_REREVIEW_RESULT_20260915.md`에 보존되어 있습니다. 제품 main과 후보 PR을 혼동하지 않습니다.

## 시작·복구

README → CURRENT → AGENTS → 관련 DECISIONS/계획을 먼저 읽습니다. 필요할 때 Persona 기억과 최근 Worklog를 확장합니다. 최신 Git과 일치하면 전 역사를 재독하지 않습니다. 과거 문서-only 범위와 후속 Git 구현 승인 관계는 `docs/execution/EXECUTION_20260915.md`에 있으며, 현재 모바일 설계 요청을 실제 설치·공개 게시 권한으로 확대하지 않습니다.

## 역할·검증

MITCHELL은 현재 채널의 설계·기억·승인된 Git 작업자입니다. PMO는 Codex WORK 작업자이며 아직 NOT_DISPATCHED입니다. IVA는 별도 독립검증자입니다. B/W의 과거 검증은 완료됐지만 본 모바일 설계의 별도 IVA 검증은 NOT_RUN입니다. 다른 Persona·페어 검증자는 미설치입니다.

IVA 최종 반환은 상세 결과를 Git에 보존하고 MITCHELL에 복사 가능한 인계 패킷으로 제공합니다. 원 규칙은 `docs/execution/IVA_RETURN_PACKET_RULE_20260915.md`입니다.

## 공개정보와 설정

공개 Git에는 정제된 프로젝트 문서와 코드만 둡니다. 대화 원문·개인 사진·실제 키·비밀번호·비공개 원문·원본 로그를 저장하지 않습니다. Git은 휴대폰 사진·운영 데이터의 클라우드 백업이 아닙니다.

`docs/CHATGPT_PROJECT_INSTRUCTIONS.md`는 등록용 안내이며 Git 저장만으로 플랫폼 설정이 바뀌지 않습니다. 코드 작성·문서 보존·검사·실제 설치·독립검증·병합·배포는 각각 증거로 보고합니다.
