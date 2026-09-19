# SNS Gateway — 로컬 게시함·알림 구현 후보 완료보고

문서 ID: SNSG-INBOX-REMINDER-COMPLETION-001  
날짜: 2026-09-19 / 작성자: MITCHELL  
상태: AUTHOR_IMPLEMENTATION_CANDIDATE / DEVICE_ACCEPTANCE_NOT_RUN / IVA_NOT_RUN

## 1. 승인과 실제 작업 범위

사용자가 SNSG-WORK-001 초안 뒤 현재 MITCHELL 채널에서 작업 진행을 승인했고, 응답 중단 후 계속 진행하도록 지시했다. 제품은 서버 없는 OS 공유 경로다. API 키 등록 질문을 SNS HTTP API/OAuth 또는 외부 서버 추가 승인으로 해석하지 않았다. 코드·작업 브랜치·Draft PR·작성자 시험을 수행한다. PMO dispatch, 사용자 기기 설치, SNS 실제 전송/공개 게시, 제품 main 병합, 배포·결제는 하지 않았다.

기준 설계: AofSpds/mitchell@3e159bf790ab3cdd775add73b8aa7cde89bf5577, docs/mobile/LOCAL_SHARING_DESIGN_v1.0_20260919.md. 실행 요약: sns-gateway/docs/WORK_PLAN_v0.1.md.

## 2. 중단 복구

기존 제품 브랜치 54cbdcbd77dd2fbcd46b28597e8e5863c7883f0b와 Draft PR #1을 먼저 복구했다. 최초 코드 9053185…의 TypeScript/Expo 호환성 문제는 이전 작업에서 21a9105…로 교정됐으며, CI 35422061756의 checks/Android/iOS simulator 성공이 존재했다. 마지막 54cbdcbd…는 문서 전용 변경이었다. 기존 코드를 재생성하거나 성공 CI를 무정보 재실행하지 않았다.

## 3. 정확한 새 후보

- Repository: AofSpds/sns-gateway
- Branch / PR: work/sns-gateway-v0.1 / #1 (Draft, unmerged)
- Main base: 13d83595e31867c24fb8154074a7ad76d5995f95
- Resume base: 54cbdcbd77dd2fbcd46b28597e8e5863c7883f0b
- Candidate head: f5dfbe964789a757bdbdc6fa1c78bf7c861f64b3
- Candidate tree: edb619aad4a710801fe9d29b84e8938b4a832726
- Author CI: 35424606874
- CI result: SUCCESS — checks / android / ios-simulator

## 4. 작성한 기능

| 영역 | 실제 구현 | 한계 |
|---|---|---|
| 앱 화면 | 오늘/사진/이력/설정, 합성 1장·3장 시험 유지 | 실기 렌더링·접근성 수락은 미실행 |
| 로컬 가져오기 | iOS PHPicker+허용된 PhotoKit 자산 / Android 로컬 문서 선택 | iOS cloud-only 및 권한 외 사진, Android 미허용 공급자는 건너뜀 |
| 사진 준비 | 크기 제한·방향 반영 JPEG 사본·준비 사본 해시·원본 보존 | 사진 내용 자체의 개인정보 제거 아님; 실제 EXIF/색상/기기 검증 미실행 |
| 로컬 보관 | private/no-backup 게시함, SQLite 메타데이터·문구 | 삭제·보존기간 관리 미완성; 앱 삭제 후 이력 복구 보장 없음 |
| 등록/복구 | 동일 준비 사본 재등록시 날짜 유지, orphan은 날짜 미확인 | 시각적 중복 이미지 전체 탐지 아님 |
| 사진 선정 | KST 00:00~09:00 등록분, 최대10장, 수동 포함/제외·순서 | 외부 폴더 추가시각 추정 없음; 자정 이후 오래된 알림 날짜별 batch 미완성 |
| 공유 | foreground 확인, immutable staging, OS 단일/다중 공유 | 원하는 SNS와 실제 선택한 앱이 다를 수 있음 |
| 이력 | 요청·미확인·취소·사용자 진술 분리, 대상별 자동 재선정 방지 | remote post ID/실제 SNS 게시 성공 확인 없음 |
| 알림 | iOS 반복 KST calendar / Android inexact AlarmManager, 권한·ON/OFF·시험 | 정시 수신/실게시 보장 없음; Android 요청 기록과 실제 시스템 전달 구분 |
| 이전 DB | v1→v2 마이그레이션과 FK 확인, 기존 공유 기록 유지 | 실패하면 보존 후 오류; 자동 초기화 없음 |

API 키, 앱 로그인, Meta OAuth, Supabase, 서버·외부 사진 저장소, 원격 Push를 추가하지 않았다. 공식 SNS 앱의 로그인과 최종 게시는 사용자 작업이다.

## 5. 작성자 검사

| 검사 | 증거 | 판정 |
|---|---|---|
| Node 단위/SQLite/정적 계약 | 로컬 68/68, CI exact source | PASS |
| lint / TypeScript | CI checks | PASS |
| Expo SDK compatibility | CI expo install --check | PASS |
| Android/iOS JS bundles | CI expo export | PASS |
| Android unsigned release + no INTERNET manifest | native CI | PASS |
| iOS unsigned simulator Release | native CI | PASS |
| 실제 Galaxy/iPhone 실행·사진·알림·SNS | 기기 미연결 | NOT_RUN |
| 독립 IVA | 별도 실행 없음 | NOT_RUN |

최초 로컬 작성자 시험 중 schema2 INSERT의 placeholder 개수 불일치를 발견해 제품 SQL과 fixture를 수정했다. 새 기능과 충돌한 과거 ‘알림 미구현’ 정적 시험은 현재 정확한 기능/미검증 표시 계약으로 교체했다. 테스트 기대치를 낮춰 remote 게시 성공을 만들어내지 않았다. Kotlin 메서드는 예약어와 충돌하지 않는 importPhotos로 명명했다. 이 지역 수정들은 위 후보 커밋 전에 이루어졌다.

## 6. 산출물 동일성

| Artifact | ID | 외부 SHA-256 |
|---|---|---|
| Source/evidence | 10579011809 | 274d20e0935f082613bba15feef9bdb40bb2f4a4ada9e146341444511b4dd64f |
| SNS_GATEWAY_ANDROID_UNSIGNED_CANDIDATE_20260919.zip | 10578728569 | afe512c56a531b04fd796332537fd128742d24bd59b1ad82f7813024cbe233b4 |
| SNS_GATEWAY_IOS_SIMULATOR_CANDIDATE_20260919.zip | 10578232470 | 7a93f190f9e0e3a08ff62ce085c33e05efe99c825523c569b5583a275ca269ca |

내부 source ZIP SHA-256: `b81a9d0842fc15bfba9a181e69a3f51df3c54e4533366f07060b5ddc8bb3d075`. 원본 파일54개이며 재계산 tree가 후보 tree와 일치했다.


소스 ZIP의 경로 안전성과 무결성을 확인하고 추출된 파일로 Git tree를 재계산했다. 최초 로컬 작업본과 최종 artifact의 변경18파일 중 정적 계약 시험 파일 끝 빈 줄1개 차이만 있었고 실행 의미 차이는 없었다. 실제 artifact에서 시험68개를 다시 실행했다. 이는 작성자 검사이며 IVA가 아니다.

Android unsigned APK는 그대로 설치하는 서명된 APK가 아니다. iOS simulator .app은 iPhone 기기용 IPA가 아니다. JS 번들을 포함한 빌드와 실제 설치/공유 성공을 혼동하지 않는다.

## 7. 원 작업계획 대비 상태

- SG-00 기반/CI: 구현 및 작성자 검사.
- SG-01 합성 공유 bridge: 기존 구현 보존; 실기 호환성 미실행.
- SG-02 화면/Provider: 구현 후보.
- SG-03 로컬 게시함/사본: 핵심 구현; 보존·용량 관리 수락 남음.
- SG-04 지정 폴더/앨범 지속 연결: NOT_IMPLEMENTED.
- SG-05 날짜 선정/순서/문구: 핵심 구현; 날짜별 batch/revision 등 남음.
- SG-06 OS 알림: 코드 구현; 잠금/전원/재부팅/시간 변경 등 기기 시험 남음.
- SG-07 OS 공유 bridge: 구현; SNS별 사진/문구 수신·다중/순서 NOT_RUN.
- SG-08 이력/복구: 핵심 구현; 자동 보존 정리·포괄적 보안 수락 남음.
- SG-09 작성자 정적/빌드 검사: 위 결과. 실기 NOT_RUN.
- SG-10 본 후보 완료보고·정확 대상 고정: 본 문서와 Git readback.
- SG-11 IVA: NOT_RUN.
- SG-12 병합·실사용 배포: NOT_DONE.

전체 v0.1 완성, 지정 폴더 자동 감시, 무인 SNS 예약 게시를 주장하지 않는다. 다음 독립 구현 범위는 SG-04와 보존/로컬 삭제 관리이며, 실기 공유 검증은 실제 사용자 권한·기기가 필요하다. 사용자에게 키를 요구하거나 같은 설계 질문을 반복하지 않는다.
