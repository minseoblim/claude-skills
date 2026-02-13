# UTM 빌더 설계

## 1. 입력 필드
| 필드 | 타입 | 필수 | 선택지/규칙 |
|------|------|------|-------------|
| 기본 URL | Text Input | Y | https:// 로 시작 |
| utm_source | Dropdown | Y | (허용값) |
| utm_medium | Dropdown | Y | (source에 따라 필터링) |
| utm_campaign | Auto-generate | Y | YYYYMM_product_objective_audience |
| utm_content | Text Input | N | 소문자, 언더스코어만 |
| utm_term | Text Input | N | 소문자, 언더스코어만 |

## 2. source → medium 매핑
| source 선택 시 | medium 필터 |
|---------------|-------------|
| | |

## 3. 유효성 검사 규칙
| 규칙 | 검사 내용 | 에러 메시지 |
|------|-----------|-------------|
| 필수값 | source, medium, campaign 비어있는지 | |
| 금지 문자 | 공백, 한글, 대문자, 특수문자 | |
| URL 형식 | https:// 시작 여부 | |
| 허용값 | source/medium 허용 목록 확인 | |

## 4. 히스토리 로그 구조
| Column | 설명 |
|--------|------|
| created_at | 생성 일시 |
| created_by | 생성자 |
| full_url | 전체 URL |
| utm_source~term | 파라미터별 |
| purpose | 사용 목적 메모 |

## 5. 구현 권장 방식
- Google Sheets + Apps Script
- 또는 Notion DB + 수식
- 또는 자체 웹 도구
