# 이벤트 택소노미

## 1. 네이밍 규칙

| 규칙 | 내용 | 예시 |
|------|------|------|
| 형식 | {object}_{action} | page_view, button_click |
| 대소문자 | snake_case 통일 | signup_completed (O), SignupCompleted (X) |
| 언어 | 영문만 사용 | purchase_completed (O) |
| 금지 문자 | 공백, 한글, 특수문자 금지 | |
| 최대 길이 | 40자 이내 | |

## 2. 이벤트 카테고리

| 카테고리 | 설명 | 예시 이벤트 |
|----------|------|-------------|
| Page | 페이지 관련 | page_view, page_scroll |
| Account | 계정 관련 | signup_started, login |
| Product | 상품/서비스 관련 | product_view |
| Conversion | 전환 관련 | form_submit, purchase_completed |
| Campaign | 캠페인 관련 | banner_click |
| Engagement | 참여 관련 | review_write, share_click |

## 3. 공통 속성 (모든 이벤트에 포함)

| 속성명 | 타입 | 설명 | 예시 |
|--------|------|------|------|
| event_timestamp | datetime | 발생 시각 | |
| user_id | string | 회원 ID (로그인 시) | |
| session_id | string | 세션 ID | |
| page_location | string | 현재 URL | |
| page_title | string | 페이지 제목 | |
| device_category | string | 기기 유형 | mobile/desktop/tablet |
| utm_source | string | UTM 소스 | |
| utm_medium | string | UTM 매체 | |

## 4. 금지 패턴
- 공백 포함 이벤트명
- 한글 이벤트명/속성명
- 특수문자 포함
- 대소문자 혼용
- PII 속성 직접 포함
