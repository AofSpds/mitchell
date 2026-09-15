# MITCHELL

Windows 초보 개발환경과 GitHub 기반 AI 개발을 위한 운영·계획 저장소입니다. 현재 메인 페르소나는 MITCHELL입니다.

## 현재 상태

**Bootstrap과 Web Starter의 구현 후보가 작업 브랜치·Draft PR에 있으며 작성자 CI가 성공했습니다. 제품 main 병합·IVA 독립검증·실사용 배포는 아직 아닙니다.**

| 계층 | 위치 | 상태 |
|---|---|---|
| 운영·계획 | AofSpds/mitchell | 현재/정책/결정/기억/계획/완료보고 |
| Windows 설치 도구 | AofSpds/bootstrap, PR #1 | work/bootstrap-v0.1 구현 후보 |
| 웹 템플릿 | AofSpds/web-starter, PR #1 | work/web-starter-v0.1 구현 후보 |
| 실제 사진 게시 앱 | 별도 저장소 | 아직 미구현 |

제품 main에는 초기 README만 있습니다. 구현 검토에는 CURRENT에 고정한 PR head나 전달 후보 ZIP을 사용하세요. bootstrap은 Windows 설치기이며 HLOM의 운영규범 배포 Bootstrap과 다른 제품입니다.

## 시작·복구 순서

1. `CURRENT.md` — 실제 완료, 고정 head/tree, 남은 작업과 다음 절차.
2. `AGENTS.md` — 역할·권한·검증·기억 정책.
3. `DECISIONS.md` 및 `docs/execution/EXECUTION_20260915.md` — 이후 승인으로 대체된 과거 경계 확인.
4. `docs/IMPLEMENTATION_PLAN_v1.0.md` — 상위 구현 계획; 존재만으로 전체 구현 완료가 아님.
5. `docs/execution/COMPLETION_20260915.md` — 구현 후보와 실제 CI의 완료보고.
6. 필요한 경우 `memory/MITCHELL.md`와 `WORKLOG.md`의 최근 항목.

## 검증 전달

`docs/execution/IVA_REVIEW_PACKET_v0.1.md`에 별도 IVA가 읽을 정확한 대상과 검증 범위를 모았습니다. 패킷 작성은 dispatch나 PASS가 아닙니다. `CANDIDATE_MANIFEST_20260915.json`에는 후보 ZIP checksum과 파일 비교 결과가 있습니다.

| 이름 | 역할 | 실제 실행 상태 |
|---|---|---|
| MITCHELL | 메인 대화·설계·기억·승인된 Git 실행 | 현재 채널 수행 |
| PMO | Codex WORK 작업자 | NOT_DISPATCHED |
| IVA | 독립 검증자 | NOT_RUN |

다른 Persona·페어 검증자는 미설치이며 자동 추가하지 않습니다.

## 설정과 공개정보

`docs/CHATGPT_PROJECT_INSTRUCTIONS.md`는 ChatGPT 프로젝트 지침 등록용입니다. Git 저장만으로 플랫폼 설정이 자동 적용된다고 주장하지 않습니다.

공개 저장소에는 정책·계획·정제 요약만 둡니다. 대화 원문·개인 사진·실제 키·비밀번호·다른 비공개 프로젝트 원문·원본 로그는 올리지 않습니다. 코드 작성, Git 보존, 검사, 실제 설치, 독립검증, 병합, 배포를 각각 구분합니다.
