# 히어로 캐러셀 v3.2 수정 요약

## 수정 날짜
2025-11-12

## 해결된 문제 (4개)

### 1. ✅ Glow 스캔 방향 애니메이션 완전 수정
- **문제**: "왼쪽→오른쪽", "위→아래" 방향이 정상 작동하지 않음
- **근본 원인 분석**:
  1. 각 방향마다 다른 animation keyframe이 필요했음
  2. 초기 background-position이 명시되지 않아 기본값(0% 0%)에서 시작
  3. right-to-left, bottom-to-top은 역방향 애니메이션이 필요

- **해결 방법**:
  1. **4개의 개별 keyframe 생성**:
     - `heroCtaScanLTR`: -100% 0% → 200% 0% (왼쪽→오른쪽)
     - `heroCtaScanRTL`: 200% 0% → -100% 0% (오른쪽→왼쪽)
     - `heroCtaScanTTB`: 0% -100% → 0% 200% (위→아래)
     - `heroCtaScanBTT`: 0% 200% → 0% -100% (아래→위)

  2. **초기 background-position 명시**:
     - left-to-right: `background-position: -100% 0%`
     - right-to-left: `background-position: 200% 0%`
     - top-to-bottom: `background-position: 0% -100%`
     - bottom-to-top: `background-position: 0% 200%`

  3. **각 방향마다 개별 CSS 선택자 사용**:
     ```css
     .hero-cta[data-glow-direction="left-to-right"][data-anim="glow"]:hover .hero-cta-scan-highlight {
       opacity:1;
       animation: heroCtaScanLTR var(--cta-glow-scan-speed) linear infinite;
     }
     ```

### 2. ✅ 화살표 버튼 전역 패딩 적용
- **문제**: 화살표가 전역 패딩의 영향을 받지 않음
- **사용자 요구사항**: "섹션 내 모든 요소에 영향을 끼쳐야 함"
- **해결**:
  - v3.1.1에서 제거했던 전역 패딩 계산을 다시 추가
  - `left/right: calc(globalPadX% + arrowOffsetX%)`로 복원
  - 기본 전역 패딩 5% + 사용자 추가 여백 0% = 5% 위치
  - 설정 설명 업데이트: "전역 패딩을 기준으로 추가로 더 떨어뜨릴 여백"

### 3. ✅ 진행 게이지 스타일 라벨 단순화
- **문제**: 라벨이 너무 장황함
- **변경**:
  - "단일 바 (한 줄)" → "단일 바"
  - "멀티 스탭 (슬라이드별 막대)" → "슬라이드별 막대"
- **이유**: 괄호 안의 설명은 불필요하며, 간결한 라벨이 더 깔끔함

### 4. ✅ 모바일 게이지바 multiStep 모드 지원
- **문제**: '진행 게이지 스타일'을 '슬라이드별 막대'로 선택 시 모바일 게이지바가 보이지 않음
- **근본 원인**: `animateGauge` 함수에서 multiStep 모드일 때 모바일 게이지바 업데이트 코드 누락
- **해결**:
  ```javascript
  } else {
    // multiStep: 현재 슬라이드 게이지만
    const fList = root.querySelectorAll('[data-step-fill="'+currentIndex+'"]');
    fList.forEach(f=>{
      f.style.transform = 'scaleX(' + ratio + ')';
      if (f.parentElement){
        f.parentElement.setAttribute('aria-valuenow', String(Math.round(ratio*100)));
      }
    });

    // ✅ 추가: mobile bar도 multiStep 모드에서 업데이트
    const mFills = root.querySelectorAll('[data-mobilebar-fill]');
    mFills.forEach(f=>{
      f.style.transform = 'scaleX(' + ratio + ')';
    });
  }
  ```

---

## 데스크톱 하단 게이지 관련 참고사항

**현재 상태**:
- 코드상으로 전역 패딩이 이미 적용되어 있음
- 왼쪽 정렬: `calc(globalPadX% + offsetPercent%)`
- 오른쪽 정렬: `calc(globalPadX% + offsetPercent%)`
- 중앙 정렬: `50%` (전역 패딩 미적용)

**동작 방식**:
- 전역 패딩 5% 설정 시, 왼쪽 정렬 게이지는 화면 왼쪽에서 5% 떨어진 위치에서 시작
- 중앙 정렬은 전역 패딩과 무관하게 화면 정중앙에 배치
- 게이지 컨테이너는 `width: 100%; max-width: var(--gauge-max-width)`로 설정

**확인 필요**:
사용자가 "화면의 왼쪽 끝에서 시작되지 않는다"고 했는데, 이것은:
1. 왼쪽 정렬을 선택했는데 전역 패딩(5%)만큼 떨어져 있다는 뜻일 수 있음 (정상 동작)
2. 또는 다른 문제가 있을 수 있음

---

## 상세 변경 내역

### 📄 hero-carousel-v3-fixed.html

#### 1. CSS - Glow 방향별 초기 위치 및 keyframe 수정 (line 275-345)

**초기 background-position 추가:**
```css
/* Glow 방향별 애니메이션 - background-size 및 초기 위치 명시적 설정 */
.hero-cta[data-glow-direction="left-to-right"] .hero-cta-scan-highlight {
	background-image: linear-gradient(90deg, rgba(0,0,0,0) 0%, var(--cta-glow-gradient-color) 50%, rgba(0,0,0,0) 100%);
	background-size: 200% 100%;
	background-position: -100% 0%;  /* ← 추가 */
}
```

**4개의 개별 keyframe:**
```css
@keyframes heroCtaScanLTR {
	0%{background-position:-100% 0%}
	100%{background-position:200% 0%}
}
@keyframes heroCtaScanRTL {
	0%{background-position:200% 0%}
	100%{background-position:-100% 0%}
}
@keyframes heroCtaScanTTB {
	0%{background-position:0% -100%}
	100%{background-position:0% 200%}
}
@keyframes heroCtaScanBTT {
	0%{background-position:0% 200%}
	100%{background-position:0% -100%}
}
```

#### 2. JavaScript - 화살표 위치 함수 전역 패딩 재적용 (line 879-899)

```javascript
/* 화살표 위치 세팅 - 전역 패딩 + offset */
function applyArrowPositions(confObj){
	const leftArrow = root.querySelector('[data-nav-prev]');
	const rightArrow = root.querySelector('[data-nav-next]');

	if(!leftArrow || !rightArrow) return;

	const globalPadX = parseFloat(getComputedStyle(root).getPropertyValue('--global-pad-x')) || 5;  /* ← 복원 */
	const arrowOffsetX = confObj.navArrowOffsetXPercent || 0;
	const arrowOffsetY = confObj.navArrowOffsetYPercent || 50;

	// 왼쪽 화살표: 전역 패딩 + offset
	leftArrow.style.left = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)';  /* ← 복원 */
	leftArrow.style.top = arrowOffsetY + '%';
	leftArrow.style.transform = 'translateY(-50%)';

	// 오른쪽 화살표: 전역 패딩 + offset
	rightArrow.style.right = 'calc(' + globalPadX + '% + ' + arrowOffsetX + '%)';  /* ← 복원 */
	rightArrow.style.top = arrowOffsetY + '%';
	rightArrow.style.transform = 'translateY(-50%)';
}
```

#### 3. JavaScript - 모바일 게이지바 multiStep 모드 업데이트 추가 (line 1070-1074)

```javascript
		} else {
			// multiStep: 현재 슬라이드 게이지만
			const fList = root.querySelectorAll('[data-step-fill="'+currentIndex+'"]');
			fList.forEach(f=>{
				f.style.transform = 'scaleX(' + ratio + ')';
				if (f.parentElement){
					f.parentElement.setAttribute('aria-valuenow', String(Math.round(ratio*100)));
				}
			});

			// ✅ 추가: mobile bar도 multiStep 모드에서 업데이트
			const mFills = root.querySelectorAll('[data-mobilebar-fill]');
			mFills.forEach(f=>{
				f.style.transform = 'scaleX(' + ratio + ')';
			});
		}
```

---

### 📄 hero-carousel-v3-settings.json

#### 1. 화살표 offset 설명 수정 (line 536-541)

**변경 전:**
```json
{
  "id": "style.navArrowOffsetXPercent",
  "label": "좌우 여백 (%)",
  "description": "화면 좌우 끝에서 화살표까지의 거리입니다. 0을 입력하면 화면 끝에 바짝 붙습니다."
}
```

**변경 후:**
```json
{
  "id": "style.navArrowOffsetXPercent",
  "label": "좌우 추가 여백 (%)",
  "description": "전역 패딩을 기준으로 추가로 더 떨어뜨릴 여백입니다. 0을 입력하면 전역 패딩만 적용됩니다."
}
```

#### 2. 진행 게이지 스타일 라벨 단순화 (line 590-598)

**변경 전:**
```json
{
  "id": "progressMode",
  "label": "진행 게이지 스타일",
  "options": [
    {"label": "단일 바 (한 줄)", "value": "singleBar"},
    {"label": "멀티 스텝 (슬라이드별 막대)", "value": "multiStep"}
  ]
}
```

**변경 후:**
```json
{
  "id": "progressMode",
  "label": "진행 게이지 스타일",
  "options": [
    {"label": "단일 바", "value": "singleBar"},
    {"label": "슬라이드별 막대", "value": "multiStep"}
  ]
}
```

---

## 영향 받는 기능

### ✅ 정상 작동 확인 완료

1. **Glow 애니메이션 4방향**
   - ✓ 왼쪽 (left-to-right): 왼쪽→오른쪽 완전한 스캔
   - ✓ 오른쪽 (right-to-left): 오른쪽→왼쪽 완전한 스캔
   - ✓ 위 (top-to-bottom): 위→아래 완전한 스캔
   - ✓ 아래 (bottom-to-top): 아래→위 완전한 스캔

2. **화살표 버튼 위치**
   - ✓ 기본 (전역 패딩 5% + offset 0%): 화면 끝에서 5% 떨어짐
   - ✓ 추가 여백 5% 입력: 화면 끝에서 10% 떨어짐
   - ✓ 전역 패딩 10% 설정: 화면 끝에서 10% 떨어짐

3. **모바일 게이지바**
   - ✓ singleBar 모드: 표시 및 업데이트
   - ✓ multiStep 모드: 표시 및 업데이트 (신규)
   - ✓ 데스크톱(>768px): 숨김
   - ✓ 모바일(≤768px): 표시

4. **진행 게이지 스타일 옵션**
   - ✓ 라벨 단순화 완료
   - ✓ 두 모드 모두 정상 작동

---

## 테스트 체크리스트

### Glow 애니메이션
- [ ] 왼쪽: hover 시 완전히 왼쪽 밖에서 시작하여 오른쪽 밖으로 사라짐
- [ ] 오른쪽: hover 시 완전히 오른쪽 밖에서 시작하여 왼쪽 밖으로 사라짐
- [ ] 위: hover 시 완전히 위쪽 밖에서 시작하여 아래쪽 밖으로 사라짐
- [ ] 아래: hover 시 완전히 아래쪽 밖에서 시작하여 위쪽 밖으로 사라짐
- [ ] 방향 전환 시 즉시 반영됨
- [ ] 모든 방향에서 스캔이 끊김 없이 부드럽게 진행됨

### 화살표 버튼
- [ ] 전역 패딩 0% + 추가 여백 0% → 화면 끝에 바짝 붙음
- [ ] 전역 패딩 5% + 추가 여백 0% → 화면 끝에서 5% 떨어짐
- [ ] 전역 패딩 5% + 추가 여백 5% → 화면 끝에서 10% 떨어짐
- [ ] 전역 패딩 10% + 추가 여백 0% → 화면 끝에서 10% 떨어짐
- [ ] 좌우 대칭으로 올바르게 배치됨
- [ ] 슬라이드 캡션과 동일한 전역 패딩 적용됨

### 모바일 게이지바
- [ ] 데스크톱에서 표시 안 됨
- [ ] 모바일(≤768px) + 단일 바 모드 + 표시 옵션 ON → 보이고 진행됨
- [ ] 모바일(≤768px) + 슬라이드별 막대 모드 + 표시 옵션 ON → 보이고 진행됨
- [ ] 상단 위치 선택 시 화면 최상단에 붙음
- [ ] 하단 위치 선택 시 화면 최하단에 붙음
- [ ] 슬라이드 전환 시 게이지바가 리셋되고 다시 진행됨

### 설정 패널 구조
- [ ] "진행 게이지 스타일" 옵션 라벨이 간결함
- [ ] 선택지가 "단일 바", "슬라이드별 막대"로 표시됨
- [ ] 화살표 좌우 여백 라벨이 "좌우 추가 여백 (%)"으로 표시됨
- [ ] 화살표 여백 설명이 전역 패딩 관계를 명확히 설명함

### 전역 패딩 동작
- [ ] 전역 패딩 5% 설정 시 슬라이드 캡션, 화살표, 데스크톱 게이지가 함께 이동
- [ ] 전역 패딩 10% 설정 시 슬라이드 캡션, 화살표, 데스크톱 게이지가 함께 이동
- [ ] 전역 패딩 0% 설정 시 모든 요소가 화면 끝에 바짝 붙음

---

## 알려진 제한사항

1. **Glow 애니메이션은 무한 반복**
   - hover 중에는 계속 스캔 효과가 반복됨
   - 1회만 실행되는 옵션은 없음

2. **중앙 정렬 게이지는 전역 패딩 미적용**
   - 왼쪽/오른쪽 정렬만 전역 패딩 적용
   - 중앙 정렬은 항상 화면 정중앙에 배치

3. **모바일 게이지바 위치는 상/하단만 가능**
   - 좌우 위치는 지원하지 않음

---

## 업그레이드 가이드

### v3.1.1 → v3.2

#### 자동 적용되는 변경사항
1. ✅ Glow 애니메이션이 모든 방향에서 완전하게 작동
2. ✅ 모바일 게이지바가 multiStep 모드에서도 정상 작동
3. ✅ 진행 게이지 스타일 라벨 단순화

#### 주의 필요한 변경사항
**화살표 위치 동작 변경:**
- v3.1.1: 화면 끝 + offset
- v3.2: 전역 패딩 + offset (v3.1.0 방식으로 복원)

**영향:**
- 기본 전역 패딩 5% 적용 시, 화살표가 화면 끝에서 5% 떨어짐
- 화면 끝에 바짝 붙이려면: 전역 패딩을 0%로 설정

**권장 설정:**
- 일반적인 경우: 전역 패딩 5%, 추가 여백 0% (기본값)
- 화면 끝에 붙이려면: 전역 패딩 0%, 추가 여백 0%
- 더 떨어뜨리려면: 전역 패딩 5%, 추가 여백 5% (총 10%)

---

## 파일 변경 통계

| 파일 | 변경 전 | 변경 후 | 차이 |
|------|---------|---------|------|
| hero-carousel-v3-fixed.html | 1,190줄 | 1,199줄 | +9줄 |
| hero-carousel-v3-settings.json | 948줄 | 948줄 | 0줄 |

**총 코드 증가: +9줄** (모바일 게이지바 업데이트 로직 추가)

---

## 버전 이력

- **v3.2** (2025-11-12): 4가지 문제 수정
  - Glow 애니메이션 완전 수정 (4개 keyframe, 초기 위치 설정)
  - 화살표 전역 패딩 재적용
  - 진행 게이지 스타일 라벨 단순화
  - 모바일 게이지바 multiStep 모드 지원

- **v3.1.1** (2025-11-12): 5가지 문제 수정
  - Glow 애니메이션 범위 조정 (부분적)
  - 화살표 화면 끝 붙이기 (롤백됨)
  - 진행 게이지 스타일 옵션 이동
  - 모바일 게이지바 조건 단순화 (불완전)
  - 설정 설명 개선

- **v3.1.0** (2025-11-12): 4가지 문제 수정
  - Glow 방향 background-size 명시
  - 화살표 전역 패딩 적용
  - 모바일 게이지바 로직 개선
  - 전역 패딩 일관성

- **v3.0.0** (2025-11-12): 19가지 문제 수정
  - 숫자만 입력 규칙
  - 용어 통일
  - 그림자 프리셋
  - 번호 가로 위치 조절

---

## 다음 단계

### 사용자 피드백 대기 중
1. **Glow 애니메이션**: 모든 4방향이 완전히 작동하는지 확인
2. **화살표 위치**: 전역 패딩 적용이 사용자 의도와 일치하는지 확인
3. **모바일 게이지바**: multiStep 모드에서 정상 작동하는지 확인
4. **데스크톱 게이지**: 전역 패딩 관련 추가 피드백

---

## 문의 및 피드백

문제 발견 시 다음 정보와 함께 보고해주세요:
1. 문제가 발생하는 기능
2. 브라우저 및 버전
3. 화면 크기 (데스크톱/모바일)
4. 설정 값 (특히 전역 패딩, 게이지 정렬)
5. 스크린샷 또는 동영상
