# 히어로 캐러셀 v3 추가 문제점 분석

## 문제 개요

v3 배포 후 발견된 4가지 문제점:
1. Glow 스캔 방향: 왼쪽→오른쪽, 위→아래 작동 안 함
2. 화살표 버튼: 화면 끝과 버튼 사이 여백 존재
3. 모바일 게이지바: 모바일 화면에서 표시 안 됨
4. 전역 패딩: 화살표와 게이지가 좌우 패딩 영향 안 받음

---

## 문제 1: Glow 스캔 방향 작동 불량

### 증상
- "왼쪽→오른쪽" (left-to-right) 작동 안 함
- "위→아래" (top-to-bottom) 작동 안 함

### 원인 분석

#### 현재 코드 (hero-carousel-v3-fixed.html)

**기본 스타일 (line 265-273):**
```css
.hero-cta .hero-cta-scan-highlight {
	background-size: 200% 100%;  /* 수평 방향 기본값 */
}
```

**방향별 스타일 (line 276-289):**
```css
/* 왼쪽→오른쪽 */
.hero-cta[data-glow-direction="left-to-right"] .hero-cta-scan-highlight {
	background-image: linear-gradient(90deg, ...);
	/* background-size 오버라이드 없음 → 200% 100% 사용 */
}

/* 위→아래 */
.hero-cta[data-glow-direction="top-to-bottom"] .hero-cta-scan-highlight {
	background-image: linear-gradient(180deg, ...);
	background-size: 100% 200%;  /* 오버라이드 */
}
```

**hover 시 애니메이션 (line 297-312):**
```css
/* 수평 방향 */
.hero-cta[data-glow-direction="left-to-right"][data-anim="glow"]:hover .hero-cta-scan-highlight {
	animation: heroCtaScan ...;
}

/* 수직 방향 */
.hero-cta[data-glow-direction="top-to-bottom"][data-anim="glow"]:hover .hero-cta-scan-highlight {
	animation: heroCtaScanVertical ...;
}
```

### 근본 원인

CSS 우선순위 문제가 아니라, **기본 background-size가 모든 요소에 적용**되는 것이 문제입니다.

#### 시나리오 1: left-to-right가 안 되는 이유
- 기본 background-size가 200% 100%로 설정됨 ✓
- left-to-right는 background-size를 오버라이드하지 않음 ✓
- **하지만** 만약 이전에 다른 방향(예: top-to-bottom)을 선택했다가 left-to-right로 변경하면, 캐시된 스타일이나 브라우저 렌더링 순서 문제로 100% 200%가 남아있을 수 있음

#### 시나리오 2: top-to-bottom이 안 되는 이유
- background-size를 100% 200%로 오버라이드함 ✓
- 하지만 기본 200% 100%의 우선순위가 더 높게 적용될 수 있음 (specificity는 같음)

### 해결 방안

**옵션 A: 각 방향에 명시적으로 background-size 설정**
```css
.hero-cta[data-glow-direction="left-to-right"] .hero-cta-scan-highlight {
	background-size: 200% 100%;
}
.hero-cta[data-glow-direction="right-to-left"] .hero-cta-scan-highlight {
	background-size: 200% 100%;
}
.hero-cta[data-glow-direction="top-to-bottom"] .hero-cta-scan-highlight {
	background-size: 100% 200%;
}
.hero-cta[data-glow-direction="bottom-to-top"] .hero-cta-scan-highlight {
	background-size: 100% 200%;
}
```

**옵션 B: 기본 background-size 제거**
```css
.hero-cta .hero-cta-scan-highlight {
	/* background-size 제거 */
}
```

**선택: 옵션 A (명시적 설정)** - 더 명확하고 안전함

---

## 문제 2: 화살표 버튼 여백

### 증상
- 깃허브 스크린샷에서 화면 좌측/우측 끝과 화살표 사이에 여백 보임

### 원인 분석

#### 현재 코드 (hero-carousel-v3-fixed.html line 855-874)

```javascript
function applyArrowPositions(confObj){
	const arrowOffsetX = confObj.navArrowOffsetXPercent || 5;

	leftArrow.style.left = arrowOffsetX + '%';
	rightArrow.style.right = arrowOffsetX + '%';
}
```

#### CSS (line 324-356)
```css
.hero-nav-arrow {
	position: absolute;
	width: calc(var(--nav-arrow-size) * 2);  /* 예: 20px * 2 = 40px */
	/* left/right는 JS에서 설정 */
}
```

### 근본 원인

여백이 보이는 이유:
1. **기본값 5%**: navArrowOffsetXPercent의 기본값이 5%
2. **버튼 크기 미포함**: left: 5%는 버튼의 **왼쪽 끝**이 화면에서 5% 떨어진 위치
3. **시각적 여백**: 40px 버튼 + 5% 여백 = 상당한 시각적 거리

### 해결 방안

**옵션 A: 기본값을 0%로 변경**
```javascript
const arrowOffsetX = confObj.navArrowOffsetXPercent || 0;  // 5 → 0
```

**옵션 B: 설정 설명 수정**
```json
{
  "description": "화면 좌우 끝에서 화살표까지의 여백입니다. 0을 입력하면 화면 끝에 딱 붙습니다."
}
```

**선택: 옵션 A + B 동시 적용**

---

## 문제 3: 모바일 게이지바 표시 안 됨

### 증상
- 모바일에서 게이지바 표시 옵션을 켜도 화면에 나타나지 않음

### 원인 분석

#### 현재 코드 (hero-carousel-v3-fixed.html line 876-899)

```javascript
function applyMobileBarStyles(confObj){
	const barWrap = root.querySelector('[data-gauge-mobilebar]');
	if(!barWrap) return;

	const isMobile = window.innerWidth <= 768;

	// 조건: mobileBarShow && singleBar && 모바일
	if(!confObj.mobileBarShow || confObj.mode !== 'singleBar' || !isMobile){
		barWrap.style.display = 'none';
	}else{
		barWrap.style.display = 'block';
	}

	// 위치 설정
	if(confObj.mobileBarPosition === "bottom"){
		barWrap.style.bottom = '0px';
		barWrap.style.top = 'auto';
	}else{
		barWrap.style.top = '0px';
		barWrap.style.bottom = 'auto';
	}
}
```

#### CSS (line 465-474)
```css
.hero-progress-wrapper-mobilebar {
	position: fixed;
	z-index: var(--mobilebar-z);
	display: none;  /* 기본값 */
}
```

### 근본 원인

#### 가능한 원인 1: 초기화 시점 문제
- `applyMobileBarStyles`가 호출되는 시점에 `window.innerWidth`가 아직 정확하지 않을 수 있음
- 또는 초기 로드 시 데스크톱으로 감지되었다가 리사이즈 이벤트에서만 모바일로 변경됨

#### 가능한 원인 2: progressMode 조건
- `confObj.mode !== 'singleBar'` 조건 때문에 multiStep 모드에서는 무조건 숨겨짐
- 사용자가 multiStep 모드를 사용 중일 가능성

#### 가능한 원인 3: CSS display 우선순위
- CSS에서 `display: none`이 설정되어 있고, JS에서 `display: 'block'`으로 변경하려 하지만
- 다른 CSS 규칙이나 !important로 인해 오버라이드될 수 있음

### 해결 방안

**옵션 A: 초기화 로직 개선**
```javascript
function applyMobileBarStyles(confObj){
	const barWrap = root.querySelector('[data-gauge-mobilebar]');
	if(!barWrap) return;

	const isMobile = window.innerWidth <= 768;

	// 디버깅 로그 추가 (개발 중)
	console.log('Mobile bar check:', {
		mobileBarShow: confObj.mobileBarShow,
		mode: confObj.mode,
		isMobile: isMobile,
		windowWidth: window.innerWidth
	});

	// 조건 체크
	const shouldShow = confObj.mobileBarShow && confObj.mode === 'singleBar' && isMobile;

	barWrap.style.display = shouldShow ? 'block' : 'none';

	if(shouldShow){
		if(confObj.mobileBarPosition === "bottom"){
			barWrap.style.bottom = '0px';
			barWrap.style.top = 'auto';
		}else{
			barWrap.style.top = '0px';
			barWrap.style.bottom = 'auto';
		}
	}
}
```

**옵션 B: CSS 초기값 변경**
```css
.hero-progress-wrapper-mobilebar {
	/* display: none 제거, JS에서만 제어 */
}
```

**옵션 C: 리사이즈 이벤트 즉시 호출**
```javascript
// init() 함수 내에서
applyMobileBarStyles(confObj);
// 리사이즈 핸들러 등록 후 즉시 호출
if(init._resizeHandler){
	window.removeEventListener('resize', init._resizeHandler);
}
init._resizeHandler = function(){
	applyMobileBarStyles(getConf());  // 최신 설정으로 재호출
};
window.addEventListener('resize', init._resizeHandler);
// 초기 실행
init._resizeHandler();
```

**선택: 옵션 A + C 조합**

---

## 문제 4: 전역 패딩 미적용

### 증상
- 슬라이드 캡션만 전역 패딩(좌우) 영향을 받음
- 화살표 버튼과 데스크톱 게이지는 전역 패딩 영향 안 받음

### 원인 분석

#### 현재 코드

**슬라이드 캡션 (CSS line 172-186):**
```css
.hero-slide-caption {
	left: var(--global-pad-x);
	right: var(--global-pad-x);
	/* 전역 패딩 적용됨 ✓ */
}
```

**화살표 버튼 (JS line 855-874):**
```javascript
function applyArrowPositions(confObj){
	const arrowOffsetX = confObj.navArrowOffsetXPercent || 5;

	leftArrow.style.left = arrowOffsetX + '%';  /* 전역 패딩 무시 ✗ */
	rightArrow.style.right = arrowOffsetX + '%';  /* 전역 패딩 무시 ✗ */
}
```

**데스크톱 게이지 (JS line 822-853):**
```javascript
function applyGaugeWrapperPosition(confObj){
	const globalPadX = parseFloat(getComputedStyle(root).getPropertyValue('--global-pad-x')) || 5;
	const offsetPercent = confObj.gaugeOffsetXPercent || 0;

	if(confObj.gaugeAlign === "left"){
		wrap.style.left = 'calc(' + globalPadX + '% + ' + offsetPercent + '%)';
		/* 전역 패딩 적용됨 ✓ */
	}
}
```

### 근본 원인

v3에서 화살표를 화면 끝 기준으로 변경하면서 **의도적으로** 전역 패딩을 무시했습니다.
- v2: 전역 패딩 + offset
- v3: 화면 끝에서 offset만 (전역 패딩 무시)

하지만 사용자는 **일관성**을 원합니다:
- 슬라이드 캡션: 전역 패딩 적용
- 화살표 버튼: 전역 패딩 적용
- 데스크톱 게이지: 전역 패딩 적용

### 해결 방안

**화살표 버튼에 전역 패딩 재적용:**
```javascript
function applyArrowPositions(confObj){
	const leftArrow = root.querySelector('[data-nav-prev]');
	const rightArrow = root.querySelector('[data-nav-next]');

	if(!leftArrow || !rightArrow) return;

	const globalPadX = parseFloat(getComputedStyle(root).getPropertyValue('--global-pad-x')) || 5;
	const arrowOffsetX = confObj.navArrowOffsetXPercent || 0;  // 기본값 0

	// 전역 패딩 + offset
	leftArrow.style.left = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)';
	leftArrow.style.top = (confObj.navArrowOffsetYPercent || 50) + '%';
	leftArrow.style.transform = 'translateY(-50%)';

	rightArrow.style.right = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)';
	rightArrow.style.top = (confObj.navArrowOffsetYPercent || 50) + '%';
	rightArrow.style.transform = 'translateY(-50%)';
}
```

**데스크톱 게이지는 이미 적용됨 ✓**

---

## 수정 우선순위

### 높음 (기능 오류)
1. ✅ 모바일 게이지바 표시 안 됨 (문제 3)
2. ✅ Glow 스캔 방향 작동 불량 (문제 1)

### 중간 (설계 일관성)
3. ✅ 전역 패딩 미적용 (문제 4)

### 낮음 (기본값 조정)
4. ✅ 화살표 여백 (문제 2)

---

## 파일 수정 계획

### hero-carousel-v3-fixed.html

#### CSS 수정 (line 275-289)
```css
/* 기본 background-size 제거 또는 주석 처리 */
.hero-cta .hero-cta-scan-highlight {
	/* background-size: 200% 100%; → 제거 */
}

/* 각 방향에 명시적으로 설정 */
.hero-cta[data-glow-direction="left-to-right"] .hero-cta-scan-highlight {
	background-image: linear-gradient(90deg, ...);
	background-size: 200% 100%;
}
.hero-cta[data-glow-direction="right-to-left"] .hero-cta-scan-highlight {
	background-image: linear-gradient(270deg, ...);
	background-size: 200% 100%;
}
.hero-cta[data-glow-direction="top-to-bottom"] .hero-cta-scan-highlight {
	background-image: linear-gradient(180deg, ...);
	background-size: 100% 200%;
}
.hero-cta[data-glow-direction="bottom-to-top"] .hero-cta-scan-highlight {
	background-image: linear-gradient(0deg, ...);
	background-size: 100% 200%;
}
```

#### JavaScript 수정

**applyArrowPositions (line 855-874):**
```javascript
function applyArrowPositions(confObj){
	const globalPadX = parseFloat(getComputedStyle(root).getPropertyValue('--global-pad-x')) || 5;
	const arrowOffsetX = confObj.navArrowOffsetXPercent || 0;  // 5 → 0

	leftArrow.style.left = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)';
	rightArrow.style.right = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)';
}
```

**applyMobileBarStyles (line 876-899):**
```javascript
function applyMobileBarStyles(confObj){
	const barWrap = root.querySelector('[data-gauge-mobilebar]');
	if(!barWrap) return;

	const isMobile = window.innerWidth <= 768;
	const shouldShow = confObj.mobileBarShow && confObj.mode === 'singleBar' && isMobile;

	barWrap.style.display = shouldShow ? 'block' : 'none';

	if(shouldShow){
		if(confObj.mobileBarPosition === "bottom"){
			barWrap.style.bottom = '0px';
			barWrap.style.top = 'auto';
		}else{
			barWrap.style.top = '0px';
			barWrap.style.bottom = 'auto';
		}
	}
}
```

**getConf (line 796-819):**
```javascript
navArrowOffsetXPercent: (s.navArrowOffsetXPercent !== undefined && s.navArrowOffsetXPercent !== "") ? +s.navArrowOffsetXPercent : 0,  // 5 → 0
```

### hero-carousel-v3-settings.json

#### 화살표 여백 설명 수정 (line ~495)
```json
{
  "id": "style.navArrowOffsetXPercent",
  "label": "좌우 여백 (%)",
  "description": "전역 패딩 기준에서 추가로 더 떨어뜨릴 여백입니다. 0을 입력하면 전역 패딩만 적용됩니다. 숫자만 입력해주세요. 예: 0",
  "type": "TEXT",
  "placeholder": "0"
}
```

#### property 기본값 수정
```json
{
  "navArrowOffsetXPercent": "0"  // "5" → "0"
}
```

---

## 테스트 체크리스트

### Glow 스캔
- [ ] 왼쪽: 왼쪽에서 오른쪽으로 스캔
- [ ] 오른쪽: 오른쪽에서 왼쪽으로 스캔
- [ ] 위: 위에서 아래로 스캔
- [ ] 아래: 아래에서 위로 스캔

### 화살표 버튼
- [ ] 좌우 여백 0% → 전역 패딩만 적용
- [ ] 좌우 여백 5% → 전역 패딩 + 5%
- [ ] 전역 패딩 5% 변경 시 화살표 위치 함께 변경

### 모바일 게이지바
- [ ] 모바일 화면 (≤768px)에서 표시 옵션 활성화 시 즉시 표시
- [ ] 데스크톱에서는 표시 안 됨
- [ ] 상단/하단 위치 정상 작동

### 전역 패딩 일관성
- [ ] 슬라이드 캡션: 전역 패딩 적용 ✓
- [ ] 화살표 버튼: 전역 패딩 적용 ✓
- [ ] 데스크톱 게이지: 전역 패딩 적용 ✓
