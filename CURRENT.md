# MITCHELL Current

Updated: 2026-09-19 / Generation: 10 / Writer: MITCHELL
Expected previous generation: 9
Recovery base: edd19651259193be0320197bda26cc8d448153ce

## 목적·현재

별도 IVA의 SNS Gateway 최초 검증 FAIL을 수신하고 SGV-F001–F004만 교정했다. 현재 상태는 작성자 교정·검사 완료 후보이며 별도 IVA affected-only 재검증은 NOT_RUN이다. 최초 FAIL과 MERGE/RELEASE HOLD를 유지한다. 현재 채널의 작성자는 MITCHELL이며 PMO로 이관하지 않았다.

| 항목 | 현재 값 |
|---|---|
| Task | SNSG-IVA002-CORRECTION-001 |
| 실행 근거 | 기존 사용자 구현·계속 지시 D021 및 F001–F004 교정 처분 D024 |
| 원 IVA 결과 | docs/execution/SNS_GATEWAY_IVA_RESULT_002_20260919.md v1.0.1 |
| 원 결과 commit / blob | 5133d5fe9af59b8ed06e7bf7bedba29108c10c79 / 575fbf734d47484a14a6d3fd8a1a79db736be60f |
| 원 검증 후보 | cf6967ce920793a72f88da746af4d0a317e61ed8 / FAIL |
| 제품 / Branch / PR | AofSpds/sns-gateway / work/sns-gateway-v0.1 / #1 OPEN·DRAFT·UNMERGED |
| 새 교정 head | 4ab10f481f7da66c00591ebd6cf597de527b901c |
| 새 교정 tree | caa83524a6118272952fd669d225d253dd477edb |
| 제품 main (변경 없음) | 13d83595e31867c24fb8154074a7ad76d5995f95 |
| 최종 작성자 CI | 35439466823 / checks / android / ios-simulator 모두 SUCCESS |
| 작성자 시험 | 표적 TS/SQLite42개, 확장 Node110개, Kotlin/JVM23개, Swift 신규16+기존6개 PASS |
| 독립 재검증 / 실제 기기·SNS | NOT_RUN / NOT_RUN·INDETERMINATE |
| PMO / 기타 Persona | NOT_DISPATCHED / NOT_INSTALLED |
| 제품 병합 / release·deploy | NOT_DONE·HOLD / NOT_DONE·HOLD |

## 교정한 네 실패 경로

- F001: 실패 pending의 순환 차례·횟수·마지막 결과를 DB에 보존하고 native 호출 전에 차례를 옮겨 후속 정상 사진도 처리한다. baseline/등록일/실패 이력은 유지한다.
- F002: 미확인·오류/전체 이력 필터와 안정적인 이전 페이지를 제공한다. 100건 밖 및205개 미확인 이력에 접근할 수 있으며 삭제 보호를 제거하지 않았다.
- F003: Android의 엄격한 UUID.tmp 중단 사본을 inventory·용량·초기화에 포함한다. active writer와 최근 tmp는 보호하며 비활성 중단 tmp는1시간 유예 후 정리한다. 공유 staging의7일 유예와 다르다.
- F004: 열거/metadata/삭제 오류와 확정된 부재를 구분한다. DB-known URI도 journal에 남기고 마지막 완전 inventory가 비어야 초기화를 완료한다. 실패/부분완료 시 reset_pending을 유지한다.

SQLite migration4는 부가 retry 테이블과 이력 index만 추가한다. 기존 원본·등록일·revision·공유 snapshot·미확인 보호는 유지한다. 상세 교정·표적 시험은 아래 보고서가 소유한다.

첫 교정9847200c의 CI35438877502는 checks/iOS 성공, Android Os.unlink 공개 심볼 오류로 실패했다. 공개 Os.remove와 전후 lstat로 수정한4ab10f48가 최종 후보이며 중간 실패는 이력에 보존한다. Kotlin shim 시험을 실제 Android SDK/실기 성공으로 취급하지 않는다. Android minSdk24, 의존성 잠금, 알림·SNS 공유 방식은 그대로다.

## 증거·보고서·다음

- 작성자 교정 보고: docs/execution/SNS_GATEWAY_IVA002_CORRECTION_COMPLETION_20260919.md
- exact candidate/CI/artifact: docs/execution/SNS_GATEWAY_IVA002_CORRECTED_MANIFEST_20260919.json
- IVA affected-only 입력: docs/execution/SNS_GATEWAY_IVA_AFFECTED_REREVIEW_PACKET_v0.3.md
- 과거 작성자 보고/Manifest/IVA v0.2는 당시 후보의 기록으로 보존한다.

최종 source artifact75개 파일을 로컬 검사 소스 및 Git tree와 대조하고 그 소스의 표적42개 시험을 확인했다. 원 후보 CI35433487023은 재실행하지 않았다. 새 후보의 공통 저장·파일 경계에 대한 작성자 회귀와 native 빌드이며 전역 독립검증 반복이 아니다.

다음은 새 exact 후보의 F001–F004 affected-only 재검증이다. 첫 IVA의 SGV-01/03/08 PASS와 SGV-04 INDETERMINATE 범위를 소급 변경하지 않는다. 실제 PhotoKit/SAF·권한·알림·SNS·메타데이터·백업 수락은 여전히 미실행이다. Android unsigned APK와 iOS simulator .app은 실제 기기 설치·서명·배포 완료본이 아니다.

## 권한·보존

서버·외부 사진 Storage·SNS HTTP API/OAuth·API 키·앱 로그인 없음. 최종 게시 수동이며 API 키 등록은 불필요하다. 실제 기기·개인사진·계정·SNS 전송·게시·서명·제품 main·릴리스·배포에는 변경하지 않았다. 기존 bootstrap/web-starter 및 그 과거 IVA 결과도 변경하지 않았다.

EFFECT_STATE: sns-gateway 교정 작업 브랜치·작성자 CI·Draft PR와 MITCHELL 수신/완료 기록만 변경.
LAST_WORKLOG_EVENT: E014 (초기 교정); 중간 빌드 교정과 최종 후보는 본 CURRENT 및 교정 완료보고에 보존.
OWNER_ACTION_REQUIRED: 새 IVA affected-only 패킷을 별도 IVA 채널에 전달. API 키·비밀번호·새 저장소는 불필요.
