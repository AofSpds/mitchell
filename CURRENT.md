# MITCHELL Current

Updated: 2026-09-19 / Generation: 8 / Writer: MITCHELL
Expected previous generation: 7
Recovery base: 5b11e3e39191df8a65fd8008665b93370a9ad75b

## 목적·현재

SNS Gateway의 남은 네 코드 영역인 지정 폴더·앨범 연결, 날짜별 묶음/revision, 오래된 알림 날짜 처리, 보존·삭제·용량 관리를 구현하고 작성자 검사·Android/iOS simulator 빌드까지 고정했다. 이제 이 네 영역은 코드 미구현 항목이 아니다. 실제 기기/SNS 수락·독립 IVA·서명된 설치본·병합·배포가 남아 있다.

| 항목 | 현재 값 |
|---|---|
| Persona / Git executor | MITCHELL / 현재 채널 직접 수행 |
| Task | SNSG-LIFECYCLE-001 |
| 실행 근거 | 사용자 SNSG-WORK-001 실행 승인, 남은 구현 진행·계속 지시; docs/execution/SNS_GATEWAY_EXECUTION_20260919.md와 D021/D022 |
| 제품 | AofSpds/sns-gateway |
| Branch / PR | work/sns-gateway-v0.1 / #1 OPEN·DRAFT·UNMERGED |
| Exact candidate | cf6967ce920793a72f88da746af4d0a317e61ed8 |
| Exact tree | c13cb960d960f57979df4a18a7a3236225be52e6 |
| Previous candidate | f5dfbe964789a757bdbdc6fa1c78bf7c861f64b3 |
| Main base | 13d83595e31867c24fb8154074a7ad76d5995f95 |
| CI | 35433487023 — checks / android / ios-simulator SUCCESS |
| Node 시험 | 94/94 PASS; 기존68+새26, 최종 source artifact에서도 확인 |
| Native 파일보호 시험 | 실제 Swift Foundation6개, Linux 로컬/macOS CI PASS |
| 실제 기기·SNS | NOT_RUN |
| SNS Gateway 독립 IVA | NOT_RUN / 입력 패킷 v0.2 준비 |
| PMO / 기타 Persona | NOT_DISPATCHED / NOT_INSTALLED |
| 제품 병합·개인 서명·기기 설치·릴리스·배포 | NOT_DONE |

## 구현과 운영 계약

사진 직접 가져오기·JPEG 사본·등록일 기반 선정·순서·문구·OS 공유·공유 미확인 이력을 보존하고 다음을 추가했다.

- Galaxy 선택 로컬 폴더 / iPhone 선택 PhotoKit 앨범1개 저장·연결해제, 첫 목록baseline, 완전스캔, 신규/편집 identity와 실패pending. 앱 재개/새로고침에서 처리한다.
- SQLite v1/v2→v3 추가 이관, 날짜별 immutable 사진·문구 revision, 버전충돌 거부, 공유 당시 snapshot과 날짜 연결.
- 오래된 알림tap의 날짜 보존과 확인 UI. Android 새알람 원 예정일, iOS 반복알림 전달일을 구분한다.
- 완료/폐기로 닫힌 날짜와 해결된 공유의7일 유예, 미확인/열린날짜 보호, 선택삭제/초기화 확인, journal 재개, tombstone, 관리사진사본500MiB 제한.

외부소스의 최초 관찰시각은 실제 추가시각이 아니다. 소스 사진은 날짜 확인 후보로 가져오며 자동으로 오늘9시 대상이라 단정하지 않는다. 정확한 날짜 자동선정은 앱등록시각이 있는 항목의00:00~09:00 기준이다. 소스 완전목록2,000항목·회차당신규10장·iOS앨범선택100개 제한이 있다.

API 키·SNS HTTP API/OAuth·서버/외부 Storage·Supabase·앱 로그인·원격 Push는 없다. 공식 SNS 앱에서 최종 게시한다. 공유 callback을 원격 게시 성공으로 저장하지 않는다. 원본은 읽기만 하고 앱 사본만 삭제한다. iCloud-only 원본을 자동 다운로드하지 않는다.

Android 알림은 inexact이며 정시전달을 보장하지 않는다. iOS 전달일을 원 예정일로 속이지 않고 사용자가 날짜를 확인한다. 초기화나 공유 실패 후 원래 외부효과를 무조건 재시도하지 않는다.

## 검사·산출물

Source artifact10581721459의 체크섬, 내부 소스ZIP, 파일68개와 exact tree를 대조했다. Android artifact10581822003은 unsigned Release APK, iOS artifact10581741863은 unsigned simulator .app이다. 각각 서명된 Galaxy 설치본·iPhone IPA라고 부르지 않는다.

- 완료보고: docs/execution/SNS_GATEWAY_LIFECYCLE_COMPLETION_20260919.md
- Manifest: docs/execution/SNS_GATEWAY_LIFECYCLE_MANIFEST_20260919.json
- IVA 입력: docs/execution/SNS_GATEWAY_IVA_PACKET_v0.2.md
- 제품 기능/복구 계약: sns-gateway/docs/LIFECYCLE.md

이전 SNS_GATEWAY_COMPLETION_20260919.md와 IVA v0.1 입력은 f5dfbe9 당시 기록으로 보존한다. 이번 후속 구현 상태와 혼동하지 않는다.

## 다음과 남은 수락

작성자 구현 후보를 고정했으며 별도 IVA가 SNS Gateway의 계약·코드·증거를 검증할 차례다. 실제 기기 사진권한·PhotoKit/SAF·메타데이터·소스변경·잠금/재부팅/알림·보존/초기화·SNS별 사진/문구 수신은 NOT_RUN이다. 검증에서 필요한 교정은 그 영향 범위만 다룬다. 서명·실기 설치·SNS 전송·공개 게시·제품 병합·배포는 별도 권한/증거가 필요하다.

기존 bootstrap/web-starter 코드·refs·PR·CI는 이번에 변경하지 않았다. 기존 B/W IVA 네 finding PASS는 해당 후보에 대한 기록이며 이 모바일 앱의 PASS가 아니다.

EFFECT_STATE: sns-gateway 작업 브랜치·Draft PR·작성자 CI와 운영 문서만 변경. 기기·개인사진·SNS·외부 계정·제품main에는 변경 없음.
LAST_WORKLOG_EVENT: E013.
OWNER_ACTION_REQUIRED: 별도 IVA 채널에 SNS_GATEWAY_IVA_PACKET_v0.2.md를 전달한다. 새 저장소·API 키·비밀번호는 필요 없다.

이 문서가 포함된 remote commit/readback으로 기록 완료를 판정한다. 백그라운드 후속작업 예약을 뜻하지 않는다.
