# 식스샵 프로 블록메이커 개발 지침 (완전판)

## 1) 블록 기본 구조 & 렌더링 규칙

### 필수 구조
블록은 하나의 파일에 아래 세 가지 최상위 태그를 각 1개씩 가집니다:

```html
<style>
  /* CSS 코드 (Handlebars.js 지원, 자동 스코프 처리) */
</style>

<template>
  <!-- HTML 마크업 (Handlebars.js 지원) -->
</template>

<script>
  // JavaScript (자동 IIFE 격리, bm 객체 제공)
</script>
```

### 필수 원칙
- ✅ `<template>`, `<style>`, `<script>`는 각각 **정확히 1개**만 존재
- ✅ 속성(attribute)을 달지 않음
- ✅ 필요 시 `<data value="$page" />` 등으로 외부 컨텍스트 주입
- ✅ template/style에서 `{{property.xxx}}` 로 설정값 접근
- ✅ script에서는 `bm.context.property.xxx` 로 접근

### 최소 예시
```html
<data value="$page" />
<data value="$customer" />

<style>
  .title {
    color: {{property.titleColor}};
    font-size: {{property.titleSize}}px;
  }
</style>

<template>
  <h1 class="title">{{property.titleText}}</h1>
  {{#if customer}}
    <p>안녕하세요, {{customer.name.full}}님!</p>
  {{/if}}
</template>

<script>
  const container = bm.container;
  const context = bm.context;

  bm.onContextChange = () => {
    // 설정 변경 시 처리
  };

  container.addEventListener('click', (e) => {
    if (e.target.matches('h1.title')) {
      // 이벤트 처리
    }
  }, true);

  bm.onDestroy = () => {
    // 정리 작업
  };
</script>
```

---

## 2) 세팅(Setting) 설계 — 스키마 & 값 구조

### 기본 개념
- **settings**: 에디터에서 보이는 설정 패널의 스키마(UI)
- **property**: 실제 블록에 전달되는 설정값(기본값 + 사용자 편집값)

### 🚨 중요: description 길이 제한

**필수 규칙:**
- ✅ 모든 `description` 필드는 **100자 이하**로 작성
- ✅ 상세한 설명이나 예시는 `placeholder`에 작성
- ✅ `DESCRIPTION` 타입으로 섹션별 상세 안내 제공

**잘못된 예시 (❌):**
```json
{
  "id": "menu1.items",
  "description": "드롭다운에 표시할 하위 메뉴들입니다. 각 줄에 '메뉴명|링크주소' 형식으로 입력하세요.\n\n예시:\n회사 개요|/about/overview\nCEO 메시지|/about/ceo\n연혁|/about/history",
  "type": "TEXTAREA"
}
```
→ **150자 초과로 저장 불가 에러 발생!**

**올바른 예시 (✅):**
```json
{
  "id": "menu1.items",
  "description": "각 줄에 '메뉴명|링크주소' 형식으로 입력하세요.",
  "type": "TEXTAREA",
  "placeholder": "회사 개요|/about/overview\nCEO 메시지|/about/ceo\n연혁|/about/history"
}
```
→ **29자로 저장 가능!**

### 필드 타입 표

| type | 용도 | description 권장 길이 |
|------|------|----------------------|
| TITLE | 섹션 제목 (정보성) | N/A |
| DESCRIPTION | 섹션 설명 (정보성) | 제한 없음 (약 200자 권장) |
| TEXT | 짧은 텍스트 입력 | 30~50자 |
| TEXTAREA | 여러 줄 텍스트 | 30~50자 + placeholder 활용 |
| COLOR_PICKER | 색상 선택 | 30~50자 |
| RANGE | 슬라이더 | 40~60자 |
| CHECKBOX | 체크박스 | 40~60자 |
| SELECT | 드롭다운 선택 | 30~50자 |
| IMAGE_PICKER | 이미지 선택 | 40~60자 |
| LINK | 링크 선택 | 30~50자 |

### description 작성 팁

#### ✅ 좋은 예시
```json
{
  "id": "logo.height",
  "label": "로고 높이",
  "description": "로고의 세로 크기(px)입니다. 일반적으로 24~40px이 적당합니다.",
  "type": "RANGE",
  "min": 20,
  "max": 60,
  "unit": "px"
}
```
→ **47자, 핵심만 간결하게**

#### ❌ 나쁜 예시
```json
{
  "id": "logo.height",
  "label": "로고 높이",
  "description": "로고의 세로 크기를 픽셀(px) 단위로 설정합니다. 일반적으로 24~40px이 적당하며, 헤더 높이의 40~50% 정도가 균형있어 보입니다. 너무 크면 헤더가 비대해 보이고, 너무 작으면 가독성이 떨어질 수 있습니다.",
  "type": "RANGE"
}
```
→ **150자 초과, 저장 불가!**

### 섹션 설명은 DESCRIPTION 타입 활용

**긴 설명이 필요한 경우:**
```json
{
  "type": "TITLE",
  "content": "📋 메뉴 설정"
},
{
  "type": "DESCRIPTION",
  "content": "메인 메뉴를 설정합니다. 각 메뉴는 제목, 링크, 하위 메뉴로 구성되며, 하위 메뉴는 '메뉴명|링크주소' 형식으로 한 줄에 하나씩 입력합니다. 예를 들어:\n\n회사소개|/about\n제품소개|/products\n문의하기|/contact\n\n이렇게 입력하면 3개의 하위 메뉴가 생성됩니다."
},
{
  "id": "menu.items",
  "label": "메뉴 항목",
  "description": "각 줄에 '메뉴명|링크주소' 형식으로 입력하세요.",
  "type": "TEXTAREA",
  "placeholder": "회사소개|/about\n제품소개|/products\n문의하기|/contact"
}
```

### 🚨 중요: LINK 타입 사용 규칙

**필수 규칙:**
- ✅ 링크 필드는 반드시 `type: "LINK"` 사용 (TEXT/TEXTAREA 금지)
- ✅ property 기본값은 문자열이 아닌 `{id, type, label, value}` 객체
- ✅ 템플릿에서 `.value` (URL)와 `.label` (표시 텍스트) 사용

**왜 LINK 타입을 써야 하나?**

LINK 타입을 사용해야 에디터에서:
- 내부 리소스(상품/카테고리/페이지) 선택 UI 제공
- 외부 URL 입력 모두 지원
- TEXT 타입은 URL 텍스트 입력만 가능 (드롭다운 링크 피커 없음)

**올바른 구현 방법:**

```json
// 1) Settings 스키마
{
  "id": "ctaLink",
  "label": "버튼 링크",
  "description": "내부 리소스 또는 외부 URL을 연결해요.",
  "type": "LINK"  // ✅ TEXT 아님!
}

// 2) Property 기본값
"property": {
  "ctaLink": {
    "id": null,     // ✅ 문자열이 아닌 객체!
    "type": "",
    "label": "",
    "value": ""
  }
}

// 3) Template 사용
{{#if property.ctaLink.value}}
  <a href="{{property.ctaLink.value}}">
    {{property.ctaLink.label}}
  </a>
{{/if}}
```

**빠른 점검 체크리스트:**
- [ ] 세팅에 `type: "LINK"` 사용 (TEXT/TEXTAREA 금지)
- [ ] property가 문자열이 아닌 `{id, type, label, value}` 객체
- [ ] 템플릿에서 `.value`와 `.label` 사용
- [ ] (선택) 새 창 옵션은 별도 CHECKBOX로 구현

**잘못된 예시 (❌):**
```json
// 세팅
{"id": "link", "type": "TEXT"}  // ❌

// 프로퍼티
"link": "/about"  // ❌ 문자열

// 템플릿
{{property.link}}  // ❌
```

**올바른 예시 (✅):**
```json
// 세팅
{"id": "link", "type": "LINK"}  // ✅

// 프로퍼티
"link": {"id": null, "type": "", "label": "", "value": ""}  // ✅

// 템플릿
{{property.link.value}}  // ✅
{{property.link.label}}  // ✅
```

### ID 네이밍 규칙

- ✅ 영문/숫자/점(.) 만 사용
- ✅ camelCase 권장
- ✅ `a.b`/`a.c` 형태면 `property.a = { b, c }` 로 중첩
- ❌ 한글, 특수문자, 공백 사용 금지

**예시:**
```json
{
  "id": "logo.image",      // ✅ property.logo.image
  "id": "colors.primary",  // ✅ property.colors.primary
  "id": "menu1.title",     // ✅ property.menu1.title
  "id": "logo-image",      // ❌ 하이픈 사용 금지
  "id": "로고이미지"       // ❌ 한글 사용 금지
}
```

### 세팅 JSON 템플릿

```json
{
  "settings": [
    {
      "type": "TITLE",
      "content": "📸 로고 설정"
    },
    {
      "type": "DESCRIPTION",
      "content": "웹사이트 상단에 표시될 로고를 설정합니다. SVG 또는 투명 배경 PNG를 권장합니다."
    },
    {
      "id": "logo.image",
      "label": "로고 이미지",
      "description": "웹사이트 로고 이미지를 선택하세요.",
      "type": "IMAGE_PICKER"
    },
    {
      "id": "logo.height",
      "label": "로고 높이",
      "description": "로고의 세로 크기(px)입니다. 일반적으로 24~40px이 적당합니다.",
      "type": "RANGE",
      "min": 20,
      "max": 60,
      "step": 2,
      "unit": "px"
    },
    {
      "id": "menu.items",
      "label": "메뉴 항목",
      "description": "각 줄에 '메뉴명|링크주소' 형식으로 입력하세요.",
      "type": "TEXTAREA",
      "placeholder": "회사소개|/about\n제품소개|/products\n문의하기|/contact"
    }
  ],
  "property": {
    "logo": {
      "image": "",
      "height": 32
    },
    "menu": {
      "items": "회사소개|/about\n제품소개|/products"
    }
  }
}
```

---

## 3) 템플릿/스타일 작성 요령 (Handlebars.js)

### 기본 문법

```handlebars
<!-- 값 출력 -->
{{property.xxx}}

<!-- HTML 출력 (신뢰된 데이터만) -->
{{{property.richHTML}}}

<!-- 조건문 -->
{{#if property.show}}
  표시됨
{{else}}
  숨김
{{/if}}

<!-- 반복문 -->
{{#each property.items}}
  <li>{{this.title}}</li>
{{else}}
  <p>항목이 없습니다.</p>
{{/each}}

<!-- 배열 인덱스 접근 -->
{{property.items.[0].title}}
```

### 스타일에서 동적 값

```css
.card {
  --gap: {{property.gap}}px;
  gap: var(--gap);
  color: var(--color-text-90);
}
```

---

## 4) <script> 올바른 패턴

### bm 객체 구조

```javascript
bm.container        // 블록 컨테이너 DOM 요소
bm.context          // 컨텍스트 객체
bm.context.property // 설정값
bm.onContextChange  // 설정 변경 시 콜백
bm.onDestroy        // 블록 제거 시 콜백
```

### 기본 패턴

```javascript
const container = bm.container;
const context = bm.context;

// 초기화
function init() {
  // 초기 설정
}
init();

// 설정 변경 반영
bm.onContextChange = () => {
  // context.property 값 변경 시 처리
};

// 이벤트 위임
container.addEventListener('click', (e) => {
  if (e.target.matches('[data-action="button"]')) {
    // 버튼 클릭 처리
  }
}, true);

// 정리
bm.onDestroy = () => {
  // 타이머, 리스너, 옵저버 해제
};
```

---

## 5) 보안 & 품질 체크리스트

### 구조
- [ ] `<template>`, `<style>`, `<script>` 각 정확히 1개
- [ ] 태그에 속성 없음
- [ ] `<data>`는 필요할 때만 사용

### 프로퍼티
- [ ] 코드에서 사용하는 모든 `property.*`가 settings에 선언됨
- [ ] settings에 선언한 항목은 코드에서 반드시 사용
- [ ] 미사용 필드 없음

### 설정 스키마
- [ ] **모든 description이 100자 이하** ⭐ 중요!
- [ ] 상세 설명은 placeholder 또는 DESCRIPTION 타입 사용
- [ ] ID는 영문/숫자/점(.)만 사용, camelCase
- [ ] label이 명확하고 이해하기 쉬움

### 재렌더링 & 생명주기
- [ ] 템플릿 재렌더 대응: 이벤트 위임 사용
- [ ] `bm.onContextChange` 구현
- [ ] `bm.onDestroy`에서 자원 해제

### 보안
- [ ] `eval()` 사용 금지
- [ ] 신뢰되지 않은 HTML에 `{{{...}}}` 사용 금지
- [ ] 외부 네트워크 요청 금지
- [ ] HTTPS만 사용 (혼합 콘텐츠 금지)

### UX
- [ ] 리스트에 `{{else}}` 빈 상태 처리
- [ ] 이미지에 `loading="lazy"` (로고는 `eager`)
- [ ] 접근성: `aria-label`, `title` 속성
- [ ] 네이밍 일관성 (camelCase)

---

## 6) 파일 제공 형식

### 두 개의 독립 파일로 분리

#### 파일 1: 블록 코드 (`block-name.html`)
```html
<style>
  /* CSS 전체 */
</style>

<template>
  <!-- HTML 전체 -->
</template>

<script>
  // JavaScript 전체
</script>
```
→ **한 번에 복사-붙여넣기 가능**

#### 파일 2: 설정 스키마 (`block-name-settings.json`)
```json
{
  "settings": [ /* ... */ ],
  "property": { /* ... */ }
}
```
→ **한 번에 복사-붙여넣기 가능**

---

## 7) 비개발자 친화적 UX 설계

### 카테고리 구조화

```json
{
  "settings": [
    { "type": "TITLE", "content": "📸 로고 설정" },
    { "type": "DESCRIPTION", "content": "상세 설명..." },
    // 로고 관련 필드들

    { "type": "TITLE", "content": "🎨 디자인 및 색상" },
    { "type": "DESCRIPTION", "content": "상세 설명..." },
    // 디자인 관련 필드들
  ]
}
```

### 이모지 활용 (선택사항)

```
📸 로고 설정
🎨 디자인 및 색상
📋 메뉴 구조
🔗 링크 및 버튼
📱 모바일 설정
⚙️ 고급 옵션
🌐 다국어
💡 도움말
```

### description 작성 가이드

**필수 요소:**
1. **무엇을** 설정하는가 (30자 이내)
2. **권장값** 또는 **일반적인 범위** (20자 이내)
3. 총 **100자 이하**로 제한

**좋은 예시:**
```json
{
  "description": "메뉴 간격(px)입니다. 넓은 간격 50~60px, 좁은 간격 30~40px."
}
```
→ **47자**

**나쁜 예시:**
```json
{
  "description": "메인 메뉴 항목들 사이의 가로 간격을 픽셀 단위로 설정합니다. 넓은 간격은 50~60px, 좁은 간격은 30~40px를 사용하세요. 간격이 너무 좁으면 답답해 보이고, 너무 넓으면 산만해 보일 수 있습니다."
}
```
→ **150자, 저장 불가!**

### placeholder 활용

**TEXTAREA, TEXT 타입에서 예시 제공:**
```json
{
  "id": "menu.items",
  "description": "각 줄에 '메뉴명|링크주소' 형식으로 입력하세요.",
  "type": "TEXTAREA",
  "placeholder": "회사소개|/about\n제품소개|/products\n문의하기|/contact\n\n💡 팁: 최대 10개까지 추천합니다."
}
```

---

## 8) 흔한 실수 & 바로잡기

### ❌ description이 100자를 초과함
```json
{
  "description": "드롭다운에 표시할 하위 메뉴들입니다. 각 줄에 '메뉴명|링크주소' 형식으로 입력하세요.\n\n예시:\n회사 개요|/about/overview\nCEO 메시지|/about/ceo"
}
```
→ ✅ **description은 50자 이내로, 예시는 placeholder로**

### ❌ document.querySelector() 직접 사용
```javascript
document.querySelector('button').addEventListener(...)
```
→ ✅ **bm.container 이벤트 위임**
```javascript
bm.container.addEventListener('click', (e) => {
  if (e.target.matches('button')) { ... }
})
```

### ❌ settings에만 있고 코드에서 미사용
```json
{ "id": "subtitle" }  // 선언만 하고 사용 안 함
```
→ ✅ **정의한 설정은 반드시 사용하거나 삭제**

### ❌ {{{...}}} 외부 입력에 사용
```handlebars
{{{property.userInput}}}
```
→ ✅ **신뢰된 데이터만 사용, 일반 텍스트는 {{...}}**

### ❌ 외부 네트워크 요청
```javascript
fetch('https://api.example.com/data')
```
→ ✅ **플랫폼 허용 방식만 사용 (또는 전면 금지)**

---

## 9) 코드 리뷰 체크리스트

### 필수 확인 사항
- [ ] `<template>`, `<style>`, `<script>` 각 1개
- [ ] **모든 description 100자 이하** ⭐ 중요!
- [ ] 상세 설명은 placeholder 또는 DESCRIPTION 타입 사용
- [ ] 모든 `property.*`가 settings에 선언됨
- [ ] settings의 모든 항목이 코드에서 사용됨
- [ ] 이벤트 위임 사용, 전역 DOM 직접 바인딩 회피
- [ ] `bm.onContextChange` / `bm.onDestroy` 적절히 사용
- [ ] `eval()` / 삼중 중괄호 오남용 없음
- [ ] 외부 네트워크 요청 없음
- [ ] 이미지 `loading` 속성 적용
- [ ] 네이밍 일관성(camelCase)
- [ ] 빈 리스트 `{{else}}` 처리

---

## 10) description 길이 체크 도구

### 수동 체크 방법

**Python 스크립트:**
```python
import json

with open('settings.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

errors = []
for setting in data.get('settings', []):
    if 'description' in setting:
        desc = setting['description']
        length = len(desc)
        if length > 100:
            errors.append({
                'id': setting.get('id', 'N/A'),
                'label': setting.get('label', 'N/A'),
                'length': length,
                'description': desc[:50] + '...'
            })

if errors:
    print(f"❌ {len(errors)}개 필드의 description이 100자를 초과합니다:\n")
    for err in errors:
        print(f"ID: {err['id']}")
        print(f"Label: {err['label']}")
        print(f"Length: {err['length']}자")
        print(f"Preview: {err['description']}\n")
else:
    print("✅ 모든 description이 100자 이하입니다!")
```

### 자동 수정 도구

**자동 축약 스크립트:**
```python
import json

def truncate_description(desc, max_length=100):
    if len(desc) <= max_length:
        return desc

    # 문장 단위로 잘라서 100자 이내로
    sentences = desc.split('.')
    result = ''
    for sentence in sentences:
        if len(result) + len(sentence) + 1 <= max_length:
            result += sentence + '.'
        else:
            break

    return result.strip() or desc[:max_length-3] + '...'

# settings.json 처리
with open('settings.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

for setting in data.get('settings', []):
    if 'description' in setting:
        original = setting['description']
        if len(original) > 100:
            setting['description'] = truncate_description(original)
            print(f"축약: {setting.get('label', 'N/A')}")
            print(f"  이전: {original}")
            print(f"  이후: {setting['description']}\n")

with open('settings-fixed.json', 'w', encoding='utf-8') as f:
    json.dump(data, f, ensure_ascii=False, indent=2)

print("✅ settings-fixed.json 생성 완료!")
```

---

## 11) 빠른 참고 가이드

### description 길이 기준

| 필드 타입 | 권장 description 길이 | 예시 |
|-----------|----------------------|------|
| TEXT | 30~50자 | "상단에 표시될 메뉴 이름입니다." |
| TEXTAREA | 30~50자 + placeholder | "각 줄에 '메뉴명\|링크' 형식으로 입력하세요." |
| COLOR_PICKER | 30~50자 | "헤더 배경 색상입니다." |
| RANGE | 40~60자 | "로고 높이(px)입니다. 일반적으로 24~40px." |
| CHECKBOX | 40~60자 | "체크하면 메뉴가 상단에 고정됩니다." |
| SELECT | 30~50자 | "헤더에 사용할 글꼴을 선택하세요." |
| IMAGE_PICKER | 40~60자 | "로고 이미지를 선택하세요. SVG 권장." |

### 문자 수 확인 팁

**온라인 도구:**
- https://www.charactercountonline.com/
- https://wordcounter.net/character-count

**VS Code:**
- 텍스트 선택 → 우측 하단 상태바에 문자 수 표시

**터미널 (Mac/Linux):**
```bash
echo -n "description 내용" | wc -m
```

---

## 12) 클로드에게 전달할 작업 지시

### 블록 제작 시 체크리스트

1. **세팅 JSON 먼저 작성**
   - ✅ 모든 description은 100자 이하
   - ✅ 상세 설명은 placeholder 또는 DESCRIPTION 타입
   - ✅ ID는 camelCase
   - ✅ property에 기본값 설정

2. **블록 코드 작성**
   - ✅ `<style>`, `<template>`, `<script>` 각 1개
   - ✅ Handlebars 문법으로 동적 처리
   - ✅ bm 객체 올바르게 사용
   - ✅ 이벤트 위임 패턴

3. **최종 검증**
   - ✅ description 길이 체크 (100자 이하)
   - ✅ settings와 코드 동기화 확인
   - ✅ 보안 체크리스트 통과
   - ✅ 두 개 파일로 분리 (블록 코드 + 설정 JSON)

### 에러 발생 시 우선 체크

**"description은 100자 이하로 입력 가능해요" 에러:**
1. settings.json 열기
2. 모든 description 필드 검색
3. 100자 초과 항목 찾기
4. 축약 + placeholder로 이동
5. 재저장

---

## 13) 마지막 점검 사항

### 저장 전 필수 확인

```
✅ 모든 description 100자 이하인가?
✅ placeholder에 상세 예시가 있는가?
✅ DESCRIPTION 타입으로 섹션 설명이 충분한가?
✅ 이모지로 카테고리가 구분되어 있는가?
✅ 비개발자가 이해하기 쉬운가?
✅ settings와 코드가 동기화되어 있는가?
✅ bm 객체를 올바르게 사용하는가?
✅ 이벤트 위임 패턴을 사용하는가?
✅ 보안 규칙을 준수하는가?
✅ 두 개 파일로 분리되어 있는가?
```

---

## 부록: 자주 사용하는 Handlebars 헬퍼

```javascript
// 문자열 분할
{{#each (split property.items "\n")}}
  <li>{{this}}</li>
{{/each}}

// 조건 비교
{{#if (eq property.language "ko")}}
  한국어
{{/if}}

// 산술 연산
{{add property.value 10}}
{{subtract property.value 5}}
{{multiply property.value 2}}
{{divide property.value 2}}

// 논리 연산
{{#if (and property.show property.enabled)}}
  표시됨
{{/if}}

{{#if (or property.optionA property.optionB)}}
  둘 중 하나 활성
{{/if}}
```

---

**버전**: 2.0.0 (description 길이 제한 추가)
**최종 업데이트**: 2025-01-04
**중요 업데이트**: description 100자 제한 규칙 추가 ⭐
