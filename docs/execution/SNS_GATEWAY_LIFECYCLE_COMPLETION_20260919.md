# SNS Gateway — 남은 네 코드 영역 구현·작성자 검사 보고

문서 ID: SNSG-LIFECYCLE-COMPLETION-001  
날짜: 2026-09-19 / 작성자: MITCHELL  
상태: AUTHOR_IMPLEMENTATION_CANDIDATE / DEVICE_ACCEPTANCE_NOT_RUN / IVA_NOT_RUN

## 1. 결론과 범위

기존 후보에서 미구현으로 남았던 지정 폴더·앨범 연결, 날짜별 묶음·revision, 오래된 알림의 날짜 확인, 로컬 보존·삭제·용량 관리를 코드에 반영했다. 이 네 영역의 작성자 구현은 완료했으며 실제 기기 수락과 제품 전체 검증·배포 완료를 뜻하지 않는다.

사용자의 남은 구현 진행 및 중단 후 계속 지시를 근거로 현재 MITCHELL 채널에서 직접 수행했다. 외부 서버/스토리지/SNS HTTP API/OAuth/API 키는 추가하지 않았다. 최종 SNS 게시는 공식 앱에서 사용자가 수행한다. PMO는 NOT_DISPATCHED이며 SNS Gateway IVA는 NOT_RUN이다. 기존 bootstrap/web-starter를 변경하지 않았다.

## 2. 정확한 제품 후보

| 필드 | 값 |
|---|---|
| Repository | AofSpds/sns-gateway |
| Branch / PR | work/sns-gateway-v0.1 / #1, OPEN·DRAFT·UNMERGED |
| Candidate head | cf6967ce920793a72f88da746af4d0a317e61ed8 |
| Candidate tree | c13cb960d960f57979df4a18a7a3236225be52e6 |
| Resume / parent | f5dfbe964789a757bdbdc6fa1c78bf7c861f64b3 |
| Main base | 13d83595e31867c24fb8154074a7ad76d5995f95 |
| Diff against parent | 36 files / +1263 / -203 |
| Candidate source inventory | 68 files |
| Commit message | feat: add persistent photo sources daily revisions and safe local retention |
| Author CI | 35433487023 |

기존 실행 설계는 mitchell@3e159bf790ab3cdd775add73b8aa7cde89bf5577의 docs/mobile/LOCAL_SHARING_DESIGN_v1.0_20260919.md, 제품의 실행 요약은 docs/WORK_PLAN_v0.1.md다. 이전 f5dfbe9 보고서는 당시 범위의 역사적 기록으로 보존한다. 본 보고서는 후속 구현 상태이며 과거 미실행을 소급 PASS로 바꾸지 않는다.

## 3. 구현한 네 영역

| 영역 | 실제 구현 | 제한·수락 범위 |
|---|---|---|
| 지정 소스 | Android 지속 읽기 SAF 폴더, iOS PhotoKit 앨범 선택·저장·해제, 최초 baseline, 신규·편집 identity, 실패 재시도 | 연결1개, 완전한 목록 최대2,000항목, 회차당 신규10장. 앱 재개/새로고침 때 동작, 상시 백그라운드 감시 아님 |
| 날짜별 묶음 | SQLite v3, service_date별 사진·순서·문구 revision, 이전 revision 불변, expected revision 충돌 거부, 공유 당시 snapshot 연결 | 원격 게시 성공과 무관. 실제 모바일 DB/앱 재개는 기기 시험 필요 |
| 오래된 알림 | Android 원 예약일 전달, iOS 반복 알림의 실제 전달일 보존, cold/warm tap 경로, 날짜 확인 후 명시적 묶음 선택 | iOS 전달일을 원 예정일로 단정하지 않음. 사용자 확인 전 오늘 사진으로 몰래 치환하지 않음 |
| 보존·삭제·용량 | 닫힌 묶음/해결된 공유의7일 유예, 미확인 보호,500MiB 관리 사본 제한, 선택삭제, 전체 초기화, journal 재개, tombstone | 앱 사본만 삭제, 원본/원격 SNS 글 보존. 실제 저장공간 부족·기기별 파일 보호 수락은 미실행 |

### 소스와 날짜

Galaxy는 com.android.externalstorage.documents가 제공하는 선택 폴더를 직접 항목만 읽는다. 하위 폴더·숨김/임시 파일·클라우드 공급자는 기본에서 제외한다. iPhone 앨범 연결은 전체 사진 읽기 허용이 필요하며 제한 권한에서는 직접 사진 가져오기 경로를 사용한다. 앨범 선택 목록은100개 이내다. iCloud-only 원본을 가져오기 위해 네트워크를 켜지 않는다.

처음 연결한 기존 목록은 기준 목록으로만 기록한다. 같은 소스 항목이 사라졌다 재등장해도 새 사진으로 반복 가져오지 않는다. 불완전한 스캔은 성공 snapshot으로 저장하지 않는다. 실패한 가져오기는 PENDING으로 남는다.

소스에서 발견한 시각을 실제 폴더/앨범 추가시각으로 바꾸지 않는다. 새 소스 사진은 FIRST_OBSERVED/등록시각null로 남기고 사용자가 날짜 묶음에 수동 포함한다. 이미 앱에 등록된 동일 해시 사본의 등록일은 보존한다. 앱 직접 가져오기의 등록일 기준00:00~09:00 자동 선정은 유지한다.

### 버전·삭제의 안전성

공유 당시 사진순서·문구 revision을 연결하며 이후 편집으로 과거 내용을 덮어쓰지 않는다. 기존 전역 문구는 새 날짜의 기본 문구로 유지한다. 공유 상태와 완료시각은 한 트랜잭션으로 저장한다. OS 미확인 응답은 해결된 공유로 기록하지 않는다.

자동 정리는 참조하는 모든 날짜가 사용자의 완료/폐기로 닫힌 지7일 이상, 관련 공유가 모두 해결된 지7일 이상인 경우만 한다. 미공유 orphan inbox, 열린 날짜, 미확인 공유는 보호한다. 오래된 미참조 staging은7일 유예 후 정리할 수 있다.500MiB는 관리 JPEG 사본과 staging 합계이며 DB/OS 전체 저장공간 상한은 아니다.

선택삭제·초기화는 사용자 확인을 거친다. 파일 전체를 native canonical 경로·파일명 허용목록으로 검증한 뒤 삭제한다. 삭제 의도 기록→파일 삭제/부재 확인→PURGED 순서로 처리하고 중단을 재개한다. 동일 해시 재유입에도 삭제 기록을 적용한다. reset_pending이 있으면 다른 작업보다 초기화를 재개하고 원래 누른 작업을 자동 재실행하지 않는다.

## 4. 작성자 검사와 증거

| 검사 | 근거 | 결과 |
|---|---|---|
| Node/SQLite/정적 계약94개 | 로컬, CI checks, 최종 source artifact 재실행 | PASS |
| lint/typecheck/Expo SDK 호환성/JS bundle | CI35433487023 / checks105872116241 | SUCCESS |
| Swift 실제 파일삭제 허용경로6개 | Linux Foundation + macOS CI fixture | PASS |
| Android unsigned Release/JS 포함/INTERNET 권한 제거 | android105872208793 | SUCCESS |
| iOS unsigned simulator Release | ios-simulator105872208796 | SUCCESS |
| 실제 기기/SNS·독립 IVA | 미실행 | NOT_RUN |

Node 하네스는 실제 TypeScript 저장·서비스 코드를 실행하고 SQLite는 실제 인메모리 DB로 시험한다. native 파일/OS 경계만 가짜 입력으로 바꾼다.94개에는 기존68개와 새26개가 포함된다. 주요 새 시험은 baseline/재등장/실패재시도, 날짜분리/revision CAS/불변성, unknown 날짜, snapshot 불일치 rollback, 삭제 journal/초기화 중단, 기존 문구 보존, 사용자 완료 저장 실패의 원자성이다.

Swift6개는 실제 LocalManagedFiles.swift를 실행해 루트 밖 파일·외부 symlink·중복 요청 거부, 안전한 파일 삭제·반복 삭제·용량 초과 거부를 검사했다. Linux 로컬과 macOS CI의 Foundation fixture이며 PhotoKit/UIKit/iPhone 실기 시험은 아니다.

현재 commit은 새 코드 변경 때문에 CI를 수행했다. 이전 성공한 f5dfbe9/21a9105 후보를 무정보 반복 검증한 것이 아니다. 기존 잠금 의존성을 유지했고 테스트 실행에 VM module flag만 추가했다. CI에서 Node 실험 기능과 기존 개발도구 deprecation 경고가 있었으나 실패한 검사를 끄거나 성공으로 바꾸지 않았다.

## 5. 산출물 동일성

- Source artifact: 10581721459 / SNS_GATEWAY_LIFECYCLE_SOURCE_20260919.zip.
- Outer SHA-256: b77adc0c88ca0ffda5ebe60926f6fb903f3df91273c349e309d1272a51d3626e.
- Inner source ZIP SHA-256: 95d731750872ee45f35dfe3921bec493edc2ab2841b6d3389bd1c4ecac73d363.
- 경로 안전성/ZIP CRC 검사 통과. source 파일68개로 계산한 Git tree가 c13cb960d960f57979df4a18a7a3236225be52e6과 일치했다.
- CI archive의 COMMIT/TREE와 실제 후보를 대조했다. 내려받은 소스 자체에서94개 시험을 실행해 모두 통과했다. 이것도 작성자 검사다.
- Android/iOS artifact의 ID·SHA-256·파일 목록은 SNS_GATEWAY_LIFECYCLE_MANIFEST_20260919.json에 기록한다.

Android unsigned APK는 서명된 기기 설치본이 아니며 iOS simulator .app은 iPhone용 IPA가 아니다. 빌드 성공을 친구 휴대폰에서의 작동 성공으로 해석하지 않는다.

## 6. 완료 구분과 남은 수락

이번에 명시된 네 코드 영역은 모두 구현 후보에 포함됐다. 더 이상 이 네 영역을 '미구현'이라고 계속 이월하지 않는다. 다만 실제 기기/독립검증에서 발견할 결함이 없다고 보증하지 않는다.

남은 단계는 실제 iPhone/Galaxy에서 사진·권한·소스·알림·보존·공유 수락, 별도 IVA 결과, 사용자 승인 범위의 기기 서명/설치·병합·배포다. 알림 권한·사진 전체/제한 권한·소스 철회, iCloud-only, metadata/방향, cold/warm 알림, 자정/시간대변경, 저장공간부족, SNS별1장/다중/문구/취소는 NOT_RUN이다.

작성자 완료→exact 후보 고정→별도 IVA 순서로 진행한다. 새 IVA 입력은 docs/execution/SNS_GATEWAY_IVA_PACKET_v0.2.md이며 이전 v0.1 요청 대상을 이 후보로 갱신한다. 이는 기존 B/W 재검증 PASS를 재사용하는 것이 아니다. 제품코드 작성자 MITCHELL이 IVA로 이름만 바꾸어 검증하지 않는다.

사용자 기기·개인사진·계정·SNS 실제 전송·공개 게시·제품main·릴리스·배포는 변경하지 않았다. API 키 등록·새 저장소 생성은 필요 없다. 운영 Git 저장은 별도 commit/readback으로 확인한다.
