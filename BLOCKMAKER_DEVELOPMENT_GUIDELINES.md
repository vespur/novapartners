# 식스샵 프로 블록메이커 개발 종합 가이드라인

## 📋 목차
1. [코드 작성 기본 원칙](#1-코드-작성-기본-원칙)
2. [에디터 설정 패널 설계](#2-에디터-설정-패널-설계)
3. [Handlebars 템플릿 문법](#3-handlebars-템플릿-문법)
4. [CSS 스타일링](#4-css-스타일링)
5. [JavaScript 동작 제어](#5-javascript-동작-제어)
6. [테마 설정 상속](#6-테마-설정-상속)
7. [Settings 타입별 상세 가이드](#7-settings-타입별-상세-가이드)
8. [품질 체크리스트](#8-품질-체크리스트)

---

## 1. 코드 작성 기본 원칙

### 1.1 표준 준수

#### ✅ 권장 사항
- Handlebars 기본 헬퍼만 사용 (`{{#if}}`, `{{#each}}`, `{{else}}`)
- CSS 변수는 따옴표 없이 값만 사용
- 시맨틱 HTML 사용 (`<button>`, `<nav>`, `<section>` 등)

#### ❌ 금지 사항
- **`eq` 헬퍼 사용 금지**: 등록되지 않은 커스텀 헬퍼
- CSS 변수 내 불필요한 따옴표 (`"center"` → `center`)
- `console.log()` 최종 코드에 남기기

### 1.2 구조

```html
<style>
/* CSS 변수 정의 및 스타일 */
</style>

<template>
<!-- Handlebars 템플릿 -->
</template>

<script>
/* JavaScript 로직 */
</script>
```

---

## 2. 에디터 설정 패널 설계

### 2.1 UX 원칙

#### 사용자 친화적인 설명
- **존댓말 사용**: "버튼의 배경색을 설정해주세요"
- **구체적 예시 제공**: "예: 64 (픽셀 단위)"
- **맥락 설명**: "체크 해제 시 모바일에서 숨겨집니다"

#### 배치 순서
화면에서 사용자가 보는 시각적 순서대로 배치:

1. **전역 설정** (섹션 전체 영향)
2. **좌측 상단** 요소
3. **우측 상단** 요소
4. **좌측 중앙** 요소
5. **우측 중앙** 요소
6. **좌측 하단** 요소
7. **우측 하단** 요소

### 2.2 그룹핑 및 구조화

#### TITLE을 활용한 구분

```json
{
  "type": "TITLE",
  "content": "🎨 색상 및 스타일"
}
```

**규칙:**
- 2개 이상의 TITLE 사용 시 자동 구분선 생성
- 이모지 활용으로 시각적 가독성 향상
- 관련 설정들을 논리적으로 그룹핑

#### DESCRIPTION 활용

```json
{
  "type": "DESCRIPTION",
  "content": "버튼의 색상, 크기, 호버 효과를 설정해주세요."
}
```

### 2.3 isVisible을 활용한 조건부 표시

#### 기본 사용법

```json
{
  "id": "style.hoverEffectType",
  "label": "호버 효과 타입",
  "type": "RADIO",
  "options": [
    {"label": "없음", "value": "none"},
    {"label": "글로우", "value": "glow"}
  ]
},
{
  "id": "style.glowColor",
  "label": "글로우 색상",
  "type": "COLOR_PICKER",
  "isVisible": "property.style.hoverEffectType === 'glow'"
}
```

#### LIST 내부에서 사용

```json
{
  "id": "slides",
  "type": "LIST",
  "settings": [
    {
      "id": "type",
      "type": "RADIO",
      "options": [
        {"label": "이미지", "value": "image"},
        {"label": "비디오", "value": "video"}
      ]
    },
    {
      "id": "videoUrl",
      "label": "비디오 URL",
      "type": "TEXT",
      "isVisible": "property.slides[index].type === 'video'"
    }
  ]
}
```

**주의:** LIST 내에서는 `[index]` 필수!

### 2.4 글자 크기 설정 제공

**원칙:**
- 글자로 된 요소는 **기본적으로 글자 크기 설정 제공**
- 글꼴/언어에 따라 시각적 크기 변화가 크기 때문

**예외:**
- 다각도 검토 결과 크기 조절이 불필요한 경우
- 고정 크기가 디자인 의도인 경우

```json
{
  "id": "style.headlineFontSizeDesktop",
  "label": "헤드라인 크기 (PC, px)",
  "description": "데스크탑에서 표시될 글자 크기를 설정해주세요. 예: 64",
  "type": "TEXT",
  "placeholder": "64"
},
{
  "id": "style.headlineFontSizeMobile",
  "label": "헤드라인 크기 (모바일, px)",
  "description": "모바일에서 표시될 글자 크기를 설정해주세요. 예: 32",
  "type": "TEXT",
  "placeholder": "32"
}
```

---

## 3. Handlebars 템플릿 문법

### 3.1 Property 접근

```handlebars
<!-- 기본 접근 -->
{{property.headline}}

<!-- 중첩 접근 -->
{{property.style.textColor}}

<!-- 부모 컨텍스트 접근 (each 내부) -->
{{../property.style.globalColor}}
```

### 3.2 조건문

```handlebars
<!-- if/else -->
{{#if property.showTitle}}
  <h1>{{property.title}}</h1>
{{else}}
  <p>제목 없음</p>
{{/if}}

<!-- 값 비교 (직접) -->
{{#if property.layout}}
  <div class="layout-{{property.layout}}">
{{/if}}
```

**❌ 금지:**
```handlebars
<!-- eq 헬퍼 사용 금지 -->
{{#if (eq property.layout "grid")}}
```

**✅ 해결 방법:**
Settings에서 직접 CSS 클래스나 값을 매핑

### 3.3 반복문

```handlebars
{{#each property.slides}}
  <div class="slide" data-index="{{@index}}">
    <img src="{{mediaUrl}}" alt="{{caption}}">
  </div>
{{else}}
  <p>슬라이드가 없습니다</p>
{{/each}}
```

**컨텍스트 변수:**
- `{{@index}}`: 현재 인덱스 (0부터 시작)
- `{{@first}}`: 첫 번째 아이템 여부
- `{{@last}}`: 마지막 아이템 여부

### 3.4 HTML Escaping

#### 일반 텍스트 (자동 escaping)
```handlebars
<p>{{property.description}}</p>
<!-- <script> → &lt;script&gt; -->
```

#### HTML 허용 (RICH_TEXT)
```handlebars
<div class="content">
  {{{property.richTextContent}}}
</div>
```

**주의:** 3중 괄호 `{{{ }}}` 사용 시 XSS 취약점 주의

---

## 4. CSS 스타일링

### 4.1 CSS 변수 정의

```css
.my-block {
  /* ✅ 올바른 사용 */
  --text-color: {{#if property.style.textColor}}{{property.style.textColor}}{{else}}#000000{{/if}};
  --font-size: {{#if property.style.fontSize}}{{property.style.fontSize}}{{else}}16{{/if}}px;

  /* ❌ 잘못된 사용 */
  --text-align: "center"; /* 따옴표 제거 */
  --layout: {{#if (eq property.layout "grid")}}grid{{else}}flex{{/if}}; /* eq 사용 금지 */
}
```

### 4.2 반응형 디자인

```css
/* 데스크탑 기본 */
.hero-content {
  font-size: var(--font-size-desktop);
  text-align: var(--text-align-desktop);
}

/* 모바일 */
@media(max-width: 768px) {
  .hero-content {
    font-size: var(--font-size-mobile);
    text-align: var(--text-align-mobile);
  }

  /* 조건부 숨김 */
  {{#if property.style.hideOnMobile}}
  .desktop-only {
    display: none;
  }
  {{/if}}
}
```

### 4.3 커스텀 섹션 너비 준수

**원칙:**
- 블록의 너비가 커스텀 섹션을 넘어가면 안 됨
- 섹션의 "그리드 맞추기", "안쪽 여백" 설정 준수

**예외:**
- `position: absolute/fixed/sticky` 블록은 오버플로우 허용

```css
/* ✅ 올바른 예시 */
.block-wrapper {
  max-width: 100%;
  padding: 0 var(--section-padding-x);
}

/* ⚠️ 주의가 필요한 예시 */
.fixed-header {
  position: fixed;
  left: 0;
  right: 0;
  /* fixed는 섹션 영역 벗어남 OK */
}
```

---

## 5. JavaScript 동작 제어

### 5.1 기본 구조

```javascript
(function(){
  const root = bm.container;  // 블록 루트 요소
  const ctx = bm.context;     // 컨텍스트 (property 포함)

  function init() {
    // 초기화 로직
    const config = ctx.property.style || {};

    // DOM 조작, 이벤트 리스너 등
  }

  init();

  // 설정 변경 시 재초기화
  bm.onContextChange = () => {
    init();
  };

  // 블록 제거 시 정리
  bm.onDestroy = () => {
    // 타이머, 이벤트 리스너 정리
  };
})();
```

### 5.2 bm.onContextChange 필수 사용

**언제 필요한가?**
- `bm.context.property`를 JavaScript에서 사용하는 경우
- 에디터에서 설정값 변경 시 즉시 반영되어야 하는 경우

**예시:**

```javascript
function initCarousel() {
  const container = bm.container;
  const ctx = bm.context;

  const glideId = '[data-block-id="' + ctx.id + '"]';

  var glide = new Glide(glideId, {
    autoplay: ctx.property.autoplayInterval,      // property 사용
    animationDuration: ctx.property.animationDuration,
    perView: 1
  });

  glide.mount();
}

initCarousel();

// ✅ 필수: 설정 변경 시 재실행
bm.onContextChange = () => {
  initCarousel();
};
```

### 5.3 bm.onDestroy 사용

**언제 필요한가?**
- 타이머 사용 시 (`setTimeout`, `setInterval`)
- 애니메이션 프레임 사용 시 (`requestAnimationFrame`)
- 외부 이벤트 리스너 등록 시
- 외부 라이브러리 인스턴스 생성 시

```javascript
let timer = null;
let rafId = null;

function init() {
  timer = setInterval(() => {
    // 주기적 작업
  }, 1000);

  function animate() {
    // 애니메이션
    rafId = requestAnimationFrame(animate);
  }
  animate();
}

init();

bm.onDestroy = () => {
  // ✅ 필수: 메모리 누수 방지
  clearInterval(timer);
  cancelAnimationFrame(rafId);
};
```

### 5.4 블록 독립성 보장

**문제 상황:**
같은 페이지에 동일 블록 여러 개 → 하나 수정 시 전부 영향

**해결 방법:**
- 전역 변수 사용 금지
- IIFE로 스코프 격리
- `bm.container` 기준으로 DOM 탐색

```javascript
// ❌ 잘못된 예시
var slides = document.querySelectorAll('.slide'); // 전체 페이지 대상

// ✅ 올바른 예시
(function(){
  const root = bm.container;
  const slides = root.querySelectorAll('.slide'); // 현재 블록만
})();
```

### 5.5 데스크탑/모바일 양쪽 테스트

**체크 포인트:**
- [ ] 데스크탑 뷰포트에서 정상 작동
- [ ] 모바일 뷰포트에서 정상 작동
- [ ] 뷰포트 크기 변경 시 반응형 동작
- [ ] 터치 이벤트 지원 (모바일)

```javascript
// 반응형 대응 예시
let isMobile = window.innerWidth <= 768;

window.addEventListener('resize', () => {
  isMobile = window.innerWidth <= 768;
  // 모바일/데스크탑 분기 처리
});
```

### 5.6 console.log 제거

**출시 전 필수:**
- 모든 `console.log()`, `console.error()` 제거
- 디버깅 코드 정리
- 주석 처리된 테스트 코드 삭제

```javascript
// ❌ 최종 코드에 남기면 안 됨
console.log('블록 초기화');
console.error('에러 발생');

// ✅ 필요 시 조건부 로깅
if (window.DEBUG_MODE) {
  console.log('디버그 정보');
}
```

---

## 6. 테마 설정 상속

### 6.1 글꼴(Font Family) 상속

#### 제목글 (Heading)

```css
h1, h2, h3, h4, h5, h6,
.heading-text {
  font-family: var(--font-heading);
  font-weight: var(--font-weight-heading);
}
```

#### 줄글 (Body Text) 및 버튼

```css
p, span, div,
button, a {
  font-family: var(--font-body);
  font-weight: var(--font-weight-body);
}
```

**예외:**
- 블록에서 자체적으로 글꼴/굵기 설정을 제공하는 경우

### 6.2 색상 구성(COLOR_SCHEME) 상속

```css
.my-block {
  /* 배경색 */
  background-color: var(--color-scheme-background);

  /* 텍스트 색 */
  color: var(--color-scheme-text);

  /* 강조 색 */
  border-color: var(--color-scheme-accent);

  /* 보조 색 */
  .secondary {
    color: var(--color-scheme-secondary);
  }
}
```

**Settings에서 COLOR_SCHEME 사용:**

```json
{
  "id": "style.colorScheme",
  "label": "색상 구성",
  "type": "COLOR_SCHEME"
}
```

---

## 7. Settings 타입별 상세 가이드

### 7.1 RICH_TEXT

#### 기본 설정

```json
{
  "id": "description",
  "label": "설명 텍스트",
  "type": "RICH_TEXT",
  "exclude": ["heading", "link", "image"]
}
```

#### 굵게/기울임꼴 지원

**Template:**
```handlebars
<div class="rich-content">
  {{{property.description}}}
</div>
```

**CSS:**
```css
.rich-content strong,
.rich-content b {
  font-weight: 700; /* 굵게 */
}

.rich-content em,
.rich-content i {
  font-style: italic; /* 기울임꼴 */
}
```

#### exclude 옵션

불필요한 서식 도구 숨기기:

```json
{
  "exclude": [
    "heading",      // 제목 서식
    "bold",         // 굵게
    "italic",       // 기울임
    "underline",    // 밑줄
    "strikethrough",// 취소선
    "link",         // 링크
    "image",        // 이미지
    "list",         // 목록
    "alignment"     // 정렬
  ]
}
```

### 7.2 RANGE

```json
{
  "id": "style.opacity",
  "label": "투명도",
  "type": "RANGE",
  "min": 0,
  "max": 1,
  "step": 0.1,        // 0.1 단위로 조절
  "unit": "",         // 단위 없음
  "defaultValue": 1
},
{
  "id": "style.spacing",
  "label": "간격",
  "type": "RANGE",
  "min": 0,
  "max": 100,
  "step": 4,          // 4px 단위
  "unit": "px",
  "defaultValue": 16
}
```

**step 소수점 지원:**
- `0.01`: 정밀 조절
- `0.1`: 일반적
- `1`: 정수만

### 7.3 LINK

```json
{
  "id": "ctaPrimary.link",
  "label": "CTA 버튼 링크",
  "type": "LINK"
},
{
  "id": "ctaPrimary.openInNewTab",
  "label": "새 탭으로 열기",
  "type": "CHECKBOX"
}
```

**Template:**
```handlebars
<a href="{{property.ctaPrimary.link.value}}"
   {{#if property.ctaPrimary.openInNewTab}}
   target="_blank"
   rel="noopener noreferrer"
   {{/if}}>
  {{property.ctaPrimary.label}}
</a>
```

**중요:** LINK 제공 시 "새 탭 열기" 옵션 함께 제공!

### 7.4 LIST

```json
{
  "id": "slides",
  "label": "슬라이드 목록",
  "type": "LIST",
  "maxCount": 10,
  "settings": [
    {
      "id": "title",
      "label": "제목",
      "type": "TEXT"
    },
    {
      "id": "imageUrl",
      "label": "이미지 URL",
      "type": "IMAGE"
    }
  ]
}
```

### 7.5 기타 타입

```json
{
  "type": "TEXT",           // 한 줄 텍스트
  "type": "TEXTAREA",       // 여러 줄 텍스트
  "type": "COLOR_PICKER",   // 색상 선택
  "type": "IMAGE",          // 이미지 업로드
  "type": "CHECKBOX",       // 체크박스
  "type": "RADIO",          // 라디오 버튼
  "type": "MENU"            // 메뉴 선택
}
```

---

## 8. 품질 체크리스트

### 8.1 코드 품질

- [ ] `eq` 헬퍼 사용하지 않음
- [ ] CSS 변수에 불필요한 따옴표 없음
- [ ] Handlebars 문법 오류 없음
- [ ] `console.log` 모두 제거
- [ ] IIFE로 스코프 격리
- [ ] 전역 변수 사용 안 함

### 8.2 동작 검증

- [ ] 에디터에서 설정 변경 시 즉시 반영
- [ ] 데스크탑 뷰포트 정상 작동
- [ ] 모바일 뷰포트 정상 작동
- [ ] 동일 블록 여러 개 독립 작동
- [ ] 커스텀 섹션 너비 준수 (position 제외)

### 8.3 JavaScript

- [ ] `bm.container` 사용하여 DOM 탐색
- [ ] `bm.context.property` 사용 시 `onContextChange` 구현
- [ ] 타이머/RAF 사용 시 `onDestroy`에서 정리
- [ ] 이벤트 리스너 정리 로직

### 8.4 설정 패널 UX

- [ ] 관련 설정 TITLE로 그룹핑
- [ ] 존댓말 사용한 친절한 설명
- [ ] 화면 위치 순서대로 배치
- [ ] `isVisible` 적절히 활용
- [ ] 글자 요소에 크기 설정 제공

### 8.5 테마 상속

- [ ] 제목글 font-family/weight 상속
- [ ] 줄글/버튼 font-family/weight 상속
- [ ] COLOR_SCHEME CSS 변수 적용

### 8.6 Settings 타입 활용

- [ ] RICH_TEXT에 bold/italic 스타일 적용
- [ ] RICH_TEXT에 3중 괄호 사용
- [ ] RICH_TEXT exclude 설정
- [ ] RANGE에 unit/step 설정
- [ ] LINK와 함께 "새 탭 열기" 제공
- [ ] 적절한 Settings 타입 선택 (TEXT 대신 LINK 등)

### 8.7 접근성

- [ ] 시맨틱 HTML 사용
- [ ] `aria-label` 제공
- [ ] `role` 속성 추가
- [ ] 키보드 네비게이션 지원 (`tabindex`)
- [ ] `focus-visible` 스타일

---

## 9. 예시 코드 템플릿

### 완전한 블록 구조

```html
<style>
.my-block {
  /* CSS 변수 */
  --text-color: {{#if property.style.textColor}}{{property.style.textColor}}{{else}}#000000{{/if}};
  --font-size-desktop: {{#if property.style.fontSizeDesktop}}{{property.style.fontSizeDesktop}}{{else}}16{{/if}}px;
  --font-size-mobile: {{#if property.style.fontSizeMobile}}{{property.style.fontSizeMobile}}{{else}}14{{/if}}px;

  color: var(--text-color);
  font-size: var(--font-size-desktop);
  font-family: var(--font-body);
}

@media(max-width: 768px) {
  .my-block {
    font-size: var(--font-size-mobile);
  }
}
</style>

<template>
<section class="my-block" role="region" aria-label="{{property.ariaLabel}}">
  {{#if property.showTitle}}
    <h2>{{property.title}}</h2>
  {{/if}}

  <div class="content">
    {{{property.richContent}}}
  </div>

  {{#each property.items}}
    <div class="item" data-index="{{@index}}">
      <img src="{{imageUrl}}" alt="{{title}}">
      <a href="{{../property.ctaLink.value}}"
         {{#if ../property.ctaOpenNewTab}}target="_blank" rel="noopener"{{/if}}>
        {{title}}
      </a>
    </div>
  {{/each}}
</section>
</template>

<script>
(function(){
  const root = bm.container;
  const ctx = bm.context;

  let timer = null;

  function init() {
    // 초기화 로직
    const config = ctx.property.style || {};

    // 기존 타이머 정리
    if (timer) clearTimeout(timer);

    // 이벤트 리스너 등록
    const items = root.querySelectorAll('.item');
    items.forEach(item => {
      item.addEventListener('click', handleClick);
    });
  }

  function handleClick(e) {
    // 클릭 핸들러
  }

  init();

  bm.onContextChange = () => {
    init();
  };

  bm.onDestroy = () => {
    clearTimeout(timer);
    // 이벤트 리스너 정리 등
  };
})();
</script>
```

---

## 10. 참고 자료

### 공식 문서
- [식스샵 프로 블록메이커 안내서](https://help.pro.sixshop.com/design/block-maker)
- [Handlebars.js 공식 문서](https://handlebarsjs.com/)

### 내부 문서
- `HERO_CAROUSEL_FIX_SUMMARY.md`: 실제 디버깅 사례
- `hero-carousel-block-fixed.html`: 표준 준수 예시 코드

---

## 📝 버전 히스토리

### v1.0 (2025-11-12)
- 초기 가이드라인 작성
- 코드 작성 원칙, 에디터 UX, 테마 상속 통합
- 품질 체크리스트 추가

---

## ✅ 마지막 점검

블록을 출시하기 전, 이 가이드라인의 모든 항목을 체크하세요!

**핵심 3가지:**
1. ✅ `eq` 헬퍼 사용하지 않았는가?
2. ✅ `bm.onContextChange` 구현했는가?
3. ✅ 데스크탑/모바일 양쪽에서 테스트했는가?
