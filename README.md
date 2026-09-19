# MITCHELL

초보 개발환경·GitHub 기반 AI 개발·사진 공유 앱의 운영·계획 저장소입니다. 현재 메인 페르소나와 Git 작업자는 MITCHELL입니다.

## 현재

Windows Bootstrap v0.1은 PR #1을 통해 main에 병합됐습니다. macOS Bootstrap v0.2 최초 IVA는 FAIL이며 MAC-F001–F003을 교정한 새 후보가 PR #2에 있습니다. 작성자 표적 검사와 CI는 성공했지만 새 후보의 독립 affected-only 재검증과 실제 Mac 설치 수락은 아직입니다. 최초 FAIL은 보존하며 현재 exact 대상과 미실행은 CURRENT.md가 소유합니다.

| 계층 | 위치 | 상태 |
|---|---|---|
| 운영·계획 | AofSpds/mitchell | CURRENT/정책/결정/기억/작업일지/검증 기록 |
| 개발환경 설치기 | AofSpds/bootstrap | Windows PR#1 병합; macOS PR#2 교정 후보·Draft·미병합 |
| 웹 템플릿 | AofSpds/web-starter | 기존 구현 후보·Draft PR, 이번 작업 변경 없음 |
| 모바일 사진 공유 | AofSpds/sns-gateway | PR #1 병합 완료, main=1c7cd117…; 실기/release 미완료 |

Mac 구현은 .command/Bash3.2, Homebrew 기반 Core/Mobile/Optional·AI 선택입니다. 설치 기준은 Apple Silicon macOS15+이며14는 진단 전용, Intel은 명시적 opt-in 미수락 경로입니다. CLT/Homebrew 최초 설치와 Xcode·SDK·라이선스·서명·로그인은 사용자가 직접 합니다. Windows 기존18파일은 동일하게 보존했습니다.

모바일은 외부 서버·외부 사진 저장소·SNS HTTP API/OAuth·API 키 없이 동작하는 공유 도우미입니다. 사진·문구·이력은 기기에 두고, 로컬 알림 후 사용자가 공식 SNS 앱에서 최종 게시합니다. 폴더·앨범 확인은 앱 재개/새로고침 기준이며 잠긴 휴대폰에서 무인 게시하는 앱이 아닙니다.

## 복구와 다음 작업

README → CURRENT → AGENTS → 관련 DECISIONS/계획/완료보고 순서로 읽습니다. 필요한 범위만 memory/MITCHELL.md와 WORKLOG를 확장합니다. 현재 제품의 exact head/tree/CI는 CURRENT.md가 소유합니다. 제품 main과 새 작업 브랜치를 혼동하지 않습니다.

- 현재 상태: CURRENT.md
- Mac 최초 IVA 결과: docs/execution/BOOTSTRAP_MACOS_IVA_RESULT_001_20260920.md
- Mac 교정 완료보고: docs/execution/BOOTSTRAP_MACOS_IVA001_CORRECTION_COMPLETION_20260920.md
- Mac 교정 Manifest: docs/execution/BOOTSTRAP_MACOS_IVA001_CORRECTED_MANIFEST_20260920.json
- Mac affected-only IVA 입력: docs/execution/BOOTSTRAP_MACOS_IVA_AFFECTED_REREVIEW_PACKET_v0.2.md
- 이전 Mac 완료보고: docs/execution/BOOTSTRAP_MACOS_COMPLETION_20260920.md
- 이전 Mac Manifest: docs/execution/BOOTSTRAP_MACOS_MANIFEST_20260920.json
- 이전 Mac IVA 입력: docs/execution/BOOTSTRAP_MACOS_IVA_PACKET_v0.1.md
- 기존 B/W IVA 결과: docs/execution/IVA_AFFECTED_ONLY_REREVIEW_RESULT_20260915.md
- 최초 SNS Gateway IVA 결과: docs/execution/SNS_GATEWAY_IVA_RESULT_002_20260919.md
- F001–F004 교정 보고: docs/execution/SNS_GATEWAY_IVA002_CORRECTION_COMPLETION_20260919.md
- 교정 후보 Manifest: docs/execution/SNS_GATEWAY_IVA002_CORRECTED_MANIFEST_20260919.json
- affected-only IVA 재검증 입력: docs/execution/SNS_GATEWAY_IVA_AFFECTED_REREVIEW_PACKET_v0.3.md
- affected-only IVA 결과: docs/execution/SNS_GATEWAY_IVA002_AFFECTED_REREVIEW_RESULT_20260919.md
- 이전 작성자 보고: docs/execution/SNS_GATEWAY_LIFECYCLE_COMPLETION_20260919.md
- 이전 소스/빌드 Manifest: docs/execution/SNS_GATEWAY_LIFECYCLE_MANIFEST_20260919.json
- 이전 IVA 입력: docs/execution/SNS_GATEWAY_IVA_PACKET_v0.2.md
- 기준 설계: docs/mobile/LOCAL_SHARING_DESIGN_v1.0_20260919.md
- 과거 게시함 후보: docs/execution/SNS_GATEWAY_COMPLETION_20260919.md
- 원 전체 계획: docs/IMPLEMENTATION_PLAN_v1.0.md

## 역할과 경계

MITCHELL은 현재 대화의 설계·구현·기억·승인된 Git 작업자입니다. PMO는 Codex WORK 작업자이며 실제 dispatch하지 않았습니다. IVA는 별도 독립검증자입니다. SNS Gateway 최초 검증과 교정 후보의 affected-only 재검증은 완료됐고, F001–F004는 모두 PASS입니다. Owner 승인으로 PR #1도 병합됐습니다. Mac Bootstrap 최초 IVA는 FAIL이며 새 교정 후보의 affected-only 재검증은 NOT_RUN입니다. 다른 Persona·페어 검증자는 미설치입니다.

B/W 최초 지적4건의 affected-only IVA PASS는 해당 후보의 결과일 뿐 새 macOS 코드나 SNS Gateway의 검증으로 재사용하지 않습니다. IVA 최종 결과는 상세 보고서를 Git에 기록하고 MITCHELL에 복사 가능한 패킷으로 반환합니다.

공개 Git에는 정제된 문서·코드·합성 fixture만 둡니다. 대화 원문·개인사진·실제키·서명키·원본 로그·비공개 원문을 저장하지 않습니다. 문서/코드 작성, Git 보존, 검사, native 빌드, 실기 사용, 독립검증, 병합, 배포는 서로 다른 완료입니다.

docs/CHATGPT_PROJECT_INSTRUCTIONS.md는 프로젝트 지침 등록용이며 Git 파일만으로 플랫폼 설정을 자동 변경하지 않습니다.
