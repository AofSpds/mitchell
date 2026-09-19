# MITCHELL

초보 개발환경·GitHub 기반 AI 개발·사진 공유 앱의 운영·계획 저장소입니다. 현재 메인 페르소나와 Git 작업자는 MITCHELL입니다.

## 현재

**SNS Gateway의 로컬 사진 게시함·지정 폴더/앨범·날짜별 묶음·예약 알림·공유·보존 관리가 작업 브랜치의 코드 후보로 구현됐습니다. 작성자 검사와 Android/iOS simulator 빌드는 성공했지만, 실제 기기/SNS·독립 IVA·서명된 설치본·병합·배포는 아직 아닙니다.**

| 계층 | 위치 | 상태 |
|---|---|---|
| 운영·계획 | AofSpds/mitchell | CURRENT/정책/결정/기억/작업일지/검증 입력 |
| Windows 설치기 | AofSpds/bootstrap | 기존 구현 후보·Draft PR, 이번 작업 변경 없음 |
| 웹 템플릿 | AofSpds/web-starter | 기존 구현 후보·Draft PR, 이번 작업 변경 없음 |
| 모바일 사진 공유 | AofSpds/sns-gateway, PR #1 | 코드 후보·작성자 CI, Draft·미병합 |

모바일은 외부 서버·외부 사진 저장소·SNS HTTP API/OAuth·API 키 없이 동작하는 공유 도우미입니다. 사진·문구·이력은 기기에 두고, 로컬 알림 후 사용자가 공식 SNS 앱에서 최종 게시합니다. 폴더·앨범 확인은 앱 재개/새로고침 기준이며 잠긴 휴대폰에서 무인 게시하는 앱이 아닙니다.

## 복구와 다음 작업

README → CURRENT → AGENTS → 관련 DECISIONS/계획/완료보고 순서로 읽습니다. 필요한 범위만 memory/MITCHELL.md와 WORKLOG를 확장합니다. 현재 제품의 exact head/tree/CI는 CURRENT.md가 소유합니다. 제품 main의 초기 문서와 작업 브랜치를 혼동하지 않습니다.

- 현재 상태: CURRENT.md
- 최신 작성자 보고: docs/execution/SNS_GATEWAY_LIFECYCLE_COMPLETION_20260919.md
- 소스/빌드 Manifest: docs/execution/SNS_GATEWAY_LIFECYCLE_MANIFEST_20260919.json
- 별도 IVA 입력: docs/execution/SNS_GATEWAY_IVA_PACKET_v0.2.md
- 기준 설계: docs/mobile/LOCAL_SHARING_DESIGN_v1.0_20260919.md
- 과거 게시함 후보: docs/execution/SNS_GATEWAY_COMPLETION_20260919.md
- 원 전체 계획: docs/IMPLEMENTATION_PLAN_v1.0.md

## 역할과 경계

MITCHELL은 현재 대화의 설계·구현·기억·승인된 Git 작업자입니다. PMO는 Codex WORK 작업자이며 실제 dispatch하지 않았습니다. IVA는 별도 독립검증자이고 SNS Gateway 검증은 NOT_RUN입니다. 다른 Persona·페어 검증자는 미설치입니다.

B/W 최초 지적4건의 affected-only IVA PASS는 해당 후보의 결과일 뿐 SNS Gateway의 검증으로 재사용하지 않습니다. IVA 최종 결과는 상세 보고서를 Git에 기록하고 MITCHELL에 복사 가능한 패킷으로 반환합니다.

공개 Git에는 정제된 문서·코드·합성 fixture만 둡니다. 대화 원문·개인사진·실제키·서명키·원본 로그·비공개 원문을 저장하지 않습니다. 문서/코드 작성, Git 보존, 검사, native 빌드, 실기 사용, 독립검증, 병합, 배포는 서로 다른 완료입니다.

docs/CHATGPT_PROJECT_INSTRUCTIONS.md는 프로젝트 지침 등록용이며 Git 파일만으로 플랫폼 설정을 자동 변경하지 않습니다.
