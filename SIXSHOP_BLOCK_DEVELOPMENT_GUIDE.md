# 식스샵 블록메이커 개발 규칙 및 기준

> 식스샵 프로 블록메이커 개발 시 반드시 준수해야 할 모든 규칙, 기준, 제약사항

---

## 📋 목차

### Part 1: 핵심 규칙
1. [파일 구조 규칙](#1-파일-구조-규칙)
2. [핵심 제약사항 (MUST)](#2-핵심-제약사항-must)
3. [Handlebars 문법 및 제약](#3-handlebars-문법-및-제약)
4. [JSON 설정 규칙](#4-json-설정-규칙)
5. [CSS 작성 규칙](#5-css-작성-규칙)
6. [JavaScript API 규칙](#6-javascript-api-규칙)

### Part 2: 식스샵 프로 기준
7. [식스샵 프로 필수 요건](#7-식스샵-프로-필수-요건)
8. [커스텀 블록 필수 점검](#8-커스텀-블록-필수-점검)
9. [헤더/푸터 블록 기준](#9-헤더푸터-블록-기준)

### Part 3: 개발 원칙
10. [사용자 요청 개발 원칙](#10-사용자-요청-개발-원칙)
11. [최종 체크리스트](#11-최종-체크리스트)

---

## Part 1: 핵심 규칙

## 1. 파일 구조 규칙

### 1-1) 필수 파일 분리

```
block-name.html        # HTML + CSS + JavaScript
block-name.json        # 설정 스키마 + 기본값
```

**규칙:**
- HTML과 JSON을 반드시 분리
- 사용자가 복붙할 수 있도록 독립적으로 작성

### 1-2) HTML 파일 구조

```html
<style>
  /* CSS 스타일 */
  .block-name {
    property: {{property.value}};
  }
</style>

<template>
  <!-- HTML 템플릿 -->
  {{#each property.items}}
    <div>{{title}}</div>
  {{/each}}
</template>

<script>
  // JavaScript
  const container = bm.container;
  const context = bm.context;
</script>
```

### 1-3) JSON 파일 구조

```json
{
  "settings": [
    {
      "type": "TITLE",
      "content": "섹션 제목"
    },
    {
      "id": "propertyId",
      "label": "설정명",
      "type": "TEXT"
    }
  ],
  "property": {
    "propertyId": "기본값"
  }
}
```

---

## 2. 핵심 제약사항 (MUST)

### 2-1) ⛔ 중첩 설정 절대 금지

```json
// ❌ 절대 금지 - 저장 실패
{
  "id": "menuItems",
  "type": "LIST",
  "settings": [
    {
      "id": "subItems",
      "type": "LIST"  // 에러!
    }
  ]
}
```

**금지 대상:**
- LIST 안에 LIST
- LIST 안에 COLOR_SCHEME_LIST
- LIST 안에 ACCORDION
- LIST 안에 TAB

### 2-2) ⛔ LIST 필드 수 제한

**규칙:**
- LINK 필드: 최대 1-2개
- TEXT 필드: 최대 2-3개
- 총 필드: 최대 5개 권장

```json
// ✅ 올바른 예
{
  "type": "LIST",
  "settings": [
    {"id": "name", "type": "TEXT"},
    {"id": "link", "type": "LINK"}
  ]
}

// ❌ 위험한 예 - 저장 실패 가능
{
  "type": "LIST",
  "settings": [
    {"id": "name", "type": "TEXT"},
    {"id": "link1", "type": "LINK"},
    {"id": "link2", "type": "LINK"},
    {"id": "link3", "type": "LINK"},
    {"id": "link4", "type": "LINK"}  // 과다!
  ]
}
```

### 2-3) ⛔ Reserved ID 금지

```
❌ 사용 금지: menus, products, collections, pages
```

### 2-4) ✅ ID 명명 규칙

```
✅ camelCase만 사용: logoImage, menuItems
❌ snake_case 금지: logo_image, menu_items
❌ 언더스코어 금지
```

### 2-5) ⛔ SECTION/DIVIDER 타입 금지

```json
// ❌ 지원하지 않음 - 저장 실패
{"type": "SECTION"}
{"type": "DIVIDER"}

// ✅ TITLE만 사용 (자동 구분선)
{"type": "TITLE", "content": "섹션명"}
```

### 2-6) ⛔ description 길이 제한

```json
// ❌ 100자 초과 - 저장 실패
{
  "description": "이 필드는 매우 중요한 설정입니다. 사용자가 이 옵션을 선택하면 다양한 효과가 적용되며, 특히 모바일 환경에서 더욱 유용하게 활용할 수 있습니다."
}

// ✅ 100자 이하
{
  "description": "중요 설정. 모바일에서 유용함"
}
```

---

## 3. Handlebars 문법 및 제약

### 3-1) 지원되는 헬퍼 ✅

**수학 연산:**
- `{{add}}` - 덧셈
- `{{sub}}` - 뺄셈

**조건문/논리:**
- `{{#if}}`, `{{#else}}`, `{{#unless}}`
- `{{#eq}}` - 같음 비교
- `{{#gt}}`, `{{#lt}}` - 크기 비교
- `{{#or}}`, `{{#and}}` - 논리 연산

**반복/유틸:**
- `{{#each}}`
- `{{size}}` - 배열 크기

### 3-2) 지원되지 않는 헬퍼 ❌

```handlebars
❌ {{div}} - 나눗셈
❌ {{mul}} - 곱셈
❌ {{mod}} - 나머지
❌ {{ne}} - 같지 않음
❌ {{gte}}, {{lte}} - 이상/이하
```

**사용 시 증상:** 블록 전체가 화면에서 사라짐

### 3-3) 회피 전략

**나눗셈 필요 시:**
```json
// Option 1: 별도 옵션 추가
{"id": "paddingFull"},
{"id": "paddingHalf"}

// Option 2: JavaScript 사용
```

**같지 않음(ne) 필요 시:**
```handlebars
❌ {{#if (ne value "something")}}

✅ {{#unless (eq value "something")}}
✅ {{#if value}}  // 빈 값은 자동 false
```

**곱셈 필요 시:**
```handlebars
❌ {{mul property.size 2}}

✅ {{add property.size property.size}}
```

### 3-4) 변수 출력

```handlebars
<!-- 일반 변수 -->
{{property.title}}

<!-- HTML 출력 (SVG, RICH_TEXT) -->
{{{property.iconSvg}}}
{{{property.richText}}}
```

### 3-5) 조건문

```handlebars
{{#if property.show}}
  표시
{{else}}
  숨김
{{/if}}

{{#unless property.hide}}
  표시
{{/unless}}
```

### 3-6) 반복문

```handlebars
{{#each property.items}}
  <div>
    {{title}}
    {{@index}}
    {{#if @first}}첫번째{{/if}}
    {{#if @last}}마지막{{/if}}
  </div>
{{/each}}
```

---

## 4. JSON 설정 규칙

### 4-1) 지원되는 타입

| 타입 | 용도 | 필수 속성 |
|------|------|----------|
| TITLE | 섹션 제목 | content |
| DESCRIPTION | 설명 (지원 불확실) | content |
| TEXT | 텍스트 입력 | id, label, type |
| TEXTAREA | 여러 줄 텍스트 | id, label, type |
| IMAGE_PICKER | 이미지 선택 | id, label, type |
| COLOR_PICKER | 색상 선택 | id, label, type |
| RANGE | 범위 슬라이더 | id, label, type, min, max |
| RADIO | 라디오 버튼 | id, label, type, options |
| CHECKBOX | 체크박스 | id, label, type |
| LIST | 리스트 | id, label, type, settings |
| LINK | 링크 선택 | id, label, type |

### 4-2) TITLE 사용법

```json
{
  "type": "TITLE",
  "content": "🎨 디자인 설정"
}
```

**규칙:**
- 2번째 TITLE부터 자동 구분선 생성
- 이모지 활용 권장 (🏢 🎬 📐 🎨 📱)

### 4-3) 설정 옵션 속성

```json
{
  "id": "propertyId",           // 필수
  "label": "설정명",             // 필수
  "description": "설명 (100자)",  // 선택
  "type": "TEXT",                // 필수
  "placeholder": "예시",         // 선택
  "isVisible": "조건식",         // 선택
  "default": "기본값"            // 선택 (권장)
}
```

### 4-4) isVisible 조건식

```json
// 일반 조건
{
  "isVisible": "property.showOption === true"
}

// LIST 내부 조건
{
  "isVisible": "property.items[index].enabled === true"
}

// 복합 조건
{
  "isVisible": "property.type === 'custom' && property.enabled === true"
}
```

### 4-5) RANGE 옵션

```json
{
  "type": "RANGE",
  "min": 0,
  "max": 100,
  "step": 5,        // 조절 단위
  "unit": "px"      // 표시 단위
}
```

### 4-6) RADIO 옵션

```json
{
  "type": "RADIO",
  "options": [
    {"label": "왼쪽", "value": "left"},
    {"label": "가운데", "value": "center"}
  ]
}
```

### 4-7) LIST 옵션

```json
{
  "type": "LIST",
  "maxCount": 10,
  "settings": [
    {"id": "name", "type": "TEXT"},
    {"id": "link", "type": "LINK"}
  ]
}
```

---

## 5. CSS 작성 규칙

### 5-1) Handlebars 변수 사용

```css
.block {
  background-color: {{property.backgroundColor}};
  padding: {{property.padding}}px;
  font-size: {{property.fontSize}}px;
}
```

### 5-2) 조건부 CSS

```css
{{#if property.showBorder}}
.block {
  border: 1px solid {{property.borderColor}};
}
{{/if}}
```

### 5-3) 테마 CSS 변수 (필수)

```css
/* 제목 */
.heading {
  font-family: var(--font-family-heading);
  font-weight: var(--font-weight-heading);
}

/* 본문 */
.body-text {
  font-family: var(--font-family-body);
  font-weight: var(--font-weight-body);
}

/* 색상 */
.block {
  background-color: var(--color-background-100);
  color: var(--color-text-100);
  border-color: var(--color-border-100);
}

.accent {
  color: var(--color-accent-100);
}
```

### 5-4) 반응형 규칙

```css
/* 데스크톱 */
.block {
  padding: {{property.paddingDesktop}}px;
}

/* 모바일 */
@media (max-width: 768px) {
  .block {
    padding: {{property.paddingMobile}}px;
  }
}
```

### 5-5) ⛔ SVG 속성을 CSS로 조작 금지

```css
❌ 절대 금지:
.icon svg line {
  x1: {{value}};
  y1: {{value}};
}

✅ HTML 속성으로만 설정
```

### 5-6) 애니메이션 성능 최적화

```css
/* GPU 가속 */
.animated {
  transform: translateZ(0);
  backface-visibility: hidden;
}

/* 부드러운 전환 (0.3s 권장) */
.element {
  transition: transform 0.3s ease, opacity 0.3s ease;
}

/* ⚠️ will-change 최소 사용 */
.slider-track {
  will-change: transform;  // 꼭 필요한 경우만
}
```

---

## 6. JavaScript API 규칙

### 6-1) bm 객체 필수 메서드

```javascript
const container = bm.container;  // 블록 컨테이너
const context = bm.context;      // 설정 값

// ✅ 필수: 설정 변경 시 재실행
bm.onContextChange = () => {
  init();
};

// ✅ 필수: 이벤트 리스너 등록 시 cleanup
bm.onDestroy = () => {
  window.removeEventListener('scroll', handleScroll);
};
```

### 6-2) ⛔ 이벤트 위임 필수

```javascript
// ❌ 잘못된 방법 - 재렌더링 시 무효화
const button = container.querySelector('.button');
button.addEventListener('click', () => {});

// ✅ 올바른 방법 - container에 위임
bm.container.addEventListener('click', (e) => {
  if (e.target.closest('.button')) {
    // 처리
  }
}, true);
```

### 6-3) bm.onContextChange 필수 구현

```javascript
function init() {
  const value = context.property.someValue;
  // 초기화 로직
}

init();

// ✅ 필수! 에디터에서 즉시 반영
bm.onContextChange = () => {
  init();
};
```

### 6-4) bm.onDestroy 필수 구현

```javascript
function handleScroll() {
  // 스크롤 처리
}

window.addEventListener('scroll', handleScroll);

// ✅ 필수! 메모리 누수 방지
bm.onDestroy = () => {
  window.removeEventListener('scroll', handleScroll);
  document.body.style.overflow = '';
};
```

### 6-5) ⛔ console.log 제거

```javascript
❌ 최종 코드에 절대 남기면 안 됨:
console.log('debug');
console.error('test');
```

---

## 7. 식스샵 프로 필수 요건

### 7-1) 편집 용이성

- [ ] 에디터에서 모든 기능이 오류 없이 작동
- [ ] 사용자가 직접 편집 가능한 요소만 포함
- [ ] 통이미지 사용 금지
- [ ] 코드 결과물 금지 (편집 불가능)
- [ ] 저작권 문제 없는 리소스만 사용

### 7-2) 콘텐츠 구성

- [ ] 내 페이지 3개 이상 구성
- [ ] 온전한 헤더/푸터 메뉴 구성
- [ ] 고품질 이미지 사용
- [ ] 의미 없는 더미 텍스트 없음
- [ ] 일관된 디자인 스타일

---

## 8. 커스텀 블록 필수 점검

### 8-1) 설정 패널

- [ ] 블록 이름 한글 작성
- [ ] 모든 문구 한글 작성
- [ ] TITLE로 섹션 그룹핑
- [ ] 외부 API 연동 방법 안내
- [ ] isVisible 적절히 사용
- [ ] 글자 크기 설정 제공

### 8-2) 블록 동작

- [ ] 블록 너비가 커스텀 섹션 뚫고 나가지 않음
- [ ] 설정 변경 시 즉시 반영 (bm.onContextChange)
- [ ] 데스크톱/모바일 모두 정상 작동
- [ ] 동일 블록 여러 개 독립 작동
- [ ] bm.onDestroy 구현 (필요 시)
- [ ] console.log 모두 삭제

### 8-3) 테마 설정 상속

- [ ] 제목글: font-family, font-weight 적용
- [ ] 본문: font-family, font-weight 적용
- [ ] COLOR_SCHEME CSS 변수 적용

### 8-4) 개별 설정

- [ ] RICH_TEXT: 굵게, 기울임꼴 작동
- [ ] RICH_TEXT: HTML 코드가 아닌 정상 글자로 표시
- [ ] RICH_TEXT: 불필요한 속성 숨김 (exclude)
- [ ] RANGE: unit, step 적절히 적용
- [ ] LINK: 새 탭 열기 함께 제공

---

## 9. 헤더/푸터 블록 기준

### 9-1) 헤더 필수 기능

- [ ] **빈 링크 클릭 방지**
```javascript
bm.container.addEventListener('click', (e) => {
  const link = e.target.closest('a');
  if (link) {
    const href = link.getAttribute('href');
    if (!href || href === '' || href === '#') {
      e.preventDefault();
      e.stopPropagation();
    }
  }
}, true);
```

- [ ] **글자 크기 조정** (모든 글자)
- [ ] **컨테이너 최대 너비** (데스크톱)
  - max-width on/off
  - max-width 값 조절 (px)
  - 좌우 padding 조절

### 9-2) 푸터 필수 정보

- [ ] 상호명
- [ ] 대표자명
- [ ] 사업자등록번호
- [ ] 통신판매업 신고번호
- [ ] 주소
- [ ] 대표 전화번호
- [ ] 이메일 주소

---

## 10. 사용자 요청 개발 원칙

### 10-1) 파일 분리 원칙

**규칙:** HTML과 JSON을 항상 분리하여 복붙 가능하게 작성

### 10-2) 에디터 구조화 원칙

**규칙:** TITLE로 논리적 섹션 구분

**권장 순서 (헤더 블록 예시):**
1. 헤더 기본 설정 (전체 여백, 배경, 높이)
2. 로고 옵션 (좌측)
3. 메뉴 옵션 (중앙)
4. 하위 메뉴 옵션
5. 유틸리티 메뉴 (우측)
6. 다국어 버튼 (우측 끝)

### 10-3) 비개발자를 위한 에디터 UX 원칙 ⭐

**목적:** 식스샵 프로 템플릿 납품용 블록 - 비개발자가 쉽게 사용 가능해야 함

**필수 규칙:**

**1. 존댓말 사용**
```json
// ✅ 올바른 예
{
  "label": "제목 텍스트",
  "description": "블록의 제목을 입력해주세요. 비워두시면 제목이 표시되지 않습니다."
}

// ❌ 잘못된 예
{
  "label": "제목",
  "description": "제목 입력"
}
```

**2. 친절하고 상세한 설명**
```json
// ✅ 올바른 예
{
  "label": "배경 색상",
  "description": "블록의 배경색을 설정해주세요. 밝은 색상을 선택하시면 텍스트는 어두운 색으로 자동 조정됩니다."
}

// ❌ 잘못된 예
{
  "label": "배경색",
  "description": "배경"
}
```

**3. 옵션명은 명확하게**
- 전문 용어보다 일상 용어 사용
- 예: "padding" → "여백", "justify-content" → "정렬"

**4. 선택사항 명시**
```json
{
  "label": "부제목 (선택사항)",
  "description": "부제목을 입력해주세요. 비워두셔도 됩니다."
}
```

**5. 기본값 동작 설명**
```json
{
  "label": "자동 재생",
  "description": "체크하시면 페이지 로드 시 자동으로 재생됩니다. 해제하시면 사용자가 직접 재생 버튼을 눌러야 합니다."
}
```

**라벨/설명 작성 체크리스트:**
- [ ] 존댓말 사용
- [ ] 구체적이고 친절한 설명 (무엇을, 어떻게, 효과)
- [ ] 선택사항 여부 명시
- [ ] 기본값 동작 설명
- [ ] 비개발자가 이해 가능한 용어

### 10-4) JSON-HTML 동기화 원칙 ⭐

**필수:** JSON에 옵션 추가 시 HTML에서 반드시 사용

```json
// 1. JSON에 옵션 추가
{
  "id": "mobileHeaderHeight",
  "type": "RANGE"
}
```

```css
/* 2. HTML에서 사용 - 필수! */
@media (max-width: 768px) {
  .header {
    height: {{property.mobileHeaderHeight}}px;
  }
}
```

**체크리스트:**
- [ ] CSS에서 property 사용
- [ ] 미디어쿼리 필요 시 @media 추가
- [ ] JavaScript 로직 필요 시 구현
- [ ] bm.onContextChange에서 동적 업데이트

### 10-5) 애니메이션 끊김 방지 원칙 ⭐

**무한 스크롤 슬라이드 구현 규칙:**

**❌ JavaScript 동적 복제 금지:**
```javascript
// 타이밍 이슈로 끊김 발생
const track = container.querySelector('.track');
// 동적 복제 시도...
```

**✅ 템플릿 기반 정적 복제:**
```html
<template>
  <div class="track">
    <!-- 6세트 복제 (4K 화면까지 커버) -->
    {{#each property.items}}...{{/each}}
    {{#each property.items}}...{{/each}}
    {{#each property.items}}...{{/each}}
    {{#each property.items}}...{{/each}}
    {{#each property.items}}...{{/each}}
    {{#each property.items}}...{{/each}}
  </div>
</template>
```

```css
@keyframes scroll-left {
  from { transform: translateX(0); }
  to { transform: translateX(-50%); }
}
```

**핵심 원칙:**
1. 템플릿에서 최소 6세트 복제
2. 정확히 -50% 이동
3. JavaScript 의존성 최소화

### 10-6) eq 헬퍼 의존성 제거 원칙

**문제:** CSS 변수 내 eq 헬퍼 사용 시 에러 가능

```css
❌ 잘못된 방법:
--justify: {{#if (eq property.align "left")}}flex-start{{else}}center{{/if}};

✅ 올바른 방법:
--justify: {{property.alignJustify}};
```

```json
// 별도 옵션 추가
{
  "id": "alignJustify",
  "type": "RADIO",
  "options": [
    {"label": "왼쪽", "value": "flex-start"},
    {"label": "가운데", "value": "center"}
  ]
}
```

### 10-7) 접근성 개선 원칙

```html
<!-- button 태그 사용 -->
<button type="button" tabindex="0" aria-label="설명">
  내용
</button>

<!-- focus 스타일 -->
<style>
.button:focus-visible {
  outline: 2px solid var(--color-accent-100);
  outline-offset: 2px;
}
</style>
```

### 10-8) 스크롤 인터렉션 원칙 ⭐

**목적:** 사용자 경험 극대화 - 화려한 스크롤 효과로 몰입도 향상

**적용 대상:**
- 히어로 섹션
- 제품/서비스 소개 블록
- 포트폴리오/갤러리 블록
- 타임라인/스토리텔링 블록

**구현 방법:**

**1. Intersection Observer 사용 (권장)**
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('animate-in');
    }
  });
}, {
  threshold: 0.2  // 20% 보이면 트리거
});

container.querySelectorAll('.animate-on-scroll').forEach(el => {
  observer.add(el);
});

// cleanup 필수
bm.onDestroy = () => {
  observer.disconnect();
};
```

**2. CSS 애니메이션 정의**
```css
/* 초기 상태 */
.animate-on-scroll {
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.6s ease-out;
}

/* 스크롤 인 */
.animate-on-scroll.animate-in {
  opacity: 1;
  transform: translateY(0);
}

/* 딜레이 적용 (순차 등장) */
.animate-on-scroll:nth-child(1) { transition-delay: 0s; }
.animate-on-scroll:nth-child(2) { transition-delay: 0.1s; }
.animate-on-scroll:nth-child(3) { transition-delay: 0.2s; }
```

**3. 다양한 애니메이션 효과**
```css
/* 페이드업 */
.fade-up {
  opacity: 0;
  transform: translateY(30px);
}
.fade-up.animate-in {
  opacity: 1;
  transform: translateY(0);
}

/* 페이드 좌/우 */
.fade-left {
  opacity: 0;
  transform: translateX(-30px);
}
.fade-left.animate-in {
  opacity: 1;
  transform: translateX(0);
}

/* 스케일 */
.scale-in {
  opacity: 0;
  transform: scale(0.9);
}
.scale-in.animate-in {
  opacity: 1;
  transform: scale(1);
}
```

**적용 시 주의사항:**
- [ ] 애니메이션 시간: 0.4~0.8s 권장
- [ ] threshold: 0.1~0.3 권장 (너무 늦지 않게)
- [ ] 모바일에서도 부드럽게 작동 확인
- [ ] bm.onDestroy로 observer cleanup
- [ ] 과도한 애니메이션 지양 (3~5개 요소까지)

### 10-9) 텍스트 크기 기본값 ⭐

**필수:** 모든 블록에서 아래 기본값 적용

**제목 텍스트:**
```json
{
  "id": "titleFontSizeDesktop",
  "label": "제목 크기 (데스크톱)",
  "type": "RANGE",
  "min": 24,
  "max": 72,
  "step": 2,
  "unit": "px",
  "default": 36
}
```
```json
{
  "id": "titleFontSizeMobile",
  "label": "제목 크기 (모바일)",
  "type": "RANGE",
  "min": 20,
  "max": 48,
  "step": 2,
  "unit": "px",
  "default": 24
}
```

**본문 텍스트:**
```json
{
  "id": "bodyFontSizeDesktop",
  "label": "본문 크기 (데스크톱)",
  "type": "RANGE",
  "min": 14,
  "max": 28,
  "step": 2,
  "unit": "px",
  "default": 20
}
```
```json
{
  "id": "bodyFontSizeMobile",
  "label": "본문 크기 (모바일)",
  "type": "RANGE",
  "min": 12,
  "max": 24,
  "step": 2,
  "unit": "px",
  "default": 16
}
```

**CSS 적용:**
```css
.title {
  font-size: {{property.titleFontSizeDesktop}}px;
}

.body-text {
  font-size: {{property.bodyFontSizeDesktop}}px;
}

@media (max-width: 768px) {
  .title {
    font-size: {{property.titleFontSizeMobile}}px;
  }

  .body-text {
    font-size: {{property.bodyFontSizeMobile}}px;
  }
}
```

**기본값 요약:**
| 텍스트 종류 | 데스크톱 | 모바일 |
|------------|---------|--------|
| 제목 | 36px | 24px |
| 본문 | 20px | 16px |

---

## 11. 최종 체크리스트

### 코드 품질
- [ ] console.log 모두 제거
- [ ] bm.onContextChange 구현
- [ ] bm.onDestroy 구현 (필요 시)
- [ ] 이벤트를 bm.container에 위임
- [ ] SVG는 3중 괄호로 렌더링

### 설정 스키마
- [ ] Reserved ID 미사용
- [ ] camelCase 명명
- [ ] LIST 중첩 없음
- [ ] LIST 내 LINK 필드 1-2개
- [ ] TITLE로 섹션 구분
- [ ] SECTION/DIVIDER 미사용
- [ ] description 100자 이하
- [ ] isVisible 적절히 사용
- [ ] 모든 문구 한글 작성

### Handlebars
- [ ] 지원되는 헬퍼만 사용
- [ ] div, mul, ne 등 미지원 헬퍼 회피
- [ ] eq 헬퍼 CSS 변수 내 사용 금지
- [ ] SVG 속성 CSS 조작 금지

### 기능 테스트
- [ ] 에디터 설정 변경 시 즉시 반영
- [ ] 데스크톱/모바일 정상 작동
- [ ] 동일 블록 여러 개 독립 작동
- [ ] 빈 링크 클릭 방지
- [ ] 커스텀 섹션 너비 준수

### JSON-HTML 동기화
- [ ] JSON 옵션이 HTML에서 사용됨
- [ ] CSS에 property 연동
- [ ] JavaScript 로직 구현
- [ ] bm.onContextChange 업데이트

### 애니메이션
- [ ] 무한 스크롤은 템플릿 6세트 복제
- [ ] -50% 이동으로 정확한 루프
- [ ] JavaScript 동적 복제 미사용
- [ ] 0.3s 전환 애니메이션

### 디자인 품질
- [ ] 테마 폰트 적용
- [ ] COLOR_SCHEME CSS 변수 사용
- [ ] 고품질 이미지 사용
- [ ] 일관된 스타일

### 파일 구조
- [ ] HTML/JSON 분리
- [ ] 복붙 가능하게 독립적 작성

### 비개발자 UX (템플릿 납품용)
- [ ] 모든 description 존댓말 사용
- [ ] 친절하고 상세한 설명 (무엇을, 어떻게, 효과)
- [ ] 선택사항 명시
- [ ] 기본값 동작 설명
- [ ] 전문 용어 대신 일상 용어 사용

### 스크롤 인터렉션 (적합한 블록)
- [ ] Intersection Observer 구현
- [ ] 애니메이션 시간 0.4~0.8s
- [ ] threshold 0.1~0.3 설정
- [ ] bm.onDestroy로 cleanup
- [ ] 모바일 작동 확인

### 텍스트 기본값
- [ ] 제목 데스크톱: 36px
- [ ] 제목 모바일: 24px
- [ ] 본문 데스크톱: 20px
- [ ] 본문 모바일: 16px

---

## 규칙 요약

### ⛔ 절대 금지
1. LIST 중첩
2. LIST 내 LINK 필드 5개 이상
3. Reserved ID 사용
4. SECTION/DIVIDER 타입
5. description 100자 초과
6. 미지원 Handlebars 헬퍼 (div, mul, ne 등)
7. SVG 속성 CSS 조작
8. 개별 요소 이벤트 바인딩
9. console.log 남기기
10. JavaScript 동적 복제 (무한 스크롤)
11. 반말 사용 (템플릿 납품용)

### ✅ 필수 준수
1. HTML/JSON 파일 분리
2. camelCase ID 명명
3. TITLE로 섹션 구분
4. bm.onContextChange 구현
5. bm.onDestroy 구현 (이벤트 리스너 시)
6. bm.container 이벤트 위임
7. 테마 CSS 변수 사용
8. 빈 링크 클릭 방지 (헤더/푸터)
9. JSON-HTML 동기화
10. 템플릿 기반 정적 복제 (무한 스크롤)
11. 존댓말 + 친절한 설명 (템플릿 납품용)
12. 텍스트 기본값 (제목 36/24px, 본문 20/16px)
13. 스크롤 인터렉션 (적합한 블록)

---

**문서 버전**: 2.1 (통합 + 템플릿 납품 기준)
**최종 업데이트**: 2025-01-13
**통합 세션**:
- 엔터프라이즈 헤더 (claude/sixshop-pro-web-builder-continue-011CV1vwWJU3kNEADVdqJbxY)
- 히어로 비디오 슬라이드 (커밋 4e1cc53)
- 로고 배너 슬라이드 (현재 세션)

**추가 기준 (v2.1)**:
- 비개발자를 위한 에디터 UX (존댓말, 친절한 설명)
- 스크롤 인터렉션 구현 가이드
- 텍스트 크기 기본값 (제목 36/24px, 본문 20/16px)
