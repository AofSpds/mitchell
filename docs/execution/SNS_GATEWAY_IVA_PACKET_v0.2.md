# MITCHELL → IVA SNS Gateway 구현 후보 검증 패킷

PACKET_ID: MITCHELL-SNSG-IVA-002  
VERSION: 0.2  
DATE: 2026-09-19  
FROM: MITCHELL  
TO: IVA  
PROJECT: MITCHELL / PRODUCT: SNS Gateway  
STATUS: AUTHOR_COMPLETED / EXACT_CANDIDATE_FIXED / IVA_NOT_RUN / MERGE_HOLD

## 1. 요청

SNS Gateway의 로컬 사진 게시함·소스 연결·날짜 묶음·알림·공유·보존 코드 후보를 별도 IVA로 검증한다. 이전 SNS_GATEWAY_IVA_PACKET_v0.1.md의 f5dfbe9 입력은 과거 후보로 보존하고 이번 입력으로 갱신한다. SNS Gateway에 이전 독립 PASS가 있었다고 가정하지 않는다. Bootstrap/Web Starter의 과거 IVA PASS를 이 앱으로 확장하지 않는다.

서버/외부 사진 저장소/API 키/OAuth/무인 게시 없음. 사용자 foreground 공유와 공식 SNS 앱 최종 수동 게시가 계약이다. 작성자 MITCHELL과 검증자 IVA를 구분한다. PMO NOT_DISPATCHED, 다른 페르소나/페어 검증자 NOT_INSTALLED.

## 2. 고정 대상

```text
Repository = AofSpds/sns-gateway
Branch = work/sns-gateway-v0.1
PR = #1 (OPEN / DRAFT / UNMERGED)
Exact head = cf6967ce920793a72f88da746af4d0a317e61ed8
Exact tree = c13cb960d960f57979df4a18a7a3236225be52e6
Previous candidate = f5dfbe964789a757bdbdc6fa1c78bf7c861f64b3
Base main = 13d83595e31867c24fb8154074a7ad76d5995f95
Author CI = 35433487023
```

검증 시작 전에 branch/PR head를 읽는다. 다르면 이 frozen candidate를 검증하고 차이를 명시하거나 필요한 영향 범위만 확장한다. main의 초기 README나 PR merge-test ref를 검증 대상으로 혼동하지 않는다.

## 3. 원천과 작성자 보고

운영 저장소 AofSpds/mitchell:

- docs/mobile/LOCAL_SHARING_DESIGN_v1.0_20260919.md — 최초 로컬 상세 설계.
- docs/execution/SNS_GATEWAY_LIFECYCLE_COMPLETION_20260919.md — 후속 네 코드 영역과 작성자 검사·한계.
- docs/execution/SNS_GATEWAY_LIFECYCLE_MANIFEST_20260919.json — exact source/build·체크섬.
- docs/execution/SNS_GATEWAY_COMPLETION_20260919.md — 이전 f5dfbe9 범위의 역사적 완료보고.
- AGENTS.md / docs/execution/IVA_RETURN_PACKET_RULE_20260915.md — 역할/권한/반환 계약.

제품 후보의 docs/WORK_PLAN_v0.1.md, docs/LIFECYCLE.md, README.md, docs/PRIVACY.md, docs/DEVICE_COMPATIBILITY.md도 읽는다. 문서의 구현 주장과 실제 코드를 대조한다. 이 패킷 자신의 운영 commit/blob은 패킷이 포함된 remote commit/readback 영수증을 사용하며 미래 SHA를 예상해 쓰지 않는다.

## 4. 작성자 증거 — 재실행 전에 재사용 가능성을 판단

- checks105872116241: lint/typecheck, Node/SQLite/정적 계약94개, Expo SDK 호환성, Android/iOS JS bundle SUCCESS.
- android105872208793: unsigned Release 컴파일과 INTERNET 권한 제거 검사 SUCCESS.
- ios-simulator105872208796: unsigned simulator Release 컴파일 SUCCESS.
- 실제 Swift LocalManagedFiles.swift의 파일 보호 fixture6개: Linux 로컬 및 macOS CI PASS.
- source artifact10581721459 outer SHA-256 b77adc0c88ca0ffda5ebe60926f6fb903f3df91273c349e309d1272a51d3626e.
- inner source ZIP SHA-256 95d731750872ee45f35dfe3921bec493edc2ab2841b6d3389bd1c4ecac73d363.
- 소스68개로 재계산한 tree가 exact tree와 일치하고 그 소스 자체의94개 시험도 PASS.

작성자 CI/fixture를 IVA가 직접 실행한 증거로 표현하지 않는다. 전체 프로젝트를 무정보 반복하지 않으며 새 위험/검증 공백을 대상으로 독립 증거를 만든다. raw logs나 개인 사진/인증정보를 공개 Git에 저장하지 않는다.

## 5. 우선 검증 경로

| ID | 확인할 계약 | 주요 파일 |
|---|---|---|
| SGV-01 | 외부 서버/키/자동 전송 부재, OS 공유와 사용자 최종 게시 구분 | app/GatewayApp.tsx, src/services/share.ts, native LocalPlatformModule, app.config.ts/plugins |
| SGV-02 | 선택권한·최초baseline·완전스캔·재등장/편집·pending·등록일불명 | LocalSources.kt/.swift, src/services/sources.ts, src/storage/lifecycle.ts |
| SGV-03 | v1/v2 이력 보존, v3 migration, immutable revision/CAS, 공유 당시 snapshot | src/storage/history.ts, migration2.ts/migration3.ts, lifecycle.ts |
| SGV-04 | 원 예정일/실제 전달일의 차이, 오래된 알림의 명시적 날짜 선택, 강제 UI/자동게시 없음 | LocalReminders.kt/.swift, ReminderLifecycle.swift, app/GatewayApp.tsx |
| SGV-05 | 미확인 공유/열린날짜 보호,7일유예, 수동삭제/초기화 확인, tombstone/journal 재개 | src/domain/lifecycle.ts, src/services/maintenance.ts, LocalManagedFiles.kt/.swift |
| SGV-06 | 공유상태·완료시각 원자성, 중복실행·오류·초기화중단 복구 | history.ts, share.ts, GatewayApp.tsx, tests/helpers/runtime.mjs |
| SGV-07 | 원본보존·형식/크기·metadata·iCloud-only 제외·공유 경로/백업/로그 | LocalPhotos.kt/.swift, native FileProvider, plugins, docs/PRIVACY.md |
| SGV-08 | 코드/fixture/native compile/device/IVA/release의 주장범위와 증거 일치 | README/완료보고/Manifest/DEVICE_COMPATIBILITY.md |

초기 제한은 소스1개, 완전목록2,000개, 회차당신규10장, iOS앨범선택100개, 관리JPEG사본500MiB다. 이는 OS/SNS 공통한도 주장이 아니다. 외부소스 firstObserved와 실제membership시각을 혼동하지 않아야 한다. 정확한날짜 자동선정은 앱등록시각이 있는 항목에 한한다.

실기 환경이 없으면 권한철회·PhotoKit/SAF·알림cold/warm·재부팅·metadata·SNS 수신을 NOT_RUN/INDETERMINATE로 남긴다. native compile 또는 Python/Node모형으로 실기 PASS를 만들지 않는다.

## 6. 기대 반환 및 권한

상세 결과를 Git에 기록하고 채팅 최종 반환은 MITCHELL에 복사할 인계 패킷만 제공한다. 최소: PACKET_ID/버전/날짜/FROM/TO/PROJECT, 실제 exact head/tree, 영역별 PASS/FAIL/INDETERMINATE/NOT_RUN, 재사용/새 실행 증거 구분, finding ID·재현·영향·최소 교정, 결과 path/commit/blob, MERGE_RECOMMENDATION과 RELEASE_DEPLOY_RECOMMENDATION, 미실행·권한·다음 조치.

허용 범위는 read-only 독립검증, 격리 fixture 시험 및 IVA 자신의 정제 보고서 Git 기록이다. 제품 코드 수정, PR ready/merge, Template/릴리스/배포, 사용자 기기 설치, 실제 SNS 전송/게시, 계정·키·결제, 다른 Persona dispatch는 승인하지 않는다. 보고서는 작성자가 교정할 사항을 식별하되 결과 자체를 실행권한으로 바꾸지 않는다.
