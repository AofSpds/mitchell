# MITCHELL

초보자가 Windows 개발환경을 준비하고 GitHub 기반 AI 개발을 시작하도록 지원하는 프로젝트의 운영·계획 저장소입니다.

## 먼저 읽을 문서

1. `CURRENT.md` — 현재 단계, 완료·미완료, 다음 행동
2. `AGENTS.md` — 프로젝트 범위, 역할, 실행·검증·기억 정책
3. `DECISIONS.md` — 사용자 확정사항과 설계 기본값의 구분
4. `docs/IMPLEMENTATION_PLAN_v1.0.md` — Bootstrap → Web Starter → 첫 사진 게시 앱의 상세 구현 계획
5. `memory/MITCHELL.md` — 지속적으로 보존할 맥락; 전체 대화 원문이 아님
6. `WORKLOG.md` — 최근 작업 사건과 재개 지점

초기화 도중 위 파일이 아직 없으면 `FOUNDATION_IN_PROGRESS`입니다. 파일 존재와 내용을 직접 확인하기 전에는 구축 완료로 판단하지 마세요.

## 역할

| 구분 | 이름 | 역할 |
|---|---|---|
| 메인 대화 페르소나 | MITCHELL | 요구사항·설계·현재 상태·승계와 승인된 Git 작업 |
| Codex WORK 작업자 | PMO | 별도 활성화된 범위의 구현과 자체 점검 |
| 독립 검증자 | IVA | 구현 완료 후 고정된 산출물 검증 |

다른 페르소나·페어 검증자는 설치하지 않습니다. 역할을 문서에 정의한 것과 별도 실행 세션이 실제 가동된 것은 다릅니다.

## 저장소 경계

```text
AofSpds/mitchell     운영정책 / 결정 / 계획 / 정제 기억
        |
AofSpds/bootstrap   Windows 설치·검사 도구
        |
AofSpds/web-starter Next.js / TypeScript / Tailwind / Supabase 템플릿 (예정)
        |
실제 앱 저장소        daily-photo-publisher 등 (예정)
```

`bootstrap`은 PC 설치 도구입니다. HLOM의 공통 운영규범 배포 Bootstrap과 같은 제품으로 취급하지 않습니다.

## ChatGPT 프로젝트 설정

`docs/CHATGPT_PROJECT_INSTRUCTIONS.md`는 프로젝트 설정에 한 번 등록할 지침입니다. Git 파일 생성만으로 ChatGPT의 프로젝트 설정이 자동 변경되지는 않습니다. 새 채널에서는 이 저장소의 현재 문서를 다시 읽어 상태를 복구합니다.

## 공개 정보 원칙

이 저장소에는 공개 가능한 정책·계획·정제 요약만 둡니다. 개인 사진, 대화 원문, 다른 비공개 프로젝트의 원문, 비밀번호, 토큰, 실제 환경변수와 원본 로그는 올리지 않습니다.

## 현재 완료의 의미

정책·계획 문서의 Git 반영과 실제 프로그램 구현·설치·검증·배포를 구분합니다. 현재 상태의 권위는 `CURRENT.md`와 해당 Git readback에 있습니다.
