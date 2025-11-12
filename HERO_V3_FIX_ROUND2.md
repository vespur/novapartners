# 히어로 캐러셀 v3 수정 라운드 2

## 수정 날짜
2025-11-12

## 해결된 문제 (5개)

### 1. ✅ Glow 스캔 방향 애니메이션 수정
- **문제**: "왼쪽→오른쪽", "위→아래" 방향이 정상 작동하지 않음
- **원인**: `background-position` 애니메이션 값이 잘못됨 (0% → 200%)
- **해결**:
  - 수평 애니메이션: `-100%` → `100%`로 변경
  - 수직 애니메이션: `0% -100%` → `0% 100%`로 변경
  - 200% 크기의 배경에서 올바른 스캔 효과를 만들기 위해 범위 조정

### 2. ✅ 화살표 버튼을 화면 끝에 바짝 붙이기
- **문제**: 화살표가 화면 끝에서 5% 떨어져 있음 (전역 패딩이 자동 적용됨)
- **사용자 요구사항**: "화살표가 화면의 왼쪽과 오른쪽 끝에 바짝 붙어있는 기본 상태"
- **해결**:
  - 전역 패딩 계산을 제거
  - `left/right: arrowOffsetX%`로 단순화
  - 기본값 0% = 화면 끝에 바짝 붙음
  - 사용자가 offset 값을 조정하여 원하는 만큼 떨어뜨릴 수 있음

### 3. ✅ 진행 게이지 스타일 옵션 이동
- **문제**: "진행 게이지 스타일" 옵션이 슬라이드 설정 섹션에 있어서 맥락이 맞지 않음
- **해결**:
  - 옵션을 "데스크톱 하단 게이지" 섹션으로 이동
  - `isVisible: "property.style.desktopGaugeShow === true"` 조건 추가
  - 데스크톱 게이지를 표시할 때만 스타일 옵션이 보이도록 개선

### 4. ✅ 모바일 게이지바 멀티스텝 모드 지원
- **문제**: "진행 게이지 스타일"을 "단일 바"로 선택해야만 모바일 게이지바가 표시됨
- **원인**: `confObj.mode === 'singleBar'` 조건 체크
- **해결**:
  - mode 체크 제거
  - `shouldShow = confObj.mobileBarShow && isMobile`로 단순화
  - 이제 singleBar와 multiStep 모드 모두에서 모바일 게이지바 사용 가능

### 5. ✅ 전역 패딩 일관성 확인
- **현재 상태**:
  - 슬라이드 캡션: 전역 패딩 적용 ✓ (CSS 변수 `--global-pad-x` 사용)
  - 데스크톱 게이지: 전역 패딩 적용 ✓ (JavaScript에서 `calc(globalPadX% + offset%)`)
  - 화살표 버튼: 전역 패딩 미적용 (사용자 요구사항에 따라 화면 끝 기준)
- **참고**: 화살표는 사용자의 명시적 요청에 따라 화면 끝에 바짝 붙는 것이 기본 동작

---

## 상세 변경 내역

### 📄 hero-carousel-v3-fixed.html

#### 1. CSS - Glow 애니메이션 수정 (line 316-323)

**변경 전:**
```css
@keyframes heroCtaScan {
	0%{background-position:0% 0%}
	100%{background-position:200% 0%}
}
@keyframes heroCtaScanVertical {
	0%{background-position:0% 0%}
	100%{background-position:0% 200%}
}
```

**변경 후:**
```css
@keyframes heroCtaScan {
	0%{background-position:-100% 0%}
	100%{background-position:100% 0%}
}
@keyframes heroCtaScanVertical {
	0%{background-position:0% -100%}
	100%{background-position:0% 100%}
}
```

**설명**: 200% 크기의 배경에서 -100%부터 100%까지 이동하면 glow 효과가 완전히 스캔됩니다.

#### 2. JavaScript - 화살표 위치 함수 수정 (line 857-876)

**변경 전:**
```javascript
/* 화살표 위치 세팅 - 전역 패딩 + offset */
function applyArrowPositions(confObj){
	const leftArrow = root.querySelector('[data-nav-prev]');
	const rightArrow = root.querySelector('[data-nav-next]');
	if(!leftArrow || !rightArrow) return;

	const globalPadX = parseFloat(getComputedStyle(root).getPropertyValue('--global-pad-x')) || 5;
	const arrowOffsetX = confObj.navArrowOffsetXPercent || 0;
	const arrowOffsetY = confObj.navArrowOffsetYPercent || 50;

	// 왼쪽 화살표: 전역 패딩 + offset
	leftArrow.style.left = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)';
	leftArrow.style.top = arrowOffsetY + '%';
	leftArrow.style.transform = 'translateY(-50%)';

	// 오른쪽 화살표: 전역 패딩 + offset
	rightArrow.style.right = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)';
	rightArrow.style.top = arrowOffsetY + '%';
	rightArrow.style.transform = 'translateY(-50%)';
}
```

**변경 후:**
```javascript
/* 화살표 위치 세팅 - 화면 끝 기준 + offset */
function applyArrowPositions(confObj){
	const leftArrow = root.querySelector('[data-nav-prev]');
	const rightArrow = root.querySelector('[data-nav-next]');
	if(!leftArrow || !rightArrow) return;

	const arrowOffsetX = confObj.navArrowOffsetXPercent || 0;
	const arrowOffsetY = confObj.navArrowOffsetYPercent || 50;

	// 왼쪽 화살표: 화면 끝에서 offset만큼 떨어뜨림 (기본 0% = 바짝 붙음)
	leftArrow.style.left = arrowOffsetX + '%';
	leftArrow.style.top = arrowOffsetY + '%';
	leftArrow.style.transform = 'translateY(-50%)';

	// 오른쪽 화살표: 화면 끝에서 offset만큼 떨어뜨림 (기본 0% = 바짝 붙음)
	rightArrow.style.right = arrowOffsetX + '%';
	rightArrow.style.top = arrowOffsetY + '%';
	rightArrow.style.transform = 'translateY(-50%)';
}
```

**설명**: 전역 패딩 계산을 완전히 제거하여 화살표가 화면 끝에 바짝 붙도록 수정

#### 3. JavaScript - 모바일 게이지바 함수 수정 (line 878-901)

**변경 전:**
```javascript
/* 모바일 게이지바 스타일 세팅 */
function applyMobileBarStyles(confObj){
	const barWrap = root.querySelector('[data-gauge-mobilebar]');
	if(!barWrap) return;

	const isMobile = window.innerWidth <= 768;

	// 조건: mobileBarShow && singleBar && 모바일
	const shouldShow = confObj.mobileBarShow && confObj.mode === 'singleBar' && isMobile;

	barWrap.style.display = shouldShow ? 'block' : 'none';

	// 표시할 때만 위치 설정
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

**변경 후:**
```javascript
/* 모바일 게이지바 스타일 세팅 */
function applyMobileBarStyles(confObj){
	const barWrap = root.querySelector('[data-gauge-mobilebar]');
	if(!barWrap) return;

	const isMobile = window.innerWidth <= 768;

	// 조건: mobileBarShow && 모바일 (singleBar/multiStep 모두 지원)
	const shouldShow = confObj.mobileBarShow && isMobile;

	barWrap.style.display = shouldShow ? 'block' : 'none';

	// 표시할 때만 위치 설정
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

**설명**: `confObj.mode === 'singleBar'` 조건을 제거하여 multiStep 모드에서도 작동

---

### 📄 hero-carousel-v3-settings.json

#### 1. 화살표 offset 설명 수정 (line 546-551)

**변경 전:**
```json
{
  "id": "style.navArrowOffsetXPercent",
  "label": "좌우 여백 (%)",
  "description": "전역 패딩 기준에서 추가로 더 떨어뜨릴 여백입니다. 0을 입력하면 전역 패딩만 적용됩니다. 숫자만 입력해주세요. 예: 0",
  "type": "TEXT",
  "placeholder": "0",
  "isVisible": "property.style.navArrowShow === true"
}
```

**변경 후:**
```json
{
  "id": "style.navArrowOffsetXPercent",
  "label": "좌우 여백 (%)",
  "description": "화면 좌우 끝에서 화살표까지의 거리입니다. 0을 입력하면 화면 끝에 바짝 붙습니다. 숫자만 입력해주세요. 예: 0",
  "type": "TEXT",
  "placeholder": "0",
  "isVisible": "property.style.navArrowShow === true"
}
```

#### 2. progressMode 옵션 이동 (line 104-113 → line 589-599)

**이전 위치**: 슬라이드 설정 섹션 (line 104)
**새 위치**: 데스크톱 하단 게이지 섹션 (line 589)

**추가 변경**:
```json
{
  "id": "progressMode",
  "label": "진행 게이지 스타일",
  "description": "데스크톱 하단 진행 게이지의 표시 방식을 선택해주세요.",
  "type": "RADIO",
  "options": [
    {"label": "단일 바 (한 줄)", "value": "singleBar"},
    {"label": "멀티 스텝 (슬라이드별 막대)", "value": "multiStep"}
  ],
  "isVisible": "property.style.desktopGaugeShow === true"  // ← 새로 추가
}
```

**설명**: 데스크톱 게이지를 표시할 때만 스타일 옵션이 보이도록 개선

---

## 영향 받는 기능

### ✅ 정상 작동 확인 필요

1. **Glow 애니메이션 4방향**
   - 왼쪽 (left-to-right): 왼쪽→오른쪽 스캔
   - 오른쪽 (right-to-left): 오른쪽→왼쪽 스캔
   - 위 (top-to-bottom): 위→아래 스캔
   - 아래 (bottom-to-top): 아래→위 스캔

2. **화살표 버튼 위치**
   - 기본 (0%): 화면 좌우 끝에 바짝 붙음 ✓
   - 5% 입력: 화면 끝에서 5% 떨어짐 ✓
   - 10% 입력: 화면 끝에서 10% 떨어짐 ✓

3. **모바일 게이지바**
   - singleBar 모드: 표시 ✓
   - multiStep 모드: 표시 ✓
   - 데스크톱(>768px): 숨김 ✓
   - 모바일(≤768px): 표시 ✓

4. **진행 게이지 스타일 옵션**
   - 데스크톱 게이지 섹션에 올바르게 그룹핑됨 ✓
   - 데스크톱 게이지를 표시할 때만 보임 ✓

5. **전역 패딩**
   - 슬라이드 캡션: 전역 패딩 적용 ✓
   - 데스크톱 게이지: 전역 패딩 적용 ✓
   - 화살표 버튼: 수동 offset 제어 (화면 끝 기준) ✓

---

## 테스트 체크리스트

### Glow 애니메이션
- [ ] 왼쪽: hover 시 왼쪽에서 오른쪽으로 빛 스캔
- [ ] 오른쪽: hover 시 오른쪽에서 왼쪽으로 빛 스캔
- [ ] 위: hover 시 위에서 아래로 빛 스캔
- [ ] 아래: hover 시 아래에서 위로 빛 스캔
- [ ] 방향 전환 시 즉시 반영됨

### 화살표 버튼
- [ ] 좌우 여백 0% → 화면 끝에 바짝 붙음
- [ ] 좌우 여백 5% → 화면 끝에서 5% 떨어짐
- [ ] 좌우 여백 10% → 화면 끝에서 10% 떨어짐
- [ ] 좌우 대칭으로 올바르게 배치됨

### 모바일 게이지바
- [ ] 데스크톱에서 표시 안 됨
- [ ] 모바일(≤768px) + 단일 바 모드 + 표시 옵션 ON → 보임
- [ ] 모바일(≤768px) + 멀티스텝 모드 + 표시 옵션 ON → 보임
- [ ] 상단 위치 선택 시 화면 최상단에 붙음
- [ ] 하단 위치 선택 시 화면 최하단에 붙음

### 설정 패널 구조
- [ ] "진행 게이지 스타일" 옵션이 "데스크톱 하단 게이지" 섹션에 있음
- [ ] 데스크톱 게이지 표시를 끄면 "진행 게이지 스타일" 옵션이 숨겨짐
- [ ] 설정 순서: 표시 → 모바일 숨기기 → 스타일 → 정렬 → 오프셋

### 전역 패딩 동작
- [ ] 전역 패딩 5% 설정 시 슬라이드 캡션과 데스크톱 게이지가 함께 이동
- [ ] 전역 패딩 10% 설정 시 슬라이드 캡션과 데스크톱 게이지가 함께 이동
- [ ] 화살표는 전역 패딩과 무관하게 화면 끝 기준으로 동작

---

## 알려진 제한사항

1. **화살표는 전역 패딩 영향을 받지 않음**
   - 사용자 요구사항: "화면 끝에 바짝 붙는 기본 상태"
   - 전역 패딩을 원하는 경우: offset 값을 전역 패딩과 동일하게 설정

2. **모바일 게이지바 위치는 상/하단만 가능**
   - 좌우 위치는 지원하지 않음
   - 이유: 모바일에서 전체 가로 게이지바는 상/하단이 최적

3. **Glow 애니메이션은 무한 반복**
   - hover 중에는 계속 스캔 효과가 반복됨
   - 1회만 실행되는 옵션은 없음

---

## 업그레이드 가이드

### v3.1.0 → v3.1.1

#### 자동 적용되는 변경사항
1. Glow 애니메이션이 모든 방향에서 정상 작동
2. 모바일 게이지바가 multiStep 모드에서도 표시됨
3. 설정 패널 구조 개선 (진행 게이지 스타일 위치 이동)

#### 수동 조정 필요
**화살표 위치가 변경될 수 있음:**
- v3.1.0: 전역 패딩(5%) + offset(0%) = 5% 떨어짐
- v3.1.1: offset(0%) = 화면 끝에 붙음

**해결 방법:**
- 기존처럼 5% 떨어뜨리려면: offset 5% 입력
- 화면 끝에 붙이려면: offset 0% 유지 (새 기본값)

---

## 파일 변경 통계

| 파일 | 변경 전 | 변경 후 | 차이 |
|------|---------|---------|------|
| hero-carousel-v3-fixed.html | 1,193줄 | 1,190줄 | -3줄 |
| hero-carousel-v3-settings.json | 948줄 | 948줄 | 0줄 |

**총 코드 감소: -3줄** (단순화로 인한 감소)

---

## 버전 이력

- **v3.1.1** (2025-11-12): 5가지 문제 수정
  - Glow 애니메이션 범위 수정
  - 화살표 화면 끝 붙이기
  - 진행 게이지 스타일 옵션 이동
  - 모바일 게이지바 multiStep 지원
  - 설정 설명 개선

- **v3.1.0** (2025-11-12): 4가지 문제 수정
  - Glow 방향 background-size 명시
  - 화살표 전역 패딩 적용 (후에 롤백)
  - 모바일 게이지바 로직 개선
  - 전역 패딩 일관성 시도

- **v3.0.0** (2025-11-12): 19가지 문제 수정
  - 숫자만 입력 규칙
  - 용어 통일
  - 그림자 프리셋
  - 번호 가로 위치 조절

---

## 다음 단계

### 사용자 피드백 필요
1. **화살표 위치**: 현재는 화면 끝 기준. 전역 패딩 적용 옵션이 필요한가요?
2. **모바일 게이지바**: multiStep 모드에서도 잘 작동하는지 확인
3. **Glow 애니메이션**: 모든 4방향이 올바르게 스캔되는지 확인

### 추가 개선 가능 항목
1. 화살표에 "전역 패딩 적용" 체크박스 추가
2. Glow 애니메이션 속도를 각 방향별로 다르게 설정하는 옵션
3. 모바일 게이지바 좌우 위치 지원

---

## 문의 및 피드백

문제 발견 시 다음 정보와 함께 보고해주세요:
1. 문제가 발생하는 기능
2. 브라우저 및 버전
3. 화면 크기 (데스크톱/모바일)
4. 설정 값
5. 스크린샷 또는 동영상
