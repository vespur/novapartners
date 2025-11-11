# 식스샵 프로 블록 메이커 개발 지침

> 이 문서는 식스샵 프로(SixShop Pro) 웹빌더에서 블록 메이커를 사용하여 커스텀 블록을 개발할 때 준수해야 할 모든 지침을 포함합니다.

---

## 목차
0. [AI 에이전트 개발 지침](#0-ai-에이전트-개발-지침)
1. [핵심 제약사항](#1-핵심-제약사항)
2. [블록 메이커 기본 구조](#2-블록-메이커-기본-구조)
3. [필수 요건 (미충족 시 등록 불가)](#3-필수-요건-미충족-시-등록-불가)
4. [커스텀 블록 점검표](#4-커스텀-블록-점검표)
5. [고품질 블록 인증 기준](#5-고품질-블록-인증-기준)
6. [헤더/푸터 블록 전용 체크리스트](#6-헤더푸터-블록-전용-체크리스트)
7. [이미지 공유 방법](#10-이미지-공유-방법)
8. [CSS 레이아웃 디버깅 프로토콜 (필수)](#11-css-레이아웃-디버깅-프로토콜-필수)

---

## 0. AI 에이전트 개발 지침

> Claude 등 AI 에이전트가 이 프로젝트를 개발할 때 반드시 준수해야 할 지침

### 0-1) 토큰 사용량 최소화 원칙 ⚠️

**목표**: 불필요한 토큰 낭비 방지, 효율적 작업 수행

#### ✅ 반드시 지킬 것

1. **결과만 간결하게 보고**
   - 긴 설명, 분석 과정 생략
   - 완성된 코드 + 핵심 개선사항만 보고
   - 예시: "✅ 완료. description 100자 제한 적용, DIVIDER 제거"

2. **불필요한 파일 읽기 금지**
   - 이미 읽은 파일 재확인 금지
   - 추측 가능한 내용 확인 금지
   - 필요한 부분만 offset/limit로 정확히 읽기

3. **중복 설명 금지**
   - 같은 내용 반복 설명 금지
   - 커밋 메시지로 충분한 경우 별도 설명 불필요

4. **병렬 작업 최대한 활용**
   - 독립적인 작업은 한 번에 실행
   - 예: Read 여러 파일 동시에

#### ❌ 하지 말 것

```
❌ "이제 문제를 분석하겠습니다. 먼저..." (분석 과정 설명)
❌ "파일을 확인해보니..." (확인 과정 설명)
❌ "다음과 같이 수정했습니다: 1. ... 2. ... 3. ..." (장황한 나열)
❌ 이미 읽은 파일 다시 읽기
❌ 사용자가 이미 알고 있는 내용 반복
```

#### ✅ 올바른 응답 패턴

```
✅ 완료
- description 100자 제한 적용
- DIVIDER 타입 제거 (미지원)
- 커밋: 06efd7e
```

### 0-2) description 필드 제약사항 ⚠️

**중요**: SixShop Pro JSON 설정에서 description은 **100자 이하**만 가능

```json
// ❌ 100자 초과 - 저장 실패
{
  "description": "이 필드는 매우 중요한 설정입니다. 사용자가 이 옵션을 선택하면 다양한 효과가 적용되며, 특히 모바일 환경에서 더욱 유용하게 활용할 수 있습니다. 반드시 확인하시기 바랍니다."
}

// ✅ 100자 이하 - 정상
{
  "description": "중요 설정. 모바일에서 유용하게 활용됨"
}
```

### 0-3) DIVIDER/DESCRIPTION 타입 제약사항 ⚠️

```json
// ❌ DIVIDER 타입 - 저장 실패
{
  "type": "DIVIDER"
}

// ❌ DESCRIPTION 타입 - 지원 불확실
{
  "type": "DESCRIPTION",
  "content": "설명 텍스트"
}

// ✅ TITLE만 사용 (자동 구분선 생성)
{
  "type": "TITLE",
  "content": "섹션 제목"
}
```

### 0-4) JSON 설정-HTML 코드 동기화 원칙 ⚠️

**중요:** JSON에 옵션을 추가하면 **반드시** HTML 렌더링 코드에도 연동해야 함

#### 문제 사례 (2025-11-11)
```
❌ 잘못된 개발 순서:
1. global-header.json에 모바일 옵션 33개 추가 (mobileHeaderHeight, hamburgerSize 등)
2. 커밋 및 푸시
3. 사용자 테스트: "옵션이 작동하지 않음"
4. 원인 파악: HTML 파일에서 해당 값들을 전혀 사용하지 않음

✅ 올바른 개발 순서:
1. global-header.json에 옵션 추가
2. global-header.html에서 해당 옵션 사용하는 코드 구현
   - CSS에서 {{property.mobileHeaderHeight}} 적용
   - 미디어쿼리에 모바일 전용 스타일 추가
3. 커밋 및 푸시
4. 정상 작동 확인
```

#### 동기화 체크리스트

**JSON에 새 옵션 추가 시:**
- [ ] HTML `<style>` 태그에서 해당 property 사용
- [ ] 미디어쿼리 필요 시 `@media` 블록 추가
- [ ] JavaScript 로직 필요 시 `<script>` 태그에 구현
- [ ] `bm.onContextChange`에서 동적 업데이트 처리

**예시: 모바일 헤더 높이**
```json
// 1. JSON 설정 추가
{
  "id": "mobileHeaderHeight",
  "label": "모바일 헤더 높이",
  "type": "RANGE",
  "min": 50,
  "max": 100
}
```

```css
/* 2. HTML에서 사용 - 필수! */
@media (max-width: 768px) {
  .global-header {
    height: {{property.mobileHeaderHeight}}px !important;
  }
}
```

#### 검증 방법
```
1. 에디터에서 새로 추가한 옵션 수정
2. 즉시 변경사항이 화면에 반영되는지 확인
3. 반영 안 되면 → HTML 코드 연동 누락
```

### 0-5) 가이드라인 자동 업데이트 원칙 ⚠️

**원칙:** 문제 해결 후 사용자가 재수정 요청하지 않으면 = 정상 작동으로 간주
→ **즉시 가이드라인에 해결 방법 추가**하여 재발 방지

#### 업데이트 트리거
```
✅ 가이드라인 업데이트가 필요한 상황:
- 사용자가 문제 보고
- 문제 해결 완료
- 사용자가 재수정 요청하지 않음 (= 정상 작동 확인)

❌ 업데이트하지 않는 상황:
- 일반적인 기능 개발 (문제가 아닌 경우)
- 사용자가 추가 수정 요청한 경우 (아직 미해결)
```

#### 업데이트 프로세스
```
1. 문제 원인 분석 및 해결
2. 커밋 및 푸시
3. 사용자 확인 대기 (재수정 요청 없으면 정상)
4. BLOCK_MAKER_GUIDELINES.md에 다음 추가:
   - 문제 사례
   - 올바른 해결 방법
   - 체크리스트
   - 예시 코드
5. 버전 이력 업데이트
```

#### 문서화 형식
```markdown
### 섹션 번호) 문제 제목 ⚠️

**중요:** 핵심 원칙 1줄 요약

#### 문제 사례 (날짜)
// 무엇이 잘못되었는지
❌ 잘못된 방법:
...

✅ 올바른 방법:
...

#### 체크리스트
- [ ] 확인 항목 1
- [ ] 확인 항목 2

#### 예시 코드
// 구체적인 코드 예시
```

---

## 1. 핵심 제약사항

### 1-1) 중첩 설정 금지 ⚠️
```
❌ 절대 금지: LIST, COLOR_SCHEME_LIST, ACCORDION, TAB 타입을 다른 설정 내부에 중첩
```

**예시:**
```json
// ❌ 잘못된 예시 - LIST 안에 LIST 중첩
{
  "id": "menuItems",
  "type": "LIST",
  "settings": [
    {
      "id": "subMenuItems",
      "type": "LIST"  // 에러 발생!
    }
  ]
}
```

**해결 방법:**
- 개별 필드로 분리 (sub1Name, sub1Link, sub2Name, sub2Link...)
- 단, 이 방법도 필드가 너무 많으면 저장 실패 가능

### 1-2) LIST 설정 필드 수 제한 ⚠️
```
권장: LIST 내부에는 1-2개 TEXT + 1-2개 LINK 필드만 사용
```

**이유:**
- LINK 필드는 복잡한 객체 (단순 값이 아님)
- 10개 이상의 LINK 필드 → 저장 실패
- 5개 이상의 LINK 필드도 불안정

**예시:**
```json
// ✅ 올바른 예시 - 최소 필드
{
  "id": "menuItems",
  "type": "LIST",
  "maxCount": 10,
  "settings": [
    {
      "id": "name",
      "type": "TEXT"
    },
    {
      "id": "link",
      "type": "LINK"
    }
  ]
}

// ❌ 피해야 할 예시 - 과도한 LINK 필드
{
  "id": "menuItems",
  "type": "LIST",
  "settings": [
    { "id": "name", "type": "TEXT" },
    { "id": "link", "type": "LINK" },
    { "id": "sub1Link", "type": "LINK" },
    { "id": "sub2Link", "type": "LINK" },
    // ... 10개 이상 → 저장 실패!
  ]
}
```

### 1-3) Reserved ID 금지 ⚠️
```
❌ 사용 금지 ID: menus, products, collections, pages 등 시스템 예약어
```

### 1-4) ID 명명 규칙
```
✅ camelCase만 사용 (logoImage, menuItems)
❌ snake_case 금지 (logo_image, menu_items)
❌ 언더스코어 금지
```

---

## 2. 블록 메이커 기본 구조

### 2-1) 4가지 주요 태그
```html
<style>
  /* CSS 스타일 - Handlebars.js 사용 가능 */
</style>

<template>
  <!-- HTML 템플릿 - Handlebars.js 사용 가능 -->
</template>

<script>
  // JavaScript - bm 객체 사용
</script>

<data>
  // JSON 설정 스키마
</data>
```

### 2-2) Context 객체
- `context.id`: 블록 고유 ID
- `context.property`: 블록 설정 값
- `context.$page`: 현재 페이지 정보
- `context.$customer`: 고객 정보
- `context.$cart`: 장바구니 정보

### 2-3) bm 객체 메서드
```javascript
// 필수 메서드
bm.container              // 블록 컨테이너 DOM 요소
bm.context                // Context 객체 접근
bm.onContextChange = () => {}  // 설정 변경 시 콜백 (필수!)
bm.onDestroy = () => {}   // 블록 제거 시 cleanup

// 기타 메서드
bm.apply()                // 강제 재렌더링
bm.config(key, value)     // 설정 값 get/set
bm.do(action, data)       // 액션 실행
bm.call(request)          // HTTP 요청
bm.localStorage           // 로컬 스토리지
bm.sessionStorage         // 세션 스토리지
```

### 2-4) DOM 이벤트 패턴 ⚠️
```javascript
// ✅ 올바른 패턴 - bm.container에 위임
bm.container.addEventListener('click', (e) => {
  if (e.target.closest('.button')) {
    // 처리
  }
}, true);

// ❌ 잘못된 패턴 - 개별 요소에 바인딩
const button = container.querySelector('.button');
button.addEventListener('click', () => {}); // 재렌더링 시 무효화됨!
```

**이유:** 템플릿이 재렌더링되면 개별 요소가 새로 생성되므로, 항상 `bm.container`에 이벤트를 위임해야 합니다.

### 2-5) SVG 렌더링
```handlebars
<!-- ✅ 올바른 방법 - 3중 괄호 사용 -->
{{{iconSvg}}}

<!-- ❌ 잘못된 방법 - HTML 이스케이프됨 -->
{{iconSvg}}
```

```css
/* SVG 스타일링 - 모든 자식 요소에 적용 */
.icon svg * {
  stroke: {{property.iconColor}};
  fill: {{property.iconColor}};
}
```

### 2-6) bm.onContextChange 필수 사용 ⚠️
```javascript
// JS에서 context.property를 사용하는 경우, 반드시 onContextChange 구현!

const context = bm.context;

function init() {
  // context.property 사용하는 초기화 로직
  const value = context.property.someValue;
  // ...
}

init();

// ✅ 필수! 설정 변경 시 즉시 반영
bm.onContextChange = () => {
  init();
};
```

**없으면:** 에디터에서 설정을 바꿔도 블록에 즉시 반영되지 않음

### 2-7) bm.onDestroy 필수 사용 (이벤트 리스너 등록 시)
```javascript
// window 이벤트 등록 시 cleanup 필수!

function handleScroll() {
  // 스크롤 처리
}

window.addEventListener('scroll', handleScroll);

// ✅ 필수! 블록 제거 시 이벤트 리스너 정리
bm.onDestroy = () => {
  window.removeEventListener('scroll', handleScroll);
  document.body.style.overflow = ''; // 기타 cleanup
};
```

### 2-8) console.log 제거 ⚠️
```javascript
// ❌ 최종 코드에 절대 남기면 안 됨!
console.log('debug');
console.error('test');

// 배포 전 모두 삭제 필수
```

---

## 3. 필수 요건 (미충족 시 등록 불가)

### 3-1) 편집 용이성

#### ✅ 체크리스트
- [ ] 에디터에서 수정 시, 모든 기능이 오류 없이 작동
- [ ] 커스텀 코드로 인한 기능/성능 문제 없음
- [ ] 템플릿에 커스텀 블록 포함 시 정상 작동
- [ ] 사용자가 직접 편집할 수 있는 섹션/블록만 포함

#### ❌ 지양 예시
- **통이미지 사용**: 글자를 이미지에 넣어 편집 불가능
- **코드로 만든 결과물**: 텍스트/이미지/스타일 편집 불가능

#### ✅ 허용 예시
- [테마 설정 > 커스텀 코드]로 디자인 요소 일괄 변형 (편집성 해치지 않는 범위)

#### 저작권 점검
- [ ] 템플릿 내 콘텐츠/리소스는 저작권 문제 없이 재사용 가능

### 3-2) 콘텐츠 구성

#### ✅ 체크리스트
- [ ] **내 페이지 3개 이상** 구성 (서로 다른 레이아웃/내용)
- [ ] 기본 제공 페이지와 함께 온전한 헤더/푸터 메뉴 구성
- [ ] 동일한 페이지 구성 반복 없음, 무의미한 페이지 없음
- [ ] 단순 상품 진열 위주가 아닌 활용도 높은 템플릿 구성
- [ ] **고품질 이미지** 사용

#### 이미지 가이드

**❌ 지양해야 하는 이미지**
- 해상도가 낮거나, 배경이 뿌옇고 노이즈가 낀 이미지
- 과한 그림자, 그라데이션, 윤곽선 등 오래된 웹 그래픽 스타일
- 스톡 이미지 티가 나는 과도한 포즈/연출 (가짜 오피스, 웃는 비즈니스맨)
- 과한 보정, 인위적인 색감 (네온톤, 과한 대비)
- 배경이 산만하거나 시대감이 느껴지는 연출

**✅ 권장되는 이미지**
- 자연광 기반의 선명한 촬영, 명암 대비가 자연스러운 사진
- 실제 사용 맥락이 느껴지는 라이프스타일 컷
- 최신 트렌드에 맞는 톤앤매너
- 브랜드의 톤과 일관된 이미지

**참고:** [이미지 영역별 권장 사이즈](https://help.pro.sixshop.com/design/image-guide)

#### 일관성 및 품질
- [ ] 이미지/동영상, 아이콘, 구성 요소의 디자인/스타일이 일관성 있음
- [ ] 의미 없는 더미 텍스트나 중복 이미지 없이 실제 예시 콘텐츠로 구성

---

## 4. 커스텀 블록 점검표

### 4-1) 설정 패널의 정리

#### ✅ 체크리스트
- [ ] 블록 이름이 **한글**로 적절히 작성
- [ ] 설정 패널 내 모든 문구가 **한글**로 작성 (기술 용어/고유명사 제외)
- [ ] **TITLE 설정**으로 관련 설정들끼리 묶어서 정리
- [ ] 외부 API 필요 시, 연동 방법 충분히 안내 (DESCRIPTION에 링크)
- [ ] **isVisible 속성** 적재적소에 문법 오류 없이 사용
- [ ] 글자 요소는 글자 크기 설정 제공 (예외적인 경우 제외)

#### 에디터 UX 원칙 ⭐ (비개발자 사용자 중심 설계)

**목표:** 비개발자도 직관적으로 이해하고 사용할 수 있는 에디터

**1. 그룹핑과 설명**
- 모든 설정 항목에 **친절하고 자세한 description** 추가
- 복잡한 기능은 **DESCRIPTION 타입**으로 별도 설명 섹션 제공
- 이모지(📌, 💡, ⚠️ 등) 활용하여 시각적 구분
- 예시와 주의사항을 함께 제공

**2. 항목 배치 순서 원칙**

블록 전체 구조를 사용자 시선 순서로 배치:

```
1순위: 블록 전체에 영향을 미치는 요소
   └─ 예: 전체 여백, 배경색, 최대 너비

2순위: 화면 좌측 상단 요소
   └─ 예: 로고

3순위: 화면 중앙 요소 (같은 선상)
   └─ 예: 메인 메뉴

4순위: 화면 우측 요소 (같은 선상)
   └─ 예: 유틸리티 메뉴, 다국어 버튼

5순위: 다음 줄로 이동하여 좌→중→우 반복
```

**예외: 관련 옵션 그룹핑 우선**
- 부모-자식 관계가 있는 설정은 인접 배치
- 예: 메뉴 옵션 → 하위 메뉴 옵션 (별도 섹션으로 분리 가능)

**헤더 블록 권장 순서:**
```json
1. 헤더 기본 설정 (전체 여백, 배경, 높이)
2. 헤더 스크롤 디자인
3. 로고 옵션 (좌측 상단)
4. 메뉴 옵션 (중앙 또는 로고 옆)
5. 하위 메뉴 옵션 (메뉴와 관련)
6. 유틸리티 메뉴 옵션 (우측)
7. 다국어 버튼 옵션 (우측 끝)
```

**3. 사용자 안내 패턴**

복잡한 기능(하위 메뉴 그룹, LIST 중첩 제약 등)은 반드시 설명:

```json
{
  "type": "DESCRIPTION",
  "content": "📌 기능 이름\n\n• 어떤 기능인지 설명\n• 사용 방법 안내\n• 주의사항\n\n💡 TIP: 추가 팁",
  "isVisible": "조건"
}
```

**4. 라벨과 설명 작성 가이드**

```json
// ✅ 좋은 예시
{
  "id": "group1Title",
  "label": "📁 그룹 1 제목 (선택사항)",
  "description": "첫 번째 컬럼의 제목입니다 (링크 기능 없음). 제목 없이 항목만 추가할 수도 있습니다.",
  "type": "TEXT"
}

// ❌ 나쁜 예시
{
  "id": "group1Title",
  "label": "그룹 1 제목",
  "description": "제목",
  "type": "TEXT"
}
```

**핵심:**
- 라벨: 간결하지만 의미 명확
- 설명: 구체적이고 친절하게 (무엇을, 어떻게, 주의사항)
- 선택사항 여부 명시
- 기본값 동작 설명

#### TITLE 설정 활용
```json
{
  "type": "TITLE",
  "content": "첫 번째 섹션"
}
// ... 관련 설정들 ...
{
  "type": "TITLE",
  "content": "두 번째 섹션"  // 2번째부터 구분선 자동 생성
}
```

**⚠️ 중요: DIVIDER 타입은 지원하지 않음**
```json
// ❌ 지원하지 않는 타입 - 저장 오류 발생
{
  "type": "DIVIDER"
}

// ✅ TITLE 타입만 사용하면 자동으로 구분선 생성됨
{
  "type": "TITLE",
  "content": "섹션 제목"
}
```

#### isVisible 사용법
```json
// 일반 설정
{
  "id": "menuHoverColor",
  "isVisible": "property.menuHoverEffect === 'color'"
}

// LIST 내부 아이템
{
  "id": "subLink",
  "isVisible": "property.menuItems[index].hasSubmenu === true"
}
```

### 4-2) 블록의 동작 및 코드 관련 점검

#### ✅ 체크리스트
- [ ] 블록 너비가 커스텀 섹션 너비를 뚫고 나가지 않음 (position: relative일 때)
  - position: absolute/fixed/sticky는 예외 가능
- [ ] 설정 패널 값 변경 시, 블록에 **즉시 반영** (`bm.onContextChange` 구현)
- [ ] 데스크톱과 모바일 양쪽 뷰포트에서 모두 정상 작동
- [ ] 동일한 블록 여러개 존재 시, 각각 독립적으로 작동
- [ ] 외부 라이브러리 필요 시, 공통 코드 편집에 추가
- [ ] 라이브러리가 다른 섹션/블록에 영향 미치지 않음 (특히 CSS)
- [ ] **bm.onDestroy** 필요 시 구현 (window 이벤트 리스너 등)
- [ ] `<script>` 태그 내 **console.log 모두 삭제**

### 4-3) 테마 설정 - 공통 속성의 상속

#### ✅ 체크리스트
- [ ] 제목글에 테마 설정의 **font-family, font-weight** 적용
- [ ] 줄글/버튼에 테마 설정의 **font-family, font-weight** 적용
- [ ] COLOR_SCHEME 사용 시, 관련 **CSS 변수** 적용

#### CSS 변수 사용법
```css
/* 제목글 */
.heading {
  font-weight: var(--font-weight-heading);
  font-family: var(--font-family-heading);
}

/* 줄글 */
.body-text {
  font-weight: var(--font-weight-body);
  font-family: var(--font-family-body);
}

/* 색상 구성 */
.block {
  background-color: var(--color-background-100);
  color: var(--color-text-100);
  border-color: var(--color-border-100);
}

.accent {
  color: var(--color-accent-100);
}
```

**투명도 변형:**
- `--color-text-100` (불투명) ~ `--color-text-0` (완전 투명)
- `--color-accent-100` ~ `--color-accent-5`

### 4-4) 개별 설정의 점검

#### ✅ 체크리스트
- [ ] **RICH_TEXT**: 굵게(`<strong>`), 기울임꼴(`<i>`) 정상 작동
- [ ] **RICH_TEXT**: 사용자 입력 글자가 HTML 코드가 아닌 정상 글자로 표시
- [ ] **RICH_TEXT**: 불필요한 속성 숨김 (`exclude` 사용)
- [ ] **RANGE**: unit(단위)과 step(최소 조절 단위) 적절히 적용
- [ ] **LINK, MENU, TITLE** 등 세팅 빌더 기능 적절히 사용
- [ ] **LINK** 제공 시, **새 탭으로 열기** 함께 제공

#### RICH_TEXT 사용법
```handlebars
<!-- ✅ 올바른 방법 - 3중 괄호 -->
{{{property.richTextContent}}}

<!-- ❌ 잘못된 방법 - HTML 이스케이프됨 -->
{{property.richTextContent}}
```

```json
{
  "id": "description",
  "type": "RICH_TEXT",
  "exclude": ["fontSize", "fontColor"]  // 불필요한 속성 숨김
}
```

#### RANGE 사용법
```json
{
  "id": "padding",
  "type": "RANGE",
  "min": 0,
  "max": 100,
  "step": 5,        // 5px 단위로 조절
  "unit": "px"      // 단위 표시
}

{
  "id": "opacity",
  "type": "RANGE",
  "min": 0,
  "max": 1,
  "step": 0.1,      // 소수점 가능
  "unit": ""
}
```

---

## 5. 고품질 블록 인증 기준

> 추가 지원금 지급을 위한 고품질 판단 기준

### 5-1) 기본 필수 요건
- ✅ 블록에 버그 없음 (검수 과정에서 버그 발견 시 수정)
- ✅ 기존 마켓플레이스 블록과 기능적 또는 디자인적으로 차별화

### 5-2) 고품질 판단 기준

#### 1. 차별화된 기능
**예시:**
- 스크롤 감지 디자인 자동 전환
- 빈 링크 클릭 방지
- 컨테이너 최대 너비 제한 (데스크톱)
- 개별 아이콘 삽입 시스템
- 다국어 버튼 시스템

#### 2. 차별화된 애니메이션 효과 ⭐

**로딩/스크롤 시:**
- 헤더 높이 자동 축소
- 로고 크기 자동 축소
- 배경/그림자/블러 부드러운 전환

**hover 시:**
- 언더라인 슬라이드 (좌→우)
- 언더라인 확장 (중앙→양쪽)
- 배경 슬라이드 효과
- 투명도/색상/크기 변화

**클릭 시:**
- 모달/팝업 애니메이션
- 페이지 전환 효과

**슬라이드 시:**
- 커스텀 인디케이터
- 페이드/슬라이드/줌 전환

**구현 팁:**
```css
/* 부드러운 전환 - 0.3s 권장 */
.element {
  transition: all 0.3s ease;
}

/* 가상 요소 활용 */
.menu-item::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--color-accent-100);
  transition: width 0.3s ease;
}

.menu-item:hover::after {
  width: 100%;
}
```

#### 3. 차별화된 레이아웃
**예시:**
- 그리드 레이아웃 커스터마이징
- 비대칭 레이아웃
- 오버레이 섹션 활용
- 스크롤 시 블록 고정 기능

#### 4. 차별화된 디자인 요소
**예시:**
- 커스텀 아이콘 시스템
- 커스텀 버튼 스타일
- 커스텀 인디케이터
- 그라디언트 배경 (각도 조절)
- 블러/그림자 효과

---

## 6. 헤더/푸터 블록 전용 체크리스트

### 6-1) 헤더 블록 필수 점검

#### ✅ 체크리스트
- [ ] **빈 링크 클릭 방지**: 주소값 미설정 시 페이지 이동 없음
- [ ] **글자 크기 조정**: 헤더 내 모든 글자 크기 조정 가능 (하위 메뉴 제외 가능)
- [ ] **컨테이너 최대 너비 (데스크톱)**: max-width와 padding 조절 가능
  - 최대 너비 off: width 100%로 고정
  - 최대 너비 on: RANGE로 구체적인 px 조정

#### 빈 링크 방지 구현
```javascript
bm.container.addEventListener('click', (e) => {
  const link = e.target.closest('a');
  if (link) {
    const href = link.getAttribute('href');
    if (!href || href === '' || href === '#' || href === 'null' || href === 'undefined') {
      e.preventDefault();
      e.stopPropagation();
    }
  }
}, true);
```

#### 컨테이너 설정 구현
```json
{
  "id": "containerMaxWidthEnabled",
  "label": "컨테이너 최대 너비 제한 (데스크톱)",
  "type": "CHECKBOX"
},
{
  "id": "containerMaxWidth",
  "label": "컨테이너 최대 너비",
  "type": "RANGE",
  "min": 800,
  "max": 1920,
  "step": 40,
  "unit": "px",
  "isVisible": "property.containerMaxWidthEnabled === true"
},
{
  "id": "containerPaddingDesktop",
  "label": "컨테이너 좌우 여백 (데스크톱)",
  "type": "RANGE",
  "min": 0,
  "max": 200,
  "step": 10,
  "unit": "px",
  "isVisible": "property.containerMaxWidthEnabled === true"
}
```

```css
.header-container {
  {{#if property.containerMaxWidthEnabled}}
  max-width: {{property.containerMaxWidth}}px;
  margin: 0 auto;
  padding: 0 {{property.containerPaddingDesktop}}px;
  {{else}}
  max-width: 100%;
  padding: 0 {{property.headerPadding}}px;
  {{/if}}
}
```

### 6-2) 푸터 블록 필수 점검

#### ✅ 체크리스트
- [ ] **빈 링크 클릭 방지**: 주소값 미설정 시 페이지 이동 없음
- [ ] **국내 쇼핑몰 법적 필수 정보**: 다음 정보가 모두 포함되어야 함
  - 상호명
  - 대표자명
  - 사업자등록번호
  - 통신판매업 신고번호
  - 주소
  - 대표 전화번호
  - 이메일 주소

---

## 7. 개발 워크플로우

### 7-1) 개발 순서
1. **요구사항 분석**
   - 필요한 기능 목록 작성
   - LIST 중첩 여부 확인
   - 필드 수 계산 (LINK 필드 특히 주의)

2. **설정 스키마 설계** (JSON)
   - TITLE로 섹션 구분
   - isVisible로 조건부 표시
   - Reserved ID 피하기
   - camelCase 명명

3. **템플릿 작성** (HTML)
   - Handlebars.js 문법 사용
   - 3중 괄호로 HTML 렌더링
   - 테마 CSS 변수 적용

4. **스타일 작성** (CSS)
   - Handlebars.js로 동적 스타일
   - 애니메이션 효과 추가 (0.3s 권장)
   - 반응형 처리 (@media)

5. **스크립트 작성** (JavaScript)
   - bm.container에 이벤트 위임
   - bm.onContextChange 구현
   - bm.onDestroy 구현 (필요 시)
   - console.log 제거

6. **테스트**
   - 에디터에서 설정 변경 → 즉시 반영 확인
   - 데스크톱/모바일 양쪽 확인
   - 동일 블록 여러 개 추가 → 독립 작동 확인
   - 빈 링크 클릭 → 페이지 이동 없음 확인

7. **QA 체크리스트 점검**
   - 필수 요건 모두 충족
   - 커스텀 블록 점검표 완료
   - 고품질 기준 충족 (차별화 요소)

### 7-2) 자주 발생하는 오류

#### 에러 1: "중첩 설정 불가"
```
중첩 설정(LIST, COLOR_SCHEME_LIST)과 그룹 설정(ACCORDION, TAB)은 하위에 포함될 수 없어요.
```
**해결:** LIST 안에 LIST 제거. 개별 필드로 분리하되, 필드 수 최소화.

#### 에러 2: "설정 값이 저장되지 않음"
**증상:** 설정 입력 후 재진입 시 초기화됨
**원인:** LIST 내부 LINK 필드가 너무 많음 (5개 이상)
**해결:** LINK 필드 개수 줄이기 (1-2개 권장)

#### 에러 3: "설정 변경이 즉시 반영 안 됨"
**원인:** `bm.onContextChange` 미구현
**해결:**
```javascript
bm.onContextChange = () => {
  // 초기화 로직 재실행
  init();
};
```

#### 에러 4: "이벤트가 작동 안 함"
**원인:** 개별 요소에 직접 이벤트 바인딩 (템플릿 재렌더링 시 무효화)
**해결:** `bm.container`에 이벤트 위임
```javascript
bm.container.addEventListener('click', (e) => {
  if (e.target.closest('.my-button')) {
    // 처리
  }
}, true);
```

#### 에러 5: "SVG가 문자열로 표시됨"
**원인:** 2중 괄호 사용 (`{{iconSvg}}`)
**해결:** 3중 괄호 사용 (`{{{iconSvg}}}`)

#### 에러 6: "지원하지 않는 settingType입니다. (Input: DIVIDER)"
**증상:** 블록 저장 시 오류 발생
**원인:** DIVIDER 타입은 SixShop Pro Block Maker에서 지원하지 않음
**해결:** DIVIDER를 모두 제거하고 TITLE 타입만 사용
```json
// ❌ 잘못된 예시
{
  "type": "TITLE",
  "content": "섹션 제목"
},
{
  "type": "DIVIDER"  // 오류 발생!
},

// ✅ 올바른 예시
{
  "type": "TITLE",
  "content": "섹션 제목"  // TITLE만으로도 구분선이 자동 생성됨
},
```

---

## 8. 최종 체크리스트

배포 전 최종 점검:

### 코드 품질
- [ ] console.log 모두 제거
- [ ] bm.onContextChange 구현 (JS에서 property 사용 시)
- [ ] bm.onDestroy 구현 (window 이벤트 리스너 사용 시)
- [ ] 이벤트를 bm.container에 위임
- [ ] SVG는 3중 괄호로 렌더링

### 설정 스키마
- [ ] Reserved ID 미사용
- [ ] camelCase 명명
- [ ] LIST 중첩 없음
- [ ] LIST 내부 LINK 필드 최소화 (1-2개)
- [ ] TITLE로 섹션 구분
- [ ] DIVIDER 타입 미사용 (지원하지 않음)
- [ ] isVisible 적절히 사용
- [ ] 모든 문구 한글 작성

### 기능 테스트
- [ ] 에디터에서 설정 변경 시 즉시 반영
- [ ] 데스크톱/모바일 정상 작동
- [ ] 동일 블록 여러 개 독립 작동
- [ ] 빈 링크 클릭 방지 (헤더/푸터)
- [ ] 커스텀 섹션 너비 준수

### 디자인 품질
- [ ] 테마 폰트 적용
- [ ] COLOR_SCHEME CSS 변수 사용
- [ ] 고품질 이미지 사용
- [ ] 일관된 디자인 스타일
- [ ] 차별화된 애니메이션 효과 (0.3s 전환)

### 차별화 요소 (고품질 인증용)
- [ ] 차별화된 기능 1개 이상
- [ ] 차별화된 애니메이션 효과 3개 이상
- [ ] 차별화된 레이아웃 또는 디자인 요소

---

## 9. 참고 문서

- [블록 메이커 안내서](https://help.pro.sixshop.com/design/block-maker)
- [커스텀 섹션 활용하기](https://help.pro.sixshop.com/design/contents/custom)
- [이미지 영역별 권장 사이즈](https://help.pro.sixshop.com/design/image-guide)
- [스크롤 시 블록 고정 기능](https://help.pro.sixshop.com/design/default-page/sticky)

---

## 10. 이미지 공유 방법

스크린샷이나 이미지를 보여주고 싶을 때:

### ✅ 올바른 방법
1. 스크린샷을 프로젝트 폴더에 저장 (예: `screenshot.png`, `error.jpg`)
2. 파일 경로를 알려주기: "이미지 확인해: screenshot.png"
3. Read 도구로 이미지를 직접 확인 가능

### ❌ 불가능한 방법
- 채팅창에 직접 이미지 붙여넣기 (지원 안 함)
- 클립보드에서 직접 붙여넣기 (지원 안 함)

**중요:** 파일로 저장 후 경로를 알려주는 방법만 가능합니다.

---

## 11. CSS 레이아웃 디버깅 프로토콜 (필수)

### 문제 상황
2024년 11월 개발 중 유틸리티 메뉴/다국어 버튼이 화면 오른쪽 끝에 정렬되지 않는 문제로 **24시간 이상 소요**. 원인: Flexbox 중첩 구조에서 `margin-left: auto` 누락.

### 11-1) 레이아웃 문제 발생 시 즉시 확인 프로토콜

**정렬/여백 문제가 발생하면 즉시 다음을 요청:**

```
1. 브라우저 F12 개발자 도구 열기
2. 문제 요소 우클릭 → 검사
3. Elements 패널에서 요소 클릭 → 보라색/파란색 박스 확인
4. 스크린샷 촬영하여 GitHub에 업로드
```

**확인 포인트:**
- 요소의 실제 크기 (width × height)
- 내부 콘텐츠가 요소 내 어디에 위치하는가?
- 원하는 위치와 실제 위치의 차이가 어느 레이어에서 발생하는가?

### 11-2) Flexbox/Grid 정렬 핵심 원칙

#### 원칙 1: 부모의 정렬 ≠ 자식의 위치

```css
/* ❌ 잘못된 가정 */
.parent {
  display: flex;
  justify-content: flex-end;  /* 이것만으로는 불충분! */
}
.child {
  /* 자식은 여전히 왼쪽에 위치할 수 있음 */
}

/* ✅ 올바른 구현 */
.parent {
  display: flex;
  justify-content: flex-end;
}
.child {
  margin-left: auto;  /* 🔑 핵심: 남은 공간을 모두 차지 */
}
```

#### 원칙 2: Container padding 상쇄 패턴

```css
/* 문제: Container에 padding이 있어서 끝까지 안 닿음 */
.container {
  padding: 0 40px;  /* 좌우 40px 여백 */
}

/* ✅ 해결: 음수 margin + padding으로 제어 */
.content {
  margin-right: -40px;    /* Container padding 상쇄 */
  padding-right: 40px;     /* 사용자 조절 가능한 최종 여백 */
  margin-left: auto;       /* 오른쪽 끝으로 밀기 */
}
```

### 11-3) 오른쪽/왼쪽 끝 정렬 체크리스트

화면 끝에 요소를 정렬할 때 **반드시** 확인:

- [ ] **Step 1**: 부모 컨테이너의 padding 확인 (개발자 도구)
- [ ] **Step 2**: 요소가 부모 내에서 차지하는 공간 확인 (보라색 영역)
- [ ] **Step 3**: 내부 콘텐츠가 요소 내 어디에 위치하는지 확인
- [ ] **Step 4**: 필요 시 `margin-left: auto` 또는 `margin-right: auto` 적용
- [ ] **Step 5**: Container padding 상쇄 필요 시 음수 margin 사용
- [ ] **Step 6**: 개발자 도구로 최종 검증

### 11-4) 중첩 레이아웃 디버깅 순서

Grid 안의 Flex 안의 Flex 같은 중첩 구조:

```
1. 가장 바깥 레이어부터 확인 (예: .header-container)
   → Grid columns 비율 확인
   → Padding 확인

2. 중간 레이어 확인 (예: .header-right)
   → Flex direction, justify-content 확인
   → 자식 요소가 차지하는 공간 확인

3. 가장 안쪽 레이어 확인 (예: .header-right-content)
   → margin-left/right: auto 필요한지 확인
   → 음수 margin 필요한지 확인
```

### 11-5) 자주하는 실수 패턴

#### 실수 1: margin: auto의 위력 간과
```css
/* ❌ 복잡한 calc() 시도 */
.element {
  margin-right: calc(100% - var(--something));
}

/* ✅ 간단한 auto 사용 */
.element {
  margin-left: auto;  /* 훨씬 단순하고 확실함 */
}
```

#### 실수 2: 마지막 요소에만 margin 적용
```css
/* ❌ 마지막 요소에만 적용 - 효과 제한적 */
.container > *:last-child {
  margin-right: -40px;
}

/* ✅ 컨테이너 자체에 적용 - 더 확실함 */
.container {
  margin-right: -40px;
  margin-left: auto;
}
```

#### 실수 3: 브라우저 검증 없이 수정 반복
```
❌ CSS 수정 → 추측 → 다시 수정 → 추측 (반복)
✅ CSS 수정 → 개발자 도구 확인 → 정확한 원인 파악 → 수정
```

### 11-6) 긴급 디버깅 프로세스

레이아웃 문제가 3회 이상 시도해도 해결 안 되면:

1. **즉시 중단**
2. **개발자 도구 스크린샷 요청**
3. **다음 정보 수집:**
   - 문제 요소의 실제 크기
   - 부모/자식 관계
   - 현재 적용된 margin, padding 값
4. **근본 원인 파악 후 수정**

### 11-7) 핵심 교훈

> "추측하지 말고, 개발자 도구로 확인하라"

- CSS는 눈으로 보고 판단 불가
- 요소의 경계, 여백을 시각화해야 정확한 원인 파악 가능
- 24시간 허비하지 않으려면: **즉시 개발자 도구 확인**

---

## 버전 이력

- **v1.6** (2025-11-11): JSON-HTML 동기화 및 가이드라인 자동 업데이트 원칙 추가
  - 섹션 0-4: JSON 설정-HTML 코드 동기화 원칙
  - 섹션 0-5: 가이드라인 자동 업데이트 원칙
  - 모바일 옵션 작동 불가 사례 문서화
  - 문제 해결 후 가이드라인 업데이트 프로세스 정의
- **v1.5** (2025-11-11): AI 에이전트 개발 지침 추가 (섹션 0)
  - 토큰 사용량 최소화 원칙
  - description 100자 제한 명시
  - DIVIDER/DESCRIPTION 타입 제약 명시
  - 간결한 보고 패턴 정의
- **v1.4** (2025-11-11): 에디터 UX 원칙 추가 (섹션 4-1)
  - 비개발자 사용자 중심 설계 원칙
  - 항목 배치 순서 원칙 (시선 흐름 기반)
  - 그룹핑과 설명 작성 가이드
  - 라벨과 설명 작성 best practice
  - 헤더 블록 권장 순서 예시
- **v1.3** (2025-11-10): DIVIDER 타입 지원 불가 문서화
  - DIVIDER 타입 미지원 명시 (섹션 4-1)
  - 에러 6 추가: DIVIDER 저장 오류 해결법 (섹션 7-2)
  - 최종 체크리스트에 DIVIDER 미사용 항목 추가 (섹션 8)
- **v1.2** (2025-11-10): CSS 레이아웃 디버깅 프로토콜 추가 (섹션 11)
  - 24시간 허비 사례 분석
  - Flexbox/Grid 정렬 원칙
  - 오른쪽/왼쪽 끝 정렬 체크리스트
  - 긴급 디버깅 프로세스
- **v1.1** (2025-11-10): 이미지 공유 방법 추가
- **v1.0** (2025-11-10): 초기 작성
  - 블록 메이커 기본 개념
  - SixShop Pro 제약사항
  - QA 체크리스트 통합
  - 고품질 블록 인증 기준
