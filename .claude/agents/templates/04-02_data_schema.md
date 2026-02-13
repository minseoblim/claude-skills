# 데이터 스키마

## 테이블 목록
| 테이블명 | 유형 | 설명 | 파티션 | 보존 기간 |
|----------|------|------|--------|-----------|
| events_raw | 원시 | 전체 이벤트 로그 | event_date | 24개월 |
| users | 마스터 | 사용자 정보 | - | 영구 |
| sessions | 집계 | 세션 요약 | session_date | 24개월 |
| daily_metrics | 집계 | 일별 핵심 지표 | metric_date | 영구 |
| funnel_summary | 집계 | 퍼널 전환 요약 | report_date | 영구 |

## 테이블 상세

### events_raw
| Column | Type | Nullable | Description | Example |
|--------|------|----------|-------------|---------|
| event_id | STRING | N | 이벤트 고유 ID | |
| event_name | STRING | N | 이벤트명 | |
| event_timestamp | TIMESTAMP | N | 발생 시각 | |
| event_date | DATE | N | 발생 일자 (파티션) | |
| user_id | STRING | Y | 회원 ID | |
| session_id | STRING | N | 세션 ID | |
| device_category | STRING | N | 기기 유형 | |
| utm_source | STRING | Y | UTM 소스 | |
| utm_medium | STRING | Y | UTM 매체 | |
| utm_campaign | STRING | Y | UTM 캠페인 | |
| page_location | STRING | N | 페이지 URL | |
| event_params | JSON | Y | 이벤트별 추가 속성 | |

### users
| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| user_id | STRING | N | PK |
| created_at | TIMESTAMP | N | 가입 일시 |
| first_touch_channel | STRING | Y | 최초 유입 채널 |
| signup_platform | STRING | Y | 가입 플랫폼 |

(sessions, daily_metrics, funnel_summary 테이블도 작성)

## ERD 관계
events_raw.user_id → users.user_id
events_raw.session_id → sessions.session_id

## BigQuery 설정
- 데이터셋: {brand_name}_analytics
- 리전: asia-northeast3 (서울)
- 파티션: event_date 기준 일별
- 클러스터링: event_name, user_id
