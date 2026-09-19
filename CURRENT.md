# MITCHELL Current

Updated: 2026-09-19 / Generation: 7 / Writer: MITCHELL
Expected previous generation: 6
Recovery base: 3e159bf790ab3cdd775add73b8aa7cde89bf5577

## 목적·현재

SNS Gateway의 중단된 구현을 복구하고 로컬 사진 게시함·예약 알림 구현 후보를 작성자 검사와 native 빌드까지 고정했다. 현재 채널은 MITCHELL이며 PMO로 이관하지 않았다. 전체 v0.1 완성 또는 실기/SNS 수락 완료가 아니다.

| 항목 | 현재 값 |
|---|---|
| Task | SNSG-INBOX-REMINDER-001 |
| 실행 근거 | docs/execution/SNS_GATEWAY_EXECUTION_20260919.md |
| 제품 | AofSpds/sns-gateway |
| Branch / PR | work/sns-gateway-v0.1 / #1 OPEN·DRAFT·UNMERGED |
| Exact candidate | f5dfbe964789a757bdbdc6fa1c78bf7c861f64b3 |
| Exact tree | edb619aad4a710801fe9d29b84e8938b4a832726 |
| Main base | 13d83595e31867c24fb8154074a7ad76d5995f95 |
| CI | 35424606874 — checks / android / ios-simulator SUCCESS |
| 로컬 시험 | Node·SQLite·정적 계약 68/68 PASS |
| 실제 기기·SNS | NOT_RUN |
| 새 후보 독립 IVA | NOT_RUN |
| PMO / 기타 Persona | NOT_DISPATCHED / NOT_INSTALLED |
| 병합·기기 설치·릴리스·배포 | NOT_DONE |

## 구현된 범위

기본 화면·합성 공유 시험을 유지하고, iPhone/Galaxy에서 사용자 선택 로컬 사진을 가져오는 native 코드, JPEG 사본과 중복 등록 방지, SQLite v1→v2 이관, 등록일 기준 오늘 사진 선정, 순서·문구 저장, OS 공유와 대상별 미확인 이력을 추가했다. iOS 캘린더/Android inexact 알림, 권한 확인·활성화/해제·시험 알림을 구현했다.

API 키는 필요 없다. 외부 서버/Storage, Supabase, SNS HTTP API/OAuth, 앱 로그인·원격 Push를 추가하지 않았다. 실제 SNS 로그인·계정 선택·최종 게시는 해당 앱에서 사용자 작업이다. 공유 callback을 remote 게시 성공으로 기록하지 않는다.

Android unsigned Release는 JS가 포함된 컴파일 산출물이며 인터넷 권한 제거 검사가 성공했다. iOS 산출물은 unsigned simulator .app이다. 각각 서명된 기기 설치 APK 또는 iPhone IPA라고 부르지 않는다. 소스 artifact의 SHA와 Git tree를 직접 대조했다.

## 남은 범위와 다음

SG-04 지정 폴더/앨범 지속 연결, 로컬 보존·삭제·공유 캐시 관리, 날짜별 batch/revision과 오래된 알림 날짜 처리는 아직 미완성이다. 실제 기기 사진 권한·iCloud-only/로컬 파일·메타데이터·잠금/재부팅/알림 지연·SNS별 사진/문구 수신은 NOT_RUN이다. Android 알림은09:00 목표 inexact이며 정확한 전달·SNS 공개를 보장하지 않는다.

다음 독립 구현은 SG-04와 보존 관리다. 본 고정 후보의 IVA 입력은 docs/execution/SNS_GATEWAY_IVA_PACKET_v0.1.md에 준비했으며 실제 dispatch는 하지 않았다. 기존 B/W PASS를 새 앱의 검증으로 사용하지 않는다. 제품 main은 초기 진입점이며 구현 검토에는 작업 브랜치/정확한 소스를 사용한다.

완료보고: docs/execution/SNS_GATEWAY_COMPLETION_20260919.md
체크섬: docs/execution/SNS_GATEWAY_CANDIDATE_MANIFEST_20260919.json

## 기존 B/W 보존

이번 작업에서 bootstrap/web-starter 코드·refs·PR·CI를 변경하지 않았다. 마지막 확인 후보는 bootstrap f880d0297e4d092c0f7d5025ff42cc7648fb66aa / web-starter a2c63cca6bdbf0667ba2d9e1c8d8e7af2e137a17이다. 과거 IVA B001/B002/W001/W002 PASS, merge recommendation PASS, release/deploy HOLD와 실제 Windows/Supabase NOT_RUN은 해당 범위의 기록으로 보존한다.

EFFECT_STATE: sns-gateway 작업 브랜치·Draft PR·작성자 CI와 운영 문서만 변경. 사용자 기기·실사진·외부 계정·SNS 공개·제품 main에는 변경 없음.
LAST_WORKLOG_EVENT: E012.
OWNER_ACTION_REQUIRED: 현재 코드/기록 작업과 API 키 등록에는 없음. 기기 설치·실사진 공유·공개 게시·병합·배포는 별도 경계다.
