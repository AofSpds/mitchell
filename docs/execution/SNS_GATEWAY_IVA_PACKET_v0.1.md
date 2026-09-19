# MITCHELL → IVA SNS Gateway 구현 후보 검증 입력

PACKET_ID: SNSG-IVA-INBOX-REMINDER-001
VERSION: 0.1
DATE: 2026-09-19
FROM: MITCHELL
TO: IVA
PROJECT: MITCHELL / PRODUCT: sns-gateway
STATUS: AUTHOR_CANDIDATE_FROZEN / IVA_NOT_RUN / MERGE_HOLD

## 정확한 대상

Repository: AofSpds/sns-gateway
Branch / PR: work/sns-gateway-v0.1 / #1
Head: f5dfbe964789a757bdbdc6fa1c78bf7c861f64b3
Tree: edb619aad4a710801fe9d29b84e8938b4a832726
Base main: 13d83595e31867c24fb8154074a7ad76d5995f95
Author CI: 35424606874 — checks/android/ios-simulator SUCCESS
Source artifact: 10579011809, outer SHA-256 274d20e0935f082613bba15feef9bdb40bb2f4a4ada9e146341444511b4dd64f

상세 작성자 결과: docs/execution/SNS_GATEWAY_COMPLETION_20260919.md
체크섬: docs/execution/SNS_GATEWAY_CANDIDATE_MANIFEST_20260919.json
기준 모바일 설계: docs/mobile/LOCAL_SHARING_DESIGN_v1.0_20260919.md
제품 실행 요약: sns-gateway/docs/WORK_PLAN_v0.1.md

## 검증 범위

현재 존재하는 코드 후보만 독립 검증한다. 이 패킷 작성은 dispatch가 아니며 IVA는 아직 NOT_RUN이다. B/W의 과거 PASS를 이 제품에 승계하지 않는다.

1. 로컬 전용 계약: 실제 SNS HTTP/API/OAuth/서버·키 없음; 기기 파일만 OS 공유로 전달하는지.
2. 사진 import/staging: 원본 불변, 로컬 제공자/PhotoKit 네트워크 금지, 크기/픽셀 제한, 경로 이탈·파일변조·오류·권한 거부.
3. SQLite migration2: 기존 이력 보존, FK, 중복 import 날짜 유지, orphan 날짜 미확인, 요청 저장 후 콜백 유실 복구.
4. 사진 선정: KST00:00~09:00, 최대10장 초과·수동 재공유·SNS별 unknown 이력, 순서·문구 snapshot.
5. 알림: 요청/실제 전달/공개를 구분; iOS pending/권한과 Android inexact/재등록·취소·권한 거부. 백그라운드에서 SNS 실행 금지.
6. UI/주장: 희망 SNS와 실제 수신 앱 차이, 공유 callback과 remote 게시 성공 차이, 실기 미시험 안내가 정확한지.

기존 author CI는 exact source임을 확인한 후 재사용한다. native compile 성공은 기기 실행 시험이 아니다. 실제 기기가 없다면 권한/알림/사진 메타데이터/기기별 SNS 수신은 NOT_RUN·INDETERMINATE다. 전체 설계를 처음부터 무정보 재검증하거나 성공 CI를 이유 없이 반복하지 않는다.

## 알려진 미완료 범위

SG-04 지정 폴더/앨범 지속 연동, 자동 보존/삭제·공유 캐시 관리, 날짜별 batch/revision, 자정 이후 오래된 알림 처리, 실기/SNS 수락. 없는 기능을 구현된 것으로 검사하지 않는다. 후보 내부 실제 결함과 계획상 남은 기능을 분리해 보고한다.

## 반환과 권한

결과는 Git에 별도 문서로 보존하고 채팅에는 MITCHELL로 복사 가능한 인계 패킷을 반환한다. exact head/tree, 항목별 PASS/FAIL/INDETERMINATE/NOT_RUN, 실제 실행과 재사용 증거, 새 finding 재현/영향/최소 교정, merge 권고와 release/deploy 구분을 포함한다.

이 요청은 제품 수정·PMO dispatch·기기 설치·실제 사진/SNS 공개·계정 변경·main 병합·릴리스·배포를 승인하지 않는다. PMO NOT_DISPATCHED. 작성자가 IVA로 이름만 바꿔 검증하지 않는다.
