# GTM Event Setup Skill

**Purpose**: GTM 태그 이벤트 생성을 위한 자동화 스킬. 실무에서 빠르게 GTM 설정을 적용할 수 있도록 돕습니다.

**Usage**: `/gtm-event-setup` 또는 `/gtm`

---

## Task Description

당신은 GTM(Google Tag Manager) 이벤트 설정 전문가입니다. 다음 작업을 수행합니다:

### Step 0: 파일 탐색 (자동)

프로젝트에서 GTM 관련 파일을 자동으로 탐색합니다:

1. **이벤트 관리 시트 찾기**: 프로젝트 내에서 다음 패턴의 파일을 검색합니다:
   - `**/*GTM*.csv`
   - `**/*gtm*.csv`
   - `**/이벤트*.csv`
   - `**/event*.csv`

2. **설정 가이드 찾기**: 프로젝트 내에서 다음 패턴의 파일을 검색합니다:
   - `**/*GTM*.md`
   - `**/*gtm*.md`

3. **파일을 찾지 못한 경우**: 사용자에게 파일 경로를 직접 입력받습니다.

---

## CSV 컬럼 구조 (표준)

| 컬럼 순서 | 컬럼명 | 설명 | 예시 |
|---|---|---|---|
| 1 | 설치 | 체크박스 (설치 완료 여부) | ✓ |
| 2 | 카테고리/구분 | 이벤트 분류 | 주요전환, 전환-랜딩, 인게이지먼트, 광고전환, 베이스 |
| 3 | 우선 순위 | 숫자 (1: 높음, 5: 낮음) | 1, 2, 3, 5 |
| 4 | 이벤트 | GTM 태그 이름 | `SUP - GA4 - generate_lead` |
| 5 | 설명 | 이벤트 설명 | 상담신청/셀프가입 제출 완료 |
| 6 | 이벤트명 | dataLayer 이벤트명 | `generate_lead` |
| 7 | 트리거 | GTM 트리거 이름 | `셀프가입 - generate_lead` |
| 8 | 트리거 설명 | 트리거 동작 설명 | dataLayer에서 generate_lead 이벤트 감지 |
| 9 | 이벤트 매개변수 | 매개변수 이름 | `lead_type`, `carrier` |
| 10 | 이벤트 매개변수 설명 | 매개변수 설명 | 전환 유형 (consultation/self_signup) |
| 11 | 변수명 | GTM 변수 이름 | `{{SUP - self_landing - lead_type}}` |
| 12 | 예시값 | 매개변수 예시 값 | `consultation`, `KT` |
| 13 | 사용자 속성 (개발자 논의) | GA4 사용자 속성명 | `lead_type` |
| 14 | 사용자 속성 값 (개발자 논의) | 사용자 속성 값 | `consultation / self_signup` |

---

## 명명 규칙 (Naming Convention)

### 1. 태그 이름 (이벤트 컬럼)
```
[프로젝트코드] - [플랫폼] - [이벤트명]
```
- 예시: `SUP - GA4 - generate_lead`
- 예시: `SUP - Google Ads - generate_lead`
- 예시: `SUP - Meta Pixel - Lead`
- 예시: `SUP - Naver Ads - Conversion`

### 2. 트리거 이름
```
[서비스명] - [이벤트명]
```
- 예시: `셀프가입 - generate_lead`
- 예시: `셀프가입 - consultation_form_open`
- 예시: `셀프가입 - plan_view`

### 3. 변수 이름
```
{{[프로젝트코드] - [서비스명] - [변수명]}}
```
- 예시: `{{SUP - self_landing - lead_type}}`
- 예시: `{{SUP - self_landing - carrier}}`
- 예시: `{{SUP - self_landing - value}}`

---

## 재활용 규칙 (유지보수 비용 최소화)

### 핵심 원칙
**동일한 이름의 변수/트리거가 이미 존재하면 반드시 재활용합니다.**

### 1. 트리거 재활용
- **같은 이벤트명** → **같은 트리거 공유**
- 예: `generate_lead` 이벤트를 GA4, Google Ads, Meta 태그에서 모두 사용 → 트리거 `셀프가입 - generate_lead` 1개만 생성

```
태그: SUP - GA4 - generate_lead        → 트리거: 셀프가입 - generate_lead
태그: SUP - Google Ads - generate_lead → 트리거: 셀프가입 - generate_lead (재활용)
태그: SUP - Meta Pixel - Lead          → 트리거: 셀프가입 - generate_lead (재활용)
```

### 2. 변수 재활용
- **같은 dataLayer 키** → **같은 변수 공유**
- 예: `carrier` 매개변수가 여러 이벤트에서 사용 → 변수 `{{SUP - self_landing - carrier}}` 1개만 생성

```
이벤트: generate_lead        → 변수: {{SUP - self_landing - carrier}}
이벤트: consultation_form_open → 변수: {{SUP - self_landing - carrier}} (재활용)
이벤트: plan_view           → 변수: {{SUP - self_landing - carrier}} (재활용)
```

### 3. 중복 체크 로직
새 이벤트 추가 시:
1. **기존 CSV 파일 분석** → 이미 존재하는 변수/트리거 목록 추출
2. **새 이벤트의 변수/트리거** → 기존 목록과 비교
3. **일치하면** → "기존 [변수/트리거] 재활용" 표시
4. **일치하지 않으면** → "신규 생성 필요" 표시

---

## Workflow

### Step 1: 파일 확인

AskUserQuestion 도구를 사용하여 사용자에게 확인합니다:

**질문**: "GTM 이벤트 관리 시트를 찾았습니다. 이 파일이 맞나요?"
- 옵션 1: "네, 맞습니다" (권장)
- 옵션 2: "다른 파일 사용" - 파일 경로 직접 입력
- 옵션 3: "새로 만들기" - 빈 템플릿 CSV 생성

(설정 가이드 파일이 있다면 함께 확인)

### Step 2: CSV 파일 분석 및 기존 자산 파악

CSV 파일을 읽고 다음 정보를 추출합니다:

**2-1. 기존 자산 목록 추출**
```
기존 변수 목록:
- {{SUP - self_landing - lead_type}}
- {{SUP - self_landing - carrier}}
- ...

기존 트리거 목록:
- 셀프가입 - generate_lead
- 셀프가입 - consultation_form_open
- ...

기존 태그 목록:
- SUP - GA4 - generate_lead
- SUP - GA4 - consultation_form_open
- ...
```

**2-2. 명명 규칙 자동 감지**
- 프로젝트 코드: `SUP` (태그 이름 접두사에서 추출)
- 서비스명: `self_landing` 또는 `셀프가입` (변수/트리거에서 추출)
- 플랫폼 목록: `GA4`, `Google Ads`, `Meta Pixel`, `Naver Ads`

### Step 3: 사용자 선택 받기

AskUserQuestion 도구를 사용하여:

**질문 1**: "무엇을 하시겠습니까?"
- 옵션 1: "새 이벤트 추가" (권장) - CSV에 새 이벤트 추가 + GTM 설정 가이드 생성
- 옵션 2: "전체 설정 가이드" - 모든 이벤트의 GTM 설정 가이드 생성
- 옵션 3: "미설치 항목만" - 설치 체크 안된 항목만 가이드 생성
- 옵션 4: "특정 이벤트" - 이벤트명 직접 입력

**질문 2** (새 이벤트 추가 시): "어떤 플랫폼에 설정하시겠습니까?" (multiSelect: true)
- 옵션 1: "GA4" (권장)
- 옵션 2: "Google Ads"
- 옵션 3: "Meta Pixel"
- 옵션 4: "Naver Ads"

### Step 4: 재활용 분석 및 설정 가이드 생성

**4-1. 재활용 분석 결과 출력**
```markdown
## 🔄 재활용 분석 결과

### 재활용 가능한 기존 자산
| 유형 | 이름 | 재활용 대상 이벤트 |
|---|---|---|
| 트리거 | 셀프가입 - generate_lead | GA4, Google Ads, Meta 태그에서 공유 |
| 변수 | {{SUP - self_landing - carrier}} | generate_lead, plan_view에서 공유 |

### 신규 생성 필요
| 유형 | 이름 | 용도 |
|---|---|---|
| 변수 | {{SUP - self_landing - new_param}} | 새 이벤트용 |
| 트리거 | 셀프가입 - new_event | 새 이벤트 트리거 |
```

**4-2. GTM 설정 가이드 생성**

선택된 범위에 맞춰 다음 형식으로 출력:

```markdown
# GTM 설정 가이드 - [프로젝트명]

## 📋 설정 개요
- 설정할 이벤트: [개수]개
- 신규 변수: [개수]개 / 재활용: [개수]개
- 신규 트리거: [개수]개 / 재활용: [개수]개
- 신규 태그: [개수]개

---

## 1️⃣ 변수 (Variables) 생성

GTM → 변수 → 사용자 정의 변수 → 새로 만들기

| 변수 이름 | 데이터 영역 변수 이름 | 사용 이벤트 |
|---|---|---|
| ... | ... | ... |

### 변수 생성 방법
1. **변수** → 사용자 정의 변수 → **새로 만들기**
2. 변수 구성 → **데이터 영역 변수** 선택
3. **데이터 영역 변수 이름**: 위 표 참조
4. **데이터 영역 버전**: 버전 2
5. 저장

---

## 2️⃣ 트리거 (Triggers) 생성

GTM → 트리거 → 새로 만들기

| 트리거 이름 | 이벤트 이름 | 설명 |
|---|---|---|
| ... | ... | ... |

### 트리거 생성 방법
1. **트리거** → **새로 만들기**
2. 트리거 구성 → **맞춤 이벤트** 선택
3. **이벤트 이름**: 위 표 참조
4. **정규 표현식 일치 사용**: 체크 해제
5. 저장

---

## 3️⃣ 태그 (Tags) 생성

### GA4 Configuration 태그
| 항목 | 설정값 |
|---|---|
| **태그 이름** | `GA4 - Configuration` |
| **태그 유형** | Google 태그 |
| **태그 ID** | `G-XXXXXXXXXX` (사용자 입력) |
| **트리거** | All Pages |

### GA4 Event 태그
[이벤트별 상세 설정]

### 광고 플랫폼 태그
[선택된 플랫폼별 상세 설정]

---

## ✅ 설정 완료 체크리스트

### 변수
- [ ] 변수명 1
- [ ] 변수명 2
- [ ] ...

### 트리거
- [ ] 트리거명 1
- [ ] 트리거명 2
- [ ] ...

### 태그
- [ ] GA4 - Configuration
- [ ] GA4 - Event - 이벤트명
- [ ] ...

### GA4 맞춤 정의
- [ ] 맞춤 측정기준 등록
- [ ] 맞춤 측정항목 등록

### 광고 플랫폼 ID 설정
- [ ] Google Ads: 전환 ID + 라벨
- [ ] Meta: Pixel ID
- [ ] Naver: 계정 ID + 전환 유형

---

## 🧪 검증 방법

1. **GTM Preview 모드**
   - GTM → 미리보기 클릭
   - 웹사이트에서 이벤트 발생 → Tags Fired 확인

2. **브라우저 콘솔**
   ```javascript
   window.dataLayer.filter(d => d.event)
   ```

3. **GA4 DebugView**
   - GA4 관리자 → DebugView → 실시간 이벤트 확인

4. **광고 플랫폼 확인**
   - Google Ads: 전환 → 상태 "기록 중" 확인
   - Meta: Events Manager → 테스트 이벤트 확인
   - Naver: 광고 관리 → 전환 수집 상태 확인
```

### Step 5: 추가 지원

사용자가 요청하면:
- 특정 이벤트의 dataLayer.push() 코드 구현 예시 제공
- GTM 설정 중 문제 해결 지원
- CSV에 새로운 이벤트 추가 지원
- 프론트엔드 이벤트 트래킹 코드 생성

---

## CSV Template

사용자가 CSV 파일이 없는 경우, 다음 템플릿을 제공합니다:

```csv
설치,카테고리/구분,우선 순위,이벤트,설명,이벤트명,트리거,트리거 설명,이벤트 매개변수,이벤트 매개변수 설명,변수명,예시값,사용자 속성,사용자 속성 값
,주요전환,1,SUP - GA4 - generate_lead,리드 전환 이벤트,generate_lead,셀프가입 - generate_lead,dataLayer에서 generate_lead 이벤트 감지,lead_type,전환 유형,{{SUP - self_landing - lead_type}},consultation / self_signup,lead_type,consultation / self_signup
```

### 컬럼 설명
1. **설치**: 체크박스 (✓ 표시로 설치 완료 표시)
2. **카테고리/구분**: 주요전환, 전환-랜딩, 인게이지먼트, 광고전환, 베이스
3. **우선 순위**: 1(높음) ~ 5(낮음)
4. **이벤트**: GTM 태그 이름 (`[프로젝트코드] - [플랫폼] - [이벤트명]`)
5. **설명**: 이벤트 설명
6. **이벤트명**: dataLayer.push()에서 사용하는 이벤트명
7. **트리거**: GTM 트리거 이름 (`[서비스명] - [이벤트명]`)
8. **트리거 설명**: 트리거 동작 설명
9. **이벤트 매개변수**: 매개변수 이름
10. **이벤트 매개변수 설명**: 매개변수 설명
11. **변수명**: GTM 변수 이름 (`{{[프로젝트코드] - [서비스명] - [변수명]}}`)
12. **예시값**: 매개변수 예시 값
13. **사용자 속성**: GA4 사용자 속성명 (개발자 논의 필요)
14. **사용자 속성 값**: 사용자 속성 값 (개발자 논의 필요)

---

## Output Format

1. **명확한 단계별 가이드**: 1️⃣, 2️⃣, 3️⃣ 형식으로 단계 구분
2. **표 형식**: 설정 항목을 표로 정리하여 한눈에 파악 가능
3. **복사 가능한 코드**: 백틱으로 감싸서 바로 복사 가능
4. **체크리스트**: [ ] 형식으로 진행 상황 체크 가능

---

## Important Notes

1. **CSV 파싱**: CSV의 첫 행은 헤더, 2행부터 데이터. UTF-8 BOM 처리 필요.
2. **우선순위**: 숫자가 낮을수록 중요 (1이 가장 중요).
3. **명명 규칙 준수**: 기존 프로젝트의 명명 규칙을 자동 감지하고 따름
   - 태그: `[프로젝트코드] - [플랫폼] - [이벤트명]`
   - 트리거: `[서비스명] - [이벤트명]`
   - 변수: `{{[프로젝트코드] - [서비스명] - [변수명]}}`
4. **재활용 우선**: 동일한 변수/트리거가 존재하면 반드시 재활용하여 유지보수 비용 최소화
5. **플랫폼 ID**: Google Ads 전환 ID, Meta Pixel ID, Naver 계정 ID는 사용자에게 입력받음
6. **설치 체크**: CSV의 "설치" 컬럼으로 GTM 설정 완료 여부 추적

---

## Example Interaction

```
User: /gtm-event-setup