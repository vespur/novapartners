# 히어로 캐러셀 블록 문제점 분석 및 해결 방안

## 📋 문제점 분석

### 1. 하단 게이지, 스크롤 힌트, 비디오 캡션 - 하단 여백 통일

**문제:**
- 스크롤 힌트: `--scroll-hint-offset-y` (%)
- 비디오 캡션: `bottom: var(--global-pad-y)` (%)
- 데스크톱 게이지: `--gauge-bottom-desktop` (px)
- **서로 다른 단위와 변수 사용**

**원인:**
각 요소가 독립적인 변수를 사용하여 통일된 조절 불가능

**해결 방안:**
- 새로운 공통 변수 `--bottom-elements-offset` 추가 (px 단위)
- 세 요소 모두 이 변수 사용
- Settings에 "하단 요소 여백" 통합 옵션 추가

---

### 2. 하단 게이지(데스크톱) 문제들

#### 2-1. 왼쪽 정렬 시 여백 조절 불가

**문제:**
```javascript
if(confObj.gaugeAlign === "left"){
  wrap.style.left = globalPadX + '%';  // 고정된 전역 패딩 사용
}
```

**원인:**
`globalPadX`를 하드코딩하여 사용자가 조절 불가능

**해결 방안:**
- `gaugeOffsetXPercentDesktop`을 절대 위치로 사용
- 왼쪽 정렬 시: `left: offset%`
- 오른쪽 정렬 시: `right: offset%`

#### 2-2. 테두리 옵션

**문제:**
- `gaugeTrackBorderWidth/Color`
- `gaugeFillBorderWidth/Color`
- 사용자에게 혼란을 줄 수 있는 불필요한 옵션

**해결 방안:**
Settings에서 해당 필드 제거

#### 2-3. 번호 색상 작동 안 함

**문제:**
```javascript
const activeColor = cs.getPropertyValue('--counter-active-color').trim() || '#fff';
b.style.color = activeColor;
```

**원인:**
CSS 변수를 읽어오는 타이밍 문제 또는 변수명 불일치

**해결 방안:**
- CSS에서 직접 적용: `.hero-step-counter-bubble { color: var(--counter-inactive-color); }`
- 활성 상태는 data 속성으로: `[data-active="true"] { color: var(--counter-active-color); }`

#### 2-4. 비활성 트랙 색상 추가

**문제:**
```css
.hero-progress-step-shell {
  background-color: var(--gauge-track-color); /* 모든 트랙 동일 */
}
```

**원인:**
활성/비활성 트랙 색상 구분 없음

**해결 방안:**
- 새 변수 `--gauge-track-inactive-color` 추가
- CSS: `.hero-progress-step-shell { background-color: var(--gauge-track-inactive-color); }`
- 활성: `.hero-progress-step-shell[aria-current="true"] { background-color: var(--gauge-track-color); }`

---

### 3. 모바일 게이지바 표시 안 됨

**문제:**
```css
.hero-progress-wrapper-mobilebar {
  display: none; /* 기본 숨김 */
}

@media(max-width:768px){
  .hero-progress-wrapper-mobilebar{
    display:block; /* 미디어 쿼리에서 표시 */
  }
}
```

```javascript
if(!confObj.mobileBarShow || confObj.mode !== 'singleBar'){
  barWrap.style.display = 'none'; // JS에서 다시 숨김
}
```

**원인:**
1. CSS에서 `display: none` 기본값
2. 미디어 쿼리에서 `display: block` 설정
3. JS에서 조건에 따라 다시 `display: none` 덮어씀
4. **순서 문제**: CSS 미디어 쿼리 후 JS가 실행되면서 다시 숨겨짐

**해결 방안:**
- CSS에서 `display: none` 제거
- JS에서만 표시/숨김 제어
- 모바일 체크: `window.innerWidth <= 768`

---

### 4. CTA 버튼 크기 및 옵션

#### 4-1. 크기 설정 작동 안 함

**문제:**
```css
/* CSS 변수만 정의되고 실제 적용 안 됨 */
--cta-width-px: ...;
--cta-height-px: ...;
--cta-font-size-px: ...;
```

**원인:**
변수는 정의했으나 `.hero-cta` 클래스에 적용하는 코드 누락

**해결 방안:**
```css
.hero-cta {
  min-width: var(--cta-width-desktop);
  min-height: var(--cta-height-desktop);
  font-size: var(--cta-font-size-desktop);
}

@media(max-width:768px) {
  .hero-cta {
    min-width: var(--cta-width-mobile);
    min-height: var(--cta-height-mobile);
    font-size: var(--cta-font-size-mobile);
  }
}
```

#### 4-2. Underline 옵션 삭제

**문제:**
불필요한 underline 관련 변수 및 CSS 존재

**해결 방안:**
- `--cta-hover-underline-color` 변수 제거
- `.hero-cta[data-anim="underline"]` CSS 제거
- Settings에서 underline 옵션 제거

#### 4-3. Glow 스캔 방향 추가

**문제:**
현재 각도만 조절 가능, 방향(좌→우, 우→좌, 상→하, 하→상) 직관적 선택 불가능

**해결 방안:**
Settings에 RADIO 추가:
```json
{
  "id": "style.ctaGlowDirection",
  "label": "Glow 스캔 방향",
  "type": "RADIO",
  "options": [
    {"label": "왼쪽 → 오른쪽", "value": "left-to-right"},
    {"label": "오른쪽 → 왼쪽", "value": "right-to-left"},
    {"label": "위 → 아래", "value": "top-to-bottom"},
    {"label": "아래 → 위", "value": "bottom-to-top"}
  ]
}
```

CSS:
```css
.hero-cta[data-glow-direction="left-to-right"] .hero-cta-scan-highlight {
  background-position: 0% 0%;
  animation: glowLTR ...;
}
```

---

### 5. 화살표 - 글래스 블러 배경색

**문제:**
```css
.hero-nav-arrow[data-style="glass-blur"]{
  background-color: rgba(0,0,0,0.3); /* 하드코딩 */
  backdrop-filter: blur(8px);
}
```

**원인:**
`--nav-arrow-bg-color` 변수를 무시하고 하드코딩된 값 사용

**해결 방안:**
```css
.hero-nav-arrow[data-style="glass-blur"]{
  background-color: var(--nav-arrow-bg-color);
  backdrop-filter: blur(8px);
}
```

---

### 6. 전체 레이아웃

#### 6-1. 100vh 넘침 문제

**문제:**
```css
.hero-carousel-block {
  height: var(--hero-section-height); /* 100vh */
  max-height: 100vh;
}
```

**원인 분석:**
1. **모바일 브라우저 UI 바**: 주소창/툴바가 뷰포트 높이에 포함됨
2. **Box-sizing**: `content-box`일 경우 패딩이 높이에 추가됨
3. **부모 요소 여백**: 커스텀 섹션의 margin/padding

**해결 방안:**
```css
.hero-carousel-block {
  box-sizing: border-box; /* 패딩 포함 */
  height: var(--hero-section-height);
  max-height: 100vh;
  max-height: 100dvh; /* 동적 뷰포트 높이 (최신 브라우저) */
}

/* 또는 */
.hero-carousel-block {
  height: calc(var(--hero-section-height) - var(--section-margin, 0px));
}
```

#### 6-2. 좌우/상하 패딩 적용 범위

**문제:**
현재 `--global-pad-x/y`가 일부 요소에만 적용:
- Content wrapper: ✅
- 캡션: ✅
- 화살표: ❌ (독립적인 offset 사용)
- 게이지: ❌ (독립적인 offset 사용)

**해결 방안:**
모든 절대 위치 요소에 전역 패딩 반영:
```javascript
// 화살표 위치 계산 시
const arrowOffsetX = globalPadX + confObj.navArrowOffsetX;

// 게이지 위치 계산 시
const gaugeLeft = globalPadX + confObj.gaugeOffsetX;
```

---

## 📊 수정 우선순위

### High (필수)
1. ✅ 모바일 게이지바 표시 수정
2. ✅ CTA 버튼 크기 적용
3. ✅ 화살표 배경색 수정
4. ✅ 100vh 넘침 문제

### Medium (중요)
1. ✅ 하단 여백 통일
2. ✅ 번호 색상 수정
3. ✅ 비활성 트랙 색상 추가
4. ✅ 왼쪽 정렬 여백 조절

### Low (개선)
1. ✅ 테두리 옵션 제거
2. ✅ Underline 옵션 제거
3. ✅ Glow 방향 추가
4. ✅ 패딩 적용 범위 확장

---

## 🔧 수정 계획

### 1단계: CSS 변수 재구성
- 불필요한 변수 제거
- 새 변수 추가 (bottom-offset, inactive-track-color 등)
- CTA 크기 변수 적용

### 2단계: Settings 수정
- 테두리 옵션 제거
- Underline 옵션 제거
- Glow 방향 추가
- 하단 여백 통합
- 데스크탑/모바일 CTA 크기 분리

### 3단계: JavaScript 수정
- 모바일 게이지바 표시 로직 수정
- 게이지 위치 계산 개선
- 번호 색상 적용 방식 변경
- 패딩 적용 범위 확장

### 4단계: Template 수정
- 불필요한 인라인 스타일 제거
- Data 속성 추가 (glow-direction 등)

### 5단계: 통합 테스트
- 데스크탑/모바일 양쪽 확인
- 모든 옵션 조합 테스트
- 크기 조정 반응성 확인
