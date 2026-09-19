# MITCHELL — 서버 없는 모바일 사진 공유 도우미 상세 설계

- 문서 ID: `MITCHELL-MOBILE-LOCAL-001`
- 버전: `1.0`
- 작성일: `2026-09-19` / 시간대: `Asia/Seoul`
- 작성자: `MITCHELL`
- 상태: `DETAILED_DESIGN / IMPLEMENTATION_NOT_STARTED / DEVICE_COMPATIBILITY_NOT_RUN`
- 기준 운영 Git: `AofSpds/mitchell@a2ab0d75c5b72cdca7c39db1dea0c44422b7b1ec`
- 현재 범위: 상세 설계와 정제된 결정·기억의 문서 보존. 제품 구현·실기 설치·SNS 공개 게시·배포는 수행하지 않는다.
- PMO: `NOT_DISPATCHED` / 본 모바일 설계의 별도 IVA 검증: `NOT_RUN`

## 1. 제품 정의와 사용자에게 약속할 범위

**휴대폰에 있는 사진을 골라 문구와 함께 준비하고, 매일 오전 9시 로컬 알림으로 알려주며, 사용자가 공식 SNS 앱에서 최종 게시하는 로컬 전용 도우미**를 만든다.

```text
휴대폰의 지정 사진함·앨범·폴더
    → 로컬 사진 수집·날짜 판단·중복 확인
    → 로컬 공유용 사본과 문구 준비
    → 09:00 기기 알림
    → 사용자가 알림 선택·잠금 해제
    → 준비된 사진 확인·공유
    → 운영체제 공유 메뉴에서 SNS 선택
    → SNS 앱에서 필요 시 다음·문구 붙여넣기·최종 게시
```

외부 예약 서버, 외부 사진 저장소, Supabase, Firebase, S3/R2, 서버 DB, 사진 업로드 API, SNS OAuth 토큰을 사용하지 않는다. 앱 자체 로그인도 없다. SNS 로그인과 계정 선택은 공식 SNS 앱에서 사용자가 한다.

사용자가 수락한 것은 최종 게시의 수동 처리다. **터치가 문자 그대로 딱 1회라고 확정하지 않는다.** 잠금 해제, 공유 대상·게시 유형 선택, 다음, 문구 붙여넣기는 OS와 수신 SNS 앱에 따라 추가될 수 있다. 실기 호환성 시험으로 실제 최소 단계를 기록한다. [E01][E02][E03]

09:00은 알림 예약 시각이다. 사람이 누른 후 게시하므로 09:00:00 실제 공개를 보장하지 않는다. 앱이 종료된 상태에서 사진을 새로 읽고 공유 화면까지 강제로 띄우는 계약도 아니다. 알림은 OS가 전달하고 사진 수집·미완료 준비는 foreground 재개 시 수행한다. [E01][E04]

우리 앱은 사진을 네트워크에 업로드하지 않는다. 다만 사용자가 SNS를 선택하여 로컬 파일을 넘긴 뒤에는 해당 SNS 앱이 파일을 처리한다. 그 앱이 작성 화면에서 미리 업로드하는지까지 통제하거나 최종 게시 버튼 이전에는 어떤 네트워크 전송도 없다고 보장하지 않는다.

## 2. 확정 요구·설계 기본값·검증 미확정을 구분

| 분류 | 내용 |
|---|---|
| USER_CONFIRMED | 외부 서버 없음, 외부 스토리지 없음, 최종 SNS 게시 수동 |
| USER_CARRIED | 오전 9시, 지정한 사진 범위, 그날 올린 사진, 초보자가 쓰기 쉬움 |
| USER_CARRIED | MITCHELL 메인, PMO 작업자, IVA 독립 검증자. 새 역할 없음 |
| DESIGN_DEFAULT | iPhone·Galaxy 공통 구조, 로컬 게시함 기본 입력과 지정 앨범/폴더 보조 입력 |
| DESIGN_DEFAULT | React Native·Expo development build·TypeScript, 로컬 SQLite, 필요한 Swift/Kotlin 모듈 |
| DESIGN_DEFAULT | 1일 기본 공유 묶음 1개, 최대 10장, Instagram·Threads 호환성 우선 확인 |
| DESIGN_DEFAULT | 등록일 기준 당일 00:00 이상 09:00 미만. 다른 날짜는 명시적 수동 선택 |
| DEVICE_NOT_VERIFIED | 수신 SNS별 사진 수·순서·문구 수신·추가 터치·공유 취소 동작 |
| DEVICE_NOT_VERIFIED | 실제 친구의 기종·OS·SNS 앱 버전과 iOS 빌드/배포 환경 |

기존 `docs/IMPLEMENTATION_PLAN_v1.0.md`는 보존한다. 그 문서의 Windows 예약 실행·Supabase·직접 SNS API는 이번 모바일 로컬 경로의 선행조건이 아니다. 기존 Bootstrap/Web Starter의 자산과 검증 결과를 폐기하거나 새 모바일 앱의 PASS로 재사용하지 않는다.

대화에 전달된 `MITCHELL_MOBILE_PHOTO_PUBLISHER_DESIGN_v0.1_20260919.md`의 서버 동기화·임시 저장소·무인 게시 제안은 이 모바일 경로에서 비채택으로 대체한다. 촬영일과 등록일을 구분하던 원칙, 원본 보존, 대상별 이력, 불명확한 결과의 재전송 금지는 유지한다. 과거 제안 자체를 지우지 않는다.

## 3. 최소 기능과 제외 범위

### v1 필수

- 사진의 로컬 게시함 가져오기, 지정 소스 연결, 오늘 대상 계산, 후보 확인.
- 매일 오전 9시 로컬 알림, 알림 ON/OFF, 테스트 알림, 권한·다음 예정시각 표시.
- 사진 순서·제외·미리보기, 문구 템플릿·직접 수정.
- 로컬 사본의 방향 보정·호환 이미지 변환·불필요한 위치 메타데이터 제거.
- 단일/다중 사진 OS 공유, 문구 복사 대체 경로.
- SNS별 공유 시도 이력, 사용자 확인 완료, 취소/재공유 구분.
- 오프라인 사진 준비, 오류·권한 복구, 로컬 데이터 삭제, 백업 제외 설정.

### v1 제외

완전 무인 게시, 백그라운드 화면 강제 실행, 다른 앱 버튼 자동 클릭, 접근성 매크로, 비공개 SNS API, 웹 로그인 자동화, SNS 비밀번호 저장, 클라우드 동기화, AI 설명 API, 원격 Push, 광고/분석 SDK, 동영상·Reels·Stories 전용 연동, 다중 사용자·기기간 이력 동기화, 원본 자동 삭제.

네이버 블로그·티스토리도 공식 앱의 수동 편집 경로는 후속 호환성 후보다. 글쓰기 API 유무를 수동 공유 기능의 지원 여부와 혼동하지 않는다. 사진·제목·본문이 자동으로 배치된다고 미리 약속하지 않는다.

## 4. 아키텍처

```text
[휴대폰 내부]
Source Adapter
  ├─ 앱 게시함
  ├─ iOS Photos 앨범 (허용 범위)
  └─ Android 선택 폴더 (SAF)
       ↓
Import / Inventory Service
       ↓
SQLite + 비공개 로컬 사진 파일
       ↓
Selection Policy → Preparation Service → 공유 묶음 snapshot
       ↑                         ↓
OS Local Notification → Today 화면 → OS Share Bridge
                                    ↓ 사용자 선택
                         [설치된 공식 SNS 앱]
                                    ↓ 사용자가 게시
                               [해당 SNS]
```

로컬 알림은 사진 처리 엔진과 분리한다. 앱이 실행 중일 때 알림을 OS에 등록한다. 알림 내용은 고정된 ‘오늘 사진을 확인하고 공유하세요’이며, 실행하지 않은 당일 스캔의 사진 수나 완료 상태를 주장하지 않는다.

우리 앱의 배포용 실행 파일은 HTTP client·서버 endpoint·원격 설정·원격 업데이트에 의존하지 않는다. 개발 중 번들 서버와 테스트 도구의 네트워크는 사용자 사진을 다루는 배포 앱과 구분한다. 테스트에는 합성 사진만 사용한다.

## 5. 사용자 Flow

### 5.1 첫 설정

```text
앱 열기 (회원가입 없음)
 → ‘사진은 기기에 보관, SNS 전송은 공유 후 공식 앱이 처리’ 안내
 → 입력 소스 1개 선택
 → 필요한 사진/폴더 읽기 권한
 → 기존 사진 기준 목록만 등록 (과거 사진 자동 선택 금지)
 → 사진 없는 안전한 예시로 화면 안내
 → 09:00 / 한국 시간 확인
 → 사용자 동의로 알림 권한 요청·예약
 → 사진 선택·문구·공유 사용법 표시
```

SNS 계정 연결 화면 대신 ‘사용할 SNS 및 공유 도움말’을 둔다. SNS 앱 설치·로그인은 그 앱에서 진행하며 본 앱에 자격증명을 넣지 않는다. 설치 여부를 정확히 확인하지 못하면 ‘확인 필요’로 표시한다.

### 5.2 매일 사용 — 안정적인 경로

```text
아침: 앱 게시함에 사진을 추가하거나 지정 폴더/앨범에 정리
 → 앱이 활성화된 동안 필요한 사본 준비
09:00: OS가 로컬 알림 표시
 → 알림 선택·필요한 잠금 해제
 → 앱이 소스 권한과 실제 파일 상태를 확인
 → 오늘 공유 묶음 표시
 → ‘사진 공유’ 또는 ‘문구 복사 후 공유’
 → 시스템 공유 메뉴에서 SNS 선택
 → 공식 SNS 앱에서 계정·사진·문구·공개 범위 확인 후 게시
```

이미 사진·문구가 준비된 경우에는 중간 화면의 조작을 줄인다. 그러나 최초 또는 내용이 바뀐 묶음은 미리보기를 생략하지 않는다. 사용자의 share 버튼 입력 없이 외부 앱에 자료를 넘기지 않는다.

### 5.3 여러 SNS

```text
같은 사진 묶음
 → Instagram으로 공유·사용자가 게시
 → 앱으로 돌아와 Threads 공유
 → Threads에서 사용자가 게시
```

한 번의 공유가 여러 SNS에 동시 게시된다는 UI를 만들지 않는다. 첫 SNS를 공유한 뒤 두 번째 SNS가 실패해도 첫 SNS로 자동 재공유하지 않는다.

### 5.4 예외

- 사진 없음: ‘오늘 기준 대상 없음’. 폴더 접근 실패라면 사진 없음 대신 권한/조회 오류.
- 알림 늦게 선택: 오늘 9시 기준 묶음을 준비. 공유한 실제 시각은 별도로 기록.
- 자정을 넘긴 알림: 전날 묶음을 몰래 오늘 묶음과 합치지 않고 날짜를 보여준다.
- 원본 삭제/접근 취소: 유효한 로컬 사본이 있으면 이를 명시하고 미리보기, 없으면 파일 다시 선택.
- 알림 거부/집중 모드/전원 OFF: 앱을 열면 오늘 항목 표시. 정시 알림 성공을 추정하지 않는다.
- 공유 중 앱 종료: 재개 시 해당 시도를 ‘결과 미확인’으로 복구하며 자동 재전송하지 않는다.

## 6. 입력 소스와 ‘오늘’의 정의

### 6.1 앱 게시함 — 등록일의 기준 경로

앱의 ‘사진 추가’로 사진을 가져오면 로컬 사본 쓰기와 DB 등록이 성공한 시각을 `registered_at`으로 저장한다. 촬영일은 별도 참고값이다. 사용자 사진을 iCloud Drive 등의 문서 폴더에 쓰지 않는다.

향후 사진 앱의 공유 메뉴에서 본 앱을 선택하여 게시함에 넣는 수신 공유 기능을 추가할 수 있다. 그 경우 Share Extension의 로컬 staging과 본 앱의 import 경계가 필요하며, OS 수신 완료를 영구 등록 완료로 잘못 표시하지 않는다.

### 6.2 iPhone 앨범

PhotoKit에서 사용자가 허용한 자산과 선택 앨범만 조회한다. 제한된 사진 권한에서 앨범 목록이나 일부 자산을 조회할 수 없으면 앱 게시함 입력으로 안내한다. 앨범 이름은 표시용이며 안정적인 식별자는 로컬 identifier로 보관한다.

앨범은 파일시스템 폴더와 다르다. 자산 생성일/수정일/사진 보관함 추가일을 ‘이 특정 앨범에 들어온 시각’으로 대체하지 않는다. 앨범 membership snapshot을 비교하여 새로 관찰한 자산을 찾되 `first_observed_at`과 실제 추가 시각을 구분한다. [E05]

### 6.3 Galaxy 폴더

`ACTION_OPEN_DOCUMENT_TREE`로 선택된 로컬 폴더 URI와 필요한 지속 읽기 권한만 보관한다. 전체 저장소 권한을 기본 요구하지 않는다. 앱 재개/수동 새로고침 때 조회하고 백그라운드 감시는 정확성의 필수조건으로 삼지 않는다. [E06]

하위 폴더는 기본 미포함, 숨김/임시 파일은 제외, 최초 연결의 기존 항목은 기준 목록으로만 기록한다. 파일 수정시각을 폴더 이동시각으로 단정하지 않는다. 원본은 읽기만 하며 URI 권한 철회는 별도 오류다. 클라우드 문서 공급자는 기본 입력으로 연결하지 않는다.

### 6.4 날짜 정책

| 항목 | 기본값 |
|---|---|
| 시간대 | `Asia/Seoul` 고정 |
| 일일 기준 | 해당 날짜 00:00 이상, 09:00 미만 등록 |
| 기준값 | 앱 게시함 `registered_at`; 촬영일은 참고 |
| 09:00 이후 등록 | 오늘 자동 후보에서 제외. 수동으로 추가/다른 날짜 지정 가능 |
| 09:20에 알림 선택 | cutoff는 09:00 유지, 09:20까지 확대하지 않음 |
| 전날 촬영·오늘 08:10 등록 | 오늘 대상 |
| 오늘 08:59 import 시작·09:01 등록 완료 | 기본 자동 후보에서 제외, 직접 추가 가능 |
| 오후 사진의 자동 이월 | 기본 OFF. 별도 `NEXT_REMINDER_PENDING` 모드로만 제공 |
| 기존 30분 지연 제한 | 무인 서버 실행 정책이므로 이 수동 공유 경로에는 적용하지 않음 |

외부 앨범/폴더는 앱이 오래 실행되지 않았으면 정확한 추가시각을 알 수 없다. 예를 들어 08:50 스캔 이후 09:15 처음 발견한 사진을 자동으로 09:00 이전 등록으로 확정하지 않는다. `NEEDS_DATE_CONFIRMATION` 후보로 보여주고 이번 묶음에 넣을지 선택하게 한다. 이 추가 조작을 피하려면 등록시각을 직접 기록하는 앱 게시함을 사용한다.

외부 소스의 ‘최근 발견한 미공유 사진’ 모드는 제공할 수 있으나 날짜 의미가 다르므로 화면에 별도 표시하고 기본값을 조용히 바꾸지 않는다. 엄밀한 ‘그날 추가’와 앱을 전혀 열지 않는 외부 앨범 감시를 동시에 보장하지 않는다.

## 7. 9시 알림 설계

### iOS

`UNUserNotificationCenter`에 `UNCalendarNotificationTrigger`로 로컬 알림을 예약한다. OS가 알림을 전달하므로 본 앱의 서버·원격 Push 토큰은 필요 없다. 알림 클릭으로 foreground 화면에 진입한 뒤 사진을 처리한다. [E01]

시간대는 명시적으로 한국 시간으로 설정하고 여행/기기 시간대 변경 사례를 시험한다. 날짜별 사진 수를 알림 payload에 미리 고정하지 않는다. 예정 ID와 설정 버전을 로컬에 저장하고 설정 변경 때 본 앱이 만든 알림만 취소·교체한다.

단축어 Time of Day 자동화는 선택적 시제품/바로가기다. 시간 트리거가 무확인 실행 가능하다는 사실을 모든 공유 액션이 잠금 상태에서 동작한다는 뜻으로 해석하지 않는다. 본제품의 알림과 중복 등록하지 않는다. [E07]

### Android

사용자 지정 시각의 알림용 AlarmManager adapter를 두며 사진 처리와 분리한다. 정확한 알람 사용에는 OS 권한/배포 정책을 확인한다. 권한이 없으면 강제로 우회하지 않고 ‘09:00 전후 알림, 지연 가능’ 모드와 안내를 제공한다. WorkManager를 정시 시계로 사용하지 않는다. [E08]

알람 수신부는 알림 표시만 한다. `PendingIntent`로 사용자가 알림을 선택했을 때 앱을 열며, 백그라운드에서 SNS 화면을 띄우거나 full-screen intent로 우회하지 않는다. 재부팅·시간 변경·권한 변경 후 다음 알림을 재등록하고, force-stop 상태까지 정상 실행을 보장하지 않는다. [E04]

알림 표시·사진 준비·SNS 공개의 세 시각을 분리한다. 집중 모드·알림 차단·절전 등에서 사용자가 실제로 본 시각이나 정시 게시를 보장하지 않는다.

## 8. 화면과 표시 문구

| 화면 | 주 기능 |
|---|---|
| 오늘 | 기준 날짜/09:00, 준비된 사진, 날짜 확인 필요, 문구, 공유 |
| 사진함 | 로컬 등록, 출처, 순서 변경, 제외, 재선택 |
| 이력 | SNS별 공유 시도·미확인·사용자 완료 표시·다시 공유 |
| 설정 | 입력 소스, 날짜 모드, 알림, 문구 템플릿, 로컬 용량, 데이터 삭제 |

```text
오늘의 사진                         기기 내부 보관
9월 19일 · 오전 9시 기준

[사진 1] [사진 2] [사진 3]
준비 3장 / 날짜 확인 필요 1장

오늘의 기록입니다.
#일상 #오늘의사진

[사진 공유] [문구 복사 후 공유]

공유 이력
Instagram  공유 시도 · 게시 여부 미확인
Threads    아직 공유 안 함

다음 알림: 내일 오전 9시 (한국 시간)
```

화면의 SNS 이름은 사용자 설정의 의도일 수 있다. 실제 공유 메뉴에서 다른 앱을 선택하면 관측한 actual target을 별도로 기록한다. OS가 target을 알려주지 않으면 ‘알 수 없음’이며 사용자가 설정한 이름을 사실처럼 채우지 않는다.

‘공유용 준비 완료’와 ‘SNS 게시 완료’를 다른 라벨로 사용한다. 앱 자체 로그인·SNS 비밀번호·API key 입력 화면은 만들지 않는다. 기본 잠금화면 알림에는 사진 썸네일·문구·파일명을 표시하지 않는다.

## 9. 공유 adapter와 호환성

### 9.1 공통 흐름

```text
미리보기와 내용 검증
 → 변경 불가한 공유 snapshot 생성
 → 유효한 로컬 파일 URI 확보
 → 공유 시도 레코드 먼저 저장
 → foreground에서 OS 공유 화면 표시
 → OS callback 기록
 → 게시 결과는 미확인으로 유지하거나 사용자가 별도 표시
```

iOS는 `UIActivityViewController`에 로컬 사진 URL/지원 데이터와 선택적 텍스트를 넘긴다. Android는 `ACTION_SEND` 또는 `ACTION_SEND_MULTIPLE`, `EXTRA_STREAM`, `ClipData`, 임시 읽기 권한을 사용한다. Android `FileProvider`는 공유 staging 하위만 노출하고 DB·원본 전체 경로를 노출하지 않는다. [E02][E03]

### 9.2 문구 전달

SNS 수신 앱이 사진과 텍스트를 함께 받는지는 실기에서 확인한다. 공통 성공으로 미리 가정하지 않는다. 기본 경로는 사진 공유이며, 사용자가 원할 때 ‘문구 복사 후 공유’를 누른다. 텍스트가 전달되지 않으면 SNS 작성 화면에서 붙여넣는다.

iOS의 복사는 `UIPasteboard` localOnly와 만료시각을 사용한다. Android는 민감 clipboard 표시와 지원되는 보호를 사용하되 제조사 기기간 복사까지 제어한다고 주장하지 않는다. 엄격한 로컬 정책에서는 기기간 클립보드 설정을 확인하거나 복사 기능을 끄고 직접 입력한다. 자동 클립보드 읽기·원격 전송은 하지 않는다. [E09]

### 9.3 여러 사진

초기 시제품은 JPEG 1장부터 한다. 기본 제품 상한은 1묶음 최대 10장이지만 이는 앱의 제한이며 SNS의 공통 지원 한도가 아니다. 다중 이미지 지원·순서·게시 유형은 수신 앱별로 검증한다. 상한을 넘으면 사용자가 고르게 하고 자동 분할 게시하지 않는다.

수신 앱이 사진+텍스트를 거부하면 사진만 공유+문구 복사로 낮춘다. 여러 사진을 거부하면 지원되는 수량으로 사용자가 선택한다. 공유 대상 자체가 없으면 시스템 공유 메뉴 확인과 공식 앱 직접 첨부 방법을 안내한다. 이 마지막 fallback은 ‘사진 자동 첨부 완료’로 판정하지 않는다.

### 9.4 게시 성공 판정 금지

iOS completion handler는 activity 수행/취소 결과다. Android chooser callback도 공유 대상 선택 등에 대한 신호다. 모든 SNS의 원격 게시물 생성 영수증으로 취급할 계약이 없으므로 `remotePostId`를 만들어내거나 callback=true를 게시 성공으로 저장하지 않는다. [E02][E10]

앱으로 돌아왔을 때 강제 팝업 대신 작은 ‘게시 완료로 표시 / 취소했음’ 조작을 제공한다. 사용자가 아무것도 하지 않아도 앱은 사용 가능하며 해당 시도는 미확인 상태로 남는다. 다음 정기 회차에 자동으로 재공유하지 않는다. SNS 계정·공개 범위의 최종 확인은 그 앱에서 사용자가 한다.

### 9.5 호환성 기록 양식

`OS / OS version / device / target app / app version / 1-image / multi-image / order / text / taps / cancel / missing-app / result-signal / tested_at`을 기록한다. 대상은 우선 iPhone×Instagram, iPhone×Threads, Galaxy×Instagram, Galaxy×Threads다. 현재 모든 조합은 `NOT_RUN`이다. 네이버·티스토리는 후속 실기 후보다.

## 10. 로컬 데이터 모델

SQLite를 사용하고 외부 DB는 없다. 시간은 UTC instant와 표시용 zone을 분리한다. 데이터베이스·WAL·SHM·설정·사진 사본 모두 백업 제외 정책의 대상이다.

| 테이블 | 핵심 필드·책임 |
|---|---|
| sources | id, kind(inbox/ios_album/android_folder), local_locator, permission_state, baseline_at, last_scan_at |
| assets | id, source_id, source_asset_id, source_revision, original_hash, prepared_hash, registered_at, first_observed_at, captured_at, timestamp_basis, local_path, state |
| source_snapshots | source_id, snapshot_id, observed_at, known_asset_ids; 원자적 스캔 완료 기준 |
| reminder_rules | id, enabled, zone, local_time, window_mode, revision, os_request_ids |
| batches | id, rule_id, service_date, cutoff_at, revision, caption_snapshot, state |
| batch_assets | batch_id, asset_id, position, prepared_path, prepared_hash, inclusion_basis |
| share_attempts | id, batch_id, batch_revision, intended_target, observed_target, started_at, os_outcome, user_outcome, safe_error_code |
| settings | schema_version, retention_policy, source/date defaults, local privacy settings |

외부 소스 identifier+revision으로 우선 재탐지를 막고 필요한 경우 해시를 계산한다. 같은 bytes의 복제는 중복 후보로 다루되 다른 SNS 공유는 가능하다. 편집된 이미지까지 시각적으로 같다고 판단하는 AI 중복 탐지는 초기 제외다.

`UNIQUE(rule_id, service_date)`로 같은 날짜의 기본 묶음을 중복 생성하지 않는다. 수정은 revision을 올리고, 이미 공유한 시도는 당시 사진·문구 snapshot을 참조한다. 같은 이미지의 다른 날짜 재공유는 사용자 명시 동작으로만 허용한다.

DB와 파일은 단일 원자 트랜잭션이 아니므로 staging write→검증→rename→DB commit과 복구 스캔을 둔다. 저장공간 부족/중단 시 반쪽 사진을 READY로 만들지 않는다. 공유 버튼 double tap은 foreground mutex와 하나의 active attempt로 막는다.

### 상태

```text
사진
DISCOVERED → IMPORTING → READY
    ├─ NEEDS_DATE_CONFIRMATION
    ├─ SOURCE_UNAVAILABLE
    ├─ INVALID_MEDIA
    └─ EXCLUDED

묶음
DRAFT → PREPARING → READY → ARCHIVED
               └─ NEEDS_ATTENTION

공유 시도
REQUESTED → SHEET_PRESENTED → HANDOFF_UNCONFIRMED
                         ├─ CANCELLED_OBSERVED
                         └─ SHARE_ERROR
HANDOFF_UNCONFIRMED → USER_MARKED_POSTED
                   → USER_REPORTED_CANCELLED
                   → 그대로 미확인
```

모든 OS에서 취소를 관측할 수 있다고 가정하지 않는다. callback 없음은 취소가 아니라 미확인이다. 원격 게시 성공 상태는 이 제품 모델에 없다. 사용자 완료 표시의 증거 수준은 항상 `USER_REPORTED`다.

## 11. 사진 처리·수명·보안

- 휴대폰 원본은 수정·이동·삭제하지 않는다. 공유용 사본은 앱 전용 비공개 로컬 디렉터리에 둔다.
- MIME/실제 decode 결과·크기·픽셀 수를 확인하고 한 장씩 downsample/변환한다. HEIC/Live Photo는 지원되는 정지 이미지 경로로만 처리한다.
- 방향을 반영한 JPEG 사본을 기본으로 하고 GPS·불필요한 EXIF/IPTC/XMP를 제거한다. 실제 메타데이터 시험으로 확인하며 사진 내용 자체의 신원/장소 노출까지 없앤다고 주장하지 않는다.
- 플랫폼 공통 자동 crop은 하지 않는다. SNS 규격 때문에 변형이 필요하면 미리보기와 사용자 선택을 거친다.
- 공유 직후 파일을 바로 지우지 않는다. 수신 앱이 아직 읽는 중일 수 있으므로 immutable staging을 유지한다. 사용자가 완료/폐기로 정리한 묶음의 사본만 기본 7일 이후 다음 앱 실행 때 정리한다. 미확인·미공유 사진은 자동 삭제하지 않고 용량 경고를 제공한다.
- 기본 로컬 용량 경고 500MB는 제안값이며, 초과 시 대량 원본 삭제 대신 새 import를 멈추고 정리 화면을 제공한다.
- 로컬 로그에는 사진·본문·실제 경로·개인 식별자를 넣지 않고 단계/오류 코드 중심으로 보관한다. 외부 crash/analytics 전송은 없다.
- iOS의 app-private 파일은 백업 제외 속성으로 관리하고 iCloud container를 사용하지 않는다. Android는 no-backup 경로와 backup/data-extraction 규칙으로 앱 데이터를 제외한다. 제조사별 동작까지 별도로 확인한다. [E11][E12]
- PhotoKit 요청의 networkAccessAllowed는 false로 두고 기기에 없는 iCloud 원본을 앱이 자동 다운로드하지 않는다. 사용할 사진을 먼저 기기에 준비하도록 안내한다. [E13]
- 사용자가 이미 켜 놓은 Photos/갤러리 클라우드 백업이나 수신 SNS의 저장 정책은 앱이 끄거나 통제하지 못한다. 우리 앱이 새 중간 저장소를 만들지 않는 것과 기기 전체의 외부 저장 부재는 별개다.
- 삭제/재설치하면 로컬 이력과 앱 사본은 복구되지 않을 수 있다. 원본은 유지되지만 전역 중복 방지를 보장하지 않는다. 재설치 후 소스 기존 항목은 다시 baseline 처리한다.

## 12. 구현 스택과 모듈 계약

React Native+Expo development build+TypeScript를 기본 후보로 한다. 화면·선정 정책·로컬 이력은 TypeScript, OS 접근·여러 파일 공유·백업 제외·로컬 클립보드 제어는 필요한 native bridge로 분리한다. Expo는 Swift/Kotlin 모듈을 지원하지만 Expo Go만으로 모든 native 기능을 제공한다고 가정하지 않는다. [E14]

`expo-sharing.shareAsync`는 공식 문서상 단일 local file URL 진입점이다. 다중 사진·텍스트·정확한 callback 계약은 단일 API 호출로 다 된다고 가정하지 않고 ShareBridge에서 처리한다. 검증된 SDK 기능이 충분하면 재사용하되 native fallback을 둔다. [E15]

```typescript
interface SourceAdapter {
  checkAccess(): Promise<AccessState>;
  scan(previous: Snapshot | null): Promise<ScanResult>;
  readLocal(ref: AssetRef): Promise<LocalAssetResult>;
}
interface ReminderAdapter {
  replace(rule: ReminderRule): Promise<ReminderRegistration>;
  cancel(ids: string[]): Promise<void>;
  inspect(): Promise<ReminderHealth>;
}
interface ShareBridge {
  present(input: ShareBundle): Promise<OsShareOutcome>;
}
// OsShareOutcome는 게시 성공이 아님.
// shown / selection_observed / activity_completed / cancelled / error / unknown
```

PhotoPreparationService는 소스 읽기→로컬 사본→형식/방향/메타데이터 처리→해시→READY 저장을 담당한다. BatchService는 시간대·날짜창·순서·제외·revision을 담당한다. HistoryService는 OS 신호와 사용자 응답을 별도 보존한다.

### 예정 저장소 구조

```text
daily-photo-publisher/              실제 앱 저장소, 아직 생성하지 않음
  app/                             today / photos / history / settings 화면
  src/domain/                      selection / dates / batches / share-state
  src/services/                    import / prepare / reminders / share
  src/storage/                     SQLite migrations / repositories
  src/adapters/                    inbox / ios-album / android-folder
  modules/local-platform/           Swift·Kotlin bridge
  tests/unit/                      날짜·상태·중복·복구
  tests/fixtures/                  합성 사진만
  docs/                            기기 호환성·권한·개인정보·초보 안내
  AGENTS.md
  CLAUDE.md
```

별도 웹 backend, supabase 디렉터리, OAuth callback 서버는 만들지 않는다. 기존 bootstrap은 개발도구용으로 유지한다. 기존 web-starter는 별도 제품이며 이 앱의 필수 의존성이 아니다. iOS 로컬 빌드에는 macOS/Xcode 경로가 필요하고, Windows만으로 iOS native 빌드·실기 검증이 완료된다고 안내하지 않는다. 빌드·서명·배포 경로의 계정과 비용은 실제 단계에서 별도로 확인한다. [E16]

버전은 구현 시작 시 공식 stable SDK/OS 조합을 확인해 lockfile에 고정한다. 이 설계에서 확인하지 않은 최신 patch를 임의 기입하지 않는다. 앱 개발·배포 비용과 외부 사진 서버 운영비를 혼동하지 않는다.

## 13. 구현 WBS와 완료 기준

| 단계 | 작업 | 완료 기준 |
|---|---|---|
| L00 | iPhone/Galaxy 공식 공유 경로 소규모 spike | 합성 JPEG 1장·3장·문구의 수신 화면/순서/실제 터치 수를 기록. 게시 전 중단 가능 |
| L01 | 프로젝트 골격·로컬 DB·개인정보 기본 설정 | 회원가입·서버 없이 시작, 외부 API/analytics 없음, DB migration 실행 |
| L02 | 앱 게시함·사진 사본 처리 | 등록시각/해시/원본 보존/메타데이터 제거·오프라인 동작 |
| L03 | 지정 폴더·앨범 adapter | 초기 baseline, 신규 발견, 권한 거부/철회, 시각 불명 후보를 구분 |
| L04 | 9시 로컬 알림·재개 | 잠금 상태 알림, 클릭 후 안전 재개, timezone/재부팅/차단 상태 시험 |
| L05 | 공유·문구·대상별 이력 | OS 신호와 게시 확인 분리, double tap·취소·앱 종료 복구 |
| L06 | 초보 안내·전체 로컬 시험 | 새 설치부터 사용, 네트워크 없음, 백업 제외·용량·재설치 한계 확인 |
| L07 | 완료보고·exact candidate·별도 IVA | 작성자 점검과 독립검증 분리; 미실기/미게시 항목은 NOT_RUN |
| L08 | 승인된 친구 pilot·배포 | 본인 설치·SNS 로그인·지정 사진 최종 게시를 사용자가 수행 |

L00를 UI 대량 구현보다 먼저 한다. 수신 앱이 요구하는 추가 단계는 개발자 임의 추정 대신 호환성 표로 남긴다. 한 SNS가 다중 사진을 받지 못하면 그 기능만 제한하고 독립적인 로컬 준비/다른 SNS를 계속 진행한다.

기본 한 번의 설계 범위 합의 후 파일명·함수·작은 UI·일반 테스트마다 Owner에게 다시 묻지 않는다. 사용자 실사진 공개, 새 결제, 기기 설치, OS 전체 설정 변경, 저장소 생성/공개범위, 병합·배포는 별도 실행 경계다.

## 14. 필수 시험 목록

| ID | 사례 | 기대 결과 |
|---|---|---|
| T01 | 네트워크 차단 후 로컬 사진 등록·준비·문구·이력 | 우리 앱 기능 정상, 외부 호출 의존 없음 |
| T02 | 앱 종료/잠금 상태 9시 알림 | 알림과 앱 코드 실행 구분, 강제 SNS 화면 없음 |
| T03 | 알림 권한 거부·집중 모드·정확 알람 미허용 | 사용 가능 경로와 한계 표시, 정시 성공 허위 표시 없음 |
| T04 | 초기 폴더 100장 | 모두 자동 오늘 후보로 선택하지 않음 |
| T05 | 전날 촬영·오늘 등록 | 등록일 기준으로 분류 |
| T06 | 08:59:59 / 09:00:00 / 09:00:01 등록 | half-open cutoff 정확, 경계 자동 확대 없음 |
| T07 | 09:20 탭·다음 날 탭·시간대 변경 | 원래 기준 날짜와 실제 공유 시각 분리 |
| T08 | 늦게 발견한 외부 앨범 항목 | 등록시각 추정 금지, 날짜 확인 필요 표시 |
| T09 | 사진 제한 권한·폴더 철회·클라우드 전용 원본 | 원인별 오류, 사진 0장으로 오표시하지 않음 |
| T10 | HEIC·세로·큰 이미지·손상 파일 | 정상 변환/명확한 제외, 원본 불변 |
| T11 | GPS 포함 합성 사진 | 사본 metadata 제거 확인, 픽셀 방향 정상 |
| T12 | 사진 1장·3장·10장 공유 | 실제 SNS별 수량/순서/문구 호환성 기록 |
| T13 | 문구를 무시하는 수신 앱 | 복사·붙여넣기 fallback, 자동 입력 성공 과장 없음 |
| T14 | 공유 취소·SNS 미설치·앱 종료·callback 누락 | 취소/오류/미확인 분리, 자동 재공유 없음 |
| T15 | OS completed=true 또는 target 선택 | 원격 게시 성공으로 저장하지 않음 |
| T16 | 두 번 빠르게 공유 버튼 누름 | 중복 sheet/attempt 방지 |
| T17 | Instagram 시도 후 Threads 취소 | 대상별 이력 유지, 첫 대상 자동 재전송 없음 |
| T18 | 저장공간 부족·파일 쓰기 중단 | 반쪽 READY 없음, 로컬 복구 가능 |
| T19 | 공유 후 즉시 앱 복귀 | 수신 앱의 파일 접근을 조기 삭제로 끊지 않음 |
| T20 | 백업·기기간 클립보드·배포 앱 통신 검사 | 관리 범위의 외부 저장 방지와 기기 설정 한계 기록 |
| T21 | 앱 재설치·이력 삭제 | 과거 게시를 안다고 가정하지 않음, 재baseline |
| T22 | 사용자 최종 게시·공개 범위 확인 | 승인된 pilot에서만 실시, 성공은 사용자 확인 근거로 기록 |

성능 목표는 실기 기준으로 확정한다. 정해진 합성 fixture의 사진 준비 시간·최대 메모리·알림 지연·터치 수를 측정하고 미측정 수치를 제품 보장으로 제시하지 않는다.

## 15. 실행 인계와 현재 완료의 의미

다음 구현의 첫 작업은 L00 공유 호환성 spike다. 코드 작성 전에 다시 서버 여부를 묻지 않는다. 외부 사진 저장소를 ‘임시’라는 이유로 재도입하지 않는다. 로컬 알림의 실제 설정이나 오늘/내일 게시 자동화를 이 대화가 예약한 것으로 오해하지 않는다.

현재 산출물은 상세 설계다. 제품 소스·모바일 빌드·실기 시험·SNS 공유 시험·사용자 실제 게시·스토어 제출은 `NOT_RUN / NOT_STARTED`다. 기존 B/W의 IVA PASS와 본 설계의 검증 상태는 별개다. MITCHELL이 현재 설계를 작성하며 PMO·IVA를 이름만 바꿔 실행하지 않는다.

본 설계를 Git에 보존한 증거는 문서가 포함된 실제 remote commit과 readback으로 확정한다. 문서 안에 자신의 미래 commit을 미리 쓰지 않는다. 사용자 추가 행동은 설계 수신 단계에서는 없다.

## 16. 출처와 근거 범위

확인일: 2026-09-19. 아래는 구현 가능 경계의 공식 자료이며, 수신 SNS 앱별 호환성 시험을 대신하지 않는다. 용량·수량·보존기간·구현순서는 본 문서의 제안값이다. 앱의 상태 모델과 데이터 모델은 설계 판단이다.

- G01: `AofSpds/mitchell@a2ab0d75c5b72cdca7c39db1dea0c44422b7b1ec`의 README, CURRENT, AGENTS, DECISIONS, memory/MITCHELL, WORKLOG.
- G02: 기존 `docs/IMPLEMENTATION_PLAN_v1.0.md`, blob `3f4e6910d191f263baa08a1a4e4d1ce7bb4e7635`; 대화 첨부 모바일 서버형 설계 v0.1. 새 수동/무외부 요구가 모바일 실행 경로를 변경한다.
- E01: Apple, Scheduling a notification locally from your app — https://developer.apple.com/documentation/UserNotifications/scheduling-a-notification-locally-from-your-app
- E02: Apple, UIActivityViewController — https://developer.apple.com/documentation/uikit/uiactivityviewcontroller/
- E03: Android, Send data to other apps — https://developer.android.com/develop/ui/compose/sharing/send
- E04: Android, Activity security / Background activity launch restrictions — https://developer.android.com/guide/components/activities/secure-bal
- E05: Apple, PHAsset metadata (creation/modification/library added date를 앨범 membership 시각과 혼동하지 않음) — https://developer.apple.com/documentation/photos/phasset/modificationdate
- E06: Android, Storage Access Framework — https://developer.android.com/training/data-storage/shared/documents-files
- E07: Apple, Enable or disable a personal automation — https://support.apple.com/en-ke/guide/shortcuts/apd602971e63/ios
- E08: Android, Schedule exact alarms are denied by default — https://developer.android.com/about/versions/14/changes/schedule-exact-alarms
- E09: Apple, UIPasteboard.OptionsKey — https://developer.apple.com/documentation/uikit/uipasteboard/optionskey
- E10: Apple, CompletionWithItemsHandler — https://developer.apple.com/documentation/uikit/uiactivityviewcontroller/completionwithitemshandler-swift.typealias
- E11: Apple, URLResourceValues.isExcludedFromBackup — https://developer.apple.com/documentation/foundation/urlresourcevalues/isexcludedfrombackup (본문은 도구에서 JavaScript 안내만 확인됨; 구현 시 SDK 계약 재확인 필요)
- E12: Android, Auto Backup / exclusions — https://developer.android.com/identity/data/autobackup
- E13: Apple, PHImageRequestOptions.networkAccessAllowed — https://developer.apple.com/documentation/photos/phimagerequestoptions/isnetworkaccessallowed?language=objc
- E14: Expo, Add custom native code — https://docs.expo.dev/workflow/customizing/
- E15: Expo, Sharing — https://docs.expo.dev/versions/latest/sdk/sharing/
- E16: Apple, Xcode SDK and system requirements — https://developer.apple.com/xcode/system-requirements
