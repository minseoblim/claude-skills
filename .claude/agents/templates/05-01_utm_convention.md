# UTM 컨벤션 규칙

## 1. 기본 규칙
| 규칙 | 내용 |
|------|------|
| 대소문자 | 소문자 통일 |
| 구분자 | 언더스코어(_) 사용 |
| 금지 문자 | 공백, 한글, 특수문자 |
| 필수 파라미터 | utm_source, utm_medium, utm_campaign |

## 2. utm_source 허용값
| source | 설명 | 사용 예 |
|--------|------|---------|
| | | |

## 3. utm_medium 허용값
| medium | 설명 | 매핑 source |
|--------|------|-------------|
| | | |

## 4. utm_campaign 네이밍 포맷
형식: `{YYYYMM}_{product}_{objective}_{audience}`

| 세그먼트 | 규칙 | 예시 |
|----------|------|------|
| YYYYMM | 캠페인 시작 년월 | 202401 |
| product | 제품/서비스 코드 | brand |
| objective | 캠페인 목적 | awareness, conversion |
| audience | 타겟 세그먼트 | all, new_user |

## 5. utm_content / utm_term
| 파라미터 | 용도 | 예시 |
|----------|------|------|
| utm_content | 소재 구분 | banner_a, video_30s |
| utm_term | 키워드 (검색광고) | brand_keyword |

## 6. 예시 URL
| 채널 | URL |
|------|-----|
| | |
