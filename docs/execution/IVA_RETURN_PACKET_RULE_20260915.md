# IVA 운영 규칙 반영 패킷

**PACKET_ID:** `MITCHELL-IVA-RETURN-PACKET-RULE-ACK`
**VERSION:** `1.0`
**DATE:** `2026-09-15`
**FROM:** `IVA`
**TO:** `OWNER / MITCHELL`
**PROJECT:** `MITCHELL`
**STATUS:** `ACKNOWLEDGED / EFFECTIVE_IMMEDIATELY`

## 적용 규칙

앞으로 IVA 작업의 최종 반환은 항상 **복사해서 MITCHELL에 바로 전달할 수 있는 인계 패킷 형식**으로 제공한다.

검증 결과가 긴 경우에는 다음 원칙을 적용한다.

```text
상세 검증 결과 = Git에 기록
채팅 반환       = Git 경로와 핵심 판정을 담은 MITCHELL 전달 패킷
```

반환 패킷에는 최소한 다음 정보를 포함한다.

| 항목 | 내용 |
|---|---|
| 식별 | `PACKET_ID`, 버전, 날짜, FROM, TO, PROJECT |
| 상태 | 검증 완료·교정 필요·PASS·HOLD 등의 정확한 상태 |
| 대상 | Repository, branch/PR, exact head/tree |
| 결과 | finding별 `PASS / FAIL / INDETERMINATE / NOT_RUN` |
| Git 기록 | 결과 문서 경로, commit, blob |
| 권고 | `MERGE_RECOMMENDATION`, release/deploy 구분 |
| 미실행 | Windows·Supabase 등 실제로 수행하지 않은 범위 |
| 권한 경계 | 수정·병합·배포·계정 변경 수행 여부 |
| 다음 | MITCHELL이 취할 정확한 다음 조치 |

최초 독립검증 입력도 검증 결과를 MITCHELL에 구조화해 반환하도록 요구하고 있으므로, 이 규칙은 기존 검증 계약과 일치한다.

```text
RETURN_PACKET_REQUIRED = ALWAYS
DETAILED_RESULT_TO_GIT = YES
CHAT_RETURN_PACKET_ONLY = DEFAULT
RULE_EFFECTIVE = IMMEDIATE
```

**사용자 추가 행동: 없음.**
