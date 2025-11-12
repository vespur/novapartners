# 히어로 캐러셀 v3 문제점 분석 및 해결 방안

## 문제 영역 개요

총 6개 영역, 19개 세부 문제점 식별:
1. CTA 버튼 (7개 문제)
2. 화살표 버튼 (2개 문제)
3. 데스크톱 하단 게이지 (2개 문제)
4. 모바일 게이지바 (2개 문제)
5. 스크롤 힌트 (1개 문제)
6. 전체 레이아웃 (3개 문제)

---

## 1. CTA 버튼 문제

### 문제 1-1: 버튼 가로 크기에 숫자만 입력

**현재 상태:**
- 사용자가 "auto" 또는 "200px" 같은 형식으로 입력해야 함
- `ctaWidthDesktop`, `ctaWidthMobile` 필드

**문제 원인:**
- 비개발자 사용자가 CSS 단위를 이해하고 입력해야 함
- 일관성 없음 (다른 필드는 px만 입력)

**해결 방안:**
1. 숫자만 입력받고 자동으로 px 단위 추가
2. "auto"는 특수 케이스로 처리 (빈 값 또는 0일 때)
3. 다른 모든 수치 입력 필드에도 동일 원칙 적용

**구현:**
- CSS: `{{#if property.style.ctaWidthDesktop}}{{#if (eq property.style.ctaWidthDesktop "0")}}auto{{else}}{{property.style.ctaWidthDesktop}}px{{/if}}{{else}}auto{{/if}}`
- Settings: placeholder를 "200" (숫자만)으로 변경, description에 "0을 입력하면 auto"라고 안내

---

### 문제 1-2: PC → 데스크탑 표현 수정

**현재 상태:**
- Settings.json에 "PC"라는 표현 사용
- Line 116, 138, 170, 196, 258, 310, 316, 324 등

**문제 원인:**
- 용어 불일치 (일부는 "데스크톱", 일부는 "PC")

**해결 방안:**
- 모든 "PC"를 "데스크탑"으로 일괄 변경

**적용 위치:**
- 헤드라인 크기 (PC, px) → (데스크톱, px)
- 텍스트 정렬 (PC) → (데스크톱)
- CTA 정렬 (PC) → (데스크톱)
- 버튼 가로 크기 (PC) → (데스크톱)
- 버튼 세로 크기 (PC, px) → (데스크톱, px)
- 버튼 글자 크기 (PC, px) → (데스크톱, px)

---

### 문제 1-3: 버튼 가로 크기(모바일) 정상 조절 안됨

**현재 상태:**
- CSS line 62: `--cta-width-mobile: {{#if property.style.ctaWidthMobile}}{{property.style.ctaWidthMobile}}{{else}}100{{/if}}%;`
- 디폴트가 "100%"

**문제 원인:**
- 사용자가 숫자만 입력하면 단위가 없어서 CSS가 무시됨
- 예: "280" 입력 → `width: 280;` (단위 없음) → 작동 안 함

**해결 방안:**
1. 숫자만 입력받고 px 단위 자동 추가
2. "100%"는 디폴트로 유지 (특수 처리)
3. 빈 값 또는 "100"이면 100%, 아니면 입력값px

**구현:**
```handlebars
--cta-width-mobile: {{#if property.style.ctaWidthMobile}}{{#if (eq property.style.ctaWidthMobile "100")}}100%{{else}}{{property.style.ctaWidthMobile}}px{{/if}}{{else}}100%{{/if}};
```

---

### 문제 1-4: glow 스캔 방향 작동 안 함

**현재 상태:**
- "왼쪽 → 오른쪽", "위 → 아래" 작동 안 함
- CSS line 278-291에 4개 방향 정의됨

**문제 원인:**
- `@keyframes heroCtaScan` (line 303-306)이 `background-position: 0% 0%` → `200% 0%`만 애니메이션
- 수평 방향(left-to-right, right-to-left)만 작동
- 수직 방향(top-to-bottom, bottom-to-top)은 background-position이 `0% 200%`로 변해야 하는데 그렇지 않음

**해결 방안:**
1. 수직 방향용 별도 keyframe 추가: `@keyframes heroCtaScanVertical`
2. 수직 방향 CSS에서 새 애니메이션 적용
3. 또는 JavaScript로 동적 처리

**구현:**
```css
@keyframes heroCtaScan {
  0%{background-position:0% 0%}
  100%{background-position:200% 0%}
}
@keyframes heroCtaScanVertical {
  0%{background-position:0% 0%}
  100%{background-position:0% 200%}
}

.hero-cta[data-glow-direction="top-to-bottom"]:hover .hero-cta-scan-highlight,
.hero-cta[data-glow-direction="bottom-to-top"]:hover .hero-cta-scan-highlight {
  animation: heroCtaScanVertical var(--cta-glow-scan-speed) linear infinite;
}
```

---

### 문제 1-5: glow 스캔 방향 → glow 스캔 시작 방향

**현재 상태:**
- Settings.json line 362: "Glow 스캔 방향"

**해결 방안:**
- "Glow 스캔 시작 방향"으로 변경

---

### 문제 1-6: 방향 라벨 수정

**현재 상태:**
- "왼쪽 → 오른쪽", "오른쪽 → 왼쪽", "위 → 아래", "아래 → 위"

**해결 방안:**
- "왼쪽", "오른쪽", "위", "아래"로 간소화

**적용:**
- Settings.json line 365-370

---

### 문제 1-7: hover 시 그림자 옵션 개선

**현재 상태:**
- Line 382-387: TEXTAREA로 CSS box-shadow 문법 직접 입력
- 예: "0 20px 40px rgba(0,0,0,.5)"

**문제 원인:**
- 비개발자가 CSS 문법을 알아야 함
- 실수하기 쉬움

**해결 방안:**
여러 옵션 중 선택:

**옵션 A: 프리셋 방식 (추천)**
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

**옵션 B: 분리된 필드**
- 그림자 크기 (px)
- 그림자 흐림 정도 (px)
- 그림자 투명도 (0-100)

**선택: 옵션 A (프리셋)가 더 간단하고 실용적**

---

## 2. 화살표 버튼 문제

### 문제 2-1: 이름 수정

**현재 상태:**
- Settings.json line 415: "⬅➡ 화살표 네비게이션"

**해결 방안:**
- "⬅➡ 화살표 버튼"으로 변경

---

### 문제 2-2: 좌우 여백 기준 변경

**현재 상태:**
- Line 492-497: "화면 좌우 가장자리(전역 패딩 기준)에서 화살표까지의 추가 여백"
- JavaScript line 853: `leftArrow.style.left = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)'`

**문제 원인:**
- 전역 패딩에 추가로 더해지는 방식
- 사용자가 원하는 것: 화면 끝(0%)에서부터의 절대 거리

**해결 방안:**
1. 전역 패딩을 무시하고 화면 끝에서 직접 계산
2. Description 수정: "화면 좌우 끝에서 화살표까지의 여백입니다."

**구현:**
```javascript
// 변경 전
leftArrow.style.left = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)';
rightArrow.style.right = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)';

// 변경 후
leftArrow.style.left = arrowOffsetX + '%';
rightArrow.style.right = arrowOffsetX + '%';
```

---

## 3. 데스크톱 하단 게이지 문제

### 문제 3-1: 번호 가로 위치 조절 옵션 추가

**현재 상태:**
- 번호가 항상 중앙 정렬 (line 452: `left: 50%; transform: translateX(-50%);`)

**해결 방안:**
1. 새 설정 추가: `slideCounterOffsetXPercent`
2. CSS 변수 추가
3. CSS 적용

**구현:**
```css
/* CSS 변수 */
--counter-offset-x-percent: {{#if property.style.slideCounterOffsetXPercent}}{{property.style.slideCounterOffsetXPercent}}{{else}}0{{/if}}%;

/* CSS 적용 */
.hero-step-counter-bubble {
  left: calc(50% + var(--counter-offset-x-percent));
  transform: translateX(-50%);
}
```

**Settings 추가:**
```json
{
  "id": "style.slideCounterOffsetXPercent",
  "label": "번호 가로 위치 조절 (%)",
  "description": "번호를 중앙에서 좌우로 이동시킵니다. 양수는 오른쪽, 음수는 왼쪽입니다. 예: -5, 0, 5",
  "type": "TEXT",
  "placeholder": "0",
  "isVisible": "property.style.desktopGaugeShow === true && property.progressMode === 'multiStep'"
}
```

---

### 문제 3-2: 이름 수정

**현재 상태:**
- Line 642: "번호 Y 오프셋 (px)"

**해결 방안:**
- "번호 세로 위치 조절 (px)"로 변경
- Description도 더 명확하게: "슬라이드 번호를 게이지 막대에서 위로 띄우는 정도입니다."

---

## 4. 모바일 게이지바 문제

### 문제 4-1: 상/하단 기준 오프셋 옵션 제거

**현재 상태:**
- Settings.json line 675-681에 `mobileGaugeBarOffsetPx` 필드 존재

**해결 방안:**
1. 해당 설정 필드 완전 제거
2. CSS에서 항상 0px로 고정
3. JavaScript에서도 해당 변수 사용 제거

**구현:**
```javascript
// 변경 전
if(confObj.mobileBarPosition === "bottom"){
  barWrap.style.bottom = confObj.mobileBarOffsetPx + 'px';
  barWrap.style.top = 'auto';
}else{
  barWrap.style.top = confObj.mobileBarOffsetPx + 'px';
  barWrap.style.bottom = 'auto';
}

// 변경 후
if(confObj.mobileBarPosition === "bottom"){
  barWrap.style.bottom = '0px';
  barWrap.style.top = 'auto';
}else{
  barWrap.style.top = '0px';
  barWrap.style.bottom = 'auto';
}
```

---

### 문제 4-2: 모바일 게이지바 표시 안 됨

**현재 상태:**
- JavaScript line 864-886의 `applyMobileBarStyles` 함수

**문제 원인 추정:**
1. 모바일 감지 로직 오류 (`window.innerWidth <= 768`)
2. 초기 로드 시 조건 체크 타이밍 문제
3. CSS `display: none` 초기값과 충돌

**해결 방안:**
1. 초기화 순서 조정
2. 리사이즈 이벤트 즉시 실행
3. CSS 초기값을 조건부로 설정

**검증 체크리스트:**
- [ ] `mobileBarShow` 옵션이 true인가?
- [ ] `progressMode`가 'singleBar'인가?
- [ ] `window.innerWidth <= 768`이 true인가?
- [ ] `barWrap.style.display`가 'block'으로 설정되는가?

---

## 5. 스크롤 힌트 문제

### 문제 5-1: 애니메이션 이름 한글로만 표기

**현재 상태:**
- Settings.json line 754-758
- "Bounce (튀기기)", "Pulse (펄스)", "None (효과 없음)"

**해결 방안:**
- "튀기기", "펄스", "효과 없음"으로 변경

---

## 6. 전체 레이아웃 / 공통 설정 문제

### 문제 6-1: 배치 순서 변경

**현재 상태:**
- Settings.json line 770-824가 마지막에 위치

**해결 방안:**
- "전체 레이아웃 / 공통 설정" 섹션을 settings 배열의 맨 처음으로 이동
- 논리적 순서: 전체 → 슬라이드 → 헤드라인 → CTA → 화살표 → 게이지 → 힌트

---

### 문제 6-2: 전역 패딩 영향 수정

**현재 상태:**
- 화살표: 전역 패딩 + offset (line 853)
- 게이지: 전역 패딩 + offset (line 825)

**사용자 요구사항 해석:**
"화살표 버튼, 데스크톱 하단 게이지도 전체 레이아웃 좌우 패딩의 영향을 받도록 수정"

**혼란 포인트:**
- 문제 2-2에서는 "화면 끝 기준"으로 변경하라고 함
- 여기서는 "전역 패딩 영향을 받도록"이라고 함

**명확화 필요:**
두 가지 해석 가능:

**해석 A: 화살표만 화면 끝 기준, 게이지는 기존 유지**
- 화살표: 전역 패딩 무시, 화면 끝에서 직접 계산
- 게이지: 전역 패딩 영향 유지 (현재 상태)

**해석 B: 둘 다 전역 패딩 적용 방식 개선**
- 현재 코드가 이미 전역 패딩을 반영하고 있음
- 추가 작업 불필요

**선택: 해석 A 적용 (화살표만 화면 끝 기준으로 변경)**

---

### 문제 6-3: 상하 패딩 옵션 제거

**현재 상태:**
- Settings.json line 793-798: `heroPaddingYPercent`
- CSS line 19, 197-198에서 사용

**해결 방안:**
1. Settings에서 필드 제거
2. CSS 변수 제거
3. CSS에서 `padding-top`, `padding-bottom` 제거 또는 0으로 고정

**영향 범위:**
- `.hero-content-wrapper` (line 197-198)

**구현:**
```css
/* 변경 전 */
padding-top: var(--global-pad-y);
padding-bottom: var(--global-pad-y);

/* 변경 후 - 제거 */
/* padding-top, padding-bottom 삭제 */
```

---

## 수정 우선순위

### 높음 (기능 오류)
1. ✅ 버튼 가로 크기(모바일) 작동 안 됨 (1-3)
2. ✅ glow 스캔 방향 작동 안 됨 (1-4)
3. ✅ 모바일 게이지바 표시 안 됨 (4-2)

### 중간 (UX 개선)
1. ✅ 숫자만 입력 (1-1)
2. ✅ PC → 데스크톱 (1-2)
3. ✅ 그림자 옵션 개선 (1-7)
4. ✅ 화살표 좌우 여백 기준 (2-2)
5. ✅ 번호 가로 위치 조절 (3-1)

### 낮음 (라벨/텍스트 수정)
1. ✅ glow 스캔 방향 → 시작 방향 (1-5)
2. ✅ 방향 라벨 수정 (1-6)
3. ✅ 화살표 네비게이션 → 버튼 (2-1)
4. ✅ 번호 Y 오프셋 → 세로 위치 (3-2)
5. ✅ 애니메이션 한글화 (5-1)
6. ✅ 배치 순서 변경 (6-1)
7. ✅ 상하 패딩 제거 (6-3)
8. ✅ 모바일 오프셋 제거 (4-1)

---

## 공통 개발 지침 업데이트

### 수치 입력 필드 통일 규칙

**원칙:**
1. 사용자는 **숫자만** 입력
2. 단위(px, %, vh 등)는 코드에서 자동 추가
3. 특수 값(auto, none 등)은 빈 값 또는 0으로 처리

**적용 대상:**
- ✅ 모든 크기 관련 필드 (width, height, font-size, padding, margin, offset 등)
- ✅ placeholder는 숫자만 표시 (예: "200" 대신 "200px" 제거)
- ✅ description에 "숫자만 입력하세요" 또는 "예: 40" 명시

**예외:**
- COLOR_PICKER: 색상 값
- TEXT(URL): URL 문자열
- TEXTAREA: 복잡한 문법 (단, 가능한 한 프리셋으로 대체)

---

## 테스트 체크리스트

### CTA 버튼
- [ ] 데스크톱 가로 크기: 200 입력 → 200px 적용
- [ ] 데스크톱 가로 크기: 0 입력 → auto 적용
- [ ] 모바일 가로 크기: 280 입력 → 280px 적용
- [ ] 모바일 가로 크기: 100 입력 → 100% 적용
- [ ] Glow 왼쪽: 왼쪽에서 오른쪽으로 스캔
- [ ] Glow 오른쪽: 오른쪽에서 왼쪽으로 스캔
- [ ] Glow 위: 위에서 아래로 스캔
- [ ] Glow 아래: 아래에서 위로 스캔
- [ ] 그림자 프리셋 적용 확인

### 화살표 버튼
- [ ] 좌우 여백 5% → 화면 끝에서 5% 위치

### 데스크톱 게이지
- [ ] 번호 가로 위치 -10 → 왼쪽으로 이동
- [ ] 번호 가로 위치 10 → 오른쪽으로 이동

### 모바일 게이지바
- [ ] 모바일에서 게이지바 표시 옵션 켜면 즉시 보임
- [ ] 상단/하단 위치 모두 0px 고정

### 전체 레이아웃
- [ ] 설정 패널에서 "전체 레이아웃"이 맨 위에 표시
- [ ] 상하 패딩 옵션 제거됨
- [ ] 모든 "PC" 표현이 "데스크톱"으로 변경됨

---

## 파일 수정 계획

1. **hero-carousel-v3-fixed.html**
   - CSS 변수 수정 (숫자 단위 처리)
   - Glow 애니메이션 keyframe 추가
   - 패딩 관련 CSS 수정

2. **hero-carousel-v3-settings.json**
   - 전체 레이아웃 섹션을 맨 위로 이동
   - 모든 라벨/description 수정
   - 그림자 프리셋 필드 추가
   - 번호 가로 위치 필드 추가
   - 불필요한 필드 제거 (상하 패딩, 모바일 오프셋)

3. **JavaScript (hero-carousel-v3-fixed.html 내)**
   - 화살표 위치 계산 로직 수정
   - 모바일 게이지바 표시 로직 개선
   - 번호 가로 위치 적용 로직 추가
