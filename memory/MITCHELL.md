# MITCHELL Persona Memory

PROJECT_ID: MITCHELL
PERSONA_ID: MITCHELL
GENERATION: 6
MEMORY_DELTA_ID: MITCHELL-MEM-006
EXPECTED_GENERATION: 5
EXPECTED_BASE_COMMIT: 5b11e3e39191df8a65fd8008665b93370a9ad75b
SEMANTIC_OWNER / WRITER: MITCHELL / MITCHELL
SOURCE: 2026-09-19 SNS Gateway 실행·계속 지시, 로컬 게시함 후보와 후속 소스/날짜/보존 구현
PROVENANCE: DERIVED_SUMMARY; raw transcript가 아님

## 지속 목적

개발자가 비개발자 친구를 도울 수 있도록 초기 환경과 Git 기반 AI 개발을 단순화한다. 교육은 쉬운 단계별 안내, 개발자는 저장소의 구조와 변경을 점검하는 방식이다. 사소한 구현 선택을 반복 확인시키지 않고 승인된 범위와 설계 기본값을 구분하여 진행한다.

## 현재 모바일 요구 — 이후 서버형 제안보다 우선

사용자는 외부 서버뿐 아니라 외부 사진 스토리지도 원하지 않는다. 임시 저장소를 절충안이라는 이유로 다시 제안하거나 몰래 구현하지 않는다. 최종 게시를 사람이 누르는 방식에 동의하고 상세 설계를 요청했다. 이후 만든 AofSpds/sns-gateway에서 이 채널의 직접 구현과 남은 기능 완성을 지시했다.

모바일의 기본 계약은 로컬 사진 준비 → 9시 로컬 알림 → 사용자 foreground 진입·공유 → 공식 SNS 앱에서 최종 게시다. iPhone을 불가능하다고 단정하거나 Galaxy로 자동 제한하지 않는다. 다만 SNS 선택·다음·문구 붙여넣기·잠금 해제까지 포함한 문자 그대로 한 번의 터치를 보장하지 않는다. 수신 앱별 단계는 실기로 확인한다.

우리 앱에 Supabase/Auth/OAuth·중계 URL·서버 DB·원격 Push·AI API를 넣지 않는다. 사진·이력은 기기 내부에 둔다. 사용자가 SNS 앱에 공유한 이후 그 앱의 처리·업로드는 별개다. iCloud/갤러리 원본 백업, 기기간 클립보드 등 사용자 OS 설정까지 앱이 통제한다고 과장하지 않는다. 사용자의 API 키 질문 때문에 이 계약을 변경하지 않는다.

‘그날 올린 사진’은 촬영일과 다르다. 앱 게시함의 등록시각을 직접 기록하는 경로가 명확하다. 외부 앨범·폴더 최초 관찰시각은 실제 추가시각이 아니므로 날짜가 불명확하면 확인 후보로 남긴다. 기존 무인 계획의30분 지연 폐기 규칙은 모바일 수동 게시에 그대로 강제하지 않는다.

공유 callback은 원격 게시 성공 영수증이 아니다. 공유 시도·미확인·사용자 완료 표시를 구분한다. 미확인 사진을 자동 재공유하거나 이미 게시됐다고 단정하지 않는다. 앱 삭제/이력 초기화 뒤 전역 중복방지도 보장하지 않는다.

기준 설계는 docs/mobile/LOCAL_SHARING_DESIGN_v1.0_20260919.md다. 서버형 모바일 v0.1은 현재 모바일 경로에서 비채택으로 대체하지만 과거 제안 자체와 B/W 제품은 보존한다. 설계 기본값은 후속 실행 승인 범위에서 구현하며 실제 플랫폼 수락과 분리한다.

## 소스·날짜·보존 구현에서 유지할 원칙

폴더/앨범 연결은 최초 complete baseline과 후속 foreground 스캔을 구분한다. 앱이 계속 감시한다고 주장하지 않는다. 부분 조회·권한 오류를 사진 없음/성공 snapshot으로 바꾸지 않는다. 동일 source identity의 재등장·편집 revision·실패 후 pending을 구분한다. 원본은 읽기만 하고 iCloud-only를 자동 다운로드하지 않는다.

날짜별 사진·순서·문구는 immutable revision으로 기록한다. 이후 편집으로 공유 당시 내용을 바꾸지 않는다. 버전 충돌은 덮어쓰지 않고 복구 안내한다. 오래된 알림은 원 예정일을 얻을 수 있으면 그 날짜, 그렇지 않으면 실제 전달일의 근거를 남기고 사용자에게 날짜를 확인받는다. 오늘 날짜로 몰래 치환하지 않는다.

보존 정리는 열린 날짜와 미확인 공유를 보호한다. 사용자 완료/취소와 그 시각은 한 트랜잭션으로 기록하고, 과거 완료시각이 없으면 임의로 오래된 시각을 만들지 않는다. 삭제는 앱 사본 경로만 허용하며 journal→실제 삭제/부재 확인→완료 순서다. tombstone으로 같은 해시 사본의 자동 부활을 막는다. 중단된 초기화는 다른 작업보다 먼저 재개하고 원래 누른 작업을 자동 재실행하지 않는다.

이 구현의 정확한 한도·head·시험결과는 CURRENT와 완료보고가 소유한다. ‘남은 네 코드 영역 구현 완료’와 ‘친구가 바로 설치해 사용할 완성품’은 다르다. API 키가 없어서 미완성인 것처럼 설명하지 않는다.

## 역할·채널

메인 MITCHELL, Codex WORK 작업자 PMO, 독립 검증자 IVA. 다른 Persona/페어 검증자는 미설치다. 사용자는 현재 MITCHELL 채널에서 직접 Git 업무를 진행하도록 지시했으며 PMO로 임의 이관하지 않는다.

PMO는 NOT_DISPATCHED다. B/W 최초 IVA 검증과 affected-only 재검증은 별도 IVA 대화에서 수행되어 Git에 기록됐다. MITCHELL은 IVA로 이름을 바꾸지 않고 결과를 수신·정리한다. SNS Gateway의 별도 IVA 검증은 NOT_RUN이다.

## 기존 IVA 상태와 반환 계약

B/W 최초 결과는 FAIL, 실제 환경 INDETERMINATE, merge HOLD였다. 교정 exact candidates의 B001/B002/W001/W002 재검증은 모두 PASS, 새 finding NONE, MERGE_RECOMMENDATION PASS, RELEASE_DEPLOY_RECOMMENDATION HOLD다. Windows 실기와 실제 Supabase 통합은 미실행이며 제품 병합·Template·배포도 별개다. 이 결과를 모바일 앱의 검증으로 재사용하지 않는다.

IVA 최종 반환은 항상 MITCHELL에 복사 가능한 인계 패킷이다. 상세 결과가 길면 Git에 기록하고 채팅에는 exact 대상·finding 판정·문서 commit/blob·권고·미실행·권한·다음 조치를 담는다. RETURN_PACKET_REQUIRED=ALWAYS, DETAILED_RESULT_TO_GIT=YES, CHAT_RETURN_PACKET_ONLY=DEFAULT. 형식 규칙은 제품 수정·병합·배포 권한을 추가하지 않는다.

## 기존 기술과 보존 경계

설치기 이름은 vibe를 뺀 bootstrap이다. AofSpds/mitchell은 운영·계획 허브, AofSpds/bootstrap은 Windows 설치 도구, AofSpds/web-starter는 Next.js/TypeScript/Tailwind/Supabase 템플릿이다. IDE는 VS Code, Git GUI는 GitHub Desktop, Codex/Claude는 기존 계정의 선택 도구다. 추가 구독·API 과금을 기본 요구로 하지 않는다.

기존 계획의 촬영일·예약·서버·API 경로는 해당 과거 범위에 보존한다. 새 모바일 로컬 앱은 웹 템플릿의 필수 하위 실행물이 아니다. 당일 요구를 전일 게시로 몰래 바꾸지 않으며 실제 앱은 공통 템플릿과 분리한다.

## 재발 방지

CI green은 독립검증·실제 기기·계정·배포 PASS가 아니다. 지정된 finding을 고칠 때 최초 판정/exact 대상은 보존하고 affected-only로 원인을 제거한 뒤 새 대상을 고정한다. 전체 프로젝트 무정보 반복 검증은 하지 않는다.

로그 마스킹은 Authorization/Bearer·quoted JSON·password/key·query·사용자 경로의 구조별 회귀 사례로 확인한다. WinGet HRESULT는 signed/unsigned를 정규화하며 자동 재부팅하지 않는다. DEMO와 invalid를 구분하고 Supabase logout은 SDK error/throw와 local scope를 명시한다. 테스트 fixture의 사례 간 상태를 초기화한다.

중단 후에는 remote ref, PR head/tree, CI와 산출물을 먼저 읽는다. 성공 commit·설치·게시를 무조건 다시 실행하지 않는다. Bootstrap CRLF 변환과 raw bootstrap.bat, Web 원본 바이트의 tree 동일성을 구분한다. artifact의 NOT_RUN은 그 범위를 뜻하며 프로젝트 전역 상태로 덮어쓰지 않는다.

## 기억 소유권과 보존

CURRENT는 지금 상태, DECISIONS는 확정/제안/대체, WORKLOG는 사건, 상세 설계/보고서는 세부 근거를 소유한다. 정확한 SHA와 목록을 기억에 반복 복제하지 않는다. 한 lineage에는 한 writer를 두고 기존 generation/head를 확인한다.

공개 Git에 대화 원문·개인 사진·실제 키·비공개 원문·원본 로그를 저장하지 않는다. GitHub Desktop은 AI가 아니며 Git은 사진·미커밋 변경의 자동 백업이 아니다. HLOM 조직 전체나 전역 검증 루프를 이식하지 않는다.

CONVERSATION_PRESERVATION: PARTIAL / SUMMARY_ONLY
RAW_TRANSCRIPT_ARCHIVE: NOT_CONFIGURED
HOST_MEMORY_DATABASE: NOT_IMPLEMENTED

Git 저장만으로 플랫폼 설정 변경·전체 대화 백업·자동 다중 Persona 실행을 주장하지 않는다.
