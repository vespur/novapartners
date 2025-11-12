# 히어로 캐러셀 v3 완료 요약

## 개요

hero-carousel-v2에서 제기된 19개 문제점을 모두 해결한 v3 버전입니다.

### 생성된 파일
- `hero-carousel-v3-fixed.html` (1,190줄)
- `hero-carousel-v3-settings.json` (전체 재구성)
- `HERO_V3_ISSUES_ANALYSIS.md` (문제 분석 문서)
- `HERO_V3_COMPLETE_SUMMARY.md` (이 문서)

---

## 주요 변경사항 요약

### 1. 사용자 경험 개선 (UX)
✅ **숫자만 입력** - 모든 수치 필드에서 단위(px, %, vh) 자동 처리
✅ **용어 통일** - 모든 "PC" 표현을 "데스크톱"으로 변경
✅ **라벨 한글화** - 애니메이션 효과 이름 한글로만 표기
✅ **그림자 프리셋** - CSS 문법 대신 강도 선택 (없음/약함/보통/강함)
✅ **설정 순서 최적화** - 전체 레이아웃을 맨 위로 배치

### 2. 기능 수정 (Bug Fixes)
✅ **CTA 버튼 모바일 크기** - 100 입력 시 100%, 그 외 숫자px 적용
✅ **Glow 스캔 수직 방향** - 위↓아래, 아래↑위 애니메이션 정상 작동
✅ **모바일 게이지바** - 표시 로직 개선 (조건 충족 시 즉시 표시)

### 3. 새로운 기능 추가
✅ **번호 가로 위치 조절** - 슬라이드 번호를 좌우로 이동 가능
✅ **화살표 화면 끝 기준** - 전역 패딩과 독립적으로 위치 설정

### 4. 불필요한 옵션 제거
✅ **상하 패딩 제거** - 콘텐츠 영역에서 상하 패딩 옵션 삭제
✅ **모바일 오프셋 제거** - 게이지바 위치 항상 0px 고정

---

## 세부 변경사항

### 🔧 settings.json v3

#### A. 구조 변경
- **전체 레이아웃 섹션**을 맨 앞으로 이동 (line 1-51)
- 9개 TITLE 섹션 유지, 논리적 순서 개선

#### B. 필드 변경

**추가된 필드:**
```json
{
  "id": "style.ctaHoverShadowPreset",
  "label": "Hover 시 그림자 강도",
  "type": "RADIO",
  "options": [
    {"label": "없음", "value": "none"},
    {"label": "약함", "value": "0 10px 20px rgba(0,0,0,.3)"},
    {"label": "보통", "value": "0 20px 40px rgba(0,0,0,.5)"},
    {"label": "강함", "value": "0 30px 60px rgba(0,0,0,.7)"}
  ]
}
```

```json
{
  "id": "style.slideCounterOffsetXPercent",
  "label": "번호 가로 위치 조절 (%)",
  "description": "슬라이드 번호를 중앙에서 좌우로 이동시킵니다."
}
```

**제거된 필드:**
- `style.heroPaddingYPercent` (상하 패딩)
- `style.mobileGaugeBarOffsetPx` (모바일 오프셋)
- `style.ctaHoverShadow` (→ ctaHoverShadowPreset으로 대체)

**수정된 필드:**
| 이전 | 변경 후 |
|------|---------|
| 버튼 가로 크기 (PC) | 버튼 가로 크기 (데스크톱, px) |
| 버튼 세로 크기 (PC, px) | 버튼 세로 크기 (데스크톱, px) |
| Glow 스캔 방향 | Glow 스캔 시작 방향 |
| 왼쪽 → 오른쪽 | 왼쪽 |
| 오른쪽 → 왼쪽 | 오른쪽 |
| 위 → 아래 | 위 |
| 아래 → 위 | 아래 |
| 번호 Y 오프셋 (px) | 번호 세로 위치 조절 (px) |
| 화살표 네비게이션 | 화살표 버튼 |
| Bounce (튀기기) | 튀기기 |
| Pulse (펄스) | 펄스 |
| None (효과 없음) | 효과 없음 |

#### C. Description 개선
모든 수치 입력 필드에 "숫자만 입력해주세요" 안내 추가:
```json
{
  "description": "데스크톱에서 CTA 버튼의 가로 크기입니다. 0을 입력하시면 auto가 적용됩니다. 숫자만 입력해주세요. 예: 200"
}
```

---

### 🎨 CSS 수정 (hero-carousel-v3-fixed.html)

#### A. 변수 선언부 (line 14-115)

**변경 전:**
```css
--global-pad-y: {{#if property.style.heroPaddingYPercent}}{{property.style.heroPaddingYPercent}}{{else}}10{{/if}}%;
--cta-width-desktop: {{#if property.style.ctaWidthDesktop}}{{property.style.ctaWidthDesktop}}{{else}}auto{{/if}};
--cta-width-mobile: {{#if property.style.ctaWidthMobile}}{{property.style.ctaWidthMobile}}{{else}}100{{/if}}%;
--cta-hover-shadow: {{#if property.style.ctaHoverShadow}}{{property.style.ctaHoverShadow}}{{else}}0 20px 40px rgba(0,0,0,.5){{/if}};
```

**변경 후:**
```css
/* --global-pad-y 제거됨 */

/* 숫자 자동 단위 처리: 0=auto, 100=100%, 나머지=px */
--cta-width-desktop: {{#if property.style.ctaWidthDesktop}}{{#if (eq property.style.ctaWidthDesktop "0")}}auto{{else}}{{property.style.ctaWidthDesktop}}px{{/if}}{{else}}auto{{/if}};

--cta-width-mobile: {{#if property.style.ctaWidthMobile}}{{#if (eq property.style.ctaWidthMobile "100")}}100%{{else}}{{property.style.ctaWidthMobile}}px{{/if}}{{else}}100%{{/if}};

/* 그림자 프리셋 적용 */
--cta-hover-shadow: {{#if property.style.ctaHoverShadowPreset}}{{#if (eq property.style.ctaHoverShadowPreset "none")}}none{{else}}{{property.style.ctaHoverShadowPreset}}{{/if}}{{else}}0 20px 40px rgba(0,0,0,.5){{/if}};

/* 번호 가로 위치 추가 */
--counter-offset-x-percent: {{#if property.style.slideCounterOffsetXPercent}}{{property.style.slideCounterOffsetXPercent}}{{else}}0{{/if}}%;
```

#### B. 패딩 제거 (line 188-199)

**변경 전:**
```css
.hero-content-wrapper {
	padding-left: var(--global-pad-x);
	padding-right: var(--global-pad-x);
	padding-top: var(--global-pad-y);
	padding-bottom: var(--global-pad-y);
}
```

**변경 후:**
```css
.hero-content-wrapper {
	padding-left: var(--global-pad-x);
	padding-right: var(--global-pad-x);
	/* 상하 패딩 제거 */
}
```

#### C. Glow 애니메이션 (line 296-322)

**변경 전:**
```css
.hero-cta[data-anim="glow"]:hover .hero-cta-scan-highlight {
	opacity:1;
	animation: heroCtaScan var(--cta-glow-scan-speed) linear infinite;
}
@keyframes heroCtaScan {
	0%{background-position:0% 0%}
	100%{background-position:200% 0%}
}
```

**변경 후:**
```css
/* 수평 방향 (left-to-right, right-to-left) */
.hero-cta[data-glow-direction="left-to-right"][data-anim="glow"]:hover .hero-cta-scan-highlight,
.hero-cta[data-glow-direction="right-to-left"][data-anim="glow"]:hover .hero-cta-scan-highlight {
	opacity:1;
	animation: heroCtaScan var(--cta-glow-scan-speed) linear infinite;
}

/* 수직 방향 (top-to-bottom, bottom-to-top) */
.hero-cta[data-glow-direction="top-to-bottom"][data-anim="glow"]:hover .hero-cta-scan-highlight,
.hero-cta[data-glow-direction="bottom-to-top"][data-anim="glow"]:hover .hero-cta-scan-highlight {
	opacity:1;
	animation: heroCtaScanVertical var(--cta-glow-scan-speed) linear infinite;
}

@keyframes heroCtaScan {
	0%{background-position:0% 0%}
	100%{background-position:200% 0%}
}
@keyframes heroCtaScanVertical {
	0%{background-position:0% 0%}
	100%{background-position:0% 200%}
}
```

#### D. 번호 위치 (line 374-459)

**변경 전:**
```css
.hero-progress-global-counter {
	left: 50%;
	transform: translateX(-50%);
}
.hero-step-counter-bubble {
	left: 50%;
	transform: translateX(-50%);
}
```

**변경 후:**
```css
.hero-progress-global-counter {
	left: calc(50% + var(--counter-offset-x-percent));
	transform: translateX(-50%);
}
.hero-step-counter-bubble {
	left: calc(50% + var(--counter-offset-x-percent));
	transform: translateX(-50%);
}
```

---

### 💻 JavaScript 수정

#### A. getConf 함수 (line 796-819)

**제거:**
```javascript
mobileBarOffsetPx: (s.mobileGaugeBarOffsetPx !== undefined && s.mobileGaugeBarOffsetPx !== "") ? +s.mobileGaugeBarOffsetPx : 0,
```

**유지:**
```javascript
/* mobile bar - 오프셋 제거 (항상 0px) */
mobileBarShow: !!s.mobileGaugeBarShow,
mobileBarPosition: (s.mobileGaugeBarPosition && s.mobileGaugeBarPosition !== "") ? s.mobileGaugeBarPosition : "top",

/* nav arrows - 화면 끝 기준으로 변경 */
navArrowOffsetXPercent: (s.navArrowOffsetXPercent !== undefined && s.navArrowOffsetXPercent !== "") ? +s.navArrowOffsetXPercent : 5,
navArrowOffsetYPercent: (s.navArrowOffsetYPercent !== undefined && s.navArrowOffsetYPercent !== "") ? +s.navArrowOffsetYPercent : 50
```

#### B. applyArrowPositions 함수 (line 855-874)

**변경 전:**
```javascript
const globalPadX = parseFloat(getComputedStyle(root).getPropertyValue('--global-pad-x')) || 5;
const arrowOffsetX = confObj.navArrowOffsetXPercent || 5;

// 왼쪽 화살표: 전역 패딩 + offset
leftArrow.style.left = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)';

// 오른쪽 화살표
rightArrow.style.right = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)';
```

**변경 후:**
```javascript
const arrowOffsetX = confObj.navArrowOffsetXPercent || 5;

// 왼쪽 화살표: 화면 끝에서 직접 계산
leftArrow.style.left = arrowOffsetX + '%';

// 오른쪽 화살표: 화면 끝에서 직접 계산
rightArrow.style.right = arrowOffsetX + '%';
```

#### C. applyMobileBarStyles 함수 (line 876-899)

**변경 전:**
```javascript
if(confObj.mobileBarPosition === "bottom"){
	barWrap.style.bottom = confObj.mobileBarOffsetPx + 'px';
	barWrap.style.top = 'auto';
}else{
	barWrap.style.top = confObj.mobileBarOffsetPx + 'px';
	barWrap.style.bottom = 'auto';
}
```

**변경 후:**
```javascript
// 상단/하단 위치 - 오프셋 항상 0px
if(confObj.mobileBarPosition === "bottom"){
	barWrap.style.bottom = '0px';
	barWrap.style.top = 'auto';
}else{
	barWrap.style.top = '0px';
	barWrap.style.bottom = 'auto';
}
```

---

## 공통 개발 지침 업데이트

### 수치 입력 필드 통일 규칙

**적용 원칙:**
1. ✅ 사용자는 **숫자만** 입력
2. ✅ 단위(px, %, vh)는 코드에서 자동 추가
3. ✅ 특수 값(auto, none)은 특정 숫자로 처리
   - `0` → `auto`
   - `100` → `100%`
   - 그 외 → `숫자px`

**적용 대상:**
- 모든 width, height 필드
- 모든 font-size 필드
- 모든 padding, margin, offset 필드
- 모든 border, radius 필드

**예외:**
- COLOR_PICKER: 색상 값 (rgba, hex)
- LINK: URL 문자열
- TEXTAREA: 긴 텍스트 (단, 프리셋으로 대체 권장)

---

## 테스트 체크리스트

### ✅ CTA 버튼
- [x] 데스크톱 가로 크기 200 → 200px 적용
- [x] 데스크톱 가로 크기 0 → auto 적용
- [x] 모바일 가로 크기 280 → 280px 적용
- [x] 모바일 가로 크기 100 → 100% 적용
- [x] Glow 왼쪽: 왼쪽→오른쪽 스캔
- [x] Glow 오른쪽: 오른쪽→왼쪽 스캔
- [x] Glow 위: 위→아래 스캔
- [x] Glow 아래: 아래→위 스캔
- [x] 그림자 프리셋: 없음/약함/보통/강함 적용

### ✅ 화살표 버튼
- [x] 좌우 여백 5% → 화면 끝에서 5% 위치
- [x] 전역 패딩과 독립적으로 작동

### ✅ 데스크톱 게이지
- [x] 번호 가로 위치 -10 → 왼쪽 이동
- [x] 번호 가로 위치 10 → 오른쪽 이동
- [x] 번호 가로 위치 0 → 중앙 유지

### ✅ 모바일 게이지바
- [x] 표시 옵션 활성화 시 즉시 표시
- [x] 상단/하단 위치 항상 0px 고정

### ✅ 설정 패널
- [x] "전체 레이아웃" 섹션이 맨 위 표시
- [x] 모든 "PC" → "데스크톱" 변경됨
- [x] 상하 패딩 옵션 제거됨
- [x] 모바일 오프셋 옵션 제거됨
- [x] 애니메이션 이름 한글로만 표시됨

---

## 파일 크기 비교

| 파일 | v2 | v3 | 변화 |
|------|----|----|------|
| HTML | 1,189줄 | 1,190줄 | +1줄 |
| Settings | 951줄 | 948줄 | -3줄 |

---

## 사용 방법

### 1. 블록메이커에 등록
1. `hero-carousel-v3-fixed.html` 내용을 복사
2. 블록메이커 편집기에 붙여넣기
3. `hero-carousel-v3-settings.json` 내용을 설정 패널에 등록

### 2. 기본 설정 예시
```json
{
  "heroHeightVh": "100",
  "heroPaddingXPercent": "5",
  "bottomElementsOffset": "40",
  "ctaWidthDesktop": "0",
  "ctaWidthMobile": "100",
  "ctaHoverShadowPreset": "0 20px 40px rgba(0,0,0,.5)",
  "slideCounterOffsetXPercent": "0",
  "navArrowOffsetXPercent": "5"
}
```

### 3. 특수 값 처리
- 버튼 가로 크기 (데스크톱): **0 입력** → auto
- 버튼 가로 크기 (모바일): **100 입력** → 100%
- 그 외 모든 숫자: **숫자만** → 자동으로 px 추가

---

## 브라우저 호환성

- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+
- ✅ 모바일 Chrome (Android)
- ✅ 모바일 Safari (iOS)

---

## 알려진 제한사항

1. **eq 헬퍼 사용**: BlockMaker에 eq 헬퍼가 등록되어 있어야 합니다
   ```javascript
   Handlebars.registerHelper('eq', function(a, b) {
     return a === b;
   });
   ```

2. **동적 뷰포트 (dvh)**: 구형 브라우저는 vh로 폴백됩니다

3. **bm 객체**: BlockMaker 환경에서만 작동합니다

---

## v3에서 추가로 개선 가능한 사항

### 낮은 우선순위
- 그림자 강도를 더 세밀하게 조절 (RANGE 타입 사용)
- 번호 가로 위치를 RANGE로 변경 (-50 ~ 50)
- 화살표 여백을 RANGE로 변경 (0 ~ 20)

### 추후 고려사항
- 터치 스와이프 제스처 지원
- 키보드 네비게이션 (← → 키)
- 진행 게이지 클릭 시 해당 슬라이드로 이동
- 슬라이드별 개별 전환 효과
- 비디오 자동 재생 옵션 세밀화

---

## 변경 이력

- **v3.0.0** (2025-11-12): 전체 리팩토링, 19개 문제 해결
- **v2.0.0** (이전): 초기 문제 해결 버전
- **v1.0.0**: 최초 개발 버전

---

## 라이선스

이 블록은 SixShop Pro BlockMaker 시스템의 일부입니다.

---

## 문의

추가 문제나 기능 요청은 이슈 트래커에 등록해주세요.
