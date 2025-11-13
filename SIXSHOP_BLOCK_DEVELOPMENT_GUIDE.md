# 식스샵 블록메이커 개발 가이드

> 식스샵 커스텀 블록 개발을 위한 완벽한 가이드

## 목차

1. [파일 구조](#파일-구조)
2. [템플릿 문법](#템플릿-문법)
3. [에디터 설정](#에디터-설정)
4. [스크립트 API](#스크립트-api)
5. [스타일 가이드](#스타일-가이드)
6. [애니메이션 구현](#애니메이션-구현)
7. [성능 최적화](#성능-최적화)
8. [트러블슈팅](#트러블슈팅)
9. [베스트 프랙티스](#베스트-프랙티스)

---

## 파일 구조

식스샵 블록은 **HTML 파일**과 **JSON 파일**로 분리하여 개발합니다.

```
block-name.html        # 템플릿 + 스타일 + 스크립트
block-name.json        # 에디터 설정 + 기본값
```

### HTML 파일 구조

```html
<style>
  /* CSS 스타일 */
  .block-container {
    /* 스타일 정의 */
  }
</style>

<template>
  <div class="block-container">
    <!-- HTML 템플릿 -->
    {{#each property.items}}
      <div>{{title}}</div>
    {{/each}}
  </div>
</template>

<script>
  // JavaScript 로직
  const container = bm.container;
  const context = bm.context;

  // 초기화 코드
</script>
```

### JSON 파일 구조

```json
{
  "settings": [
    {
      "type": "TITLE",
      "content": "섹션 제목"
    },
    {
      "id": "propertyId",
      "label": "설정 레이블",
      "type": "TEXT",
      "description": "설명"
    }
  ],
  "property": {
    "propertyId": "기본값"
  }
}
```

---

## 템플릿 문법

식스샵 블록메이커는 **Handlebars** 템플릿 엔진을 사용합니다.

### 변수 출력

```handlebars
{{property.title}}
{{property.backgroundColor}}
{{property.fontSize}}
```

### 조건문

```handlebars
{{#if property.showTitle}}
  <h2>{{property.title}}</h2>
{{/if}}

{{#unless property.hideDescription}}
  <p>{{property.description}}</p>
{{/unless}}
```

### 반복문

```handlebars
{{#each property.items}}
  <div class="item">
    <h3>{{title}}</h3>
    <p>{{description}}</p>
    <img src="{{image}}" alt="{{name}}">
  </div>
{{/each}}
```

### 인덱스 사용

```handlebars
{{#each property.items}}
  <div class="item-{{@index}}">
    {{#if @first}}First Item{{/if}}
    {{#if @last}}Last Item{{/if}}
    Index: {{@index}}
  </div>
{{/each}}
```

---

## 에디터 설정

### 지원하는 타입

#### ⚠️ 중요: SECTION 타입은 지원하지 않음!

식스샵은 `SECTION` 타입을 지원하지 않습니다. 대신 `TITLE`과 `DESCRIPTION`을 사용하여 그룹화합니다.

#### TITLE (섹션 제목)

```json
{
  "type": "TITLE",
  "content": "🎨 디자인 설정"
}
```

#### DESCRIPTION (설명)

```json
{
  "type": "DESCRIPTION",
  "content": "디자인 관련 설정을 조정할 수 있습니다."
}
```

#### TEXT (텍스트 입력)

```json
{
  "id": "title",
  "label": "제목",
  "description": "블록의 제목을 입력하세요",
  "type": "TEXT",
  "placeholder": "예: 메인 타이틀"
}
```

#### IMAGE_PICKER (이미지 선택)

```json
{
  "id": "image",
  "label": "이미지",
  "description": "배경 이미지를 선택하세요",
  "type": "IMAGE_PICKER"
}
```

#### COLOR_PICKER (색상 선택)

```json
{
  "id": "backgroundColor",
  "label": "배경 색상",
  "description": "배경색을 선택하세요",
  "type": "COLOR_PICKER"
}
```

#### RANGE (범위 슬라이더)

```json
{
  "id": "fontSize",
  "label": "폰트 크기",
  "description": "텍스트 크기를 조절하세요",
  "type": "RANGE",
  "min": 12,
  "max": 48,
  "step": 2,
  "unit": "px"
}
```

#### RADIO (라디오 버튼)

```json
{
  "id": "alignment",
  "label": "정렬",
  "description": "텍스트 정렬 방향을 선택하세요",
  "type": "RADIO",
  "options": [
    {
      "label": "왼쪽",
      "value": "left"
    },
    {
      "label": "중앙",
      "value": "center"
    },
    {
      "label": "오른쪽",
      "value": "right"
    }
  ]
}
```

#### LIST (리스트)

```json
{
  "id": "items",
  "label": "아이템 목록",
  "description": "아이템을 추가하세요",
  "type": "LIST",
  "maxCount": 10,
  "settings": [
    {
      "id": "title",
      "label": "제목",
      "type": "TEXT"
    },
    {
      "id": "image",
      "label": "이미지",
      "type": "IMAGE_PICKER"
    }
  ]
}
```

### 에디터 구조화 예시

```json
{
  "settings": [
    {
      "type": "TITLE",
      "content": "🏢 콘텐츠 관리"
    },
    {
      "type": "DESCRIPTION",
      "content": "표시할 콘텐츠를 추가하고 관리합니다."
    },
    {
      "id": "items",
      "label": "아이템 목록",
      "type": "LIST",
      "maxCount": 20,
      "settings": [...]
    },

    {
      "type": "TITLE",
      "content": "🎨 디자인 설정"
    },
    {
      "id": "backgroundColor",
      "label": "배경 색상",
      "type": "COLOR_PICKER"
    },

    {
      "type": "TITLE",
      "content": "📱 반응형 설정"
    },
    {
      "id": "mobilePadding",
      "label": "모바일 여백",
      "type": "RANGE",
      "min": 10,
      "max": 60,
      "step": 5,
      "unit": "px"
    }
  ],
  "property": {
    "items": [],
    "backgroundColor": "#FFFFFFFF",
    "mobilePadding": 20
  }
}
```

---

## 스크립트 API

### 기본 API

#### bm.container

현재 블록의 DOM 컨테이너를 반환합니다.

```javascript
const container = bm.container;
const element = container.querySelector('.my-element');
```

#### bm.context

현재 블록의 컨텍스트(설정값)를 반환합니다.

```javascript
const context = bm.context;
const title = context.property.title;
const items = context.property.items;
```

#### bm.onContextChange

설정값이 변경될 때 호출되는 콜백 함수입니다.

```javascript
bm.onContextChange = () => {
  // 설정 변경 시 실행할 코드
  updateDisplay();
};
```

### 실전 예시

```javascript
const container = bm.container;
const context = bm.context;

function initializeBlock() {
  const items = context.property.items;

  // 초기화 로직
  items.forEach((item, index) => {
    console.log(`Item ${index}: ${item.title}`);
  });
}

// 초기 실행
initializeBlock();

// 설정 변경 시 재실행
bm.onContextChange = () => {
  initializeBlock();
};
```

---

## 스타일 가이드

### CSS 변수 사용

템플릿 변수를 CSS에서 직접 사용할 수 있습니다.

```css
.block-container {
  background-color: {{property.backgroundColor}};
  padding: {{property.padding}}px;
  font-size: {{property.fontSize}}px;
}

.title {
  color: {{property.titleColor}};
  text-align: {{property.alignment}};
}
```

### 반응형 디자인

```css
/* 데스크톱 */
.block-container {
  padding: {{property.padding}}px;
}

/* 태블릿 */
@media (max-width: 1024px) {
  .block-container {
    padding: {{property.tabletPadding}}px;
  }
}

/* 모바일 */
@media (max-width: 768px) {
  .block-container {
    padding: {{property.mobilePadding}}px;
  }
}
```

### 이모지를 활용한 시각적 구분

설정 제목에 이모지를 사용하여 가독성을 높입니다.

```json
{
  "type": "TITLE",
  "content": "🏢 콘텐츠 관리"
},
{
  "type": "TITLE",
  "content": "🎬 애니메이션 설정"
},
{
  "type": "TITLE",
  "content": "🎨 디자인 설정"
},
{
  "type": "TITLE",
  "content": "📱 반응형 설정"
}
```

---

## 애니메이션 구현

### CSS 애니메이션 기본

```css
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.item {
  animation: fadeIn 0.5s ease-out;
}
```

### 무한 스크롤 슬라이드 애니메이션

**⚠️ 중요: JavaScript 동적 복제는 타이밍 이슈로 끊김 발생!**

**해결책: 템플릿에서 충분히 복제하기**

```html
<style>
  .slider-track {
    display: flex;
    gap: 20px;
    animation: scroll-left 30s linear infinite;
  }

  @keyframes scroll-left {
    from {
      transform: translateX(0);
    }
    to {
      transform: translateX(-50%);
    }
  }
</style>

<template>
  <div class="slider">
    <div class="slider-track">
      <!-- 충분한 세트를 복제 (최소 4~6세트) -->
      {{#each property.items}}
        <div class="item">{{title}}</div>
      {{/each}}

      {{#each property.items}}
        <div class="item">{{title}}</div>
      {{/each}}

      {{#each property.items}}
        <div class="item">{{title}}</div>
      {{/each}}

      {{#each property.items}}
        <div class="item">{{title}}</div>
      {{/each}}

      {{#each property.items}}
        <div class="item">{{title}}</div>
      {{/each}}

      {{#each property.items}}
        <div class="item">{{title}}</div>
      {{/each}}
    </div>
  </div>
</template>
```

### 애니메이션 끊김 방지 원칙

1. **템플릿 기반 복제**: JavaScript 복제 대신 템플릿에서 직접 복제
2. **충분한 복제**: 화면 너비의 3배 이상을 커버하도록 4~6세트 복제
3. **50% 이동**: 전체의 50%만 이동하여 정확한 루프 구현
4. **순수 CSS**: JavaScript 의존성 최소화

```
[1세트][2세트][3세트][4세트][5세트][6세트]
↑ 시작

-50% 이동 (3세트 길이)
              ↑ 종료 = [4세트] 시작 = [1세트] 복사본

→ 완벽한 무한 루프!
```

---

## 성능 최적화

### GPU 가속 활성화

```css
.animated-element {
  transform: translateZ(0);
  backface-visibility: hidden;
  will-change: transform; /* 필요한 경우에만 사용 */
}
```

### 이미지 최적화

```html
<img
  src="{{image}}"
  alt="{{name}}"
  loading="lazy"
  onerror="this.src='https://fallback-image-url.jpg'"
>
```

### 애니메이션 최적화

```css
/* transform과 opacity만 애니메이션 (GPU 가속) */
.item {
  transition: transform 0.3s ease, opacity 0.3s ease;
}

/* width, height, margin 등은 피하기 (리플로우 발생) */
```

### will-change 사용 주의

```css
/* ❌ 나쁜 예: 모든 요소에 적용 */
* {
  will-change: transform, opacity;
}

/* ✅ 좋은 예: 필요한 요소에만 적용 */
.slider-track {
  will-change: transform;
}

/* 또는 애니메이션이 필요 없을 때는 사용하지 않기 */
```

---

## 트러블슈팅

### 문제 1: "지원하지 않는 settingType입니다 (Input: SECTION)"

**원인**: 식스샵은 `SECTION` 타입을 지원하지 않습니다.

**해결책**: `TITLE`과 `DESCRIPTION`을 사용하세요.

```json
// ❌ 잘못된 예
{
  "type": "SECTION",
  "label": "디자인 설정",
  "settings": [...]
}

// ✅ 올바른 예
{
  "type": "TITLE",
  "content": "🎨 디자인 설정"
},
{
  "type": "DESCRIPTION",
  "content": "디자인 관련 설정을 조정할 수 있습니다."
},
{
  "id": "backgroundColor",
  "label": "배경 색상",
  "type": "COLOR_PICKER"
}
```

### 문제 2: 슬라이드 애니메이션이 끊김

**원인**: JavaScript 동적 복제가 CSS 애니메이션보다 늦게 실행되어 타이밍 이슈 발생

**해결책**: 템플릿에서 충분히 복제 (4~6세트)

```html
<!-- ❌ JavaScript 동적 복제 (끊김 발생) -->
<template>
  <div class="track">
    {{#each property.items}}
      <div>{{title}}</div>
    {{/each}}
  </div>
</template>
<script>
  // 동적으로 복제 시도 → 타이밍 이슈!
  const track = container.querySelector('.track');
  // ... 복제 로직
</script>

<!-- ✅ 템플릿 기반 정적 복제 (완벽) -->
<template>
  <div class="track">
    {{#each property.items}}...{{/each}}
    {{#each property.items}}...{{/each}}
    {{#each property.items}}...{{/each}}
    {{#each property.items}}...{{/each}}
    {{#each property.items}}...{{/each}}
    {{#each property.items}}...{{/each}}
  </div>
</template>
```

### 문제 3: 이미지가 표시되지 않음

**원인**: 이미지 로드 실패

**해결책**: onerror 속성으로 폴백 이미지 제공

```html
<img
  src="{{image}}"
  alt="{{name}}"
  onerror="this.src='https://ss3-prod-static-files.s3.ap-northeast-2.amazonaws.com/block-image-library/lifestyle/image1.jpg'"
>
```

### 문제 4: 설정 변경이 반영되지 않음

**원인**: `bm.onContextChange` 미구현

**해결책**: 컨텍스트 변경 리스너 추가

```javascript
bm.onContextChange = () => {
  // 설정 변경 시 실행할 코드
  updateDisplay();
};
```

---

## 베스트 프랙티스

### 1. 파일 분리

HTML과 JSON을 명확하게 분리하여 복붙 가능하도록 합니다.

```
block-name.html    # 템플릿 + 스타일 + 스크립트
block-name.json    # 설정 + 기본값
```

### 2. 에디터 구조화

TITLE로 논리적 섹션을 구분합니다.

```json
{
  "settings": [
    {"type": "TITLE", "content": "🏢 콘텐츠"},
    // 콘텐츠 설정들...

    {"type": "TITLE", "content": "🎨 디자인"},
    // 디자인 설정들...

    {"type": "TITLE", "content": "📱 반응형"},
    // 반응형 설정들...
  ]
}
```

### 3. 명확한 설명 제공

각 설정에 명확한 label과 description을 제공합니다.

```json
{
  "id": "speed",
  "label": "스크롤 속도",
  "description": "숫자가 작을수록 빠르게 움직입니다 (10초 = 빠름, 60초 = 느림)",
  "type": "RANGE",
  "min": 10,
  "max": 60,
  "step": 5,
  "unit": "초"
}
```

### 4. 기본값 제공

모든 property에 적절한 기본값을 제공합니다.

```json
{
  "property": {
    "title": "기본 제목",
    "backgroundColor": "#FFFFFFFF",
    "fontSize": 16,
    "padding": 20
  }
}
```

### 5. 반응형 설정 분리

데스크톱과 모바일 설정을 명확히 구분합니다.

```json
{
  "id": "padding",
  "label": "여백 (데스크톱)",
  "type": "RANGE",
  "min": 20,
  "max": 100,
  "step": 5,
  "unit": "px"
},
{
  "id": "mobilePadding",
  "label": "여백 (모바일)",
  "type": "RANGE",
  "min": 10,
  "max": 60,
  "step": 5,
  "unit": "px"
}
```

### 6. 접근성 고려

alt 텍스트, aria-label 등 접근성을 고려합니다.

```html
<img src="{{image}}" alt="{{name}}" />
<button aria-label="다음 슬라이드">→</button>
```

### 7. 성능 최적화

- 이미지 lazy loading
- GPU 가속 활용
- 불필요한 리플로우 방지

```html
<img src="{{image}}" loading="lazy" alt="{{name}}">
```

```css
.animated {
  transform: translateZ(0);
  backface-visibility: hidden;
}
```

### 8. 순수 CSS 우선

가능한 한 JavaScript 의존성을 줄이고 순수 CSS로 구현합니다.

```css
/* ✅ 좋은 예: 순수 CSS 애니메이션 */
.item:hover {
  transform: scale(1.05);
  transition: transform 0.3s ease;
}

/* ❌ 나쁜 예: JavaScript로 hover 처리 */
```

---

## 실전 예제: 로고 배너 슬라이드

### HTML 파일

```html
<style>
  .logo-banner {
    width: 100%;
    overflow: hidden;
    background-color: {{property.backgroundColor}};
    padding: {{property.paddingY}}px {{property.paddingX}}px;
  }

  .logo-track {
    display: flex;
    align-items: center;
    gap: {{property.logoSpacing}}px;
    width: max-content;
    animation: scroll-{{property.direction}} {{property.speed}}s linear infinite;
    transform: translateZ(0);
    backface-visibility: hidden;
  }

  .logo-item {
    flex-shrink: 0;
    height: {{property.logoHeight}}px;
    display: flex;
    align-items: center;
    justify-content: center;
    filter: grayscale({{property.grayscale}}%);
    opacity: {{property.opacity}};
    transition: all 0.3s ease;
  }

  .logo-item:hover {
    filter: grayscale(0%);
    opacity: 1;
    transform: scale(1.05);
  }

  .logo-item img {
    max-height: 100%;
    max-width: 200px;
    object-fit: contain;
  }

  @keyframes scroll-left {
    from { transform: translateX(0); }
    to { transform: translateX(-50%); }
  }

  @keyframes scroll-right {
    from { transform: translateX(-50%); }
    to { transform: translateX(0); }
  }

  .logo-banner:hover .logo-track {
    animation-play-state: {{property.pauseOnHover}};
  }

  @media (max-width: 768px) {
    .logo-banner {
      padding: {{property.mobilePaddingY}}px {{property.mobilePaddingX}}px;
    }
    .logo-item {
      height: {{property.mobileLogoHeight}}px;
    }
    .logo-track {
      gap: {{property.mobileLogoSpacing}}px;
      animation-duration: {{property.mobileSpeed}}s;
    }
  }
</style>

<template>
  <div class="logo-banner">
    <div class="logo-track">
      {{#each property.logos}}
        <div class="logo-item">
          <img src="{{image}}" alt="{{name}}" loading="lazy">
        </div>
      {{/each}}
      {{#each property.logos}}
        <div class="logo-item">
          <img src="{{image}}" alt="{{name}}" loading="lazy">
        </div>
      {{/each}}
      {{#each property.logos}}
        <div class="logo-item">
          <img src="{{image}}" alt="{{name}}" loading="lazy">
        </div>
      {{/each}}
      {{#each property.logos}}
        <div class="logo-item">
          <img src="{{image}}" alt="{{name}}" loading="lazy">
        </div>
      {{/each}}
      {{#each property.logos}}
        <div class="logo-item">
          <img src="{{image}}" alt="{{name}}" loading="lazy">
        </div>
      {{/each}}
      {{#each property.logos}}
        <div class="logo-item">
          <img src="{{image}}" alt="{{name}}" loading="lazy">
        </div>
      {{/each}}
    </div>
  </div>
</template>

<script>
  // 순수 CSS 애니메이션으로 처리
</script>
```

### JSON 파일

```json
{
  "settings": [
    {
      "type": "TITLE",
      "content": "🏢 로고 관리"
    },
    {
      "type": "DESCRIPTION",
      "content": "고객사나 파트너사의 로고를 추가하여 브랜드 신뢰도를 높여보세요."
    },
    {
      "id": "logos",
      "label": "로고 목록",
      "type": "LIST",
      "maxCount": 20,
      "settings": [
        {
          "id": "name",
          "label": "회사명",
          "type": "TEXT",
          "placeholder": "예: 삼성전자"
        },
        {
          "id": "image",
          "label": "로고 이미지",
          "type": "IMAGE_PICKER"
        }
      ]
    },
    {
      "type": "TITLE",
      "content": "🎬 애니메이션 설정"
    },
    {
      "id": "direction",
      "label": "스크롤 방향",
      "type": "RADIO",
      "options": [
        {"label": "← 왼쪽으로", "value": "left"},
        {"label": "→ 오른쪽으로", "value": "right"}
      ]
    },
    {
      "id": "speed",
      "label": "스크롤 속도 (데스크톱)",
      "description": "숫자가 작을수록 빠르게 움직입니다",
      "type": "RANGE",
      "min": 10,
      "max": 60,
      "step": 5,
      "unit": "초"
    },
    {
      "id": "pauseOnHover",
      "label": "마우스 호버 시",
      "type": "RADIO",
      "options": [
        {"label": "일시정지", "value": "paused"},
        {"label": "계속 움직임", "value": "running"}
      ]
    },
    {
      "type": "TITLE",
      "content": "🎨 디자인 설정"
    },
    {
      "id": "backgroundColor",
      "label": "배경 색상",
      "type": "COLOR_PICKER"
    },
    {
      "id": "grayscale",
      "label": "회색조 효과",
      "description": "0% = 원본 색상, 100% = 완전 회색",
      "type": "RANGE",
      "min": 0,
      "max": 100,
      "step": 10,
      "unit": "%"
    }
  ],
  "property": {
    "logos": [],
    "direction": "left",
    "speed": 30,
    "pauseOnHover": "paused",
    "backgroundColor": "#000000FF",
    "grayscale": 100
  }
}
```

---

## 체크리스트

개발 완료 전 확인사항:

- [ ] HTML과 JSON 파일이 분리되어 있는가?
- [ ] SECTION 대신 TITLE/DESCRIPTION을 사용했는가?
- [ ] 모든 설정에 label과 description이 있는가?
- [ ] 기본값(property)이 제공되는가?
- [ ] 반응형 디자인이 구현되었는가?
- [ ] 이미지에 alt 텍스트가 있는가?
- [ ] 이미지에 loading="lazy"가 적용되었는가?
- [ ] 애니메이션이 끊김 없이 작동하는가?
- [ ] GPU 가속이 활성화되어 있는가?
- [ ] 불필요한 JavaScript가 제거되었는가?

---

## 참고 자료

### 지원하는 설정 타입 요약

| 타입 | 용도 | 예시 |
|-----|------|-----|
| TITLE | 섹션 제목 | `"🎨 디자인 설정"` |
| DESCRIPTION | 설명 | `"디자인 관련 설정을..."` |
| TEXT | 텍스트 입력 | 제목, 설명 등 |
| IMAGE_PICKER | 이미지 선택 | 배경, 로고 등 |
| COLOR_PICKER | 색상 선택 | 배경색, 텍스트색 등 |
| RANGE | 범위 슬라이더 | 크기, 여백 등 |
| RADIO | 라디오 버튼 | 정렬, 방향 등 |
| LIST | 리스트 | 아이템 목록 |

### 애니메이션 끊김 해결 공식

```
필요한 복제 개수 = Math.ceil(화면 너비 × 3 / 원본 너비)

권장: 최소 4~6세트 템플릿 복제
```

---

## 마무리

이 가이드는 실제 개발 경험을 바탕으로 작성되었습니다. 특히 **애니메이션 끊김 문제**는 템플릿 기반 복제로 완벽하게 해결됩니다. JavaScript 동적 복제는 타이밍 이슈로 인해 권장하지 않습니다.

**핵심 원칙:**
1. ✅ 템플릿에서 충분히 복제 (4~6세트)
2. ✅ 순수 CSS 애니메이션 우선
3. ✅ SECTION 대신 TITLE/DESCRIPTION 사용
4. ✅ HTML/JSON 파일 분리
5. ✅ 명확한 설명과 기본값 제공

---

**문서 버전**: 1.0
**최종 업데이트**: 2025-01-13
**작성자**: Nova Partners Development Team
