# MITCHELL 출처 기록

조회 기준일: 2026-09-15. 원문·해석·계획상의 제안을 구분한다. 공개 파일에는 출처 위치와 정제된 적용 범위만 기록하며 비공개 원문을 재배포하지 않는다.

## 사용자 입력

| ID | 형태 | 사용 범위 | 한계 |
|---|---|---|---|
| U01 | 현재 사용자의 MITCHELL 저장소·역할·정책·계획 요청 | Persona 이름, 타 Persona 미설치, 현 채널 Git, 문서 작성·보존 권한 | 현재 대화에서 직접 제공; 영구 message/channel ID 미제공; 원문 미복제 |
| U02 | 현재 대화의 상위 요구사항 및 앞선 Architecture Baseline | Windows·Git·초보 온보딩·Next/Supabase·사진 게시 방향 | 사용자 요구와 이전 assistant 권고가 섞여 있어 DECISIONS에서 구분 |

## HLOM — 선택적 현지 적용 근거

Repository: `AofSpds/human-llm-operating-model`
Readback main commit: `e01a99560dd7145c6643dd0769e01f43610c1d41`
Readback tree: `0e2b9ac29e6772a0d1ba3fc8df30de3a243799bd`

| 파일 | 조회 범위 | 적용한 의미 |
|---|---|---|
| README.md | main의 진입 설명 | current-first, 공통정책/Project Profile 분리, 조직 자동 이식 금지 |
| CURRENT.md | 위 exact ref의 current/resolver | 문서 속 과거 active 수치와 실제 main readback 구분 |
| OWNER_INTERFACE.md | exact ref, 1–220행 | 한국어, 실제 상태 우선, 반복 승인 최소화, 채널/권한 구분 |
| COMMON_KERNEL.md | exact ref, 1–260행 | 사용자 목적/경계·작업자 방법, 외부효과 조정, 완료 후 독립검증 |
| PROJECT_PROFILE.md | exact ref, 1–210행 | HLOM 전용 조직은 복사하지 않고 실행/검증 분리 원칙만 현지화 |
| CONTINUITY_CONTRACT.md | exact ref, 1–370행 | 현재/결정/기억/작업일지 구분, Persona 소유, 직렬 비덮어쓰기, 공개 raw 금지 |

확인한 blob: OWNER_INTERFACE `d8d219e1e850d0494d96712476bb2b43f66ea266`; COMMON_KERNEL `0ef1cfed9e9a7f98c98e5fa35b6c7f161987e029`; PROJECT_PROFILE `a60840e0a85ed4f4feb94d0ba0037fb8dfd35299`; CONTINUITY_CONTRACT `be6ba587797a4baa4600d599e4d01513801ee0f2`.

[고정 README 진입](https://github.com/AofSpds/human-llm-operating-model/blob/e01a99560dd7145c6643dd0769e01f43610c1d41/README.md)

HLOM 전 규범을 전부 읽거나 적합성 인증을 수행한 것은 아니다. AAA 조직과 내부 정책을 별도로 전수 조사했다고 주장하지 않는다. 사용자 요청의 'AAA 또는 HLOM' 중 공통정책 원천인 HLOM을 사용했다. 본 프로젝트의 Windows 설치기 bootstrap은 HLOM Global Bootstrap과 별개다.

## 실제 저장소 확인

| 저장소 | 확인 내용 |
|---|---|
| AofSpds/mitchell, ID 1370951500 | 최초 조회 Public, default main, contents 응답 empty. 초기 README commit은 WORKLOG 참조 |
| AofSpds/bootstrap, ID 1370936370 | Public, default main, contents 응답 empty. 이번 단계 mutation 없음 |

연결 API에 표시된 계정 권한은 특정 승인 없는 변경권한과 동일하지 않다.

## 공식 기술 자료

| ID | 공식 자료 | 확인한 내용 / 계획에서의 한계 |
|---|---|---|
| T01 | [WinGet install](https://learn.microsoft.com/en-us/windows/package-manager/winget/install) | exact ID/source/scope/version, no-upgrade, 약관 옵션. 대상 Windows 실기 설치는 미수행 |
| T02 | [Next.js Installation](https://nextjs.org/docs/app/getting-started/installation) | App Router/TS/Tailwind 생성 경로, 지원 Node 요구, npm 스크립트. 구현 시 안정판/보안 패치 재확인 |
| T03 | [Supabase Next.js Quickstart](https://supabase.com/docs/guides/getting-started/quickstarts/nextjs) | with-supabase 예제, Cloud 프로젝트와 환경변수 설정의 별도 단계 |
| T04 | [Supabase API keys](https://supabase.com/docs/guides/getting-started/api-keys) | publishable/secret 구분, secret의 RLS 우회, 서버 전용 키와 사용자 권한 분리 |
| T05 | [ChatGPT Projects](https://help.openai.com/en/articles/10169521-projects-in-chatgpt) | 프로젝트 설정에서 지침 추가. 이 실행 환경에서 사용자의 설정을 실제 변경하지 않음 |
| T06 | [Codex IDE](https://developers.openai.com/codex/ide/) | 공식 IDE 확장 경로. 리디렉션되는 공식 문서를 확인; 실제 계정/Windows 설치 미검증 |
| T07 | [Claude Code VS Code](https://code.claude.com/docs/en/vs-code) | 공식 확장 사용 경로. 친구의 구독·로그인·권한 미검증 |
| T08 | [Vercel Cron usage/pricing](https://vercel.com/docs/cron-jobs/usage-and-pricing) | Hobby Cron의 일일 실행·시간 단위 정밀도 제한. 정시 보장 대안으로 사용하지 않음 |
| T09 | [Meta Instagram 공식 Postman](https://www.postman.com/meta/instagram/collection/6yqw8pt/instagram-api) | Professional/Business/Creator 대상 게시 API 개요. 실제 앱 권한·선택 로그인 방식 재검증 필요 |
| T10 | [Meta Threads 공식 Postman](https://www.postman.com/meta/threads/collection/dht3nzz/threads-api) | 앱·승인·토큰 기반 게시 API. 최신 세부 기능은 developer 원문 확인 필요 |
| T11 | [티스토리 종료 공지](https://notice.tistory.com/2664) | 2023-12-22 공지, 2023-12 말부터 이듬해 2월 말까지 순차 종료 안내. 글/파일 관련 Open API 종료 설명 |
| T12 | [네이버 종료 공지](https://developers.naver.com/notice/article/7527) | 이번 조회는 종료 공지 제목만 회수. 상세 본문·현행 대안은 미검증으로 남김 |

Meta developer의 content-publishing/Threads 본문 URL 일부는 조회 오류가 발생했다. 공식 Postman 자료에서 API 개요만 확인했으며 상세 endpoint·permission·요금·rate limit을 완전 검증했다고 표현하지 않는다.

## 본 문서의 증거 범위

기술 자료 조회와 설계상 선택은 프로그램의 실행 증거가 아니다. Windows 설치, Supabase 계정, SNS token/게시, ChatGPT host 설정, IVA 실행은 각각 NOT_RUN 또는 NOT_VERIFIED다. 공식 가격·사용한도는 계정과 실행 시점에 다시 확인하며 과거 대화의 가격 설명을 현재 확정값으로 고정하지 않는다.
