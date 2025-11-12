# 히어로 캐러셀 v3.3 수정 요약 - CSS 전역 패딩 완전 적용

## 수정 날짜
2025-11-12

## 해결된 문제 (4개)

### 1. ✅ Glow 스캔 방향 라벨 단순화
- **변경**: "Glow 스캔 시작 방향" → "Glow 스캔 방향"
- **이유**: "시작"이라는 단어가 불필요하고, 실제로는 스캔의 방향을 나타냄
- **설명 업데이트**: "Glow 효과가 시작되는 방향" → "Glow 효과가 스캔되는 방향"

### 2. ✅ 화살표 버튼 전역 패딩 적용 (CSS 방식)
- **문제**: 화살표가 전역 패딩의 영향을 받지 않음
- **근본 원인**: JavaScript로 동적 설정하면서 전역 패딩이 제대로 적용되지 않음
- **해결 방법**:
  - **CSS 변수 추가**: `--nav-arrow-offset-x: {{navArrowOffsetXPercent}}%`
  - **CSS에서 직접 적용**:
    ```css
    .hero-nav-arrow[data-nav-prev] {
      left: calc(var(--global-pad-x) + var(--nav-arrow-offset-x));
    }
    .hero-nav-arrow[data-nav-next] {
      right: calc(var(--global-pad-x) + var(--nav-arrow-offset-x));
    }
    ```
  - **JavaScript 함수 제거**: `applyArrowPositions()` 호출 제거

**동작**:
- 전역 패딩 5% + 추가 여백 0% = 화면 끝에서 5% 떨어짐
- 전역 패딩 5% + 추가 여백 5% = 화면 끝에서 10% 떨어짐
- 전역 패딩 0% + 추가 여백 0% = 화면 끝에 바짝 붙음

### 3. ✅ 데스크톱 하단 게이지 전역 패딩 적용 (CSS 방식)
- **문제**: 게이지가 화면 왼쪽 끝에서 시작되지 않고 전역 패딩의 영향을 받지 않음
- **근본 원인**: JavaScript로 동적 설정하면서 전역 패딩이 제대로 적용되지 않음
- **해결 방법**:
  - **HTML에 data 속성 추가**:
    ```html
    <div class="hero-progress-wrapper-desktop"
         data-gauge-wrapper
         data-align="{{gaugeAlignDesktop}}">
    ```
  - **CSS 변수 추가**: `--gauge-offset-x: {{gaugeOffsetXPercentDesktop}}%`
  - **CSS에서 정렬별 처리**:
    ```css
    .hero-progress-wrapper-desktop[data-align="left"] {
      left: calc(var(--global-pad-x) + var(--gauge-offset-x));
      right: auto;
    }
    .hero-progress-wrapper-desktop[data-align="right"] {
      left: auto;
      right: calc(var(--global-pad-x) + var(--gauge-offset-x));
    }
    .hero-progress-wrapper-desktop[data-align="center"] {
      left: 50%;
      right: auto;
      transform: translateX(calc(-50% + var(--gauge-offset-x)));
    }
    ```
  - **JavaScript 함수 제거**: `applyGaugeWrapperPosition()` 호출 제거

**동작**:
- 왼쪽 정렬: 화면 왼쪽 끝에서 전역 패딩(5%) + 오프셋(0%) = 5% 떨어진 위치에서 시작
- 오른쪽 정렬: 화면 오른쪽 끝에서 전역 패딩(5%) + 오프셋(0%) = 5% 떨어진 위치에서 시작
- 중앙 정렬: 화면 정중앙 + 오프셋

### 4. ✅ 전역 패딩 일관성 확보
- **문제**: 슬라이드 캡션만 전역 패딩 영향을 받고, 화살표와 게이지는 받지 못함
- **해결**: 모든 하단 요소가 CSS에서 직접 `var(--global-pad-x)`를 사용하도록 통일

**현재 전역 패딩 적용 상태**:
- ✅ 슬라이드 캡션: `left: var(--global-pad-x); right: var(--global-pad-x);`
- ✅ 화살표 버튼: `left/right: calc(var(--global-pad-x) + var(--nav-arrow-offset-x))`
- ✅ 데스크톱 게이지: `left/right: calc(var(--global-pad-x) + var(--gauge-offset-x))`
- ✅ 콘텐츠 wrapper: `padding-left: var(--global-pad-x); padding-right: var(--global-pad-x);`

---

## 핵심 변경 사항

### CSS 변수 추가 (2개)

```css
/* NAV ARROWS */
--nav-arrow-offset-x: {{#if property.style.navArrowOffsetXPercent}}{{property.style.navArrowOffsetXPercent}}{{else}}0{{/if}}%;

/* DESKTOP GAUGE */
--gauge-offset-x: {{#if property.style.gaugeOffsetXPercentDesktop}}{{property.style.gaugeOffsetXPercentDesktop}}{{else}}0{{/if}}%;
```

### HTML 속성 추가 (1개)

```html
<div class="hero-progress-wrapper-desktop"
     data-gauge-wrapper
     data-align="{{#if property.style.gaugeAlignDesktop}}{{property.style.gaugeAlignDesktop}}{{else}}center{{/if}}">
```

### CSS 규칙 추가 (5개)

```css
/* 화살표 top 위치 */
.hero-nav-arrow {
  top: {{#if property.style.navArrowOffsetYPercent}}{{property.style.navArrowOffsetYPercent}}{{else}}50{{/if}}%;
  transform: translateY(-50%);
}

/* 화살표 좌우 위치 */
.hero-nav-arrow[data-nav-prev] {
  left: calc(var(--global-pad-x) + var(--nav-arrow-offset-x));
}
.hero-nav-arrow[data-nav-next] {
  right: calc(var(--global-pad-x) + var(--nav-arrow-offset-x));
}

/* 게이지 정렬별 위치 */
.hero-progress-wrapper-desktop[data-align="left"] {
  left: calc(var(--global-pad-x) + var(--gauge-offset-x));
  right: auto;
}
.hero-progress-wrapper-desktop[data-align="right"] {
  left: auto;
  right: calc(var(--global-pad-x) + var(--gauge-offset-x));
}
.hero-progress-wrapper-desktop[data-align="center"] {
  left: 50%;
  right: auto;
  transform: translateX(calc(-50% + var(--gauge-offset-x)));
}
```

### JavaScript 변경

**함수 호출 제거**:
```javascript
// 이전
applyGaugeWrapperPosition(confObj);
applyArrowPositions(confObj);
populateStepNumbers();
applyMobileBarStyles(confObj);

// 이후 (화살표와 게이지는 CSS로 처리)
populateStepNumbers();
applyMobileBarStyles(confObj);
```

**함수 정의 제거**:
- `applyGaugeWrapperPosition()` - 제거됨
- `applyArrowPositions()` - 제거됨
- 주석으로 설명 추가: "화살표와 게이지 위치는 이제 CSS에서 직접 처리합니다"

---

## 장점

### 1. 성능 개선
- JavaScript 실행 없이 CSS만으로 즉시 배치
- 리플로우/리페인트 최소화
- 초기 렌더링 속도 향상

### 2. 유지보수 개선
- CSS 변수로 중앙 집중식 관리
- JavaScript 코드 감소
- 디버깅 용이 (브라우저 개발자 도구에서 CSS 값 직접 확인 가능)

### 3. 일관성 확보
- 모든 하단 요소가 동일한 방식으로 전역 패딩 적용
- CSS 변수 변경 시 모든 요소에 즉시 반영

### 4. 반응형 처리 용이
- 미디어 쿼리로 CSS 변수만 변경하면 됨
- JavaScript 재실행 불필요

---

## 테스트 체크리스트

### 전역 패딩 동작 확인
- [ ] 전역 패딩 0% 설정 시:
  - [ ] 슬라이드 캡션이 화면 끝에 붙음
  - [ ] 화살표가 화면 끝에 붙음
  - [ ] 왼쪽 정렬 게이지가 화면 왼쪽 끝에 붙음

- [ ] 전역 패딩 5% 설정 시:
  - [ ] 슬라이드 캡션이 화면 끝에서 5% 떨어짐
  - [ ] 화살표가 화면 끝에서 5% 떨어짐
  - [ ] 왼쪽 정렬 게이지가 화면 왼쪽 끝에서 5% 떨어짐

- [ ] 전역 패딩 10% 설정 시:
  - [ ] 모든 요소가 화면 끝에서 10% 떨어짐

### 화살표 추가 여백
- [ ] 전역 패딩 5% + 추가 여백 0% = 5% 떨어짐
- [ ] 전역 패딩 5% + 추가 여백 5% = 10% 떨어짐
- [ ] 전역 패딩 5% + 추가 여백 10% = 15% 떨어짐

### 게이지 정렬
- [ ] 왼쪽 정렬: 전역 패딩 5% 적용, 화면 왼쪽에서 5% 떨어진 위치에서 시작
- [ ] 오른쪽 정렬: 전역 패딩 5% 적용, 화면 오른쪽에서 5% 떨어진 위치에서 끝남
- [ ] 중앙 정렬: 화면 정중앙에 배치 (전역 패딩 미적용, offset만 적용)

### 게이지 오프셋
- [ ] 왼쪽 정렬 + 오프셋 5%: 전역 패딩 5% + 오프셋 5% = 10% 떨어짐
- [ ] 중앙 정렬 + 오프셋 5%: 중앙에서 5% 오른쪽으로 이동

### 브라우저 호환성
- [ ] Chrome/Edge: 정상 작동
- [ ] Firefox: 정상 작동
- [ ] Safari: 정상 작동
- [ ] Mobile Safari: 정상 작동

---

## 업그레이드 가이드

### v3.2 → v3.3

#### 자동 적용되는 변경사항
1. ✅ 화살표와 게이지가 전역 패딩의 영향을 자동으로 받음
2. ✅ 성능 개선 (JavaScript 실행 감소)
3. ✅ 일관성 확보 (모든 요소가 동일한 전역 패딩 적용)

#### 주의사항
**전역 패딩 기본값 5%가 모든 요소에 적용됨**:
- 이전: 슬라이드 캡션만 전역 패딩 5% 적용
- 이후: 슬라이드 캡션, 화살표, 게이지 모두 전역 패딩 5% 적용

**화면에 바짝 붙이고 싶다면**:
- 전역 패딩을 0%로 설정

**더 띄우고 싶다면**:
- 전역 패딩을 10%로 설정
- 또는 전역 패딩 5% + 화살표/게이지 추가 여백 5% = 10%

---

## 파일 변경 통계

| 파일 | 변경 전 | 변경 후 | 차이 |
|------|---------|---------|------|
| hero-carousel-v3-fixed.html | 1,199줄 | 1,177줄 | -22줄 |
| hero-carousel-v3-settings.json | 948줄 | 948줄 | 0줄 |

**총 코드 감소: -22줄** (JavaScript 함수 제거로 코드 간소화)

---

## 버전 이력

- **v3.3** (2025-11-12): CSS 전역 패딩 완전 적용
  - Glow 스캔 방향 라벨 단순화
  - 화살표 CSS 전역 패딩 적용
  - 데스크톱 게이지 CSS 전역 패딩 적용
  - JavaScript 위치 설정 함수 제거 (CSS로 대체)
  - 전역 패딩 일관성 확보

- **v3.2** (2025-11-12): 4가지 문제 수정
  - Glow 애니메이션 완전 수정 (4개 keyframe)
  - 화살표 전역 패딩 재적용 (JavaScript 방식)
  - 진행 게이지 스타일 라벨 단순화
  - 모바일 게이지바 multiStep 모드 지원

- **v3.1.1** (2025-11-12): 5가지 문제 수정
  - Glow 애니메이션 범위 조정
  - 화살표 화면 끝 붙이기
  - 진행 게이지 스타일 옵션 이동
  - 모바일 게이지바 조건 단순화
  - 설정 설명 개선

---

## 기술적 세부사항

### CSS calc() 함수 사용
```css
/* 전역 패딩 + 추가 여백 */
left: calc(var(--global-pad-x) + var(--nav-arrow-offset-x));

/* 중앙 정렬 + 오프셋 */
transform: translateX(calc(-50% + var(--gauge-offset-x)));
```

### data 속성을 이용한 CSS 선택자
```css
/* 정렬 방식에 따라 다른 CSS 적용 */
.hero-progress-wrapper-desktop[data-align="left"] { ... }
.hero-progress-wrapper-desktop[data-align="right"] { ... }
.hero-progress-wrapper-desktop[data-align="center"] { ... }
```

### Handlebars 템플릿 변수
```handlebars
{{! CSS 변수에 직접 주입 }}
--nav-arrow-offset-x: {{#if property.style.navArrowOffsetXPercent}}{{property.style.navArrowOffsetXPercent}}{{else}}0{{/if}}%;

{{! HTML 속성에 주입 }}
<div data-align="{{#if property.style.gaugeAlignDesktop}}{{property.style.gaugeAlignDesktop}}{{else}}center{{/if}}">
```

---

## 문제 해결 가이드

### 전역 패딩이 적용되지 않는 것처럼 보일 때
1. 브라우저 개발자 도구 열기
2. 요소 검사하여 CSS 변수 값 확인:
   ```
   --global-pad-x: 5%;
   --nav-arrow-offset-x: 0%;
   left: calc(5% + 0%);  // 결과: 5%
   ```
3. Computed 탭에서 최종 계산된 값 확인

### 게이지가 왼쪽 끝에서 시작되지 않을 때
1. data-align 속성 확인: `data-align="left"`인지 확인
2. CSS 변수 확인: `--global-pad-x`와 `--gauge-offset-x` 값 확인
3. 왼쪽 정렬 CSS 규칙이 적용되었는지 확인

### 화살표가 움직이지 않을 때
1. CSS 변수 `--nav-arrow-offset-x` 값 확인
2. `left/right` 속성의 계산된 값 확인
3. transform 속성 확인 (translateY(-50%)만 있어야 함)

---

## 다음 단계

### 사용자 피드백 필요
1. **전역 패딩 동작**: 모든 요소에 일관되게 적용되는지 확인
2. **화살표 위치**: 전역 패딩 + 추가 여백이 올바르게 작동하는지 확인
3. **게이지 정렬**: 왼쪽/오른쪽/중앙 정렬이 모두 전역 패딩을 올바르게 적용하는지 확인

### 추가 개선 가능 항목
1. 모바일 게이지바도 전역 패딩 적용 옵션
2. 스크롤 힌트도 전역 패딩 적용 옵션
3. CSS 변수 기반 반응형 처리 강화

---

## 문의 및 피드백

문제 발견 시 다음 정보와 함께 보고해주세요:
1. 브라우저 및 버전
2. 전역 패딩 설정 값
3. 화살표/게이지 추가 여백 설정 값
4. 브라우저 개발자 도구 Computed 탭 스크린샷
5. 예상 동작 vs 실제 동작
