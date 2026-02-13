# GTM 설계 문서

## 1. 컨테이너 구조
| 컨테이너 | 유형 | 용도 | 설치 위치 |
|----------|------|------|-----------|
| GTM-XXXXX | Web | 클라이언트 사이드 추적 | head 태그 |
| GTM-YYYYY | Server | 서버 사이드 추적 (선택) | Cloud Run |

## 2. dataLayer 스키마

### 페이지 로드 시 기본 push
```javascript
dataLayer.push({
  'event': 'page_data_ready',
  'page_type': '{page_type}',
  'user_id': '{user_id}',
  'user_login_status': '{logged_in}',
  'page_location': '{url}',
  'page_title': '{title}'
});
```

### 이벤트 발생 시 push
```javascript
dataLayer.push({
  'event': '{event_name}',
  'event_category': '{category}',
  // 이벤트별 속성
});
```

## 3. GTM 변수 목록
| 변수명 | 타입 | dataLayer Key | 용도 |
|--------|------|---------------|------|
| | | | |

## 4. 태그 템플릿
(GA4, Meta Pixel, Google Ads 등 매체별)

## 5. 트리거 네이밍 규칙
형식: `{trigger_type} - {description}`

| 접두사 | GTM 트리거 유형 | 예시 |
|--------|----------------|------|
| CE - | Custom Event | CE - signup_completed |
| PV - | Page View | PV - product_detail |
| CL - | Click | CL - cta_button |

## 6. 개발팀 전달 스펙 (dataLayer push 목록)
| 페이지/액션 | dataLayer push 코드 | 구현 시점 | 담당 |
|-------------|---------------------|-----------|------|
| | | | |
