# 블록 에디터 구조 개발 가이드

> 식스샵 프로 블록 에디터의 최적화된 구조를 정의하는 문서입니다.
> 사용자 경험을 최우선으로 하며, 직관적이고 체계적인 에디터 구성을 목표합니다.

---

## 📋 목차

1. [핵심 원칙](#핵심-원칙)
2. [에디터 섹션 구성](#에디터-섹션-구성)
3. [텍스트 컨트롤 표준](#텍스트-컨트롤-표준)
4. [반응형 설정 패턴](#반응형-설정-패턴)
5. [실제 적용 사례](#실제-적용-사례-1-1-1-1-히어로-섹션)
6. [체크리스트](#체크리스트)

---

## 핵심 원칙

### 1️⃣ **사용자 중심 설계**
- 모든 옵션에 `description` 필드로 친절한 설명 제공
- 선택사항 여부를 명확히 표시 (예: "(선택사항)", "(모바일 전용)")
- 기본값으로 완성도 높은 상태 제공

### 2️⃣ **명확한 섹션 구분**
- TITLE로 논리적 그룹 구성
- 이모지 활용으로 시각적 구분
- 순서: 콘텐츠 → 디자인 → 크기 → 텍스트 → 애니메이션

### 3️⃣ **데스크톱/모바일 완전 분리**
- 접두사로 명확히 구분: `desktopXxx`, `mobileXxx`
- 각각의 섹션을 따로 배치
- 같은 옵션도 별도로 관리 (예: 글자 크기, 여백)

### 4️⃣ **세밀한 커스터마이징**
- 글자: 크기, 색상, 굵기, 줄바꿈 높이 모두 제어
- 배경: 그래디언트, 오버레이, 데코레이션 선택
- 애니메이션: 활성화/비활성화 + 타입 + 속도

---

## 에디터 섹션 구성

### 표준 섹션 순서

```
1. 📝 텍스트 콘텐츠
   ├─ 제목, 설명, 라벨 등의 텍스트 입력
   └─ LIST 형태의 항목 관리

2. 🔘 버튼 설정 (필요시)
   ├─ 버튼 텍스트, 링크, 스타일
   └─ 최대 개수 제한

3. 🎨 배경 & 색상
   ├─ 그래디언트 색상
   ├─ 오버레이 투명도
   ├─ 배경 이미지 (필요시)
   └─ 데코레이션 요소

4. 📏 데스크톱 크기 및 간격
   ├─ 최소 높이
   ├─ 여백 (위아래, 좌우)
   └─ 요소 간격

5. 📝 데스크톱 텍스트 스타일
   ├─ 각 텍스트별 글자 크기
   ├─ 텍스트 색상
   ├─ 텍스트 굵기
   └─ 줄바꿈 높이

6. 📱 모바일 크기 및 간격
   ├─ 최소 높이
   ├─ 여백 (위아래, 좌우)
   └─ 요소 간격

7. 📝 모바일 텍스트 스타일
   ├─ 각 텍스트별 글자 크기
   ├─ 텍스트 굵기
   └─ 줄바꿈 높이

8. ✨ 애니메이션 (필요시)
   ├─ 애니메이션 활성화
   ├─ 애니메이션 타입
   └─ 애니메이션 속도
```

---

## 텍스트 컨트롤 표준

### 텍스트 글자 크기 (RANGE)

```json
{
  "id": "desktopTitleFontSize",
  "label": "제목 글자 크기",
  "type": "RANGE",
  "min": 32,
  "max": 64,
  "step": 2,
  "unit": "px",
  "default": 48
}
```

| 텍스트 타입 | Min | Max | Step | Default | 모바일 Min-Max |
|----------|-----|-----|------|---------|----------------|
| 라벨 | 10 | 16 | 1 | 13 | 10-14 |
| 제목 | 32 | 64 | 2 | 48 | 24-42 |
| 설명 | 14 | 24 | 1 | 18 | 13-18 |
| 버튼 | 13 | 18 | 1 | 16 | 12-16 |

### 텍스트 색상 (COLOR_PICKER)

```json
{
  "id": "desktopTitleColor",
  "label": "제목 색상",
  "type": "COLOR_PICKER",
  "default": "#000000"
}
```

**색상 형식 제약:**
- ✅ 6자리 HEX: `#000000`, `#0055ff`
- ✅ 8자리 HEX (Alpha): `#000000FF`, `#0055ff80`
- ❌ RGBA: `rgba(0, 0, 0, 0.5)` (지원 안 함!)
- ❌ RGB: `rgb(0, 0, 0)` (지원 안 함!)
- ❌ 색상명: `red`, `blue` (지원 안 함!)

**기본값 가이드:**
- 제목: `#000000` (검정)
- 라벨: `#0055ff` (NOVA 브랜드 파란색)
- 설명: `#666666` (회색)
- 강조: `#0055ff` (파란색)
- 투명 검정 (8%): `#0000001A` (RGBA 0.08 대신 HEX 사용)
- 투명 흰색 (20%): `#FFFFFF33` (RGBA 0.2 대신 HEX 사용)

### 텍스트 굵기 (RADIO)

```json
{
  "id": "desktopTitleFontWeight",
  "label": "제목 굵기",
  "type": "RADIO",
  "options": [
    {"label": "보통 (400)", "value": "400"},
    {"label": "진함 (600)", "value": "600"},
    {"label": "매우 진함 (700)", "value": "700"}
  ],
  "default": "700"
}
```

**라벨용:**
```json
"options": [
  {"label": "얇음 (400)", "value": "400"},
  {"label": "중간 (500)", "value": "500"},
  {"label": "진함 (600)", "value": "600"},
  {"label": "매우 진함 (700)", "value": "700"}
]
```

### ⚠️ RADIO 옵션 label 제약사항

**제약:**
- **최대 20자 이하** (식스샵 프로 에디터 제한)
- label이 길어야 하면 `description` 필드에 상세 설명 추가

**❌ 잘못된 예:**
```json
{
  "label": "아래에서 위로 페이드 (fadeInUp)",  // 21자 초과 - 에러!
  "value": "fadeInUp"
}
```

**✅ 올바른 예:**
```json
{
  "id": "animationType",
  "label": "애니메이션 타입",
  "description": "페이드인=아래에서 올라오며 페이드, 슬라이드인=위에서 내려오며 슬라이드, 스케일인=확대되며 나타남",
  "type": "RADIO",
  "options": [
    {"label": "페이드인", "value": "fadeInUp"},         // 4자 ✓
    {"label": "슬라이드인", "value": "slideInUp"},      // 5자 ✓
    {"label": "스케일인", "value": "scaleIn"}            // 4자 ✓
  ]
}
```

### 줄바꿈 높이 (LINE-HEIGHT, RANGE)

```json
{
  "id": "desktopTitleLineHeight",
  "label": "제목 줄바꿈 높이",
  "description": "1.1 = 좁음, 1.5 = 넓음. 값이 작을수록 줄이 좁혀집니다",
  "type": "RANGE",
  "min": 1.0,
  "max": 1.6,
  "step": 0.1,
  "unit": "",
  "default": 1.2
}
```

| 텍스트 타입 | Min | Max | Step | Default |
|----------|-----|-----|------|---------|
| 제목 | 1.0 | 1.6 | 0.1 | 1.2 |
| 설명 | 1.3 | 1.9 | 0.1 | 1.7 |

---

## 반응형 설정 패턴

### 패턴: 데스크톱 + 모바일 분리

```json
{
  "type": "TITLE",
  "content": "📏 데스크톱 크기 및 간격"
},
{
  "id": "desktopMinHeight",
  "label": "최소 높이",
  "description": "데스크톱 화면에서의 섹션 최소 높이입니다",
  "type": "RANGE",
  "min": 400,
  "max": 800,
  "step": 20,
  "unit": "px",
  "default": 600
},
{
  "id": "desktopPaddingY",
  "label": "위아래 여백",
  "type": "RANGE",
  "min": 40,
  "max": 120,
  "step": 10,
  "unit": "px",
  "default": 80
},
{
  "id": "desktopPaddingX",
  "label": "좌우 여백",
  "type": "RANGE",
  "min": 20,
  "max": 100,
  "step": 10,
  "unit": "px",
  "default": 40
},

// 모바일 섹션 (별도)
{
  "type": "TITLE",
  "content": "📱 모바일 크기 및 간격"
},
{
  "id": "mobileMinHeight",
  "label": "최소 높이",
  "description": "모바일 화면(768px 이하)에서의 섹션 최소 높이입니다",
  "type": "RANGE",
  "min": 300,
  "max": 600,
  "step": 20,
  "unit": "px",
  "default": 450
},
{
  "id": "mobilePaddingY",
  "label": "위아래 여백",
  "type": "RANGE",
  "min": 30,
  "max": 80,
  "step": 10,
  "unit": "px",
  "default": 50
},
{
  "id": "mobilePaddingX",
  "label": "좌우 여백",
  "type": "RANGE",
  "min": 16,
  "max": 60,
  "step": 4,
  "unit": "px",
  "default": 20
}
```

### 패턴: 데스크톱/모바일 텍스트 분리

각 텍스트 요소마다:

```json
{
  "type": "TITLE",
  "content": "📝 데스크톱 텍스트 스타일"
},
{
  "id": "desktopTitleFontSize",
  "label": "제목 글자 크기",
  "type": "RANGE",
  "min": 32,
  "max": 64,
  "step": 2,
  "unit": "px",
  "default": 48
},
{
  "id": "desktopTitleColor",
  "label": "제목 색상",
  "type": "COLOR_PICKER",
  "default": "#000000"
},
{
  "id": "desktopTitleFontWeight",
  "label": "제목 굵기",
  "type": "RADIO",
  "options": [
    {"label": "보통 (400)", "value": "400"},
    {"label": "진함 (600)", "value": "600"},
    {"label": "매우 진함 (700)", "value": "700"}
  ],
  "default": "700"
},
{
  "id": "desktopTitleLineHeight",
  "label": "제목 줄바꿈 높이",
  "type": "RANGE",
  "min": 1.0,
  "max": 1.6,
  "step": 0.1,
  "unit": "",
  "default": 1.2
},

// 모바일 섹션
{
  "type": "TITLE",
  "content": "📝 모바일 텍스트 스타일"
},
{
  "id": "mobileTitleFontSize",
  "label": "제목 글자 크기",
  "type": "RANGE",
  "min": 24,
  "max": 42,
  "step": 2,
  "unit": "px",
  "default": 32
},
// ... 나머지 모바일 속성
```

---

## 실제 적용 사례: 1-1-1-1 히어로 섹션

### 에디터 구조

```
📝 텍스트 콘텐츠
  ├─ label (TEXT, 선택)
  ├─ title (TEXT)
  └─ description (TEXTAREA)

🔘 버튼 설정
  └─ buttons (LIST, max 2)
      ├─ buttonText (TEXT)
      ├─ buttonUrl (LINK)
      ├─ buttonStyle (RADIO: primary/secondary)
      └─ openNewTab (CHECKBOX)

🎨 배경 & 색상
  ├─ bgGradientStart (COLOR_PICKER)
  ├─ bgGradientEnd (COLOR_PICKER)
  ├─ bgOverlayOpacity (RANGE: 0-1)
  └─ enableDecorationCircles (CHECKBOX)

📏 데스크톱 크기 및 간격
  ├─ desktopMinHeight (RANGE)
  ├─ desktopPaddingY (RANGE)
  ├─ desktopPaddingX (RANGE)
  └─ desktopButtonGap (RANGE)

📝 데스크톱 텍스트 스타일
  ├─ desktopLabelFontSize (RANGE)
  ├─ desktopLabelColor (COLOR_PICKER)
  ├─ desktopLabelFontWeight (RADIO)
  ├─ desktopTitleFontSize (RANGE)
  ├─ desktopTitleColor (COLOR_PICKER)
  ├─ desktopTitleFontWeight (RADIO)
  ├─ desktopTitleLineHeight (RANGE)
  ├─ desktopDescriptionFontSize (RANGE)
  ├─ desktopDescriptionColor (COLOR_PICKER)
  ├─ desktopDescriptionLineHeight (RANGE)
  └─ desktopButtonFontSize (RANGE)

📱 모바일 크기 및 간격
  ├─ mobileMinHeight (RANGE)
  ├─ mobilePaddingY (RANGE)
  ├─ mobilePaddingX (RANGE)
  └─ mobileButtonGap (RANGE)

📝 모바일 텍스트 스타일
  ├─ mobileLabelFontSize (RANGE)
  ├─ mobileLabelFontWeight (RADIO)
  ├─ mobileTitleFontSize (RANGE)
  ├─ mobileTitleFontWeight (RADIO)
  ├─ mobileTitleLineHeight (RANGE)
  ├─ mobileDescriptionFontSize (RANGE)
  ├─ mobileDescriptionLineHeight (RANGE)
  └─ mobileButtonFontSize (RANGE)

✨ 애니메이션
  ├─ enableAnimation (CHECKBOX)
  ├─ animationType (RADIO: fadeInUp/slideInUp/scaleIn)
  └─ animationDuration (RANGE: 0.3-1.5초)
```

### 총 설정 개수: **48개**
- 콘텐츠: 3개
- 버튼: 4개 (LIST 내)
- 배경/색상: 4개
- 데스크톱 크기: 4개
- 데스크톱 텍스트: 11개
- 모바일 크기: 4개
- 모바일 텍스트: 11개
- 애니메이션: 3개

---

## 체크리스트

### 에디터 설계 시

- [ ] TITLE로 섹션 명확히 구분
- [ ] 각 섹션에 이모지 사용
- [ ] 모든 옵션에 `description` 필드 추가
- [ ] 선택사항 여부 명시
- [ ] 기본값(default) 설정
- [ ] 데스크톱과 모바일을 완전히 분리
- [ ] 접두사 사용: `desktop*`, `mobile*`
- [ ] 텍스트는 크기, 색상, 굵기, 줄바꿈 높이 모두 제어
- [ ] description은 100자 이하로 제한
- [ ] 모든 문구는 한글로 작성
- [ ] camelCase ID 명명

### HTML 구현 시

- [ ] JSON의 모든 property를 CSS에서 사용
- [ ] `property.desktopXxx` vs `property.mobileXxx` 명확히 구분
- [ ] @media (max-width: 768px)에서 모바일 스타일 재정의
- [ ] 각 텍스트에 color, font-size, font-weight, line-height 적용
- [ ] **CSS 선택자 내부에 {{#if}} 조건문 금지** (검은 화면 원인)
- [ ] **@media 쿼리 내부에 {{#if}} 조건문 금지**
- [ ] 조건부 스타일은 JavaScript에서 data-visible/data-animated 속성으로 제어
- [ ] **bm.property 사용** (bm.context.property ❌)
- [ ] Animation 설정: 개별 속성(animationName, animationDuration) ❌ → shorthand 사용 ✅
- [ ] 모든 JavaScript 코드 try-catch 블록으로 감싸기
- [ ] bm.onContextChange 필수 구현
- [ ] bm.onDestroy 구현 (이벤트 리스너 있을 경우)
- [ ] 빈 링크 클릭 방지 (헤더/푸터/버튼이 있는 경우)
- [ ] 성능 최적화: transform: translateZ(0), backface-visibility: hidden

### JSON/HTML 동기화 체크리스트

- [ ] **JSON fields와 HTML 템플릿의 필드명이 일치하는가?**
  - JSON에서 필드를 제거했다면 HTML 템플릿에서도 제거
  - HTML에서 {{#if fieldName}}로 참조하는 모든 필드가 JSON fields에 정의되어 있는가?
- [ ] **LIST type의 default 배열이 모든 fields를 포함하는가?**
  - fields에 정의된 모든 id가 default 배열의 각 객체에 포함되어야 함
- [ ] **LIST 필드 개수가 5개 이하인가?** (식스샵 가이드라인)
- [ ] **description 길이가 100자 이하인가?** (식스샵 가이드라인)
- [ ] **RADIO 옵션 label이 20자 이하인가?** (식스샵 가이드라인)

---

## 추가 팁

### 오버레이 투명도 설정 시

```json
{
  "id": "bgOverlayOpacity",
  "label": "배경 오버레이 투명도",
  "description": "0 = 투명, 1 = 불투명",
  "type": "RANGE",
  "min": 0,
  "max": 1,
  "step": 0.05,
  "unit": "",
  "default": 0
}
```

HTML에서:
```css
{{#if (gt property.bgOverlayOpacity 0)}}
.section::before {
  background-color: rgba(0, 0, 0, {{property.bgOverlayOpacity}});
}
{{/if}}
```

### 애니메이션 타입 정의

```json
{
  "id": "animationType",
  "label": "애니메이션 타입",
  "type": "RADIO",
  "options": [
    {"label": "아래에서 위로 페이드 (fadeInUp)", "value": "fadeInUp"},
    {"label": "위에서 아래로 슬라이드 (slideInUp)", "value": "slideInUp"},
    {"label": "확대되며 나타남 (scaleIn)", "value": "scaleIn"}
  ],
  "default": "fadeInUp"
}
```

HTML에서:
```css
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes slideInUp {
  from { opacity: 0; transform: translateY(60px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes scaleIn {
  from { opacity: 0; transform: scale(0.9); }
  to { opacity: 1; transform: scale(1); }
}

.content {
  {{#if property.enableAnimation}}
  animation: {{property.animationType}} {{property.animationDuration}}s ease forwards;
  {{/if}}
}
```

---

## ⚠️ CSS와 Handlebars 상호작용 제약사항

### 문제 상황

**CSS 선택자 내부에서 Handlebars 조건문 사용 금지:**

```css
/* ❌ 절대 금지 - 화면이 검게 변함 */
.item:nth-child(1) {
  {{#if property.enableAnimation}}
  animation: fadeIn 0.8s ease;
  {{/if}}
}
```

**CSS calc() 함수에서 변수 계산 금지:**

```css
/* ❌ 절대 금지 - CSS 파서 오류 */
.item {
  animation-delay: calc(2 * {{property.staggerDelay}}s);
}
```

### 원인 분석

1. **조건문으로 인한 CSS 파싱 오류**
   - Handlebars가 처리되지 않거나 조건이 false일 때 빈 규칙이 생성
   - CSS 파서가 유효하지 않은 규칙으로 인식
   - 전체 스타일 시트가 로드되지 않아 화면이 검게 됨

2. **calc() 함수의 단위 문제**
   - `calc(2 * 0.2s)` 는 유효한 CSS 문법이 아님
   - 계산 후 단위를 붙여야 하는데 Handlebars로는 처리 불가

3. **Handlebars 계산식의 CSS 파싱 오류** (필수 해결!)
   - `margin-bottom: {{property.desktopCardGap * 3}}px;` 형태의 계산식은 CSS 파서가 인식 불가
   - CSS가 로드되지 않아 "style 문법 오류"로 표시됨

### 해결 방법

**방법 1: 항상 유효한 CSS 유지**

```css
/* ✅ 올바른 방법 - CSS는 항상 완전한 상태 */
.item[data-animated] {
  animation-duration: {{property.animationDuration}}s;
  animation-timing-function: ease;
  animation-fill-mode: forwards;
}
```

**방법 1-2: Handlebars 계산식 금지**

```css
/* ❌ 절대 금지 - CSS 파서 오류 */
.header {
  margin-bottom: {{property.cardGap * 3}}px;
}

.grid {
  grid-template-columns: repeat({{property.columnCount}}, 1fr);
}

/* ✅ 올바른 방법 - 고정값 사용 */
.header {
  margin-bottom: 24px;  /* 기본값 또는 고정값 */
}

.grid {
  grid-template-columns: repeat(3, 1fr);  /* 고정 열 개수 */
}
```

**방법 2: JavaScript에서 동적 애니메이션 처리**

```javascript
function initAnimation() {
  if (bm.context.property && bm.context.property.enableAnimation) {
    const animationType = bm.context.property.animationType;
    const staggerDelay = bm.context.property.staggerDelay;

    const items = container.querySelectorAll('.item');
    items.forEach((item, index) => {
      item.setAttribute('data-animated', 'true');
      item.style.animationName = animationType;
      // JavaScript에서 계산: index * staggerDelay
      item.style.animationDelay = (index * staggerDelay) + 's';
    });
  }
}
```

### 적용 규칙

| 상황 | ❌ 하지 말 것 | ✅ 할 것 |
|------|------------|--------|
| 조건부 애니메이션 | CSS에서 `{{#if enableAnimation}}` | JavaScript에서 처리 |
| 동적 딜레이 계산 | CSS `calc()` 함수 사용 | JavaScript에서 계산 후 style 적용 |
| 조건부 스타일 | CSS 선택자 안에 Handlebars | 항상 유효한 CSS 선택자 사용 |
| 변수 보간 | 복잡한 계산식 | 단순 값 대입만 허용 |
| 동적 간격 계산 | `margin: {{property.gap * 3}}px` | 고정값 사용 (24px 등) |
| 동적 열 개수 | `repeat({{property.columns}}, 1fr)` | 고정값 사용 (repeat(3, 1fr) 등) |

---

## 🚨 JavaScript 에러 처리 및 API 사용 주의사항

### 문제 상황

**런타임 에러: "화면이 검게 변하고 콘솔에 JavaScript 에러 메시지"**

주요 원인:
1. `bm.context.property` 사용 (이는 존재하지 않음)
2. Animation 속성을 개별로 설정하다 에러 발생
3. 에러 처리 부재로 스크립트 실행 중단

### 올바른 API 사용

**❌ 잘못된 방법:**

```javascript
if (bm.context.property && bm.context.property.enableAnimation) {
  title.style.animationName = animationType;
  title.style.animationDuration = animationDuration + 's';
  title.style.animationDelay = animationDelay + 's';
  title.style.animationTimingFunction = 'ease';
  title.style.animationFillMode = 'forwards';
}
```

**✅ 올바른 방법:**

```javascript
try {
  if (bm.property && bm.property.enableAnimation) {
    const animationType = bm.property.animationType || 'fadeInLeft';
    const animationDuration = bm.property.animationDuration || 0.8;
    const staggerDelay = bm.property.staggerDelay || 0.1;

    // Animation shorthand 사용
    title.style.animation = `${animationType} ${animationDuration}s ease forwards 0s`;

    items.forEach((item, index) => {
      const delay = (2 + index) * staggerDelay;
      item.style.animation = `${animationType} ${animationDuration}s ease forwards ${delay}s`;
    });
  }
} catch (error) {
  console.error('Animation initialization failed:', error);
}
```

### API 레퍼런스

| API | 용도 | 비고 |
|-----|------|------|
| `bm.container` | 블록의 DOM 컨테이너 | 항상 사용 가능 |
| `bm.property` | 에디터 설정값 접근 | `bm.context.property` ❌ |
| `bm.onContextChange` | 설정 변경 시 콜백 | 필수 구현 |
| `bm.onDestroy` | 블록 삭제 시 콜백 | 정리 작업용 |

### Animation 설정 규칙

**Animation Shorthand 형식:**
```javascript
element.style.animation = 'animationName duration timing delay iteration direction fill-mode';

// 예시
element.style.animation = 'fadeInLeft 0.8s ease forwards 0.2s';
```

**개별 속성 사용 금지:**
```javascript
// ❌ 에러 발생 가능
element.style.animationName = 'fadeInLeft';
element.style.animationDuration = '0.8s';
element.style.animationDelay = '0.2s';

// ✅ 올바른 방법
element.style.animation = 'fadeInLeft 0.8s ease forwards 0.2s';
```

### 에러 처리 필수

모든 JavaScript 코드를 try-catch로 감싸세요:

```javascript
function initBlock() {
  try {
    // 메인 로직
    const items = container.querySelectorAll('.item');
    items.forEach(item => {
      // 처리 로직
    });
  } catch (error) {
    // 에러 로그 출력
    console.error('Block initialization failed:', error);
    // 화면이 검게 되지 않도록 silent fail
  }
}
```

---

## ⚠️ JSON LIST 필드 검증 규칙

### 문제 상황

**JSON 저장 실패: "코드를 붙여넣을 수 없다"는 에러**

LIST 타입의 설정에서 "default" 배열의 객체들이 모든 필드를 포함하지 않을 때 발생합니다.

**❌ 잘못된 예:**

```json
"default": [
  {
    "problemIcon": "❌",
    "problemTitle": "문제"
    // problemIconType 필드 누락! → 저장 실패
  }
]
```

**✅ 올바른 예:**

```json
"default": [
  {
    "problemIconType": "emoji",
    "problemIcon": "❌",
    "problemTitle": "문제"
    // 모든 필드 포함! → 저장 성공
  }
]
```

### 검증 규칙

LIST의 "default" 배열의 각 객체는:
1. **fields에 정의된 모든 id를 포함해야 함**
2. **각 필드의 값이 해당 type과 일치해야 함**
3. **모든 객체의 구조가 동일해야 함**

### 체크리스트

- [ ] default 배열의 모든 객체에 fields의 모든 id가 포함되어 있는가?
- [ ] RADIO 필드의 value가 options의 value 중 하나인가?
- [ ] default 배열의 모든 객체 구조가 동일한가?

---

## 🔴 흔한 에러 패턴과 해결 방법

### 1. "검은 화면" 에러 (Application error)

**원인**: CSS 내부의 Handlebars 조건문

```css
/* ❌ 절대 금지 */
.item {
  {{#if property.enableAnimation}}
  animation: fadeIn 0.8s;
  {{/if}}
}

/* ❌ @media 내부도 금지 */
@media (max-width: 768px) {
  {{#if property.showContent}}
  display: block;
  {{/if}}
}
```

**해결**: JavaScript에서 처리

```css
/* ✅ 올바른 방법 */
.item {
  display: none;
}

.item[data-visible] {
  display: block;
}
```

```javascript
if (bm.property && bm.property.showContent) {
  element.setAttribute('data-visible', 'true');
}
```

---

### 2. JSON 코드 붙여넣기 실패

**원인 1: HTML과 JSON 필드 불일치**

```json
{
  "fields": [
    { "id": "title" },
    { "id": "description" }
  ],
  "default": [{ "title": "...", "description": "..." }]
}
```

HTML에서 참조:
```html
{{#if solutionTitle}}  <!-- ❌ 이 필드는 JSON에 없음! -->
```

**해결**: JSON에서 제거한 필드는 HTML에서도 제거

```html
<!-- ✅ 제거됨 -->
```

---

**원인 2: LIST default 배열의 필드 누락**

```json
{
  "fields": [
    {"id": "fieldA"},
    {"id": "fieldB"}
  ],
  "default": [
    {"fieldA": "value"}  /* ❌ fieldB 누락! */
  ]
}
```

**해결**: 모든 필드를 포함

```json
{
  "default": [
    {
      "fieldA": "value",
      "fieldB": "value"  /* ✅ 모든 필드 포함 */
    }
  ]
}
```

---

### 3. JavaScript 런타임 에러

**원인**: 잘못된 API 사용

```javascript
/* ❌ 잘못된 방법 */
if (bm.context.property && bm.context.property.enableAnimation) {
  element.style.animationName = 'fadeIn';
  element.style.animationDuration = '0.8s';
}
```

**해결**: 올바른 API와 shorthand 사용

```javascript
/* ✅ 올바른 방법 */
try {
  if (bm.property && bm.property.enableAnimation) {
    element.style.animation = 'fadeIn 0.8s ease forwards';
  }
} catch (error) {
  console.error('Animation failed:', error);
}
```

---

### 4. 식스샵 가이드라인 위반

| 항목 | 제약 | 체크 |
|------|------|------|
| LIST 필드 개수 | 최대 5개 | fields 배열 길이 확인 |
| description 길이 | 최대 100자 | 문자열 길이 검사 |
| RADIO label 길이 | 최대 20자 | 각 option의 label 확인 |

---

## 🚨 SixShop Pro 호환성 - LIST 구조 및 필드 타입

### LIST 구조: "settings" vs "fields" (필수)

**SixShop Pro는 LIST 내에 "settings" 키만 지원합니다!**

**❌ 절대 금지 (지원 안 함):**
```json
{
  "id": "comparisonItems",
  "type": "LIST",
  "fields": [           // ❌ SixShop Pro 미지원!
    { "id": "title", "type": "TEXT" }
  ],
  "default": [ ... ]    // ❌ default 배열 인식 안 함
}
```

**✅ 올바른 방법:**
```json
{
  "id": "comparisonItems",
  "type": "LIST",
  "maxCount": 3,        // ← maxItems 대신 maxCount 사용!
  "settings": [         // ← fields 대신 settings 사용!
    { "id": "title", "type": "TEXT", "default": "기본값" }
  ]
  // ← default 배열 제거! (설정 각 필드의 default로 충분)
}
```

### 변경사항 정리

| 항목 | ❌ 기존 | ✅ SixShop Pro |
|-----|--------|------------|
| LIST 내 필드 정의 | "fields" | "settings" |
| 최대 개수 속성 | "maxItems" | "maxCount" |
| 기본값 정의 | "default" 배열 | 각 필드의 "default" |

### 지원하는 필드 타입 (필수 확인)

**✅ SixShop Pro 지원 필드 타입:**
- `TEXT` - 한 줄 텍스트
- `TEXTAREA` - 여러 줄 텍스트 (스크롤바 있음)
- `CHECKBOX` - 체크박스
- `COLOR_PICKER` - 색상 선택
- `LINK` - URL/링크
- `RADIO` - 라디오 버튼 (선택지)
- `RANGE` - 슬라이더 (숫자 범위)
- `LIST` - 목록 추가/편집
- `TITLE` - 섹션 제목 (편집 불가)

**❌ 지원하지 않는 필드 타입:**
- `TEXT_LONG` → **TEXTAREA로 변경해야 함!**
- `SELECT` (지원 안 함)
- `DATE` (지원 안 함)
- `FILE` (지원 안 함)
- `CUSTOM` (지원 안 함)

### TEXT_LONG → TEXTAREA 변환 규칙

**❌ 절대 금지:**
```json
{
  "id": "description",
  "type": "TEXT_LONG",   // ❌ 지원 안 함
  "placeholder": "설명 입력..."
}
```

**✅ 올바른 방법:**
```json
{
  "id": "description",
  "type": "TEXTAREA",    // ✅ 여러 줄 텍스트 표시
  "placeholder": "설명 입력...",
  "default": "기본 설명"
}
```

### 체크리스트

LIST 구조 검증:
- [ ] LIST에 "fields" 사용했나? → **"settings"로 변경**
- [ ] "maxItems" 사용했나? → **"maxCount"로 변경**
- [ ] "default" 배열이 있나? → **제거하고 각 필드의 default로 통합**

필드 타입 검증:
- [ ] TEXT_LONG 사용했나? → **TEXTAREA로 변경**
- [ ] 지원하지 않는 타입 사용했나? → **지원 타입 목록 확인**
- [ ] RADIO 옵션 label이 20자 이상? → **축약하고 description에 설명 추가**

### 실제 수정 사례

**1-1-1-2.service-overview.json (수정 전/후)**

변경 전:
```json
{
  "id": "comparisonItems",
  "type": "LIST",
  "maxItems": 3,
  "fields": [
    {
      "id": "problemDescription",
      "type": "TEXT_LONG"   // ❌ 지원 안 함
    }
  ],
  "default": [            // ❌ SixShop Pro 인식 안 함
    { "problemDescription": "설명..." }
  ]
}
```

변경 후:
```json
{
  "id": "comparisonItems",
  "type": "LIST",
  "maxCount": 3,          // ← maxItems → maxCount
  "settings": [           // ← fields → settings
    {
      "id": "problemDescription",
      "type": "TEXTAREA", // ← TEXT_LONG → TEXTAREA
      "default": "기본 설명"
    }
  ]
  // ← default 배열 제거
}
```

---

## 🚨 필수 검증 절차 (새 블록 개발 시 반드시 실행)

### Phase 1: JSON 문법 검증

**1️⃣ JSON 파싱 검증**
```bash
python3 -m json.tool 1-1-X-X.your-block.json > /dev/null && echo "✅ 파싱 성공" || echo "❌ 파싱 실패"
```

**2️⃣ 스마트 따옴표 제거**
```python
import json

with open('1-1-X-X.your-block.json', 'r', encoding='utf-8') as f:
    content = f.read()

# 스마트 따옴표 검출
smart_left = content.count('\u201c')
smart_right = content.count('\u201d')

if smart_left + smart_right > 0:
    print(f"❌ 스마트 따옴표 발견: {smart_left + smart_right}개")
    # 수정: 모두 일반 따옴표로 변환
    fixed = content.replace('\u201c', '"').replace('\u201d', '"')
    with open('1-1-X-X.your-block.json', 'w', encoding='utf-8') as f:
        f.write(fixed)
else:
    print("✅ 스마트 따옴표 없음")
```

**3️⃣ COLOR_PICKER 색상 형식 검증**
```python
import json
import re

with open('1-1-X-X.your-block.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

for setting in data.get('settings', []):
    if setting.get('type') == 'COLOR_PICKER':
        color = setting.get('default', '')
        # HEX 형식 확인: #000000 또는 #00000000
        if not re.match(r'^#[0-9A-Fa-f]{6}([0-9A-Fa-f]{2})?$', str(color)):
            print(f"❌ {setting.get('id')}: {color} (HEX 형식 아님)")
        else:
            print(f"✅ {setting.get('id')}: {color}")
```

### Phase 2: 필드 타입 검증

**4️⃣ 지원하는 필드 타입 확인**
```python
import json

SUPPORTED_TYPES = {'TEXT', 'TEXTAREA', 'CHECKBOX', 'COLOR_PICKER', 'LINK', 'RADIO', 'RANGE', 'LIST', 'TITLE'}

with open('1-1-X-X.your-block.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

for setting in data.get('settings', []):
    field_type = setting.get('type')
    if field_type not in SUPPORTED_TYPES:
        print(f"❌ 지원 안 함: {setting.get('id')} ({field_type})")

    # LIST 내부 필드도 검증
    if 'settings' in setting:
        for subsetting in setting['settings']:
            sub_type = subsetting.get('type')
            if sub_type not in SUPPORTED_TYPES:
                print(f"❌ 지원 안 함: LIST {setting.get('id')} → {subsetting.get('id')} ({sub_type})")
```

**5️⃣ TEXT_LONG → TEXTAREA 변환 확인**
```python
import json

with open('1-1-X-X.your-block.json', 'r', encoding='utf-8') as f:
    content = f.read()

if 'TEXT_LONG' in content:
    print("❌ TEXT_LONG 발견! TEXTAREA로 변경 필요")
else:
    print("✅ TEXT_LONG 없음")
```

### Phase 3: LIST 구조 검증

**6️⃣ LIST 구조 확인**
```python
import json

with open('1-1-X-X.your-block.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

for setting in data.get('settings', []):
    if setting.get('type') == 'LIST':
        list_id = setting.get('id')

        # 반드시 "settings" 키 필요
        if 'fields' in setting:
            print(f"❌ {list_id}: 'fields' 사용 중 → 'settings'로 변경 필요")
        elif 'settings' not in setting:
            print(f"❌ {list_id}: 'settings' 없음")
        else:
            print(f"✅ {list_id}: 올바른 구조")

        # "maxItems" → "maxCount" 확인
        if 'maxItems' in setting:
            print(f"❌ {list_id}: 'maxItems' 사용 → 'maxCount'로 변경")

        # "default" 배열 제거 확인
        if 'default' in setting and isinstance(setting['default'], list):
            print(f"❌ {list_id}: 'default' 배열 있음 → 제거 필요")
```

### Phase 4: HTML/CSS 검증

**7️⃣ CSS 내 Handlebars 계산식 확인**
```python
import re

with open('1-1-X-X.your-block.html', 'r', encoding='utf-8') as f:
    content = f.read()

# <style> 추출
style_match = re.search(r'<style>(.*?)</style>', content, re.DOTALL)
if style_match:
    style_content = style_match.group(1)

    # 계산식 패턴 검사
    patterns = [
        r'{{\s*property\.\w+\s*[\*\+\-]\s*\d+\s*}}',  # {{property.x * n}}
        r'repeat\([^)]*{{\w+[^)]*}}\)',               # repeat({{...}})
        r'calc\([^)]*{{\w+[^)]*}}\)',                 # calc({{...}})
    ]

    found = False
    for pattern in patterns:
        matches = re.findall(pattern, style_content)
        if matches:
            found = True
            print(f"❌ CSS 계산식 발견: {matches}")

    if not found:
        print("✅ CSS 계산식 없음")
```

**8️⃣ CSS 조건문 확인**
```python
import re

with open('1-1-X-X.your-block.html', 'r', encoding='utf-8') as f:
    content = f.read()

# CSS 내 Handlebars 조건문 검사
style_match = re.search(r'<style>(.*?)</style>', content, re.DOTALL)
if style_match:
    style_content = style_match.group(1)
    if '{{#if' in style_content or '{{#each' in style_content:
        print("❌ CSS 내 Handlebars 조건문 발견!")
    else:
        print("✅ CSS 조건문 없음")
```

### Phase 5: 최종 검증

**9️⃣ description 길이 확인**
```python
import json

with open('1-1-X-X.your-block.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

for setting in data.get('settings', []):
    if 'description' in setting:
        desc_len = len(setting['description'])
        if desc_len > 100:
            print(f"❌ {setting.get('id')}: description {desc_len}자 (100자 이상)")
        else:
            print(f"✅ {setting.get('id')}: {desc_len}자")
```

**🔟 RADIO 옵션 label 길이 확인**
```python
import json

with open('1-1-X-X.your-block.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

for setting in data.get('settings', []):
    if setting.get('type') == 'RADIO':
        for option in setting.get('options', []):
            label = option.get('label', '')
            if len(label) > 20:
                print(f"❌ {setting.get('id')} 옵션: '{label}' ({len(label)}자)")
            else:
                print(f"✅ {setting.get('id')} 옵션: '{label}' ({len(label)}자)")
```

---

## 📋 블록 개발 체크리스트 (필수)

### JSON 검증
- [ ] `python3 -m json.tool` 파싱 성공
- [ ] 스마트 따옴표 0개
- [ ] TEXT_LONG 없음 (모두 TEXTAREA)
- [ ] LIST에 "settings" 사용 (fields ❌)
- [ ] LIST에 "maxCount" 사용 (maxItems ❌)
- [ ] LIST에 "default" 배열 없음
- [ ] COLOR_PICKER 모두 HEX 형식 (#RRGGBB 또는 #RRGGBBAA)
- [ ] description 길이 100자 이하
- [ ] RADIO label 길이 20자 이하

### HTML 검증
- [ ] CSS에 Handlebars 조건문 없음 ({{#if}} ❌)
- [ ] CSS에 계산식 없음 ({{property.x * n}} ❌)
- [ ] CSS에 repeat/calc 함수와 Handlebars 조합 없음
- [ ] @media 쿼리 내에 Handlebars 조건문 없음
- [ ] 모든 CSS 선택자는 항상 유효한 상태
- [ ] JavaScript에서 bm.property 사용 (bm.context.property ❌)
- [ ] Animation은 shorthand 형식 사용
- [ ] 모든 JavaScript 코드 try-catch로 감싸기

### JSON/HTML 동기화
- [ ] HTML에서 참조하는 모든 필드가 JSON에 정의되어 있음
- [ ] JSON에서 제거한 필드는 HTML에서도 제거
- [ ] LIST의 모든 필드가 default 데이터에 포함

---

## 🔴 발생 가능한 에러 정리 (해결책 포함)

| 에러 메시지 | 원인 | 증상 | 해결책 |
|----------|------|------|--------|
| JSON 파싱 오류 | 스마트 따옴표 사용 | 블록 생성 불가 | 모든 " 를 일반 따옴표로 변경 |
| "지원하지 않는 settingType" (TEXT_LONG) | TEXT_LONG 필드 사용 | 저장 실패 | TEXT_LONG → TEXTAREA |
| "default는 hex 코드 형식만" | COLOR_PICKER에 RGBA/RGB 사용 | 저장 실패 | RGBA → HEX with Alpha |
| "style 문법 오류" | CSS 계산식 또는 조건문 | 저장 성공하지만 렌더링 안 됨 | CSS에서 모든 계산식/조건문 제거 |
| 검은 화면 (블록 로드 안 됨) | JavaScript 런타임 에러 | 에디터에서 블록 안 보임 | try-catch로 감싸기, 콘솔 에러 확인 |

---

**문서 버전**: 2.0
**작성일**: 2025-11-13
**수정일**: 2025-11-13
**최종 수정**: 2025-11-13 (필수 검증 절차 및 체크리스트 추가)

### 📌 v1.5 주요 업데이트

1. **HTML 구현 체크리스트 강화**
   - CSS 내 {{#if}} 조건문 금지 명시
   - bm.property vs bm.context.property 구분
   - Animation shorthand 필수 사용

2. **JSON/HTML 동기화 체크리스트 신규**
   - 필드명 일치 확인 절차
   - LIST default 배열 필드 완결성 검증
   - 식스샵 가이드라인 체크항목 통합

3. **흔한 에러 패턴 섹션 신규**
   - 검은 화면 에러 원인과 해결법
   - JSON 코드 붙여넣기 실패 원인 분석
   - JavaScript 런타임 에러 패턴
   - 식스샵 가이드라인 위반 체크표

### 🔴 Critical Fixes (2025-11-13)

#### Fix 1: CSS Handlebars 조건문 오류
**발견된 문제**: CSS 내 모든 {{#if}} 조건문이 CSS 파서 오류를 유발

**수정된 사항**:
1. **1-1-1-1**: bgOverlayOpacity와 enableDecorationCircles 조건 제거
2. **1-1-1-2**: @media 쿼리 내 showDivider 조건 제거 (전체 HTML 재작성)
3. **1-1-1-5**: CSS 내 bgType과 showContactInfo 조건 제거

**적용 패턴**:
```css
/* ✅ 올바른 방법: CSS는 항상 선언, JavaScript에서 속성으로 제어 */
.element {
  display: none;  /* 기본: 숨김 */
}

.element[data-visible] {
  display: block;  /* JavaScript에서 setAttribute('data-visible', 'true') */
}
```

#### Fix 2: JSON 스마트 따옴표 파싱 오류 (필수 해결!)
**발견된 문제**: 편집기에서 따옴표가 스마트 따옴표(" ")로 자동 변환되어 JSON 파싱 실패

**문제 상황**:
- JSON만 붙여넣으면 화면 크래시 발생
- HTML 없이도 JSON 파싱에 실패 → 에디터 자체 문제

**원인**: 스마트 따옴표 (U+201C, U+201D) 사용
```json
// ❌ 잘못된 예 (스마트 따옴표 "")
{
  "label": "섹션 제목"
}
```

**정상 상황**: 기본 따옴표 (U+0022) 사용
```json
// ✅ 올바른 예
{
  "label": "섹션 제목"
}
```

**예방 방법**:
- JSON 편집 시 **스마트 따옴표 자동 변환 비활성화**
- 외부 JSON 에디터 사용 (VS Code, Sublime 등)
- 파일 저장 전 JSON 검증: https://jsonlint.com/

**수정 방법** (Python):
```python
import json
with open('file.json', 'r', encoding='utf-8') as f:
    content = f.read()
fixed = content.replace('\u201c', '"').replace('\u201d', '"')
json.loads(fixed)  # 검증
with open('file.json', 'w', encoding='utf-8') as f:
    f.write(fixed)
```

**실제 적용 사례**: 1-1-1-2 JSON 파일에서 1,238개의 스마트 따옴표 제거
