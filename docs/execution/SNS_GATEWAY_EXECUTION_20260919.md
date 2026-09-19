# SNS Gateway 실행권한 및 재개 기록

ID: SNSG-EXEC-20260919 / Writer: MITCHELL / Date: 2026-09-19
Source: 사용자의 새 저장소 지정, 작업계획 초안 요청 뒤 작업 진행 승인, API 키 질문, 중단 후 계속 진행 지시의 정제 요약. 대화 원문이 아니다.

## 현재 승인 범위

AofSpds/sns-gateway에서 SNSG-WORK-001 v0.1의 로컬 공유 경로를 현재 MITCHELL 채널이 직접 구현한다. 구현·일반 의존성 선택·자체 검사·작업 브랜치·커밋·Draft PR과 운영 기록이 범위다. PMO로 이관하지 않는다. 외부 서버/Storage, OAuth, SNS HTTP API, 앱 자체 로그인은 추가하지 않는다. API 키 등록 질문만으로 이 범위를 바꾸지 않는다.

기존 D019의 무서버/최종 수동 게시 원칙을 유지한다. D020의 세부 설계 기본값은 후속 작업계획에 따라 구현 기준으로 사용하되 모든 기기 기능이 검증된 것으로 승격하지 않는다. 모바일 설계-only 시점의 현재 상태는 이 후속 실행 기록이 코드·Git 작업 범위에서 대체한다.

## 정확한 시작 상태

- 운영 기준: mitchell@3e159bf790ab3cdd775add73b8aa7cde89bf5577
- 제품 main: sns-gateway@13d83595e31867c24fb8154074a7ad76d5995f95
- 복구 작업 head: 54cbdcbd77dd2fbcd46b28597e8e5863c7883f0b
- 첫 코드/SDK 교정: 9053185… → 21a9105…
- 기존 성공 CI: 35422061756, 합성 공유·checks·Android unsigned·iOS simulator.

재개 시 원격 refs/PR/CI/artifact를 직접 조회했고, 기존 제품을 새로 만들지 않았다. 54cbdcbd…와 성공한21a9105…의 차이는 문서뿐이었다. 실패 응답의 플랫폼 내부 원인은 미확인이다.

## 경계

실제 사용자 기기 설치, 개인사진 접근/외부 공유, SNS 공개 게시, Apple 계정/서명키, 스토어·릴리스·제품 main 병합·외부 계정/결제는 묵시적 승인에 포함하지 않는다. CI의 unsigned build는 개인 서명키를 사용하지 않는다. 기존 B/W 제품은 변경하지 않는다.

계정·기기를 기다리지 않아도 가능한 순수 로직/DB/화면/native 코드·컴파일은 이어간다. 기기/SNS 동작과 독립 IVA는 증거가 없으면 NOT_RUN이다. 이 실행권한 기록 자체는 검증 PASS가 아니다. 구체 결과는 SNS_GATEWAY_COMPLETION_20260919.md에서 고정한다.
