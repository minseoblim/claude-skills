# 데이터 파이프라인 설계

## 1. 전체 데이터 흐름

[Client] → [GTM Web] → [GA4] → [BigQuery Export (일일)]
                                      ↓
                              [events_raw 테이블]
                                      ↓
                              [Scheduled Query / dbt]
                                      ↓
                              [집계 테이블들]
                                      ↓
                              [Looker Studio / Metabase]

## 2. GA4 → BigQuery 연동
- Export 유형: Daily / Streaming
- 대상 이벤트: 전체
- 데이터셋: analytics_{ga4_property_id}

## 3. ETL 스케줄
| Job | 소스 → 타겟 | 주기 | 시간 | 비고 |
|-----|-------------|------|------|------|
| | | | | |

## 4. 매체 데이터 수집 (Paid MKT)
| 매체 | 수집 방식 | 주기 | 타겟 테이블 |
|------|-----------|------|-------------|
| | | | |

## 5. 비용 예측
| 항목 | 월 예상 비용 | 비고 |
|------|-------------|------|
| BigQuery 저장 | | |
| BigQuery 쿼리 | | |
| GA4 | 무료 | 표준 속성 기준 |
