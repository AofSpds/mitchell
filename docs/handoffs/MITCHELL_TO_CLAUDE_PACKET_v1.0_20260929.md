# MITCHELL → Claude 실행 인계 패킷

PACKET_ID = MITCHELL-CLAUDE-SNSG-HANDOFF-20260929-001
VERSION = 1.0
DATE = 2026-09-29
FROM = OWNER / MITCHELL
TO = Claude / 후속 작성자
PROJECT = MITCHELL
PRIMARY_PRODUCT = AofSpds/sns-gateway
STATUS = HANDOFF_READY / CLAUDE_NOT_DISPATCHED

## 먼저 읽을 문서

Repository = AofSpds/mitchell
Commit = 7e8c6d3b722cc6af16506d536cc419829ef48c02
Path = docs/handoffs/MITCHELL_CLAUDE_HANDOFF_v1.0_20260929.md
Blob = 016a220c0ca70bafd017310029d65dcdba041fcf
SHA256 = 75881842f33f7403fa1596c079d0b2600b40e42385c9e3a57cb6835e553da420

https://github.com/AofSpds/mitchell/blob/7e8c6d3b722cc6af16506d536cc419829ef48c02/docs/handoffs/MITCHELL_CLAUDE_HANDOFF_v1.0_20260929.md

## 현재 기준과 실행 요청

사용자로부터 이 패킷을 전달받으면 상세 이관서와 최신 refs를 먼저 읽고 SNS Gateway 후속 작업을 이어가세요. 작은 구현 선택이나 이미 확정한 서버 없음 조건을 다시 질문하지 마세요.

SNS_MAIN = 1c7cd117d18929cdf40abc0e4f73fec4c87b2c65
SNS_VALIDATED_HEAD = 4ab10f481f7da66c00591ebd6cf597de527b901c
SNS_TREE = caa83524a6118272952fd669d225d253dd477edb
SNS_PR_1 = MERGED / CLOSED
IVA_SGV_F001_F004 = ALL_PASS_IN_AFFECTED_SCOPE
SGV_04 = INDETERMINATE
DEVICE_SNS_ACCEPTANCE = NOT_RUN / INDETERMINATE
SIGNED_DEVICE_INSTALL = NOT_DONE
RELEASE_DEPLOY = HOLD / NOT_DONE

지정 폴더·앨범, 날짜별 묶음/revision, 오래된 알림 날짜 처리, 보존·삭제·용량 관리 코드는 이미 구현·교정·병합됐습니다. 과거 f5dfbe9 후보나 초기 미구현 표로 되돌아가 재구현하지 마세요.

다음 순서로 진행하세요.
1. 이관서 C0: 현재 main/PR/운영 지침/검증 계보와 가용 환경을 확인합니다.
2. C1: 현재 README 등의 오래된 미병합·IVA NOT_RUN 안내를 새 작업 브랜치에서 정리합니다. 당시 보고서와 최초 FAIL 기록은 보존합니다.
3. C2–C3: 실기 수락 체크리스트·증거 양식과 필요한 검사/빌드를 준비합니다. 변경이 없고 유효한 기존 성공 증거는 재사용합니다.
4. C4–C5: 승인된 기기·서명·SNS 전송 범위가 확보되면 묶음 실기시험을 수행하고, 관측 결함만 국소 교정합니다. 미확보 경로는 NOT_RUN으로 두고 가능한 준비 작업을 완료합니다.
5. C6: 정확한 commit/tree, 변경·시험·미실행·남은 gate를 포함한 완료보고와 MITCHELL 반환 패킷을 제공합니다.

새 브랜치 제안 = work/sns-gateway-device-readiness-20260929
기준은 최신 확인된 main입니다. 이미 닫힌 PR #1을 재개하거나 force-reset하지 마세요. 현재 Git이 위 값과 다르면 변경 delta를 먼저 확인하세요.

## 유지할 경계

- 외부 서버·외부 사진 저장소·Supabase·SNS HTTP API/OAuth·API 키·앱 로그인·무인 게시를 추가하지 않습니다. OS 공유 API와 최종 수동 게시를 유지합니다.
- 이관은 Claude를 PMO/IVA로 이름만 바꾸거나 새로운 상설 Persona를 설치하는 행위가 아닙니다. 현재 ChatGPT 채널은 MITCHELL, PMO는 NOT_DISPATCHED입니다.
- 기존 SNS 범위의 코드·자체검사·작업 브랜치·Draft PR·문서 작업만 이어갑니다. 사용자 기기 설치, 서명키/계정·결제, SNS로 사진 전달·공개 게시, 새 main 병합·릴리스·배포는 별도 허용이 필요합니다.
- Bootstrap/Web Starter 변경과 Mac 독립 재검증은 이번 SNS 작업에 자동 포함하지 않습니다. Mac 교정 후보30a06ea0…의 별도 IVA 재검증 대기는 보존합니다.
- MITCHELL CURRENT/DECISIONS/memory는 직접 덮어쓰지 말고 정제 delta로 반환합니다. 제품 문서의 최초 MITCHELL 작성자 기록도 보존합니다.
- 키·개인 사진·원본 로그·서명키를 채팅/Public Git에 요청하거나 저장하지 않습니다. 별도 IVA가 새 후보를 검증하기 전 작성자 검사를 독립 PASS로 표시하지 않습니다.

## 반환

상세 결과는 sns-gateway 작업 브랜치의 docs/execution에 기록하고, 채팅에는 FROM/TO, base/target commit·tree, PR, 코드·CI·native·실기·SNS별 실제 결과, 보고서 path/commit/blob/SHA256, artifact 설치 가능 여부, NOT_RUN/차단 사유, merge/release 권고와 실제 효과, MITCHELL memory delta, 다음 행동을 담은 패킷을 반환하세요.

Git 접근이 안 되면 해당 한계를 밝히고 첨부 소스로 읽기·허용된 격리 준비까지만 합니다. 첨부 검증 소스는 작성자head4ab10f48의75파일이며 병합main과 tree가 같지만 Git 이력 자체는 아닙니다. 현재성 미확인 상태의 원격 쓰기·기존 저장소 덮어쓰기는 하지 마세요.
