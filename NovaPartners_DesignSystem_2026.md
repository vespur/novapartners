# 🎨 노바 파트너스 디자인 시스템 2026

> AI 솔루션 전문 중기업을 위한 엔터프라이즈급 디자인 시스템  
> 포인트 컬러: #0055FF | 2026년 웹 트렌드 반영

---

## 📋 목차

1. [디자인 철학](#디자인-철학)
2. [컬러 시스템](#컬러-시스템)
3. [타이포그래피](#타이포그래피)
4. [간격 시스템](#간격-시스템)
5. [그리드 시스템](#그리드-시스템)
6. [컴포넌트 라이브러리](#컴포넌트-라이브러리)
7. [모션 & 인터랙션](#모션--인터랙션)
8. [아이콘 시스템](#아이콘-시스템)
9. [접근성 가이드라인](#접근성-가이드라인)
10. [2026 트렌드 적용](#2026-트렌드-적용)

---

## 🎯 디자인 철학

### 핵심 원칙

**1. AI-First Experience**
- 기술 신뢰성과 미래지향적 비주얼
- 데이터 중심의 명확한 정보 전달
- 인간 중심의 따뜻한 인터페이스

**2. Enterprise Clarity**
- B2B 맥락에 최적화된 프로페셔널한 톤
- 복잡한 정보의 단순화 및 계층화
- 명확한 CTA와 사용자 여정

**3. 2026 Modern Standards**
- Micro-delight 인터랙션
- Variable typography
- Purposeful motion
- Accessibility-first

**4. Cross-platform Consistency**
- 데스크톱, 태블릿, 모바일 일관성
- 브라우저 호환성
- 성능 최적화

---

## 🎨 컬러 시스템

### 컬러 비율 전략
```
포인트 컬러 (#0055FF): 20%
블랙/그레이: 30%
화이트/라이트: 50%
```

### Primary Colors (주요 색상)

#### Brand Blue (#0055FF)
```css
--color-primary-900: #00264D;  /* Darkest */
--color-primary-800: #003366;
--color-primary-700: #004080;
--color-primary-600: #004D99;
--color-primary-500: #0055FF;  /* Brand */
--color-primary-400: #3377FF;
--color-primary-300: #6699FF;
--color-primary-200: #99BBFF;
--color-primary-100: #CCDDFF;
--color-primary-50:  #E6F0FF;  /* Lightest */
```

**용도:**
- Primary CTA 버튼
- 링크 및 인터랙티브 요소
- 중요 정보 하이라이트
- 로고 및 브랜드 요소
- Progress indicators

### Neutral Colors (중립 색상)

#### Grayscale
```css
--color-gray-900: #0A0A0A;  /* 거의 검정 */
--color-gray-800: #1A1A1A;
--color-gray-700: #2D2D2D;
--color-gray-600: #4D4D4D;
--color-gray-500: #707070;
--color-gray-400: #9E9E9E;
--color-gray-300: #C2C2C2;
--color-gray-200: #E0E0E0;
--color-gray-100: #F0F0F0;
--color-gray-50:  #F8F8F8;
--color-white:    #FFFFFF;
```

**용도:**
- 본문 텍스트 (gray-900)
- 보조 텍스트 (gray-600)
- 디스에이블 상태 (gray-400)
- 테두리/구분선 (gray-200)
- 배경 (gray-50)

### Semantic Colors (의미 색상)

#### Success (성공)
```css
--color-success-700: #006B3D;
--color-success-500: #00A86B;
--color-success-100: #D4F4E2;
```

#### Warning (경고)
```css
--color-warning-700: #CC6600;
--color-warning-500: #FF9933;
--color-warning-100: #FFE5CC;
```

#### Error (오류)
```css
--color-error-700: #CC0000;
--color-error-500: #FF3333;
--color-error-100: #FFD6D6;
```

#### Info (정보)
```css
--color-info-700: #0066CC;
--color-info-500: #3399FF;
--color-info-100: #D6EBFF;
```

### Surface Colors (표면 색상)

```css
--surface-primary:   #FFFFFF;
--surface-secondary: #F8F8F8;
--surface-tertiary:  #F0F0F0;
--surface-overlay:   rgba(0, 0, 0, 0.5);
--surface-card:      #FFFFFF;
--surface-elevated:  #FFFFFF; /* with shadow */
```

### 컬러 사용 가이드라인

**Contrast Ratios (WCAG 2.1 AA 기준)**
- 일반 텍스트: 4.5:1 이상
- 큰 텍스트 (18pt+): 3:1 이상
- UI 컴포넌트: 3:1 이상

**Primary Blue 사용 비율**
- Hero Section: 10-15%
- Main Content: 5-10%
- Footer: 5-10%
- Overall Page: 15-20%

**다크모드 지원**
```css
@media (prefers-color-scheme: dark) {
  --surface-primary: #0A0A0A;
  --surface-secondary: #1A1A1A;
  --color-primary-500: #3377FF; /* 약간 밝게 조정 */
}
```

---

## ✍️ 타이포그래피

### 폰트 패밀리

**Primary Font (본문/UI)**
```css
font-family: 'Pretendard Variable', -apple-system, BlinkMacSystemFont, 
             'Segoe UI', 'Roboto', 'Helvetica Neue', Arial, sans-serif;
```

**Fallback System Fonts**
- macOS/iOS: -apple-system (San Francisco)
- Windows: Segoe UI
- Android: Roboto

**숫자/데이터 전용**
```css
font-family: 'Roboto Mono', 'SF Mono', 'Monaco', 'Courier New', monospace;
font-variant-numeric: tabular-nums; /* 숫자 정렬 */
```

### 타입 스케일 (Desktop)

#### Display (특대)
```css
--font-display-xl: 72px;   /* Hero 타이틀 */
--font-display-lg: 60px;   /* 섹션 대제목 */
--font-display-md: 48px;   /* 페이지 타이틀 */
```

#### Heading (제목)
```css
--font-h1: 40px;   /* 주요 섹션 */
--font-h2: 32px;   /* 서브 섹션 */
--font-h3: 28px;   /* 카드 제목 */
--font-h4: 24px;   /* 소제목 */
--font-h5: 20px;   /* 작은 제목 */
--font-h6: 18px;   /* 최소 제목 */
```

#### Body (본문)
```css
--font-body-xl: 20px;   /* 리드 텍스트 */
--font-body-lg: 18px;   /* 큰 본문 */
--font-body-md: 16px;   /* 기본 본문 */
--font-body-sm: 14px;   /* 작은 텍스트 */
--font-body-xs: 12px;   /* 캡션/힌트 */
```

### 타입 스케일 (Mobile)

```css
/* 모바일에서는 -4px ~ -8px 감소 */
--font-display-xl-mobile: 48px;
--font-display-lg-mobile: 40px;
--font-display-md-mobile: 36px;
--font-h1-mobile: 32px;
--font-h2-mobile: 28px;
--font-h3-mobile: 24px;
--font-h4-mobile: 20px;
--font-body-md-mobile: 16px; /* 동일 유지 */
```

### Font Weight (가중치)

```css
--font-weight-light:    300;
--font-weight-regular:  400;
--font-weight-medium:   500;
--font-weight-semibold: 600;
--font-weight-bold:     700;
--font-weight-extrabold: 800;
```

**사용 가이드:**
- Display/H1-H2: Bold (700)
- H3-H4: Semibold (600)
- H5-H6: Medium (500)
- Body: Regular (400)
- Caption: Regular (400)

### Line Height (행간)

```css
--line-height-tight:  1.2;  /* Display, H1-H2 */
--line-height-normal: 1.5;  /* H3-H6, Body */
--line-height-relaxed: 1.75; /* 긴 본문 */
```

### Letter Spacing (자간)

```css
--letter-spacing-tight:  -0.02em;  /* Display */
--letter-spacing-normal:  0;       /* 기본 */
--letter-spacing-wide:    0.02em;  /* All-caps */
```

### Typography Tokens (조합 예시)

```css
/* H1 Desktop */
.heading-1 {
  font-size: 40px;
  font-weight: 700;
  line-height: 1.2;
  letter-spacing: -0.02em;
  color: var(--color-gray-900);
}

/* Body Medium Desktop */
.body-md {
  font-size: 16px;
  font-weight: 400;
  line-height: 1.5;
  letter-spacing: 0;
  color: var(--color-gray-700);
}

/* Caption Small */
.caption-sm {
  font-size: 12px;
  font-weight: 400;
  line-height: 1.5;
  color: var(--color-gray-600);
}
```

### Variable Font 활용 (2026 트렌드)

```css
/* Animated text weight on scroll */
@supports (font-variation-settings: normal) {
  .hero-title {
    font-variation-settings: 'wght' 700;
    transition: font-variation-settings 0.3s ease;
  }
  
  .hero-title:hover {
    font-variation-settings: 'wght' 800;
  }
}
```

---

## 📐 간격 시스템

### Spacing Scale (8px 기반)

```css
--space-1:   4px;    /* 0.25rem */
--space-2:   8px;    /* 0.5rem */
--space-3:   12px;   /* 0.75rem */
--space-4:   16px;   /* 1rem */
--space-5:   20px;   /* 1.25rem */
--space-6:   24px;   /* 1.5rem */
--space-8:   32px;   /* 2rem */
--space-10:  40px;   /* 2.5rem */
--space-12:  48px;   /* 3rem */
--space-16:  64px;   /* 4rem */
--space-20:  80px;   /* 5rem */
--space-24:  96px;   /* 6rem */
--space-32:  128px;  /* 8rem */
```

### Component Spacing

#### 버튼 내부 여백
```css
/* Small */
padding: 8px 16px;   /* space-2 space-4 */

/* Medium (기본) */
padding: 12px 24px;  /* space-3 space-6 */

/* Large */
padding: 16px 32px;  /* space-4 space-8 */
```

#### 카드 여백
```css
padding: 24px;       /* Desktop */
padding: 16px;       /* Mobile */
gap: 16px;           /* 내부 요소 간격 */
```

#### 섹션 간격
```css
/* Desktop */
--section-padding-y: 80px;  /* space-20 */
--section-padding-x: 40px;  /* space-10 */

/* Tablet */
--section-padding-y: 60px;
--section-padding-x: 32px;

/* Mobile */
--section-padding-y: 40px;  /* space-10 */
--section-padding-x: 20px;  /* space-5 */
```

### Layout Spacing

```css
/* 컨테이너 최대 너비 */
--container-max-width: 1280px;

/* 사이드 여백 */
--container-padding: 40px; /* Desktop */
--container-padding: 20px; /* Mobile */

/* 그리드 Gap */
--grid-gap: 24px;     /* Desktop */
--grid-gap: 16px;     /* Mobile */
```

---

## 📊 그리드 시스템

### Breakpoints

```css
/* Mobile First Approach */
--breakpoint-xs: 0px;      /* 0-575px */
--breakpoint-sm: 576px;    /* Small devices */
--breakpoint-md: 768px;    /* Tablets */
--breakpoint-lg: 992px;    /* Desktops */
--breakpoint-xl: 1200px;   /* Large desktops */
--breakpoint-xxl: 1440px;  /* Extra large */
```

### Grid Layout

#### 12-Column Grid
```css
.container {
  max-width: 1280px;
  margin: 0 auto;
  padding: 0 40px;
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 24px;
}

/* Responsive */
@media (max-width: 768px) {
  .container {
    padding: 0 20px;
    gap: 16px;
  }
}
```

#### Common Column Spans
```css
/* Full width */
.col-12 { grid-column: span 12; }

/* Half */
.col-6 { grid-column: span 6; }

/* Third */
.col-4 { grid-column: span 4; }

/* Quarter */
.col-3 { grid-column: span 3; }

/* Mobile: 모두 Full Width */
@media (max-width: 768px) {
  .col-12, .col-6, .col-4, .col-3 {
    grid-column: span 12;
  }
}
```

### Content Width Guidelines

```css
/* 읽기 최적 라인 길이 */
--content-width-narrow: 640px;   /* 블로그 포스트 */
--content-width-medium: 768px;   /* 일반 콘텐츠 */
--content-width-wide: 1024px;    /* 대시보드 */
--content-width-full: 1280px;    /* 전체 너비 */
```

---

## 🧩 컴포넌트 라이브러리

### 1. Buttons

#### Primary Button
```css
.btn-primary {
  background: var(--color-primary-500);
  color: #FFFFFF;
  padding: 12px 24px;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
}

.btn-primary:hover {
  background: var(--color-primary-600);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 85, 255, 0.3);
}

.btn-primary:active {
  transform: translateY(0);
}
```

#### Secondary Button
```css
.btn-secondary {
  background: transparent;
  color: var(--color-primary-500);
  border: 2px solid var(--color-primary-500);
  padding: 10px 22px; /* 테두리 2px 고려 */
}

.btn-secondary:hover {
  background: var(--color-primary-50);
  border-color: var(--color-primary-600);
}
```

#### Ghost Button
```css
.btn-ghost {
  background: transparent;
  color: var(--color-gray-700);
  border: none;
  padding: 12px 24px;
}

.btn-ghost:hover {
  background: var(--color-gray-100);
  color: var(--color-gray-900);
}
```

#### Button Sizes
```css
/* Small */
.btn-sm { 
  padding: 8px 16px; 
  font-size: 14px; 
}

/* Medium (default) */
.btn-md { 
  padding: 12px 24px; 
  font-size: 16px; 
}

/* Large */
.btn-lg { 
  padding: 16px 32px; 
  font-size: 18px; 
}
```

### 2. Cards

```css
.card {
  background: var(--surface-card);
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
}

.card:hover {
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
  transform: translateY(-4px);
}

/* Card with border */
.card-bordered {
  border: 1px solid var(--color-gray-200);
  box-shadow: none;
}

/* Interactive card */
.card-interactive {
  cursor: pointer;
}

.card-interactive:hover {
  border-color: var(--color-primary-500);
  box-shadow: 0 8px 24px rgba(0, 85, 255, 0.12);
}
```

### 3. Inputs

```css
.input {
  width: 100%;
  padding: 12px 16px;
  font-size: 16px;
  color: var(--color-gray-900);
  background: #FFFFFF;
  border: 1px solid var(--color-gray-300);
  border-radius: 8px;
  transition: all 0.2s ease;
}

.input:focus {
  outline: none;
  border-color: var(--color-primary-500);
  box-shadow: 0 0 0 3px rgba(0, 85, 255, 0.1);
}

.input::placeholder {
  color: var(--color-gray-400);
}

.input:disabled {
  background: var(--color-gray-50);
  color: var(--color-gray-400);
  cursor: not-allowed;
}

/* Error state */
.input-error {
  border-color: var(--color-error-500);
}

.input-error:focus {
  box-shadow: 0 0 0 3px rgba(255, 51, 51, 0.1);
}
```

### 4. Navigation

#### Header
```css
.header {
  position: sticky;
  top: 0;
  z-index: 1000;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid var(--color-gray-200);
  padding: 16px 40px;
}

.nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 1280px;
  margin: 0 auto;
}

.nav-links {
  display: flex;
  gap: 32px;
  list-style: none;
}

.nav-link {
  color: var(--color-gray-700);
  font-weight: 500;
  text-decoration: none;
  transition: color 0.2s;
  position: relative;
}

.nav-link:hover {
  color: var(--color-primary-500);
}

/* Active indicator */
.nav-link.active::after {
  content: '';
  position: absolute;
  bottom: -20px;
  left: 0;
  right: 0;
  height: 3px;
  background: var(--color-primary-500);
  border-radius: 3px 3px 0 0;
}
```

### 5. Badges & Tags

```css
.badge {
  display: inline-flex;
  align-items: center;
  padding: 4px 12px;
  font-size: 12px;
  font-weight: 600;
  border-radius: 999px;
  background: var(--color-gray-100);
  color: var(--color-gray-700);
}

.badge-primary {
  background: var(--color-primary-100);
  color: var(--color-primary-700);
}

.badge-success {
  background: var(--color-success-100);
  color: var(--color-success-700);
}
```

### 6. Modals

```css
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2000;
}

.modal {
  background: #FFFFFF;
  border-radius: 16px;
  max-width: 600px;
  width: 90%;
  max-height: 90vh;
  overflow: auto;
  padding: 32px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.2);
}
```

---

## 🎬 모션 & 인터랙션

### Animation Duration

```css
--duration-instant: 100ms;  /* 즉각 반응 */
--duration-fast:    200ms;  /* 빠른 전환 */
--duration-normal:  300ms;  /* 기본 */
--duration-slow:    500ms;  /* 느린 전환 */
```

### Easing Functions

```css
--ease-in:        cubic-bezier(0.4, 0, 1, 1);
--ease-out:       cubic-bezier(0, 0, 0.2, 1);
--ease-in-out:    cubic-bezier(0.4, 0, 0.2, 1);
--ease-bounce:    cubic-bezier(0.68, -0.55, 0.265, 1.55);
```

### Micro-interactions (2026 트렌드)

#### Button Press Feedback
```css
.btn {
  transition: all 200ms var(--ease-out);
}

.btn:active {
  transform: scale(0.98);
}
```

#### Hover Reveal
```css
.card {
  position: relative;
  overflow: hidden;
}

.card::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  border-radius: 50%;
  background: rgba(0, 85, 255, 0.1);
  transform: translate(-50%, -50%);
  transition: width 0.6s, height 0.6s;
}

.card:hover::before {
  width: 300%;
  height: 300%;
}
```

#### Scroll-triggered Animation
```css
/* Fade in up */
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-on-scroll {
  animation: fadeInUp 0.6s var(--ease-out);
}
```

#### Loading States
```css
@keyframes skeleton-loading {
  0% {
    background-position: -200% 0;
  }
  100% {
    background-position: 200% 0;
  }
}

.skeleton {
  background: linear-gradient(
    90deg,
    var(--color-gray-200) 25%,
    var(--color-gray-100) 50%,
    var(--color-gray-200) 75%
  );
  background-size: 200% 100%;
  animation: skeleton-loading 1.5s infinite;
  border-radius: 4px;
}
```

### Page Transitions

```css
/* Fade transition */
.page-enter {
  opacity: 0;
}

.page-enter-active {
  opacity: 1;
  transition: opacity 300ms var(--ease-out);
}

.page-exit {
  opacity: 1;
}

.page-exit-active {
  opacity: 0;
  transition: opacity 200ms var(--ease-in);
}
```

---

## 🎯 아이콘 시스템

### Icon Guidelines

**크기:**
```css
--icon-xs: 16px;
--icon-sm: 20px;
--icon-md: 24px;  /* 기본 */
--icon-lg: 32px;
--icon-xl: 48px;
```

**스타일:**
- Line weight: 2px
- Corner radius: 2px
- Grid: 24×24px
- Style: Outlined (기본), Filled (강조)

**컬러:**
```css
/* 기본 */
.icon {
  color: var(--color-gray-600);
}

/* Interactive */
.icon-interactive {
  transition: color 0.2s;
}

.icon-interactive:hover {
  color: var(--color-primary-500);
}

/* Brand */
.icon-brand {
  color: var(--color-primary-500);
}
```

### Icon Library
- Heroicons (추천)
- Lucide Icons
- Material Symbols
- 커스텀 SVG

---

## ♿ 접근성 가이드라인

### Color Contrast

**WCAG 2.1 Level AA**
- 일반 텍스트: 4.5:1
- 큰 텍스트 (18pt+): 3:1
- UI 컴포넌트: 3:1

### Focus States

```css
*:focus-visible {
  outline: 3px solid var(--color-primary-500);
  outline-offset: 2px;
  border-radius: 4px;
}

/* 커스텀 포커스 */
.btn:focus-visible {
  outline: none;
  box-shadow: 0 0 0 3px rgba(0, 85, 255, 0.3);
}
```

### Skip Links

```css
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: var(--color-primary-500);
  color: #FFFFFF;
  padding: 8px 16px;
  text-decoration: none;
  z-index: 9999;
}

.skip-link:focus {
  top: 0;
}
```

### Screen Reader Only

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

### Keyboard Navigation

```css
/* Visible tab order */
[tabindex="0"]:focus-visible,
button:focus-visible,
a:focus-visible {
  outline: 3px solid var(--color-primary-500);
  outline-offset: 2px;
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## 🚀 2026 트렌드 적용

### 1. Micro-Delight Animations
```css
/* Subtle bounce on button */
.btn-delight {
  animation: subtle-bounce 0.5s ease;
}

@keyframes subtle-bounce {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}
```

### 2. Variable Typography
```css
/* Responsive font weight */
.dynamic-heading {
  font-variation-settings: 'wght' 700;
}

@media (min-width: 1200px) {
  .dynamic-heading {
    font-variation-settings: 'wght' 800;
  }
}
```

### 3. Organic Shapes
```css
/* Blob background */
.blob-bg {
  background: radial-gradient(
    ellipse at top left,
    var(--color-primary-100),
    transparent 50%
  );
  border-radius: 30% 70% 70% 30% / 30% 30% 70% 70%;
}
```

### 4. Glass Morphism
```css
.glass {
  background: rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(10px) saturate(180%);
  border: 1px solid rgba(255, 255, 255, 0.3);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}
```

### 5. 3D Subtle Effects
```css
.card-3d {
  transform: perspective(1000px) rotateX(0deg);
  transition: transform 0.3s;
}

.card-3d:hover {
  transform: perspective(1000px) rotateX(2deg);
}
```

### 6. Dark Mode
```css
@media (prefers-color-scheme: dark) {
  :root {
    --surface-primary: #0A0A0A;
    --surface-secondary: #1A1A1A;
    --color-gray-900: #F0F0F0;
    --color-gray-700: #C2C2C2;
    --color-primary-500: #3377FF;
  }
}
```

### 7. Scroll-Snap Sections
```css
.scroll-container {
  scroll-snap-type: y mandatory;
  overflow-y: scroll;
  height: 100vh;
}

.section {
  scroll-snap-align: start;
  min-height: 100vh;
}
```

### 8. AI-Inspired Elements
```css
/* Gradient border */
.ai-card {
  position: relative;
  background: #FFFFFF;
  padding: 2px;
  border-radius: 12px;
  background: linear-gradient(
    135deg,
    var(--color-primary-500),
    var(--color-primary-300),
    var(--color-primary-500)
  );
}

.ai-card-content {
  background: #FFFFFF;
  border-radius: 10px;
  padding: 24px;
}
```

---

## 📦 Design Tokens (CSS Variables)

### 전체 토큰 정의

```css
:root {
  /* Colors - Primary */
  --color-primary-50: #E6F0FF;
  --color-primary-100: #CCDDFF;
  --color-primary-500: #0055FF;
  --color-primary-700: #004080;
  --color-primary-900: #00264D;
  
  /* Colors - Grayscale */
  --color-gray-50: #F8F8F8;
  --color-gray-100: #F0F0F0;
  --color-gray-200: #E0E0E0;
  --color-gray-400: #9E9E9E;
  --color-gray-600: #4D4D4D;
  --color-gray-700: #2D2D2D;
  --color-gray-900: #0A0A0A;
  --color-white: #FFFFFF;
  
  /* Typography */
  --font-display-xl: 72px;
  --font-h1: 40px;
  --font-body-md: 16px;
  --font-weight-regular: 400;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;
  --line-height-tight: 1.2;
  --line-height-normal: 1.5;
  
  /* Spacing */
  --space-2: 8px;
  --space-4: 16px;
  --space-6: 24px;
  --space-8: 32px;
  --space-10: 40px;
  --space-20: 80px;
  
  /* Layout */
  --container-max-width: 1280px;
  --section-padding-y: 80px;
  --grid-gap: 24px;
  
  /* Border Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 16px;
  --radius-full: 999px;
  
  /* Shadows */
  --shadow-sm: 0 2px 4px rgba(0, 0, 0, 0.08);
  --shadow-md: 0 4px 12px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 8px 24px rgba(0, 0, 0, 0.12);
  --shadow-xl: 0 20px 60px rgba(0, 0, 0, 0.15);
  
  /* Transitions */
  --duration-fast: 200ms;
  --duration-normal: 300ms;
  --ease-out: cubic-bezier(0, 0, 0.2, 1);
}
```

---

## 📱 모바일 최적화

### Touch Targets
```css
/* 최소 터치 영역: 44×44px */
.btn-mobile {
  min-width: 44px;
  min-height: 44px;
  padding: 12px 24px;
}
```

### Thumb-Friendly Navigation
```css
/* 하단 고정 네비게이션 */
.mobile-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: #FFFFFF;
  padding: 12px 20px;
  box-shadow: 0 -2px 8px rgba(0, 0, 0, 0.1);
}
```

### Mobile Performance
- 이미지: WebP 포맷, lazy loading
- 폰트: WOFF2, font-display: swap
- 애니메이션: transform, opacity만 사용
- CSS: Critical CSS 인라인

---

## ✅ 체크리스트

### 디자인 완성도
- [ ] 모든 컬러가 WCAG AA 대비 충족
- [ ] 타이포그래피 계층 명확
- [ ] 간격 시스템 일관성
- [ ] 모든 인터랙티브 요소에 hover/focus 상태
- [ ] 다크모드 지원
- [ ] 모바일 반응형

### 성능
- [ ] Critical CSS 최적화
- [ ] 폰트 로딩 전략
- [ ] 이미지 최적화
- [ ] 애니메이션 60fps 유지

### 접근성
- [ ] 키보드 네비게이션 가능
- [ ] 스크린리더 호환
- [ ] 색맹 고려
- [ ] Reduced motion 지원

---

## 🔗 참고 자료

### 디자인 시스템
- Microsoft Fluent Design System
- Apple Human Interface Guidelines
- Google Material Design 3
- IBM Carbon Design System

### 2026 트렌드 출처
- Muzli Design Trends 2026
- Elementor Web Design Trends
- Devolfs Design Analysis
- TheeDigital Trend Report

---

**버전:** 1.0  
**최종 업데이트:** 2025-11-19  
**작성자:** 노바 파트너스 디자인팀  
**포인트 컬러:** #0055FF  
**타겟:** AI 솔루션 B2B 엔터프라이즈 웹사이트
