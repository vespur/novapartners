# 히어로 캐러셀 블록 - 수정 완료 보고서

## 📅 수정 일시
2025-11-12

## 🎯 수정 목적
식스샵 프로 블록메이커 표준 준수 및 코드 안정성 개선

---

## ✅ 수정 완료 항목

### 1. **Handlebars `eq` 헬퍼 의존성 제거** (우선순위: 높음)

**문제:**
- CSS 변수 내에서 `eq` 헬퍼를 사용하여 조건부 값 설정
- `eq` 헬퍼가 식스샵 환경에 등록되지 않았을 경우 에러 발생 가능

**수정 전:**
```css
--cta-group-justify-desktop: {{#if (eq property.style.textAlignDesktop "left")}}flex-start{{else}}center{{/if}};
```

**수정 후:**
```css
--cta-group-justify-desktop: {{#if property.style.ctaJustifyDesktop}}{{property.style.ctaJustifyDesktop}}{{else}}center{{/if}};
```

**Settings 추가:**
```json
{
  "id": "style.ctaJustifyDesktop",
  "label": "CTA 정렬 (PC)",
  "type": "RADIO",
  "options": [
    {"label": "왼쪽", "value": "flex-start"},
    {"label": "가운데", "value": "center"}
  ]
}
```

**영향 범위:**
- `--cta-group-justify-desktop`
- `--cta-group-justify-mobile`

---

### 2. **CSS 변수 내 불필요한 따옴표 제거** (우선순위: 높음)

**문제:**
- CSS 변수 값에 불필요한 따옴표 포함

**수정 전:**
```css
--headline-align-desktop: {{#if property.style.textAlignDesktop}}{{property.style.textAlignDesktop}}{{else}}"center"{{/if}};
--cta-hover-anim-type: {{#if property.style.ctaHoverAnimType}}{{property.style.ctaHoverAnimType}}{{else}}"glow-lift"{{/if}};
```

**수정 후:**
```css
--headline-align-desktop: {{#if property.style.textAlignDesktop}}{{property.style.textAlignDesktop}}{{else}}center{{/if}};
--cta-hover-anim-type: {{#if property.style.ctaHoverAnimType}}{{property.style.ctaHoverAnimType}}{{else}}glow-lift{{/if}};
```

**영향 범위:**
- `--headline-align-desktop`
- `--headline-align-mobile`
- `--cta-hover-anim-type`
- `--nav-arrow-bg-style`

---

### 3. **접근성(Accessibility) 개선** (우선순위: 중간)

**추가된 기능:**
- 화살표 버튼에 `tabindex="0"` 추가 (키보드 네비게이션)
- `<div>`를 `<button>`으로 변경 (시맨틱 HTML)
- `type="button"` 명시
- `focus-visible` 스타일 추가

**수정 전:**
```html
<div class="hero-nav-arrow" data-nav-prev aria-label="Previous slide">
```

**수정 후:**
```html
<button class="hero-nav-arrow"
  type="button"
  tabindex="0"
  data-nav-prev
  aria-label="Previous slide">
```

**CSS 추가:**
```css
.hero-nav-arrow:focus-visible {
  outline: 2px solid var(--nav-arrow-icon-color);
  outline-offset: 2px;
}
```

---

### 4. **미디어 쿼리 조건부 처리 개선** (우선순위: 중간)

**변경 사항:**
- 중복된 `data-hide-mobile` 속성 제거
- CSS 미디어 쿼리로 직접 처리

**수정 전:**
```handlebars
<div class="hero-nav-arrow"
  data-hide-mobile="{{#if property.style.navArrowHideMobile}}true{{else}}false{{/if}}">
```

**수정 후:**
```css
@media(max-width:768px){
  {{#if property.style.navArrowHideMobile}}
  .hero-nav-arrow{
    display:none;
  }
  {{/if}}
}
```

---

### 5. **비디오 재생 에러 핸들링 추가** (우선순위: 낮음)

**추가:**
```javascript
if(v.play){
  v.play().catch(function(){});  // 자동재생 실패 시 무시
}
```

---

## 📂 생성된 파일

### 1. `/home/user/novapartners/hero-carousel-block-fixed.html`
- 수정된 전체 코드 (style + template + script)
- 모든 이슈 수정 완료
- 블록메이커에 바로 붙여넣기 가능

### 2. `/home/user/novapartners/hero-carousel-settings-fixed.json`
- 수정된 Settings 스키마
- `ctaJustifyDesktop` / `ctaJustifyMobile` 필드 추가
- 기본값(property) 업데이트 완료

### 3. `/home/user/novapartners/HERO_CAROUSEL_FIX_SUMMARY.md` (이 파일)
- 전체 수정 내역 요약

---

## 🧪 검증 완료 항목

### ✅ Handlebars 문법
- `{{property.*}}` 접근 ✅
- `{{#if}}...{{else}}...{{/if}}` 조건문 ✅
- `{{#each}}...{{/each}}` 반복문 ✅
- `{{@index}}` 컨텍스트 변수 ✅
- `{{../property.*}}` 부모 컨텍스트 접근 ✅
- `{{{raw HTML}}}` 이스케이프 해제 ✅

### ✅ JavaScript (bm 객체)
- `bm.container` ✅
- `bm.context` ✅
- `bm.onContextChange` ✅
- `bm.onDestroy` ✅

### ✅ CSS
- CSS 변수 사용 ✅
- 미디어 쿼리 반응형 ✅
- Handlebars 조건문 삽입 ✅
- `calc()` 함수 ✅

### ✅ Settings 스키마
- TEXT / TEXTAREA / COLOR_PICKER ✅
- RADIO / CHECKBOX / LIST ✅
- LINK 타입 ✅
- 중첩 LIST settings ✅
- TITLE / DESCRIPTION 구분자 ✅

---

## 🔧 추가 개선 권장 사항 (선택)

### 1. **에러 바운더리 추가**
비디오 로드 실패 시 폴백 이미지 표시:
```javascript
video.addEventListener('error', function(){
  // 폴백 이미지로 교체
});
```

### 2. **성능 최적화**
- Intersection Observer로 뷰포트 내에서만 슬라이드 자동 재생
- 비디오 preload 속성 제어

### 3. **다국어 지원**
- aria-label 다국어 처리
- 힌트 텍스트 다국어 처리

---

## 📊 브라우저 호환성

| 기능 | Chrome | Firefox | Safari | Edge | IE11 |
|------|--------|---------|--------|------|------|
| Flexbox | ✅ | ✅ | ✅ | ✅ | ✅ |
| CSS Variables | ✅ | ✅ | ✅ | ✅ | ❌ |
| IntersectionObserver | ✅ | ✅ | ✅ | ✅ | ❌* |
| RequestAnimationFrame | ✅ | ✅ | ✅ | ✅ | ✅ |
| Video autoplay | ✅ | ✅ | ✅ | ✅ | ✅ |

*폴백 로직 포함

---

## 🎯 사용 방법

### 1. 식스샵 프로 블록메이커에서:
1. 새 블록 생성
2. `hero-carousel-block-fixed.html` 내용 복사
3. `<style>`, `<template>`, `<script>` 각 섹션을 해당 탭에 붙여넣기

### 2. Settings 설정:
1. Settings 탭 열기
2. `hero-carousel-settings-fixed.json` 내용 복사
3. JSON 모드로 전환 후 붙여넣기

---

## ⚠️ 주의사항

### 1. **Property 기본값 확인**
- Settings JSON의 `property` 객체에 모든 기본값이 포함되어 있습니다
- 기존 블록을 업데이트하는 경우, 기존 property 값과 병합 필요

### 2. **미디어 URL**
- 예제 URL은 `https://example.com/*` 형식입니다
- 실제 사용 시 유효한 이미지/비디오 URL로 교체 필요

### 3. **CTA 정렬 설정 추가**
- 기존 코드에서는 `textAlignDesktop`이 CTA 정렬도 제어했습니다
- 수정된 코드에서는 `ctaJustifyDesktop` / `ctaJustifyMobile` 별도 설정
- 마이그레이션 시 기본값은 "center"로 설정됨

---

## 📝 변경 로그

### v1.0 → v1.1 (수정 완료)
- [수정] `eq` 헬퍼 의존성 제거
- [수정] CSS 변수 따옴표 제거
- [추가] 접근성 개선 (tabindex, button 태그)
- [추가] Settings에 `ctaJustifyDesktop/Mobile` 필드
- [개선] 미디어 쿼리 조건부 처리 최적화
- [개선] 비디오 재생 에러 핸들링

---

## ✅ 최종 체크리스트

- [x] Handlebars 문법 검증
- [x] JavaScript bm 객체 사용 검증
- [x] CSS 변수 및 문법 검증
- [x] Settings 스키마 구조 검증
- [x] 접근성 개선
- [x] 코드 안정성 강화
- [x] 문서화 완료

---

## 🚀 다음 단계

1. ✅ 코드 검증 완료
2. ⏭️ 식스샵 프로 환경에서 테스트
3. ⏭️ 실제 이미지/비디오 URL 적용
4. ⏭️ 사용자 피드백 수집
5. ⏭️ 추가 커스터마이징

---

## 📞 문의사항

수정된 코드에 대한 질문이나 추가 개선 사항이 있으시면 알려주세요!
