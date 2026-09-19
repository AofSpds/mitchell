# SNS Gateway — IVA-002 구현 후보 독립검증 결과

DOCUMENT_ID: MITCHELL-SNSG-IVA-002-RESULT  
VERSION: 1.0.1  
DATE: 2026-09-19  
FROM: IVA / TO: MITCHELL  
PROJECT: MITCHELL / PRODUCT: SNS Gateway  
INPUT: MITCHELL-SNSG-IVA-002 v0.2  
STATUS: REVIEW_COMPLETED / CORRECTION_REQUIRED / MERGE_HOLD

검증자는 이 IVA 대화이며 제품 작성자는 MITCHELL이다. 실제 실행은 GitHub connector 조회와 격리 Linux의 소스·SQLite·JVM·Swift Foundation fixture 시험이다. 별도 PMO, Codex WORK 또는 페어 검증자 프로세스를 실행하지 않았다. 제품 코드를 고쳐서 통과시킨 검증이 아니다.

## 1. 결론

고정 후보에 교정이 필요한 MEDIUM / P2 finding 4건을 확인했다. 정상 공유 경계와 이관·보존의 여러 방어 경로는 유지되지만, 실패 항목의 처리 순서, 오래된 미확인 공유의 접근성, 중단된 임시 사본, 파일 I/O 오류의 삭제 판정이 계약을 충족하지 못한다.

```text
SNS_GATEWAY_CANDIDATE = FAIL
INDEPENDENT_REVIEW = COMPLETED
FINDINGS = SGV-F001 / SGV-F002 / SGV-F003 / SGV-F004
MERGE_RECOMMENDATION = HOLD
RELEASE_DEPLOY_RECOMMENDATION = HOLD
DEVICE_SNS_ACCEPTANCE = NOT_RUN / INDETERMINATE
PMO_RUNTIME = NOT_DISPATCHED
```

네 기능 영역의 코드가 없다는 판정이 아니다. 코드 후보가 존재하고 작성자 빌드도 성공했으나 아래 오류 조건을 교정해야 한다는 판정이다. 실제 개인정보 유출, SNS 자동 게시, 원본 사진 파괴를 관측한 것은 아니다. MEDIUM / P2는 특정 실패·누적·중단 조건에서 진행 또는 안전한 삭제·복구 계약이 깨진다는 의미다.

### 영역별 판정

PASS는 명시한 소스·fixture 범위의 판정이며 실기 수락 PASS가 아니다. FAIL 영역 안에서도 정상으로 확인한 하위 경로는 증거로 유지한다.

| 영역 | 판정 | 근거와 한계 |
|---|---|---|
| SGV-01 로컬 전용·수동 공유 | PASS | 자체 코드의 서버/HTTP 게시·키/OAuth 경로 부재, foreground 진입·사용자 확인·OS 결과와 사용자 게시 진술 분리 확인. 실기 통신/SNS 수신은 NOT_RUN. |
| SGV-02 소스·baseline·pending | FAIL | 초기 baseline, 완전 스캔, 재등장 identity와 등록일 불명 처리는 유지. 앞선 실패 10개가 이후 정상 항목을 계속 막음: F001. |
| SGV-03 이관·revision·snapshot | PASS | exact v1/v2→v3 SQL의 이력 보존·부분 실패 rollback, revision INSERT/CAS와 snapshot 일치 검사 확인. 실제 모바일 DB 수락은 별도. |
| SGV-04 알림 날짜·재개 | INDETERMINATE | Android 예정일/iOS 전달일 구분과 명시적 날짜 선택 경로는 소스에 존재. cold/warm tap·재부팅·지연·시간대 변경을 실제 OS에서 실행하지 않아 영역 전체 수락을 확정하지 않음. |
| SGV-05 보존·삭제·journal | FAIL | 과거 revision 참조 보호·닫힌 날짜 유예·tombstone 재적용은 통과. 임시 사본 누락과 I/O 오류의 잘못된 완료 판정: F003/F004. |
| SGV-06 공유 원자성·복구 | FAIL | 공유 단일 실행·선기록·callback 저장 실패 rollback은 통과. 숨겨진 미확인 이력의 해결 불가와 잘못된 reset 완료: F002/F004. |
| SGV-07 사진·파일·개인정보 | FAIL | 원본 읽기·로컬 사본·제한된 공유 경로는 유지. 로컬 사본 삭제 누락/허위 완료: F003/F004. metadata·iCloud-only·백업·수신 앱 실기는 NOT_RUN. |
| SGV-08 증거·완료 구분 | PASS | 고정 소스와 artifact 동일성 확인. 작성자 CI, unsigned native compile, simulator, 기기/SNS·IVA·릴리스 미실행의 구분은 적절. 기능 계약의 불일치는 F001–F004로 별도 반환. |

## 2. 고정 대상과 원천

| 항목 | 값 |
|---|---|
| Repository | AofSpds/sns-gateway |
| Branch / PR | work/sns-gateway-v0.1 / #1 |
| Exact head | cf6967ce920793a72f88da746af4d0a317e61ed8 |
| Exact tree | c13cb960d960f57979df4a18a7a3236225be52e6 |
| Parent / previous candidate | f5dfbe964789a757bdbdc6fa1c78bf7c861f64b3 |
| Base main | 13d83595e31867c24fb8154074a7ad76d5995f95 |
| 시작 branch/PR 및 검토 종료 전 PR 확인 | 위 head 동일, OPEN / DRAFT / UNMERGED |
| 관측된 PR merge-test ref | 384eda9caafd95c8b9fe5dab23f41cb05ecc9aeb; 검증 대상으로 사용하지 않음 |
| 운영 원천 anchor / 쓰기 전 main | AofSpds/mitchell@b6a8475dc5d273ebe3c3153b1da7901d550d3f04 |
| 입력 문서 | docs/execution/SNS_GATEWAY_IVA_PACKET_v0.2.md |
| 입력 문서 blob | 12835f14f7089ada34852da19a963683a57056ba |

GitHub connector로 README, CURRENT, AGENTS, DECISIONS와 반환 계약을 복구했다. CURRENT generation 8과 MITCHELL writer 소유권을 확인했으며 이 검증자가 해당 lineage를 수정하지 않는다.

운영 원천은 위 anchor의 다음 문서다: `docs/mobile/LOCAL_SHARING_DESIGN_v1.0_20260919.md`, `docs/execution/SNS_GATEWAY_LIFECYCLE_COMPLETION_20260919.md`, `docs/execution/SNS_GATEWAY_LIFECYCLE_MANIFEST_20260919.json`, 과거 `SNS_GATEWAY_COMPLETION_20260919.md`, `IVA_RETURN_PACKET_RULE_20260915.md`. 제품의 README, AGENTS, WORK_PLAN, LIFECYCLE, PRIVACY, DEVICE_COMPATIBILITY와 SGV-01–08 관련 실행·저장·native 코드를 대조했다.

최초 상세 설계의 당시 미구현 상태와 후속 구현 완료보고를 구분했다. 이전 SNS Gateway v0.1 입력/f5dfbe9 후보의 기록 및 Bootstrap/Web Starter 독립 PASS를 이번 앱의 독립 PASS로 사용하지 않았다.

## 3. 소스 동일성과 작성자 증거 재사용

### 독립 계산한 소스 동일성

| 항목 | 결과 |
|---|---|
| Source artifact | 10581721459 / source-and-author-evidence |
| Outer SHA-256 | b77adc0c88ca0ffda5ebe60926f6fb903f3df91273c349e309d1272a51d3626e |
| Inner sns-gateway-source.zip SHA-256 | 95d731750872ee45f35dfe3921bec493edc2ab2841b6d3389bd1c4ecac73d363 |
| COMMIT.txt / TREE.txt | 위 exact head/tree와 동일 |
| Source inventory | 68개 파일 |
| 재계산 tree | c13cb960d960f57979df4a18a7a3236225be52e6 |
| 계산 방법 | 추출 원본 바이트의 Git blob 및 tree 재구성; 제품 파일 수정·줄바꿈 변환 없음 |
| 안전성·종료 점검 | ZIP 경로 탈출/절대경로/symlink 배제; 시험 후 68개 SHA-256·blob 및 파일 수 불변 확인 |

이는 manifest의 값을 단순 인용한 것이 아니라 내려받은 소스 artifact에서 새로 계산한 결과다. PR merge-test commit이나 main 초기 README를 후보 소스로 대신 사용하지 않았다.

### 재사용한 작성자 CI

GitHub에서 run `35433487023`의 job·step 상태와 artifact metadata를 직접 조회했다. CI 재실행과 기존 94개 전체 시험의 반복 실행은 하지 않았다.

| Job | ID | 직접 조회한 상태·단계 |
|---|---|---|
| checks | 105872116241 | SUCCESS; npm check, Expo 호환성, Android/iOS JS bundle, exact source packaging |
| android | 105872208793 | SUCCESS; unsigned Release compile/bundled JS, INTERNET 권한 제거 검사 |
| ios-simulator | 105872208796 | SUCCESS; Swift managed-file guard fixture 단계, unsigned simulator compile/package |

작성자 보고의 Node/SQLite/정적 계약 94개와 Swift 파일 보호 fixture 6개는 작성자 증거다. 이 수치를 IVA의 신규 실행 횟수로 바꾸지 않았다. 위 표는 직접 조회한 job/step 요약이며 전체 raw job log를 이번에 다시 수집·게시했다는 뜻이 아니다.

Android artifact `10581822003`의 outer digest `a3d5706d0f2342297c1056de78ff3315d4b9ec954ef63b95046d4a20679152fa`, iOS artifact `10581741863`의 outer digest `23b79f2f25a26f5db90f473cb70cc87d866cf281b97b619be0a8b6c6a228dad2`는 GitHub metadata와 작성자 manifest가 일치한다. 두 native binary를 IVA가 새로 내려받아 내부 APK/.app 또는 통신을 분석한 것은 아니다. Android unsigned APK는 서명된 기기 설치본이 아니고 iOS simulator .app은 iPhone IPA가 아니다.

## 4. IVA가 새로 실행한 증거

### 실행 환경과 대체 경계

Node `v22.16.0`의 `node:vm`, TypeScript strip, 실제 in-memory `node:sqlite`로 frozen TypeScript 저장·서비스 코드를 실행했다. 후보의 `tests/helpers/runtime.mjs`를 이용하되 공유 시험에는 별도 외부 fixture loader에서 React Native AppState만 추가 대체했다. 제품 소스와 제품 test helper 자체는 수정하지 않았다. Expo SQLite 비동기 API는 실제 SQLite에 연결하는 adapter이며 모바일 Expo runtime 자체는 아니다.

Kotlin `kotlinc-jvm 1.9.0` / OpenJDK `21.0.11`에서 실제 `LocalManagedFiles.kt`를 실행했다. Android Context의 두 디렉터리 반환값만 fixture로 대체했으며 파일 열거·권한·삭제는 실제 JVM/File I/O다. Swift `6.2.1`, target `x86_64-unknown-linux-gnu`에서 실제 `LocalManagedFiles.swift`를 실행했다. UIKit/PhotoKit은 실행하지 않았다.

native 권한 시험은 격리된 새 합성 파일 디렉터리를 비특권 사용자로 실행했고 시험 후 권한을 복구했다. 파일에는 합성 marker bytes만 넣었다. 사용자 사진, 실제 인증정보, 실제 SNS 계정은 사용하지 않았다.

### 신규 시나리오 그룹 18개

아래는 목적별 관측이다. 결함 재현 성공을 제품 PASS로 합산하지 않는다.

| 그룹 | 수 | 결과 |
|---|---:|---|
| pending 순서·이력 조회·reset 조합 | 3 | F001/F002 및 빈 inventory로 인한 reset 완료 경로 관측 |
| 과거 revision의 보존 보호·tombstone 재유입 | 2 | PASS |
| v1/v2→v3 이력 이관·부분 DDL 실패 rollback | 3 | PASS |
| 공유 서비스 foreground·선기록·동시 실행·native 오류·callback DB 오류 | 5 | PASS |
| Kotlin 실제 파일 I/O | 3 | 임시 사본 누락, 열거 오류→빈 목록, 접근 실패→삭제 성공 반환 재현 |
| Swift 실제 Foundation 파일 I/O | 2 | 열거 오류→빈 목록, 접근 실패→삭제 성공 반환 재현 |

주요 정상 관측: background AppState에서 native 공유 호출/이력 생성 0회, OS handoff 전에 REQUESTED 저장, 중복 공유 요청에서 native 호출 1회·attempt 1건, native 오류는 SHARE_ERROR·자동 재시도 없음, 취소 callback의 resolution 저장 실패는 REQUESTED로 rollback된 뒤 exact 복구 SQL로 HANDOFF_UNCONFIRMED가 됐다. 이는 앱 강제 종료/재시작 실기 시험이 아니다.

이관 시험에서는 기존 이력·문구 보존, 미확인 공유의 resolution 시각 부재, 기존 완료 상태의 보존 유예 시각을 과거 공유 시각으로 소급하지 않음, foreign_key_check 위반 0건을 확인했다. 인위적 v3 DDL 충돌 후 rollback에서도 v2 데이터·schema version이 유지됐다.

## 5. 교정 finding

이 절의 파일·줄 번호는 모두 위 frozen head 기준이다. `Android/`는 `modules/local-platform/android/src/main/java/expo/modules/localplatform/`, `iOS/`는 `modules/local-platform/ios/`를 뜻한다.

### SGV-F001 — 앞선 실패 10개가 뒤의 정상 소스 항목을 계속 막음

Severity: MEDIUM / P2. 판정: FAIL. 영역: SGV-02.

- 위치: `src/services/sources.ts:9–16`, `src/storage/lifecycle.ts:48–52`; native 건너뛰기 반환 경로 `Android/LocalSources.kt`, `iOS/LocalSources.swift`.
- 요구 근거: LIFECYCLE의 회차당 신규 10장, 실패 PENDING 재시도, UI의 ‘새 사진 확인/다음 10장 가져오기’. 일부 가져오기 실패가 뒤의 정상 사진을 영구 정체시키지 않아야 한다.
- 원인: pending은 `first_observed_at, entry_key` 순서로 고정되고 매번 `slice(0, 10)`만 선택한다. 실패 항목에는 순환·마지막 시도·보류/건너뛰기 상태가 없어 순서가 변하지 않는다.

재현: 빈 최초 baseline을 만든 다음 새 identity 11개를 한 번에 스캔한다. 앞 10개는 계속 읽을 수 없고 11번째만 정상 반환하도록 native import 경계를 합성한다. exact TS/SQLite에서 새로고침을 12회 수행했다. 매번 같은 앞 10개만 native에 전달됐으며 `healthyEntrySelected=false`, `imported=0`, `pending=11`이었다. PhotoKit/SAF를 실제 호출한 결과는 아니다.

영향: 손상 사진이나 로컬에 없는 사진 등 계속 건너뛰는 항목이 선두를 채우면, 그 뒤 정상 사진은 ‘다음 10장’ 동작으로도 처리되지 않는다. 소스 재연결은 새 baseline을 만들어 의미가 달라지므로 정상 해결책으로 간주하지 않는다.

최소 교정: 실패 항목의 이력·PENDING 의미를 보존하면서 공정하게 다음 후보로 진행하도록 마지막 시도/순환/backoff 또는 명시적 보류·건너뛰기 조작을 적용한다. 정확한 구현 방식은 작성자가 결정한다. cloud-only를 강제로 다운로드하거나 실패 사진을 IMPORTED로 위조하지 않는다.

영향 범위 재검증: 실패 선두 10개+정상 11번째, 일부 성공/일부 실패, 실패 항목의 후속 정상화, baseline·재등장 identity·FIRST_OBSERVED/null 등록일 보존.

### SGV-F002 — 최근 100건 밖의 미확인 공유를 해결할 UI가 없음

Severity: MEDIUM / P2. 판정: FAIL. 영역: SGV-06; SGV-05의 보호 상태 해제 경로에 영향.

- 위치: `src/storage/history.ts:41–42`, `app/GatewayApp.tsx:183–190`, `src/services/maintenance.ts:73–77`.
- 요구 근거: 공유 중단·미확인 이력을 사용자 진술로 해결하고, 미확인 보호를 유지한 상태에서 초기화를 재개할 수 있어야 한다.
- 원인: `listHistory()`는 상태와 무관하게 최신 100건만 반환한다. 이력 화면은 그 결과만 렌더링하며 pagination·미확인 전용 목록·이전 기록 조회가 없다. reset은 DB 전체의 미확인 상태를 검사한다.

재현: 오래된 HANDOFF_UNCONFIRMED 1건과 그보다 최신 USER_MARKED_POSTED 100건을 실제 SQLite에 넣고 exact `listHistory()`를 실행했다. 전체 101건 중 화면 입력은 최신 100건뿐이며 미확인은 0건으로 보인다. 같은 미확인 상태에 대해 exact reset 서비스는 `UNCONFIRMED_SHARES_PROTECTED`로 차단했다. UI 접근 불가는 조회 결과와 렌더링 경로를 대조한 판정이며 실기 터치 시험은 아니다.

영향: 사용자는 보호 해제를 위해 필요한 기록을 볼 수 없다. 표시된 최근 100건을 다시 해결해도 정렬/행 수가 바뀌지 않아 숨은 기록은 나타나지 않는다. 해당 기록이 참조하는 사진의 정리와 전체 초기화가 계속 막힐 수 있다.

최소 교정: 모든 미확인 기록에 도달 가능한 목록/필터와 이전 기록 pagination 또는 cursor 조회를 제공한다. 단순히 LIMIT을 더 크게 바꾸거나 미확인 보호 조건을 삭제하는 방식은 해결이 아니다. 원격 게시 여부를 자동 추정하지 않는다.

영향 범위 재검증: 100/101 경계, 미확인 100건 초과, 과거 migration 이력, 동일 started_at에서 안정적인 cursor, 사용자 해결 후 reset 허용. 중복 외부 전송은 없어야 한다.

### SGV-F003 — Android 중단 import의 임시 사진이 정리·초기화 대상에서 누락

Severity: MEDIUM / P2. 판정: FAIL. 영역: SGV-05/07.

- 위치: `Android/LocalPhotos.kt:47–53`, `Android/LocalManagedFiles.kt:14–17,20–26,32–34`, `src/services/maintenance.ts:80–88`, `app/GatewayApp.tsx:214`.
- 요구 근거: 파일 쓰기 중단 복구, 관리 사본 용량 제한, 사용자 확인 후 앱 로컬 사진 초기화, 원본 보존.
- 원인: import는 inbox에 `<UUID>.tmp`로 JPEG bytes를 쓴 뒤 해시 `.jpg`로 rename하고 finally에서 임시 파일을 지운다. 프로세스 중단으로 rename/finally 이전에 남은 `.tmp`는 managed inventory와 삭제 whitelist에 모두 포함되지 않는다.

재현: 중단 직후의 도달 가능한 파일 상태를 합성해 inbox에 정상 hash.jpg 1개와 UUID.tmp 1개를 만든다. 실제 `LocalManagedFiles.kt`를 Kotlin/JVM으로 실행한 결과 inventory는 정상 .jpg 1개만 반환했다. 반환된 모든 파일을 삭제해도 임시 파일 bytes가 남았고, 임시 URI를 명시적으로 삭제 요청하면 UNMANAGED_FILE로 거부됐다. 실제 Android import 프로세스를 강제 종료한 시험은 아니며, 그 중단 상태를 만들어 파일 관리 코드를 직접 실행한 것이다.

영향: 앱이 작성한 사진 내용이 들어 있는 임시 사본이 자동 정리와 500MiB 계산에서 누락된다. 전체 초기화가 목록에 나온 파일만 삭제하고 DB를 비우면 이 잔여 사본을 놓친 채 ‘앱 로컬 데이터를 삭제했습니다’라고 표시할 수 있다. 원본 사진 삭제나 외부 전송을 확인한 것은 아니다.

최소 교정: 앱이 소유한 임시 파일의 엄격한 위치·파일명 계약, 중단 복구/오래된 임시 파일 정리, 용량 및 초기화 완료 검사를 연결한다. 진행 중 쓰기·최근 공유·미확인 사본 보호를 유지하고 임의 파일이나 원본을 재귀 삭제하는 방식으로 넓히지 않는다.

영향 범위 재검증: 쓰기 전/도중/rename 전/DB commit 전 중단 상태, 정상 import 성공·실패 후 임시 파일 수, `.tmp` 포함 용량, reset 완료 후 앱 소유 사본 잔존 여부, 루트 밖/원본 삭제 거부.

### SGV-F004 — 파일 접근·열거 실패를 빈 목록/이미 삭제됨으로 처리

Severity: MEDIUM / P2. 판정: FAIL. 영역: SGV-05/06/07. 플랫폼: Kotlin·Swift 양쪽 소스.

- 위치: `Android/LocalManagedFiles.kt:17,20–30,35–40`, `iOS/LocalManagedFiles.swift:24–25,29–44,50–58`, `src/services/maintenance.ts:17–26,80–88`.
- 요구 근거: 실제 삭제 성공 또는 이미 없음이 확인된 뒤에만 PURGED 기록, 불완전한 정리는 journal 보존 후 재개, 잘못된 초기화 완료 금지.
- 원인: Kotlin은 `listFiles().orEmpty()` 및 예외→null, Swift는 `try? ... ?? []` 및 resource 오류 누락으로 I/O 실패를 성공한 빈/부분 inventory로 만든다. 삭제에서는 `exists/fileExists == false`를 확정된 부재로 취급해 URI를 성공 목록에 넣는다.

재현 A — 목록 오류: 비특권 합성 파일 환경에서 inbox를 execute-only 권한으로 바꿔 열거만 실패시켰다. OS/JVM `listFiles()`는 null, Swift `contentsOfDirectory`는 throw였지만, exact native inventory는 양쪽 모두 오류 없이 빈 배열을 반환했다. 기존 합성 사본이 있는데도 `requireSpace(524288000)`이 허용됐다.

재현 B — 부재 오판: 기존 합성 사본의 부모 디렉터리에 접근할 수 없게 한 뒤 exact native delete를 호출했다. Kotlin과 Swift 모두 요청 URI를 삭제 성공 목록으로 반환했다. 권한 복구 뒤 원래 합성 파일은 그대로 존재했다. 이는 실제 JVM/File 및 Swift Foundation I/O 재현이며 iPhone 잠금·Android SAF 권한 철회 실기를 재현했다는 뜻은 아니다.

재현 C — 서비스 조합: exact TypeScript reset에 위 native 실패와 같은 빈 inventory를 반환했다. 파일 삭제 호출 없이 inbox DB 행이 0개가 되고 reset_pending이 사라졌다. A/B와 C는 별도 경계 시험을 연결한 판단이며 전체 모바일 runtime 통합 실행으로 표시하지 않는다.

영향: 접근 불가를 ‘없음’으로 오인해 journal/PURGED 또는 reset 완료가 잘못 확정될 수 있다. 특히 reset은 DB/journal을 정리하므로 이후 접근이 회복돼도 남은 로컬 사본을 추적할 단서를 잃는다. 보존·용량 계산도 실제보다 작아질 수 있다. 이 현상이 실제 사용자 기기에서 얼마나 자주 일어나는지는 측정하지 않았다.

최소 교정: 확정된 not-found와 접근/I/O 실패를 구분하는 파일 API·오류 분류를 사용한다. 허용 루트에 대한 inventory가 완전하지 않으면 성공한 빈 목록으로 반환하지 않는다. 용량 판정과 reset은 불확실할 때 보류하고, 실제 삭제/확정 부재만 성공으로 반환하며 journal/reset_pending을 유지한다. 안전 경로 검사를 완화하지 않는다.

영향 범위 재검증: ENOENT와 EACCES/I/O 구분, 하위 디렉터리만 열거 실패, metadata 읽기 실패, 삭제 중 일시 오류, reset 중단·접근 회복 후 재개. 오류 때 DB와 reset_pending 유지, 이후 확인된 삭제 때만 완료. 완전히 새 설치여서 디렉터리가 아직 없는 정상 조건은 구분해서 허용한다.

## 6. 추가로 확인한 계약과 남은 실기

소스 연결은 새 source identity의 초기 baseline을 만들고 완전한 scan 입력만 저장한다. 이전 identity를 유지해 삭제·재등장을 새 사진으로 반복 처리하지 않으며, 외부 소스 가져오기는 FIRST_OBSERVED/registered_at=null을 사용한다. 기존 해시의 앱 등록일은 INSERT OR IGNORE로 보존한다. F001은 이 날짜 정책이 아니라 retry scheduling의 결함이다.

날짜 묶음은 이전 revision을 덮어쓰지 않고 expected revision 충돌을 거부한다. share request는 저장된 caption/asset 순서와 snapshot을 대조한 뒤 연결한다. 열린 날짜가 과거 revision에서만 사진을 참조해도 자동 정리하지 않는 것을 새 fixture로 확인했다. 닫힌 날짜의 7일 유예와 동일 사본 재유입 시 tombstone 처리는 유효 증거로 유지한다.

알림 경로는 Android의 `snsg_due`를 전달일과 구분하고 iOS 반복 알림은 `UNNotification.date`를 DELIVERY_DATE로 기록한다. 앱은 pending token과 날짜 선택을 연결하며 알림 callback에서 SNS 공유를 호출하지 않는다. 이 소스 대조를 잠금/재부팅/cold·warm 전달 성공으로 승격하지 않았다.

사진 코드는 원본을 읽어 로컬 JPEG 사본을 만들고 공유용 캐시 하위만 허용한다. source 1개, 완전목록 2000개, 회차당 10장, iOS 앨범 선택 100개, 관리 사본 500MiB는 앱 제한이다. 수신 SNS의 공통 지원 한도 또는 OS 전체 저장공간 상한이라고 판정하지 않는다.

| 미실행 범위 | 상태 |
|---|---|
| iPhone/Galaxy 권한 허용·제한·철회, 실제 PhotoKit/SAF·iCloud-only | NOT_RUN / INDETERMINATE |
| 실제 사진 HEIC/JPEG·회전·색상·EXIF/GPS·저메모리/저장공간 | NOT_RUN / INDETERMINATE |
| 알림 cold/warm·잠금·재부팅·집중모드·자정·시간대 변경 | NOT_RUN / INDETERMINATE |
| 실제 모바일 파일 보호·백업 제외·전체 통신 검사 | NOT_RUN / INDETERMINATE |
| Instagram/Threads 단일·다중 사진·순서·문구·취소·계정·최종 게시 | NOT_RUN / INDETERMINATE |
| 서명된 기기 설치본·설치·Template·릴리스·배포 | NOT_DONE |

## 7. 작성자에게 반환하는 최소 묶음

F001 pending 처리 공정성, F002 모든 미확인 이력의 도달 가능성, F003 임시 사본 lifecycle, F004 완전한 파일 inventory/확정 삭제를 한 교정 묶음으로 반환한다. F003/F004는 같은 관리 파일·reset 경계를 공유하므로 함께 교정하되 서로 다른 재현 조건을 유지한다.

작성자는 기존 Owner 실행 승인 범위를 확인한 뒤 해당 경로만 교정하고 자체 표적 검사·완료보고·새 exact head/tree를 반환한다. IVA는 변경된 소스와 관련 회귀 조건만 affected-only로 재검증한다. 유효한 기존 CI를 무정보 반복하거나 전체 앱을 처음부터 재구현할 것을 요구하지 않는다. 기기/SNS 수락은 별도 권한·환경·증거가 필요한 gate로 남긴다.

이 결과 자체는 제품 코드 수정·병합·배포·실제 SNS 전송의 새 실행권한이 아니다. MITCHELL은 본 결과를 수신한 뒤 자기 소유 CURRENT/WORKLOG에 정제 delta를 반영한다. 검증자는 그 기억 lineage를 덮어쓰지 않았다.

## 8. 정제된 실행 관측과 재현 절차

F001/F002는 frozen 저장소의 `tests/helpers/runtime.mjs`가 제공하는 exact TS/SQLite harness에서 위 §5의 데이터 조건을 넣어 재현할 수 있다. F001은 `syncSource()`의 native `scanSource/importSource`만 대체하고 각 요청 keys를 기록한다. F002는 `listHistory()` 반환과 reset의 전체 DB guard를 각각 검사한다. UI는 반환 목록 이외의 조회 조작이 없는지 함께 대조해야 한다.

F003/F004는 제품 native 파일을 변경하지 않고 컴파일한다. Kotlin은 같은 package의 fixture main과 `android.content.Context` 디렉터리 반환 stub을 함께 컴파일한다. Swift는 Foundation fixture main을 함께 컴파일한다. 새 임시 디렉터리와 합성 bytes만 사용하며 권한 실패 조건은 비특권 사용자로 실행하고 finally/defer에서 권한을 복구한다. 사용자 실제 앱 디렉터리나 사진에 chmod/삭제를 적용하지 않는다.

| 재현 이름 | 정제 관측 |
|---|---|
| SOURCE_FAILED_PREFIX | refreshes=12, initialPending=11, healthySelected=false, imported=0, pending=11 |
| HISTORY_UNREACHABLE | total=101, visible=100, visibleUnconfirmed=0, hidden=attempt0, reset=UNCONFIRMED_SHARES_PROTECTED |
| ANDROID_IMPORT_TEMP_ORPHAN | inventoryCount=1, temporaryBytesRemain=true, directTempDeleteRejected=true |
| KOTLIN_ENUMERATION_FAILURE | listFilesNull=true, nativeInventoryEmpty=true, photoExists=true, quotaUnderCount=true |
| SWIFT_ENUMERATION_FAILURE | osEnumerationThrew=true, nativeInventoryEmpty=true, quotaUnderCount=true |
| KOTLIN_AMBIGUOUS_ABSENCE | deleteReportedSuccess=true, rejected=false, photoExistsAfterAccessRestore=true |
| SWIFT_AMBIGUOUS_ABSENCE | deleteReportedSuccess=true, rejected=false, photoExistsAfterAccessRestore=true |
| RESET_EMPTY_INVENTORY_COMPOSITION | remainingAssetRows=0, resetPending=false, fileDeleteCalls=0 |

이 표는 앞서 실행한 관측을 정리한 것이며 추가로 실행한 별도 시험 수를 뜻하지 않는다.

## 9. 외부 공식 자료의 보조 확인

제품 요구사항의 근거는 위 Git 설계·구현 계약이다. 일반 자료로 원천 요구를 대체하지 않았다. 외부 공식 API 자료는 파일 실패 경계의 보조 확인에만 사용했다.

- Android `java.io.File.listFiles()`: 빈 디렉터리의 빈 배열과 I/O 실패 등의 null 반환을 구분하는 계약. `https://developer.android.com/reference/java/io/File#listFiles()`
- Apple `FileManager.contentsOfDirectory(at:includingPropertiesForKeys:options:)`: Swift에서 실패를 throws로 반환하는 계약. `https://developer.apple.com/documentation/foundation/filemanager/contentsofdirectory(at:includingpropertiesforkeys:options:)`

## 10. 권한·외부효과와 Git 영수증

수행한 것은 read-only Git/소스 검토, 격리 fixture 실행과 본 IVA 정제 결과 문서의 기록이다. 제품 소스·제품 PR/branch/main·사용자 기기·계정·키·SNS·릴리스·배포는 변경하지 않았다. PMO dispatch, 새 Persona 생성, 전체 CI 재실행도 하지 않았다. 공개 Git에는 개인사진·토큰·서명키·대화 원문·raw CI/기기 로그를 기록하지 않는다.

본 문서의 실제 Git 기록 완료는 쓰기 API 응답과 exact commit의 파일 readback으로 판단한다. 문서 내부에 자신의 미래 commit SHA를 예상해 넣지 않는다. 반환 패킷에 실제 결과 path/commit/blob과 최종 권고를 고정한다. v1.0.1은 최초 기록의 보조 설명을 한국어로 정리한 편집 교정이며 대상·판정·재현 결과의 변경은 없다.

MITCHELL 다음 조치: 이 보고서를 수신하고 F001–F004 교정 범위와 병합 HOLD를 반영한다. 사용자 행동: IVA 반환 패킷을 MITCHELL 작성자 채널에 전달한다.
