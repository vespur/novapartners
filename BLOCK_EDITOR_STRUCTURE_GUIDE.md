# 블록 에디터 구조 개발 가이드

> 식스샵 프로 블록 에디터의 최적화된 구조를 정의하는 문서입니다.
> 사용자 경험을 최우선으로 하며, 직관적이고 체계적인 에디터 구성을 목표합니다.

---

## 📋 목차

1. [핵심 원칙](#핵심-원칙)
2. [에디터 섹션 구성](#에디터-섹션-구성)
3. [텍스트 컨트롤 표준](#텍스트-컨트롤-표준)
4. [반응형 설정 패턴](#반응형-설정-패턴)
5. [실제 적용 사례](#실제-적용-사례-1-1-1-1-히어로-섹션)
6. [체크리스트](#체크리스트)

---

## 핵심 원칙

### 1️⃣ **사용자 중심 설계**
- 모든 옵션에 `description` 필드로 친절한 설명 제공
- 선택사항 여부를 명확히 표시 (예: "(선택사항)", "(모바일 전용)")
- 기본값으로 완성도 높은 상태 제공

### 2️⃣ **명확한 섹션 구분**
- TITLE로 논리적 그룹 구성
- 이모지 활용으로 시각적 구분
- 순서: 콘텐츠 → 디자인 → 크기 → 텍스트 → 애니메이션

### 3️⃣ **데스크톱/모바일 완전 분리**
- 접두사로 명확히 구분: `desktopXxx`, `mobileXxx`
- 각각의 섹션을 따로 배치
- 같은 옵션도 별도로 관리 (예: 글자 크기, 여백)

### 4️⃣ **세밀한 커스터마이징**
- 글자: 크기, 색상, 굵기, 줄바꿈 높이 모두 제어
- 배경: 그래디언트, 오버레이, 데코레이션 선택
- 애니메이션: 활성화/비활성화 + 타입 + 속도

---

## 에디터 섹션 구성

### 표준 섹션 순서

```
1. 📝 텍스트 콘텐츠
   ├─ 제목, 설명, 라벨 등의 텍스트 입력
   └─ LIST 형태의 항목 관리

2. 🔘 버튼 설정 (필요시)
   ├─ 버튼 텍스트, 링크, 스타일
   └─ 최대 개수 제한

3. 🎨 배경 & 색상
   ├─ 그래디언트 색상
   ├─ 오버레이 투명도
   ├─ 배경 이미지 (필요시)
   └─ 데코레이션 요소

4. 📏 데스크톱 크기 및 간격
   ├─ 최소 높이
   ├─ 여백 (위아래, 좌우)
   └─ 요소 간격

5. 📝 데스크톱 텍스트 스타일
   ├─ 각 텍스트별 글자 크기
   ├─ 텍스트 색상
   ├─ 텍스트 굵기
   └─ 줄바꿈 높이

6. 📱 모바일 크기 및 간격
   ├─ 최소 높이
   ├─ 여백 (위아래, 좌우)
   └─ 요소 간격

7. 📝 모바일 텍스트 스타일
   ├─ 각 텍스트별 글자 크기
   ├─ 텍스트 굵기
   └─ 줄바꿈 높이

8. ✨ 애니메이션 (필요시)
   ├─ 애니메이션 활성화
   ├─ 애니메이션 타입
   └─ 애니메이션 속도
```

---

## 텍스트 컨트롤 표준

### 텍스트 글자 크기 (RANGE)

```json
{
  "id": "desktopTitleFontSize",
  "label": "제목 글자 크기",
  "type": "RANGE",
  "min": 32,
  "max": 64,
  "step": 2,
  "unit": "px",
  "default": 48
}
```

| 텍스트 타입 | Min | Max | Step | Default | 모바일 Min-Max |
|----------|-----|-----|------|---------|----------------|
| 라벨 | 10 | 16 | 1 | 13 | 10-14 |
| 제목 | 32 | 64 | 2 | 48 | 24-42 |
| 설명 | 14 | 24 | 1 | 18 | 13-18 |
| 버튼 | 13 | 18 | 1 | 16 | 12-16 |

### 텍스트 색상 (COLOR_PICKER)

```json
{
  "id": "desktopTitleColor",
  "label": "제목 색상",
  "type": "COLOR_PICKER",
  "default": "#000000"
}
```

**기본값 가이드:**
- 제목: `#000000` (검정)
- 라벨: `#0055ff` (NOVA 브랜드 파란색)
- 설명: `#666666` (회색)
- 강조: `#0055ff` (파란색)

### 텍스트 굵기 (RADIO)

```json
{
  "id": "desktopTitleFontWeight",
  "label": "제목 굵기",
  "type": "RADIO",
  "options": [
    {"label": "보통 (400)", "value": "400"},
    {"label": "진함 (600)", "value": "600"},
    {"label": "매우 진함 (700)", "value": "700"}
  ],
  "default": "700"
}
```

**라벨용:**
```json
"options": [
  {"label": "얇음 (400)", "value": "400"},
  {"label": "중간 (500)", "value": "500"},
  {"label": "진함 (600)", "value": "600"},
  {"label": "매우 진함 (700)", "value": "700"}
]
```

### 줄바꿈 높이 (LINE-HEIGHT, RANGE)

```json
{
  "id": "desktopTitleLineHeight",
  "label": "제목 줄바꿈 높이",
  "description": "1.1 = 좁음, 1.5 = 넓음. 값이 작을수록 줄이 좁혀집니다",
  "type": "RANGE",
  "min": 1.0,
  "max": 1.6,
  "step": 0.1,
  "unit": "",
  "default": 1.2
}
```

| 텍스트 타입 | Min | Max | Step | Default |
|----------|-----|-----|------|---------|
| 제목 | 1.0 | 1.6 | 0.1 | 1.2 |
| 설명 | 1.3 | 1.9 | 0.1 | 1.7 |

---

## 반응형 설정 패턴

### 패턴: 데스크톱 + 모바일 분리

```json
{
  "type": "TITLE",
  "content": "📏 데스크톱 크기 및 간격"
},
{
  "id": "desktopMinHeight",
  "label": "최소 높이",
  "description": "데스크톱 화면에서의 섹션 최소 높이입니다",
  "type": "RANGE",
  "min": 400,
  "max": 800,
  "step": 20,
  "unit": "px",
  "default": 600
},
{
  "id": "desktopPaddingY",
  "label": "위아래 여백",
  "type": "RANGE",
  "min": 40,
  "max": 120,
  "step": 10,
  "unit": "px",
  "default": 80
},
{
  "id": "desktopPaddingX",
  "label": "좌우 여백",
  "type": "RANGE",
  "min": 20,
  "max": 100,
  "step": 10,
  "unit": "px",
  "default": 40
},

// 모바일 섹션 (별도)
{
  "type": "TITLE",
  "content": "📱 모바일 크기 및 간격"
},
{
  "id": "mobileMinHeight",
  "label": "최소 높이",
  "description": "모바일 화면(768px 이하)에서의 섹션 최소 높이입니다",
  "type": "RANGE",
  "min": 300,
  "max": 600,
  "step": 20,
  "unit": "px",
  "default": 450
},
{
  "id": "mobilePaddingY",
  "label": "위아래 여백",
  "type": "RANGE",
  "min": 30,
  "max": 80,
  "step": 10,
  "unit": "px",
  "default": 50
},
{
  "id": "mobilePaddingX",
  "label": "좌우 여백",
  "type": "RANGE",
  "min": 16,
  "max": 60,
  "step": 4,
  "unit": "px",
  "default": 20
}
```

### 패턴: 데스크톱/모바일 텍스트 분리

각 텍스트 요소마다:

```json
{
  "type": "TITLE",
  "content": "📝 데스크톱 텍스트 스타일"
},
{
  "id": "desktopTitleFontSize",
  "label": "제목 글자 크기",
  "type": "RANGE",
  "min": 32,
  "max": 64,
  "step": 2,
  "unit": "px",
  "default": 48
},
{
  "id": "desktopTitleColor",
  "label": "제목 색상",
  "type": "COLOR_PICKER",
  "default": "#000000"
},
{
  "id": "desktopTitleFontWeight",
  "label": "제목 굵기",
  "type": "RADIO",
  "options": [
    {"label": "보통 (400)", "value": "400"},
    {"label": "진함 (600)", "value": "600"},
    {"label": "매우 진함 (700)", "value": "700"}
  ],
  "default": "700"
},
{
  "id": "desktopTitleLineHeight",
  "label": "제목 줄바꿈 높이",
  "type": "RANGE",
  "min": 1.0,
  "max": 1.6,
  "step": 0.1,
  "unit": "",
  "default": 1.2
},

// 모바일 섹션
{
  "type": "TITLE",
  "content": "📝 모바일 텍스트 스타일"
},
{
  "id": "mobileTitleFontSize",
  "label": "제목 글자 크기",
  "type": "RANGE",
  "min": 24,
  "max": 42,
  "step": 2,
  "unit": "px",
  "default": 32
},
// ... 나머지 모바일 속성
```

---

## 실제 적용 사례: 1-1-1-1 히어로 섹션

### 에디터 구조

```
📝 텍스트 콘텐츠
  ├─ label (TEXT, 선택)
  ├─ title (TEXT)
  └─ description (TEXTAREA)

🔘 버튼 설정
  └─ buttons (LIST, max 2)
      ├─ buttonText (TEXT)
      ├─ buttonUrl (LINK)
      ├─ buttonStyle (RADIO: primary/secondary)
      └─ openNewTab (CHECKBOX)

🎨 배경 & 색상
  ├─ bgGradientStart (COLOR_PICKER)
  ├─ bgGradientEnd (COLOR_PICKER)
  ├─ bgOverlayOpacity (RANGE: 0-1)
  └─ enableDecorationCircles (CHECKBOX)

📏 데스크톱 크기 및 간격
  ├─ desktopMinHeight (RANGE)
  ├─ desktopPaddingY (RANGE)
  ├─ desktopPaddingX (RANGE)
  └─ desktopButtonGap (RANGE)

📝 데스크톱 텍스트 스타일
  ├─ desktopLabelFontSize (RANGE)
  ├─ desktopLabelColor (COLOR_PICKER)
  ├─ desktopLabelFontWeight (RADIO)
  ├─ desktopTitleFontSize (RANGE)
  ├─ desktopTitleColor (COLOR_PICKER)
  ├─ desktopTitleFontWeight (RADIO)
  ├─ desktopTitleLineHeight (RANGE)
  ├─ desktopDescriptionFontSize (RANGE)
  ├─ desktopDescriptionColor (COLOR_PICKER)
  ├─ desktopDescriptionLineHeight (RANGE)
  └─ desktopButtonFontSize (RANGE)

📱 모바일 크기 및 간격
  ├─ mobileMinHeight (RANGE)
  ├─ mobilePaddingY (RANGE)
  ├─ mobilePaddingX (RANGE)
  └─ mobileButtonGap (RANGE)

📝 모바일 텍스트 스타일
  ├─ mobileLabelFontSize (RANGE)
  ├─ mobileLabelFontWeight (RADIO)
  ├─ mobileTitleFontSize (RANGE)
  ├─ mobileTitleFontWeight (RADIO)
  ├─ mobileTitleLineHeight (RANGE)
  ├─ mobileDescriptionFontSize (RANGE)
  ├─ mobileDescriptionLineHeight (RANGE)
  └─ mobileButtonFontSize (RANGE)

✨ 애니메이션
  ├─ enableAnimation (CHECKBOX)
  ├─ animationType (RADIO: fadeInUp/slideInUp/scaleIn)
  └─ animationDuration (RANGE: 0.3-1.5초)
```

### 총 설정 개수: **48개**
- 콘텐츠: 3개
- 버튼: 4개 (LIST 내)
- 배경/색상: 4개
- 데스크톱 크기: 4개
- 데스크톱 텍스트: 11개
- 모바일 크기: 4개
- 모바일 텍스트: 11개
- 애니메이션: 3개

---

## 체크리스트

### 에디터 설계 시

- [ ] TITLE로 섹션 명확히 구분
- [ ] 각 섹션에 이모지 사용
- [ ] 모든 옵션에 `description` 필드 추가
- [ ] 선택사항 여부 명시
- [ ] 기본값(default) 설정
- [ ] 데스크톱과 모바일을 완전히 분리
- [ ] 접두사 사용: `desktop*`, `mobile*`
- [ ] 텍스트는 크기, 색상, 굵기, 줄바꿈 높이 모두 제어
- [ ] description은 100자 이하로 제한
- [ ] 모든 문구는 한글로 작성
- [ ] camelCase ID 명명

### HTML 구현 시

- [ ] JSON의 모든 property를 CSS에서 사용
- [ ] `property.desktopXxx` vs `property.mobileXxx` 명확히 구분
- [ ] @media (max-width: 768px)에서 모바일 스타일 재정의
- [ ] 각 텍스트에 color, font-size, font-weight, line-height 적용
- [ ] 조건부 CSS ({{#if}}) 활용
- [ ] bm.onContextChange 필수 구현
- [ ] bm.onDestroy 구현 (이벤트 리스너 있을 경우)
- [ ] 빈 링크 클릭 방지 (헤더/푸터/버튼이 있는 경우)
- [ ] 성능 최적화: transform: translateZ(0), backface-visibility: hidden

---

## 추가 팁

### 오버레이 투명도 설정 시

```json
{
  "id": "bgOverlayOpacity",
  "label": "배경 오버레이 투명도",
  "description": "0 = 투명, 1 = 불투명",
  "type": "RANGE",
  "min": 0,
  "max": 1,
  "step": 0.05,
  "unit": "",
  "default": 0
}
```

HTML에서:
```css
{{#if (gt property.bgOverlayOpacity 0)}}
.section::before {
  background-color: rgba(0, 0, 0, {{property.bgOverlayOpacity}});
}
{{/if}}
```

### 애니메이션 타입 정의

```json
{
  "id": "animationType",
  "label": "애니메이션 타입",
  "type": "RADIO",
  "options": [
    {"label": "아래에서 위로 페이드 (fadeInUp)", "value": "fadeInUp"},
    {"label": "위에서 아래로 슬라이드 (slideInUp)", "value": "slideInUp"},
    {"label": "확대되며 나타남 (scaleIn)", "value": "scaleIn"}
  ],
  "default": "fadeInUp"
}
```

HTML에서:
```css
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes slideInUp {
  from { opacity: 0; transform: translateY(60px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes scaleIn {
  from { opacity: 0; transform: scale(0.9); }
  to { opacity: 1; transform: scale(1); }
}

.content {
  {{#if property.enableAnimation}}
  animation: {{property.animationType}} {{property.animationDuration}}s ease forwards;
  {{/if}}
}
```

---

**문서 버전**: 1.0
**작성일**: 2025-11-13
**기준 블록**: 1-1-1-1 히어로 섹션
