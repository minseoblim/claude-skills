# KPI 정의서

## North Star Metric
| 항목 | 내용 |
|------|------|
| 지표명 | |
| 정의 | |
| 산식 | |
| 목표 | |

## 핵심 KPI

### 1. 획득(Acquisition) 지표
| Metric | 정의 | 산식 (SQL) | Dimension | Data Source | Refresh | Owner |
|--------|------|-----------|-----------|-------------|---------|-------|
| | | | | | | |

### 2. 활성화(Activation) 지표
| Metric | 정의 | 산식 (SQL) | Dimension | Data Source | Refresh | Owner |
|--------|------|-----------|-----------|-------------|---------|-------|
| | | | | | | |

### 3. 전환(Conversion) 지표
| Metric | 정의 | 산식 (SQL) | Dimension | Data Source | Refresh | Owner |
|--------|------|-----------|-----------|-------------|---------|-------|
| | | | | | | |

### 4. 유지(Retention) 지표
| Metric | 정의 | 산식 (SQL) | Dimension | Data Source | Refresh | Owner |
|--------|------|-----------|-----------|-------------|---------|-------|
| | | | | | | |

### 5. 매출(Revenue) 지표
| Metric | 정의 | 산식 (SQL) | Dimension | Data Source | Refresh | Owner |
|--------|------|-----------|-----------|-------------|---------|-------|
| | | | | | | |

## 예외 규칙
| 규칙 | 적용 대상 | 처리 방법 |
|------|-----------|-----------|
| 테스트 계정 제외 | 전체 지표 | user_id NOT IN (test_users) |
| 내부 트래픽 제외 | 전체 지표 | IP 필터 OR is_internal = false |
| 봇 트래픽 제외 | 전체 지표 | is_bot = false |
