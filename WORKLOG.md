# MITCHELL Worklog

전체 대화 원문이 아니라 의미 있는 작업 사건의 정제 기록이다.

## E017 — Windows Bootstrap 병합 복구·macOS v0.2 작성자 후보 고정 / 2026-09-20

Actor: MITCHELL. PMO NOT_DISPATCHED. macOS IVA NOT_RUN.
Recovery base: mitchell@d4abb0380a2bbef36db7e7ee225e5dda204c479e / bootstrap@78a919e9300bbb8de8fbc8843d7a8dc6b59044a1.

- 사용자가 Windows 검증 후보 병합 후 macOS 지원 설계·구현을 승인하고 중단 후 계속하도록 지시했다. GitHub connector로 최신 refs/PR·운영 정책·제품 계획을 복구했다.
- Windows PR#1은 중단 전2026-09-19에 이미 MERGED/CLOSED였으며 merge7ede3b394032b3f1da60235cb0ef2dad410df565의 tree2a28a91f8fdbbbc80bd37209206cdab9cf5e715a가 검증 후보와 같음을 확인했다. 병합을 재실행하지 않았다.
- Mac 초기78a919e…/tree92153d14…/Draft PR#2 및 성공CI35449122339·35449122295를 복구했다. .command/Bash3.2 Core/Mobile/Optional·AI 선택, 동의·lock·기존 도구 보존·receipt/사후탐지·실패 처리는 이미 구현돼 있었다.
- Homebrew 공식 Git 원문(last_review_date2026-09-17, blob d7b95f04a6cd1cbe473a0ab2462e587f68348530)을 확인해 과거14+ 설치선을15+로 교정했다.14는 Plan/Verify 진단 전용, Intel은Tier3·명시적 opt-in이며OS검사를 우회하지 않는다. 초기안과 당시14CI는 역사로 보존한다.
- 후속 제품head753bc82f63f85722cd76ba78b186bcaf46253677, tree0b5ff0331af7db7f83d23c308370a148acd4ad74를 fast-forward 저장했다. 신설지원정책8개와 기존58개Linux Bash 경계시험이 통과했다. 실제 패키지를 설치한 시험은 아니다.
- 최종macOS CI35452044776의macos-15 arm64/Intel 두job, Windows CI35452044793의PowerShell5.1/7 두job이 모두 성공했다. Mac suite의Linux전용1개skip을 실행PASS로 합산하지 않는다. nativeVerify는읽기전용이며실제Mac설치수락과 구분한다.
- Artifact10587226850 outer2b9c6815…/innerd5b8379b…의33파일·CRC/경로·테스트작업본byte equality를 확인했다. Git attributes가변환한Windows텍스트9개만expectedblob과대조해LF계산하고.bat원바이트를유지하여exacttree를재구성했다. Windows기존18개blob은동일하다.
- CURRENT generation13, DECISIONS D027/D028, 지속기억과완료보고·Manifest·최초MacIVA패킷을정제기록한다. 원검증보고서및이전작업일지의당시상태는수정하지않는다.
- CLT/Homebrew 최초준비와Xcode/SDK·license·계정/서명은사용자수동이다. 실제PC/Mac설치·실계정/키/결제·보안해제·macOS PR#2병합·릴리스/배포는하지않았다. web-starter/sns-gateway변경없음.

Results: docs/execution/BOOTSTRAP_MACOS_COMPLETION_20260920.md, BOOTSTRAP_MACOS_MANIFEST_20260920.json, BOOTSTRAP_MACOS_IVA_PACKET_v0.1.md.
Next: 고정한macOS신설경로의별도IVA. Windows기존PASS를Mac으로확대하지않고, 실제Mac수락·별도병합·릴리스는HOLD로유지한다.

## E016 — SNS Gateway PR #1 Owner 승인 병합 / 2026-09-19

Actor: MITCHELL. PMO NOT_DISPATCHED.
Recovery base: mitchell@61cf86d48c839f647c49ff52a90107778b8a5ad6 / sns-gateway PR#1 head 4ab10f481f7da66c00591ebd6cf597de527b901c.

- 사용자가 병합 여부 처분 요청에 ‘진행하세요’라고 승인했다.
- 병합 직전 PR #1이 OPEN/DRAFT, mergeable=true이고 head가 IVA affected-only PASS 대상 4ab10f48…와 일치하며 CI35439466823이 SUCCESS임을 다시 확인했다.
- Draft를 ready-for-review로 전환한 뒤 expected_head_sha=4ab10f48…를 고정하고 merge commit 방식을 사용했다.
- GitHub merge 결과 merged=true, merge commit=1c7cd117d18929cdf40abc0e4f73fec4c87b2c65. 원격 sns-gateway/main readback도 같은 SHA이며 PR #1은 MERGED/CLOSED다.
- 최초 cf6967ce 후보 FAIL과 후속 F001–F004 affected-only PASS 계보는 그대로 보존한다. 병합 자체를 최초 FAIL의 소급 변경으로 표현하지 않는다.
- RELEASE_DEPLOY_RECOMMENDATION=HOLD, SGV-04 INDETERMINATE, DEVICE_SNS_ACCEPTANCE=NOT_RUN/INDETERMINATE를 유지한다.
- 제품 코드를 병합 후 추가 수정하지 않았고 사용자 기기·개인사진·계정·키·SNS 실제 전송/게시·서명 설치·release/deploy는 수행하지 않았다.

Result: AofSpds/sns-gateway main@1c7cd117d18929cdf40abc0e4f73fec4c87b2c65 / PR#1 MERGED.
Next: 실제 iPhone/Galaxy 수락과 승인된 서명/설치 준비. release/deploy는 계속 HOLD.

## E015 — SNS Gateway IVA-002 affected-only PASS 수신 / 2026-09-19

Actor: MITCHELL. PMO NOT_DISPATCHED. 제품 merge/release/deploy 미수행.
Recovery base: mitchell@13d5befecca9c1dfb27c67b2572c729b601f62bc / sns-gateway@4ab10f481f7da66c00591ebd6cf597de527b901c.

- IVA 반환 패킷에 따라 결과 원문 docs/execution/SNS_GATEWAY_IVA002_AFFECTED_REREVIEW_RESULT_20260919.md를 exact commit/blob으로 직접 읽고 원격 main을 확인했다.
- SGV-F001/F002/F003/F004는 모두 PASS, affected scope 신규 finding NONE, IVA_AFFECTED_REREVIEW=PASS다.
- MERGE_RECOMMENDATION=PASS를 수신했으나 실제 병합 권한/실행과 구분한다. RELEASE_DEPLOY_RECOMMENDATION=HOLD, DEVICE_SNS_ACCEPTANCE=NOT_RUN/INDETERMINATE를 유지한다.
- 최초 cf6967ce 후보의 FAIL과 최초 결과 문서는 역사적 판정으로 보존하고 4ab10f48 교정 후보에 한해서 후속 PASS를 기록한다.
- IVA는 작성자 CI35439466823을 재사용 증거로 구분하고 exact 제품 코드/SQLite·Kotlin/JVM·Swift Foundation의 신규 합성 시나리오46개를 수행해46 PASS/0 FAIL을 기록했다. 실제 기기 시험으로 확대하지 않는다.
- 동일 후보의 F001–F004 재검증은 반복하지 않는다. 후보가 바뀌면 affected-only로 변경 영향만 확인한다.
- MITCHELL 소유 CURRENT/DECISIONS/WORKLOG/지속 기억을 정제 delta로 현행화하고 PR 설명을 재검증 PASS 상태로 갱신한다. 제품 코드·제품 main·기기·SNS·계정·키·서명·릴리스·배포는 변경하지 않는다.

Result: docs/execution/SNS_GATEWAY_IVA002_AFFECTED_REREVIEW_RESULT_20260919.md @ 13d5befecca9c1dfb27c67b2572c729b601f62bc / blob b61e82a87a534193ab50c420152df145319e7558.
Next: 별도 Owner merge disposition. 실기/SNS·서명·release/deploy는 별도 gate.

## E014 — SNS Gateway IVA-002 수신 및 F001–F004 affected-only 교정 / 2026-09-19

Actor: MITCHELL. PMO NOT_DISPATCHED. 새 교정 후보의 IVA 재검증 NOT_RUN.
Recovery base: mitchell@5133d5fe9af59b8ed06e7bf7bedba29108c10c79 / sns-gateway@cf6967ce920793a72f88da746af4d0a317e61ed8.

- 사용자 전달 패킷에 따라 IVA 결과 v1.0.1을 exact commit/blob으로 직접 읽었다. 최초 candidate FAIL, F001–F004 P2, MERGE/RELEASE HOLD와 SGV별 판정을 보존한다.
- 기존 Owner 실행 승인(D021) 안의 국소 교정으로 처리했다. 수신 CURRENT Generation9는 edd19651259193be0320197bda26cc8d448153ce에 먼저 기록했다. 결과 패킷을 새 실행권한이나 전체 재구현 지시로 취급하지 않았다.
- 영속 순환 재시도, 미확인/전체 이력 keyset cursor와 UI, Android UUID.tmp 관리, Kotlin/Swift I/O 오류·확정 부재 분리 및 reset 최종 재확인을 구현했다. 원본·미확인 공유 보호를 완화하지 않았다.
- 제품 교정은 19파일 +642/-100, exact head9847200c2448e98ebe0f9812cee813285dbbe865, tree a7d76e5b74713239bb8e11e6ea1a9fe613d1e92b다. 기존 잠금 의존성·알림·공유 모드는 유지했다.
- TypeScript/SQLite 표적42개, 확장된 전체 Node110개, Kotlin/JVM filesystem+Android Os shim23개, Swift Foundation 신규16개와 기존6개를 작성자로 점검했다. Kotlin은 실제 기기 Os 실행이 아니며 권한 실패는 비특권 격리 환경에서 시험했다.
- Android minSdk24와 충돌하는 NIO API26 의존을 도입하지 않고 API21+ Os 경로로 교정했다. 이는 기능 확장이 아니라 교정 코드의 기존 지원범위 유지다.
- 새 CI35438877502의 실제 최종 결과와 artifact는 교정 완료보고/Manifest가 소유한다. 이전 후보의 유효 CI35433487023은 재실행하지 않았다.
- 새 source artifact10583013453의 외부/내부 SHA, 파일75개와 Git tree를 확인하고 전체 파일 바이트가 로컬 검사 소스와 일치함을 대조했다. 받은 소스에서 표적42개도 통과했다.
- 최초 IVA 결과 문서는 수정하지 않았다. 작성자 완료와 새 exact 후보를 고정하고 F001–F004 affected-only 재검증 패킷으로 반환한다. 실제 기기/SNS·독립 재검증·서명·병합·배포는 수행하지 않았다. bootstrap/web-starter 변경 없음.

Results: docs/execution/SNS_GATEWAY_IVA002_CORRECTION_COMPLETION_20260919.md, SNS_GATEWAY_IVA002_CORRECTED_MANIFEST_20260919.json, SNS_GATEWAY_IVA_AFFECTED_REREVIEW_PACKET_v0.3.md.
Next: 새 후보의 별도 IVA affected-only 재검증. 작성자 검사 통과는 최초 FAIL의 소급 변경이나 독립 PASS가 아니다.

## E013 — SNS Gateway 남은 네 코드 영역 구현·작성자 검사 / 2026-09-19

Actor: MITCHELL
Recovery base: mitchell@5b11e3e39191df8a65fd8008665b93370a9ad75b / sns-gateway@f5dfbe964789a757bdbdc6fa1c78bf7c861f64b3

- 사용자가 남은 기능을 충분히 생각해 구현하도록 지시하고 중단 후 계속하도록 요청했다. 기존 Git/계획/AGENTS와 정확한 소스 artifact를 복구하고 첫 후보를 다시 만들지 않았다.
- Galaxy SAF 폴더/iPhone PhotoKit 앨범의 지속 연결·baseline·완전 스캔·pending, 날짜별 immutable revision, 공유 snapshot, 오래된 알림 날짜 확인, 보존·삭제·용량·초기화 journal을 추가했다.
- 외부 소스의 실제 추가시각을 추정하지 않고 FIRST_OBSERVED 날짜 확인으로 남겼다. iOS 반복 알림의 전달일과 원 예정일을 구분했다. 앱 재개/새로고침 연동이며 상시감시/무인게시를 주장하지 않는다.
- 공유상태와 완료시각을 같은 트랜잭션으로 저장하고 삭제 전 경로 전체 검증·미확인 보호·실패 journal 재개를 확인했다. 기존 DB/등록일/문구/이력을 보존했다.
- 제품 commit cf6967ce920793a72f88da746af4d0a317e61ed8, tree c13cb960d960f57979df4a18a7a3236225be52e6. 이전 대비36파일 변경, 전체68파일이다.
- CI35433487023의 checks/android/ios-simulator 모두 SUCCESS. Node94개와 실제 Swift Foundation 삭제보호6개가 통과했다. 실기/SNS 결과가 아니다.
- 새 source artifact10581721459의 외부/내부 SHA와68파일 Git tree를 재계산했고 최종 산출물 소스에서도94개 시험이 통과했다. Android unsigned/iOS simulator 산출물도 checksum/ZIP을 확인했다.
- 상세 보고·Manifest·IVA v0.2 입력과 CURRENT/DECISIONS/기억을 현행화했다. 이전 보고서는 당시 상태로 보존한다.
- API 키·서버·PMO·독립 IVA·기기 설치·개인사진·SNS 전송·공개 게시·제품main·배포는 실행하지 않았다. bootstrap/web-starter는 변경하지 않았다.

Results: docs/execution/SNS_GATEWAY_LIFECYCLE_COMPLETION_20260919.md, SNS_GATEWAY_LIFECYCLE_MANIFEST_20260919.json, SNS_GATEWAY_IVA_PACKET_v0.2.md.
Next: 고정 후보의 별도 IVA 검증, 이후 승인된 실기 수락/서명/병합·배포. 네 코드 영역을 다시 미구현으로 보고하지 않으며 실기·독립검증에서 발생할 교정은 별도다.

## E012 — SNS Gateway 재개와 로컬 게시함·알림 구현 / 2026-09-19

Actor: MITCHELL
Recovery base: mitchell@3e159bf790ab3cdd775add73b8aa7cde89bf5577 / sns-gateway@54cbdcbd77dd2fbcd46b28597e8e5863c7883f0b

- 사용자 계속 진행 지시에 따라 기존 제품 head, Draft PR #1과 성공 CI35422061756을 직접 복구했다. 성공한 합성 공유 후보를 다시 생성하지 않았다.
- 서버/API 키 없는 계약을 유지하면서 실제 로컬 사진 가져오기, JPEG 사본·해시·중복 등록 방지, SQLite v2 이관, 사진 순서/문구/이력과 iOS/Android 알림 코드를 추가했다.
- 로컬 단위/SQLite/정적 계약68개를 확인했다. 정확한 source head/tree, native CI 결과와 artifact는 아래 완료보고가 소유한다.
- 지정 폴더/앨범 지속 연동과 보존 관리·실기/SNS 시험은 미완료다. 전체 v0.1 완성으로 표시하지 않는다.
- API 키 질문은 SNS OAuth/서버 도입 승인으로 해석하지 않았다. PMO·IVA·기기·실게시·병합·배포는 실행하지 않았다. B/W는 변경하지 않았다.

Results: docs/execution/SNS_GATEWAY_EXECUTION_20260919.md, SNS_GATEWAY_COMPLETION_20260919.md, SNS_GATEWAY_IVA_PACKET_v0.1.md.

## E011 — SNS Gateway 최초 구현 후보 / 2026-09-19 (재개 시 remote 증거로 복구)

- 사용자가 생성한 AofSpds/sns-gateway에서 초기 main13d83595…와 work/sns-gateway-v0.1, Draft PR #1이 만들어졌다.
- SG-00/01 합성 JPEG 공유, Kotlin/Swift bridge, SQLite 공유 이력, 날짜 선정 기반과 CI를 구현했다.
- 최초 CI35421776429는 Expo/TypeScript 조합 검사에서 실패했다. SDK 요구에 맞게 TypeScript6.0.3으로 교정한21a9105…에서 CI35422061756의 checks/Android/iOS simulator가 성공했다.
- 문서 전용54cbdcbd…에서 API 키 불필요·기기 수락 범위를 정리했다. native build는 실제 기기 또는 SNS 호환성 PASS가 아니었다.

## E010 — 외부 서버·스토리지 없는 모바일 수동 공유 상세 설계 / 2026-09-19

Actor: MITCHELL
Recovery base: `a2ab0d75c5b72cdca7c39db1dea0c44422b7b1ec`

- 사용자가 외부 서버·외부 스토리지를 배제하고 최종 게시는 직접 누르는 방식을 선택해 상세 설계를 요청했다.
- 기존 운영 문서·기억·계획과 전달된 모바일 서버형 설계를 읽고, Apple/Android/Expo 공식 자료의 로컬 알림·공유·파일·백업 경계를 확인했다.
- `docs/mobile/LOCAL_SHARING_DESIGN_v1.0_20260919.md` 작성: 입력·날짜 의미, 9시 알림, local DB, 사진 사본, OS 공유, 문구 fallback, 공유 시도/사용자 확인 상태, 9단계 WBS와22개 시험 조건.
- 잠금 상태 강제 화면 실행, 문자 그대로 한 번 터치, 공유 callback만으로 게시 성공, 특정 앨범 추가시각 자동 확정은 보장하지 않는다.
- 기존 B/W의 PR exact head와 Draft·미병합 상태를 재조회했다. 제품 코드·PR·CI를 변경하거나 검증을 반복하지 않았다.
- 문서의 소유권에 따라 CURRENT/DECISIONS/Persona 기억과 README 진입점을 현행화했다. 과거 계획·IVA 보고서는 수정하지 않았다.
- 모바일 구현·실기/SNS 공유·새 저장소·계정/키·설치·공개 게시·배포·PMO/IVA runtime은 실행하지 않았다.

Next: L00 실제 공유 호환성 spike. 기록 자체는 기기 시험 PASS가 아니다. 저장 완료는 이 문서를 포함하는 commit/readback으로 확인한다.

## E009 — IVA 최종 반환 패킷 규칙 반영 / 2026-09-15

- 사용자가 `MITCHELL-IVA-RETURN-PACKET-RULE-ACK` 패킷을 전달했다.
- IVA의 최종 반환은 항상 MITCHELL에 그대로 전달 가능한 인계 패킷 형식으로 하도록 운영 계약에 반영했다.
- 상세 검증 결과가 길면 Git에 원문을 기록하고, 채팅에는 Git 경로와 핵심 판정·exact 대상·finding별 결과·merge 권고·미실행·권한 경계·다음 조치를 담은 패킷만 반환한다.
- 원문 패킷은 `docs/execution/IVA_RETURN_PACKET_RULE_20260915.md`에 보존했다.
- 제품 코드, PR, CI, 사용자 PC, Cloud, SNS에는 변경하지 않았다.

## E008 — affected-only IVA 재검증 결과 기록 / 2026-09-15

- IVA가 `docs/execution/IVA_AFFECTED_ONLY_REREVIEW_RESULT_20260915.md`를 Git에 기록했다.
- exact corrected candidates의 `IVA-B001`, `IVA-B002`, `IVA-W001`, `IVA-W002`가 모두 PASS였고 새 finding은 없었다.
- `MERGE_RECOMMENDATION = PASS`이나 실제 Windows·Supabase 통합은 `NOT_RUN / INDETERMINATE`, release/deploy는 HOLD다.
- 결과 commit은 `8919c23eef0d8b845e5c8cd66e89f7be85209154`, 결과 blob은 `be514d035fde03370db0c6ad90bfa916c0ffbe79`다.
- 제품 PR은 계속 Draft·미병합이며 제품 main, 사용자 PC, Cloud, SNS에는 변경이 없다.

## E007 — Exact head 안정화와 UTF-8 복구 / 2026-09-15

- PR 설명 현행화 중 `NONEXISTENT` placeholder가 commit `fc52d761e7fcd97ffab29f42cce6f1e7c7fbfd41`에 생성됐고, 확인 즉시 `f880d0297e4d092c0f7d5025ff42cc7648fb66aa`에서 삭제했다.
- 실질 교정 head `33b1b7e6…` 대비 현재 head의 파일 diff는0이며 tree는 동일한 `2a28a91f8fdbbbc80bd37209206cdab9cf5e715a`다. force-reset하지 않았다.
- 최신 Bootstrap CI run34972848451의 PowerShell5.1/7 두 job이 성공했다. artifact10397977572의 outer SHA-256은63218f527893dc5a311151f9fe354dd2839b5ca9525a19798dd4b1e4bcadf74f, inner ZIP SHA-256은7f9dcce619f0e403cb2b23dec7783a3f9c5a96a1b8cf9535a6e97d91f928735b다.
- 이전 WORKLOG blob의 비UTF-8 바이트를 확인해 UTF-8 문서로 재구성했다.
- 제품 main, 사용자 PC, Cloud, SNS에는 변경 없음. PR은 Draft·미병합으로 유지한다.

## E006 — 최초 IVA 결과와 affected-only 교정 / 2026-09-15

- IVA 결과 commit: d16166f8b390fb63f6332b77960171e803f5bd59.
- 판정: Bootstrap FAIL, Web Starter FAIL, 실환경 INDETERMINATE, merge HOLD.
- Finding: IVA-B001, IVA-B002, IVA-W001, IVA-W002.
- Bootstrap 교정 tree2a28a91f…와 Web Starter 교정 head/tree a2c63cca…/34716ab2…에서 작성자 CI 성공.
- 교정 완료보고, corrected manifest, affected-only IVA 재검증 패킷 작성. PMO dispatch·제품 병합·배포·실환경 변경 없음.

## E005 — 중단 복구와 후보 완료 정리 / 2026-09-15

- 제품 브랜치, Draft PR2개와 성공 CI를 remote 증거로 복구했다.
- 최초 후보: Bootstrap b4cabc7…, Web Starter15efb21….
- 완료보고, 후보 manifest, 최초 IVA 입력 패킷을 작성했다.

## E004 — 최초 구현 후보와 작성자 CI / 2026-09-15

- Bootstrap/Web Starter 구현 후보와 Draft PR을 생성했다.
- 작성자 CI 성공. 실제 Windows 설치·Supabase 실계정·SNS·독립검증·배포 완료는 아님.

## E003 — 현재 채널 실행권한 기록 / 2026-09-15

- 사용자가 MITCHELL 채널에서 상세 계획에 따른 GitHub 구현을 지시했다.
- 구현·작성자 점검·브랜치·커밋·PR 권한을 적용하고 계정·실게시·독립검증 경계는 유지했다.

## E002 — 정책 및 구현 계획 문서화 / 2026-09-15

- HLOM 운영·현재성·기억 정책 중 필요한 부분을 읽고 MITCHELL/PMO/IVA 계약에 적용했다.
- Bootstrap → Web Starter → 첫 사진 앱의 구현 계획과 운영 문서를 작성했다.

## E001 — 저장소 진입점 생성 / 2026-09-15

- AofSpds/mitchell과 AofSpds/bootstrap을 확인하고 MITCHELL README를 초기 등록했다.
- Bootstrap 제품 코드는 이 사건에서 변경하지 않았다.