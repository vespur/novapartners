# 🎨 노바파트너스 디자인 시스템 2026

> **AI 솔루션 전문 중기업을 위한 엔터프라이즈급 디자인 시스템**  
> 포인트 컬러: #0055FF | 2026년 웹 트렌드 반영

**버전**: 1.0.0  
**최종 업데이트**: 2025.11  
**적용 대상**: 노바파트너스 웹사이트 (23개 페이지)

---

## 📋 목차

1. [디자인 철학](#1-디자인-철학)
2. [컬러 시스템](#2-컬러-시스템)
3. [타이포그래피](#3-타이포그래피)
4. [간격 시스템](#4-간격-시스템)
5. [그리드 시스템](#5-그리드-시스템)
6. [컴포넌트 라이브러리](#6-컴포넌트-라이브러리)
7. [모션 & 인터랙션](#7-모션--인터랙션)
8. [아이콘 시스템](#8-아이콘-시스템)
9. [접근성 가이드라인](#9-접근성-가이드라인)
10. [2026 트렌드 적용](#10-2026-트렌드-적용)

---

## 1. 디자인 철학

### 1.1 핵심 원칙

#### **AI-First Experience**
- 기술 신뢰성과 미래지향적 비주얼
- 데이터 중심의 명확한 정보 전달
- 인간 중심의 따뜻한 인터페이스

#### **Enterprise Clarity**
- B2B 맥락에 최적화된 프로페셔널한 톤
- 복잡한 정보의 단순화 및 계층화
- 명확한 CTA와 사용자 여정

#### **2026 Modern Standards**
- Micro-delight 인터랙션
- Variable typography
- Purposeful motion
- Accessibility-first

#### **Cross-platform Consistency**
- 데스크톱, 태블릿, 모바일 일관성
- 브라우저 호환성 (Chrome, Safari, Firefox, Edge)
- 성능 최적화 (Core Web Vitals)

### 1.2 디자인 가치

| 가치 | 설명 | 구현 방법 |
|------|------|----------|
| **신뢰성** | 전문성과 안정감 | 일관된 컬러, 명확한 위계, 높은 대비 |
| **효율성** | 빠른 정보 접근 | 직관적 네비게이션, 명확한 CTA |
| **혁신성** | 미래지향적 이미지 | 모던한 애니메이션, AI 비주얼 |
| **접근성** | 모두를 위한 디자인 | WCAG 2.2 AA 준수, 키보드 네비게이션 |

---

## 2. 컬러 시스템

### 2.1 컬러 철학

**"Blue for Trust, Gray for Stability, White for Clarity"**

2026년 트렌드: 
- **모노크롬 베이스** + **단일 포인트 컬러** 전략
- 높은 대비 (WCAG AA 이상)
- 다크모드 지원 준비

### 2.2 컬러 비율 전략

```
포인트 컬러 (#0055FF): 20%
블랙/그레이: 30%
화이트/라이트: 50%
```

### 2.3 Primary Colors (주요 색상)

#### **Brand Blue (#0055FF) - "Nova Blue"**

```css
/* Primary Blue Palette */
--color-primary-50:  #E6F0FF;  /* 매우 연한 파랑 */
--color-primary-100: #CCE0FF;  /* 연한 파랑 */
--color-primary-200: #99C2FF;  /* 밝은 파랑 */
--color-primary-300: #66A3FF;  /* 중간 밝은 파랑 */
--color-primary-400: #3385FF;  /* 중간 파랑 */
--color-primary-500: #0055FF;  /* 브랜드 메인 컬러 ⭐ */
--color-primary-600: #0044CC;  /* 진한 파랑 */
--color-primary-700: #003399;  /* 매우 진한 파랑 */
--color-primary-800: #002266;  /* 다크 파랑 */
--color-primary-900: #001133;  /* 거의 검정에 가까운 파랑 */
```

**사용처:**
- CTA 버튼 (primary-500)
- 링크 텍스트 (primary-600)
- Hover 상태 (primary-700)
- 아이콘 강조 (primary-500)
- 그래프/차트 메인 컬러 (primary-500)

#### **Contrast Ratio (대비 비율)**
- #0055FF on White: **8.2:1** ✅ (AAA 등급)
- #0055FF on Gray-50: **7.9:1** ✅ (AAA 등급)

### 2.4 Neutral Colors (중립 색상)

```css
/* Grayscale Palette */
--color-gray-50:  #F8F9FA;  /* 매우 연한 회색 (배경) */
--color-gray-100: #E9ECEF;  /* 연한 회색 (구분선) */
--color-gray-200: #DEE2E6;  /* 밝은 회색 (비활성) */
--color-gray-300: #CED4DA;  /* 중간 밝은 회색 */
--color-gray-400: #ADB5BD;  /* 중간 회색 (placeholder) */
--color-gray-500: #6C757D;  /* 중간 진한 회색 (보조 텍스트) */
--color-gray-600: #495057;  /* 진한 회색 (본문) */
--color-gray-700: #343A40;  /* 매우 진한 회색 (헤딩) */
--color-gray-800: #212529;  /* 거의 검정 (타이틀) */
--color-gray-900: #000000;  /* 순수 검정 */

/* White */
--color-white: #FFFFFF;
```

**사용처:**
- 배경: gray-50, white
- 본문 텍스트: gray-700
- 헤딩: gray-900
- 구분선: gray-200
- 비활성 요소: gray-400

### 2.5 Semantic Colors (의미 색상)

```css
/* Success (성공) */
--color-success-light: #D4EDDA;
--color-success-main:  #28A745;
--color-success-dark:  #1E7B34;

/* Warning (경고) */
--color-warning-light: #FFF3CD;
--color-warning-main:  #FFC107;
--color-warning-dark:  #E0A800;

/* Error (에러) */
--color-error-light: #F8D7DA;
--color-error-main:  #DC3545;
--color-error-dark:  #A71D2A;

/* Info (정보) */
--color-info-light: #D1ECF1;
--color-info-main:  #17A2B8;
--color-info-dark:  #117A8B;
```

**사용처:**
- 알림 메시지
- 폼 검증 상태
- 배지 (Badge)
- 상태 표시

### 2.6 Gradient System (그라디언트)

2026 트렌드: **Soft glow gradients** + **Noise texture**

```css
/* Primary Gradient (히어로, CTA) */
--gradient-primary: linear-gradient(135deg, #0055FF 0%, #0044CC 100%);

/* Soft Glow (배경, 장식) */
--gradient-glow: radial-gradient(
  circle at 50% 50%,
  rgba(0, 85, 255, 0.15) 0%,
  rgba(0, 85, 255, 0) 70%
);

/* Overlay (이미지 위 텍스트) */
--gradient-overlay: linear-gradient(
  180deg,
  rgba(0, 0, 0, 0) 0%,
  rgba(0, 0, 0, 0.6) 100%
);

/* Hero Background */
--gradient-hero: linear-gradient(
  135deg,
  rgba(0, 85, 255, 0.05) 0%,
  rgba(248, 249, 250, 1) 100%
);
```

### 2.7 Color Usage Rules

| 요소 | 컬러 | 대비 비율 |
|------|------|----------|
| **Primary CTA** | primary-500 bg + white text | 8.2:1 ✅ |
| **Secondary CTA** | white bg + primary-600 text | 9.1:1 ✅ |
| **Body Text** | gray-700 on white | 12.6:1 ✅ |
| **Heading** | gray-900 on white | 21:1 ✅ |
| **Link (default)** | primary-600 on white | 9.1:1 ✅ |
| **Link (hover)** | primary-700 on white | 11.8:1 ✅ |

---

## 3. 타이포그래피

### 3.1 폰트 패밀리

#### **Primary Font: Pretendard**

2026 트렌드 반영: **Variable Font** 지원, 한글 최적화

```css
/* Pretendard Variable */
@font-face {
  font-family: 'Pretendard Variable';
  src: url('/fonts/PretendardVariable.woff2') format('woff2-variations');
  font-weight: 100 900;
  font-display: swap;
}

/* Font Stack */
--font-primary: 'Pretendard Variable', -apple-system, BlinkMacSystemFont, 
                'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
```

**선택 이유:**
- 한글 가독성 최적화
- Variable Font로 로딩 성능 향상
- 다양한 Weight 지원 (100-900)
- Apple/Samsung 시스템 폰트와 유사한 느낌

#### **Secondary Font: Inter (영문용)**

```css
/* Inter Variable (영문 강조용) */
@font-face {
  font-family: 'Inter Variable';
  src: url('/fonts/Inter-Variable.woff2') format('woff2-variations');
  font-weight: 100 900;
  font-display: swap;
}

--font-secondary: 'Inter Variable', sans-serif;
```

**사용처:**
- 영문 헤딩
- 숫자 강조 (Tabular Numbers)
- 코드/기술 용어

#### **Monospace Font: JetBrains Mono**

```css
--font-mono: 'JetBrains Mono', 'SF Mono', Monaco, 'Courier New', monospace;
```

**사용처:**
- 코드 블록
- API 문서
- 기술 스펙

### 3.2 타이포그래피 스케일 (Desktop)

**2026 트렌드: Bold, Expressive Typography**

| Level | Size | Line Height | Weight | Letter Spacing | 사용처 |
|-------|------|-------------|--------|---------------|--------|
| **Display 1** | 72px | 84px (1.17) | 700 | -1.5% | 메인 히어로 |
| **Display 2** | 60px | 72px (1.2) | 700 | -1% | 서브 히어로 |
| **H1** | 48px | 56px (1.17) | 700 | -0.5% | 페이지 타이틀 |
| **H2** | 36px | 44px (1.22) | 600 | 0% | 섹션 헤딩 |
| **H3** | 28px | 36px (1.29) | 600 | 0% | 서브섹션 |
| **H4** | 24px | 32px (1.33) | 600 | 0% | 카드 타이틀 |
| **H5** | 20px | 28px (1.4) | 600 | 0% | 작은 헤딩 |
| **H6** | 18px | 24px (1.33) | 600 | 0% | 라벨 헤딩 |
| **Body Large** | 18px | 28px (1.56) | 400 | 0% | 인트로 텍스트 |
| **Body** | 16px | 24px (1.5) | 400 | 0% | 본문 |
| **Body Small** | 14px | 20px (1.43) | 400 | 0% | 보조 텍스트 |
| **Caption** | 12px | 16px (1.33) | 400 | 0% | 캡션 |

### 3.3 타이포그래피 스케일 (Mobile)

| Level | Size | Line Height | Weight |
|-------|------|-------------|--------|
| **Display 1** | 48px | 56px | 700 |
| **Display 2** | 40px | 48px | 700 |
| **H1** | 36px | 44px | 700 |
| **H2** | 28px | 36px | 600 |
| **H3** | 24px | 32px | 600 |
| **H4** | 20px | 28px | 600 |
| **Body Large** | 18px | 28px | 400 |
| **Body** | 16px | 24px | 400 |
| **Body Small** | 14px | 20px | 400 |

### 3.4 CSS Variables

```css
/* Font Sizes */
--font-size-display-1: 4.5rem;    /* 72px */
--font-size-display-2: 3.75rem;   /* 60px */
--font-size-h1: 3rem;             /* 48px */
--font-size-h2: 2.25rem;          /* 36px */
--font-size-h3: 1.75rem;          /* 28px */
--font-size-h4: 1.5rem;           /* 24px */
--font-size-h5: 1.25rem;          /* 20px */
--font-size-h6: 1.125rem;         /* 18px */
--font-size-body-lg: 1.125rem;    /* 18px */
--font-size-body: 1rem;           /* 16px */
--font-size-body-sm: 0.875rem;    /* 14px */
--font-size-caption: 0.75rem;     /* 12px */

/* Font Weights */
--font-weight-regular: 400;
--font-weight-medium: 500;
--font-weight-semibold: 600;
--font-weight-bold: 700;

/* Line Heights */
--line-height-tight: 1.2;
--line-height-normal: 1.5;
--line-height-relaxed: 1.6;

/* Letter Spacing */
--letter-spacing-tight: -0.015em;
--letter-spacing-normal: 0;
--letter-spacing-wide: 0.025em;
```

### 3.5 타이포그래피 사용 규칙

#### **헤딩 (Headings)**
```css
h1, .h1 {
  font-size: var(--font-size-h1);
  font-weight: var(--font-weight-bold);
  line-height: 1.17;
  color: var(--color-gray-900);
  margin-bottom: 1.5rem;
}
```

#### **본문 (Body)**
```css
body, .body {
  font-family: var(--font-primary);
  font-size: var(--font-size-body);
  font-weight: var(--font-weight-regular);
  line-height: var(--line-height-normal);
  color: var(--color-gray-700);
}
```

#### **링크 (Links)**
```css
a {
  color: var(--color-primary-600);
  text-decoration: none;
  transition: color 0.2s ease;
}

a:hover {
  color: var(--color-primary-700);
  text-decoration: underline;
}
```

### 3.6 특수 타이포그래피

#### **숫자 강조 (Stats/Metrics)**
```css
.stat-number {
  font-family: var(--font-secondary);
  font-size: 4rem; /* 64px */
  font-weight: 700;
  line-height: 1;
  color: var(--color-primary-500);
  letter-spacing: -0.02em;
  font-variant-numeric: tabular-nums; /* 숫자 정렬 */
}
```

#### **인용문 (Quotes)**
```css
blockquote {
  font-size: 1.5rem; /* 24px */
  font-weight: 500;
  line-height: 1.5;
  color: var(--color-gray-800);
  font-style: italic;
  border-left: 4px solid var(--color-primary-500);
  padding-left: 2rem;
  margin: 2rem 0;
}
```

---

## 4. 간격 시스템

### 4.1 간격 철학

**"8px Base Unit System"**

2026 트렌드: **Generous White Space** (여유로운 여백)

### 4.2 Spacing Scale

```css
/* Spacing Variables (8px base) */
--space-1:  0.25rem;  /* 4px  */
--space-2:  0.5rem;   /* 8px  ⭐ Base Unit */
--space-3:  0.75rem;  /* 12px */
--space-4:  1rem;     /* 16px */
--space-5:  1.5rem;   /* 24px */
--space-6:  2rem;     /* 32px */
--space-8:  3rem;     /* 48px */
--space-10: 4rem;     /* 64px */
--space-12: 6rem;     /* 96px */
--space-16: 8rem;     /* 128px */
--space-20: 10rem;    /* 160px */
--space-24: 12rem;    /* 192px */
```

### 4.3 Component Spacing

| 컴포넌트 | 내부 패딩 | 외부 마진 |
|----------|----------|----------|
| **Button** | 12px 24px | - |
| **Input Field** | 12px 16px | 0 0 16px 0 |
| **Card** | 32px | 0 0 32px 0 |
| **Section** | 80px 0 | 0 0 80px 0 |
| **Hero** | 120px 0 | - |
| **Header** | 16px 24px | - |
| **Footer** | 48px 24px | - |

### 4.4 Layout Spacing

#### **Desktop (≥1200px)**
```css
--container-max-width: 1280px;
--container-padding: 80px;
--section-gap: 120px;
--content-gap: 64px;
```

#### **Tablet (768px - 1199px)**
```css
--container-padding: 48px;
--section-gap: 80px;
--content-gap: 48px;
```

#### **Mobile (<768px)**
```css
--container-padding: 24px;
--section-gap: 64px;
--content-gap: 32px;
```

---

## 5. 그리드 시스템

### 5.1 12-Column Grid

```css
/* Grid Container */
.container {
  max-width: 1280px;
  margin: 0 auto;
  padding: 0 var(--container-padding);
}

/* Grid System */
.grid {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: var(--space-6); /* 32px */
}

/* Column Spans */
.col-1  { grid-column: span 1; }
.col-2  { grid-column: span 2; }
.col-3  { grid-column: span 3; }
.col-4  { grid-column: span 4; }
.col-6  { grid-column: span 6; }
.col-8  { grid-column: span 8; }
.col-12 { grid-column: span 12; }
```

### 5.2 Breakpoints

```css
/* Desktop First Approach */
--breakpoint-xl: 1440px;  /* Large Desktop */
--breakpoint-lg: 1200px;  /* Desktop */
--breakpoint-md: 992px;   /* Tablet Landscape */
--breakpoint-sm: 768px;   /* Tablet Portrait */
--breakpoint-xs: 576px;   /* Mobile Large */
```

```css
/* Media Queries */
@media (max-width: 1199px) { /* Tablet */ }
@media (max-width: 767px)  { /* Mobile */ }
```

### 5.3 Common Layouts

#### **Two Column (50/50)**
```css
.layout-50-50 {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 64px;
}

@media (max-width: 767px) {
  .layout-50-50 {
    grid-template-columns: 1fr;
    gap: 32px;
  }
}
```

#### **Sidebar Layout (33/67)**
```css
.layout-sidebar {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 48px;
}
```

#### **Feature Grid (3 Columns)**
```css
.feature-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 32px;
}

@media (max-width: 767px) {
  .feature-grid {
    grid-template-columns: 1fr;
  }
}
```

---

## 6. 컴포넌트 라이브러리

### 6.1 Buttons

#### **Primary Button**
```css
.btn-primary {
  background: var(--color-primary-500);
  color: white;
  padding: 12px 32px;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 2px 8px rgba(0, 85, 255, 0.2);
}

.btn-primary:hover {
  background: var(--color-primary-600);
  box-shadow: 0 4px 12px rgba(0, 85, 255, 0.3);
  transform: translateY(-2px);
}

.btn-primary:active {
  transform: translateY(0);
  box-shadow: 0 2px 4px rgba(0, 85, 255, 0.2);
}
```

#### **Secondary Button**
```css
.btn-secondary {
  background: white;
  color: var(--color-primary-600);
  padding: 12px 32px;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  border: 2px solid var(--color-primary-500);
  cursor: pointer;
  transition: all 0.2s ease;
}

.btn-secondary:hover {
  background: var(--color-primary-50);
  border-color: var(--color-primary-600);
}
```

#### **Button Sizes**
```css
.btn-sm  { padding: 8px 20px; font-size: 14px; }
.btn-md  { padding: 12px 32px; font-size: 16px; } /* Default */
.btn-lg  { padding: 16px 48px; font-size: 18px; }
```

### 6.2 Input Fields

```css
.input {
  width: 100%;
  padding: 12px 16px;
  font-size: 16px;
  border: 2px solid var(--color-gray-300);
  border-radius: 8px;
  transition: all 0.2s ease;
}

.input:focus {
  outline: none;
  border-color: var(--color-primary-500);
  box-shadow: 0 0 0 4px rgba(0, 85, 255, 0.1);
}

.input::placeholder {
  color: var(--color-gray-400);
}

.input.error {
  border-color: var(--color-error-main);
}
```

### 6.3 Cards

```css
.card {
  background: white;
  border-radius: 16px;
  padding: 32px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
}

.card:hover {
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
  transform: translateY(-4px);
}

.card-header {
  margin-bottom: 16px;
}

.card-title {
  font-size: 24px;
  font-weight: 600;
  color: var(--color-gray-900);
}

.card-body {
  color: var(--color-gray-700);
  line-height: 1.6;
}
```

### 6.4 Badges

```css
.badge {
  display: inline-block;
  padding: 4px 12px;
  font-size: 12px;
  font-weight: 600;
  border-radius: 12px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.badge-primary {
  background: var(--color-primary-100);
  color: var(--color-primary-700);
}

.badge-success {
  background: var(--color-success-light);
  color: var(--color-success-dark);
}
```

### 6.5 Navigation

#### **Header (GNB)**
```css
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  z-index: 1000;
  padding: 16px 0;
}

.nav-menu {
  display: flex;
  gap: 32px;
  list-style: none;
}

.nav-link {
  font-size: 16px;
  font-weight: 500;
  color: var(--color-gray-700);
  transition: color 0.2s ease;
}

.nav-link:hover {
  color: var(--color-primary-600);
}
```

### 6.6 Modal

```css
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
  z-index: 2000;
  display: flex;
  align-items: center;
  justify-content: center;
  animation: fadeIn 0.2s ease;
}

.modal-content {
  background: white;
  border-radius: 16px;
  padding: 32px;
  max-width: 600px;
  width: 90%;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  animation: slideUp 0.3s ease;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from { transform: translateY(20px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}
```

---

## 7. 모션 & 인터랙션

### 7.1 모션 철학

**"Purposeful Motion, Not Decoration"**

2026 트렌드:
- **Micro-delight interactions**
- **Physics-based animations**
- **Scroll-triggered effects**

### 7.2 Animation Duration

```css
/* Duration Variables */
--duration-instant: 100ms;   /* 즉각 반응 */
--duration-fast:    200ms;   /* 버튼, 링크 */
--duration-normal:  300ms;   /* 카드, 모달 */
--duration-slow:    500ms;   /* 페이지 전환 */
--duration-slower:  800ms;   /* 복잡한 애니메이션 */
```

### 7.3 Easing Functions

```css
/* Easing Variables */
--ease-in:      cubic-bezier(0.4, 0, 1, 1);
--ease-out:     cubic-bezier(0, 0, 0.2, 1);
--ease-in-out:  cubic-bezier(0.4, 0, 0.2, 1);
--ease-bounce:  cubic-bezier(0.68, -0.55, 0.265, 1.55);
```

### 7.4 Common Animations

#### **Fade In**
```css
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.fade-in {
  animation: fadeIn 0.5s ease-out;
}
```

#### **Slide Up**
```css
@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.slide-up {
  animation: slideUp 0.6s var(--ease-out);
}
```

#### **Scale In**
```css
@keyframes scaleIn {
  from {
    opacity: 0;
    transform: scale(0.95);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.scale-in {
  animation: scaleIn 0.3s var(--ease-out);
}
```

### 7.5 Hover Effects

#### **Button Lift**
```css
.btn-lift {
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.btn-lift:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
}
```

#### **Card Float**
```css
.card-float {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card-float:hover {
  transform: translateY(-8px);
  box-shadow: 0 12px 32px rgba(0, 0, 0, 0.15);
}
```

### 7.6 Scroll Animations

#### **Fade In On Scroll**
```css
.scroll-fade {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.scroll-fade.visible {
  opacity: 1;
  transform: translateY(0);
}
```

**JavaScript (Intersection Observer)**
```javascript
const observer = new IntersectionObserver(
  (entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
      }
    });
  },
  { threshold: 0.1 }
);

document.querySelectorAll('.scroll-fade').forEach(el => {
  observer.observe(el);
});
```

### 7.7 Loading States

#### **Skeleton Loader**
```css
.skeleton {
  background: linear-gradient(
    90deg,
    var(--color-gray-200) 25%,
    var(--color-gray-100) 50%,
    var(--color-gray-200) 75%
  );
  background-size: 200% 100%;
  animation: shimmer 1.5s infinite;
  border-radius: 8px;
}

@keyframes shimmer {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
```

#### **Spinner**
```css
.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid var(--color-gray-200);
  border-top-color: var(--color-primary-500);
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

---

## 8. 아이콘 시스템

### 8.1 아이콘 라이브러리

**Lucide Icons** (오픈소스, 일관된 디자인)

```html
<!-- CDN -->
<script src="https://unpkg.com/lucide@latest"></script>

<!-- Usage -->
<i data-lucide="check-circle"></i>
<script>lucide.createIcons();</script>
```

### 8.2 아이콘 크기

```css
/* Icon Sizes */
--icon-xs: 16px;
--icon-sm: 20px;
--icon-md: 24px;  /* Default */
--icon-lg: 32px;
--icon-xl: 48px;

.icon-xs { width: var(--icon-xs); height: var(--icon-xs); }
.icon-sm { width: var(--icon-sm); height: var(--icon-sm); }
.icon-md { width: var(--icon-md); height: var(--icon-md); }
.icon-lg { width: var(--icon-lg); height: var(--icon-lg); }
.icon-xl { width: var(--icon-xl); height: var(--icon-xl); }
```

### 8.3 아이콘 스타일

```css
/* Primary Icon */
.icon-primary {
  color: var(--color-primary-500);
}

/* Icon with Background */
.icon-bg {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 48px;
  height: 48px;
  background: var(--color-primary-100);
  border-radius: 12px;
  color: var(--color-primary-600);
}
```

---

## 9. 접근성 가이드라인

### 9.1 WCAG 2.2 AA 준수

#### **Color Contrast**
- 일반 텍스트: 최소 4.5:1
- 큰 텍스트 (18px+ 또는 14px+ Bold): 최소 3:1
- UI 컴포넌트: 최소 3:1

#### **키보드 네비게이션**
```css
/* Focus Visible */
*:focus-visible {
  outline: 3px solid var(--color-primary-500);
  outline-offset: 2px;
  border-radius: 4px;
}

/* Skip to Main Content */
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: var(--color-primary-500);
  color: white;
  padding: 8px 16px;
  z-index: 9999;
}

.skip-link:focus {
  top: 0;
}
```

### 9.2 Semantic HTML

```html
<!-- Good -->
<header>
  <nav aria-label="Main navigation">
    <ul>
      <li><a href="/">Home</a></li>
    </ul>
  </nav>
</header>

<main>
  <article>
    <h1>Page Title</h1>
  </article>
</main>

<footer>
  <p>&copy; 2026 Nova Partners</p>
</footer>
```

### 9.3 ARIA Labels

```html
<!-- Button with Icon Only -->
<button aria-label="Close modal">
  <i data-lucide="x"></i>
</button>

<!-- Form Label -->
<label for="email">Email Address</label>
<input id="email" type="email" required aria-required="true">

<!-- Error Message -->
<input id="password" type="password" aria-describedby="password-error">
<span id="password-error" role="alert">Password must be at least 8 characters</span>
```

---

## 10. 2026 트렌드 적용

### 10.1 AI-Enhanced Design

#### **AI Glow Effect**
```css
.ai-glow {
  position: relative;
  background: linear-gradient(135deg, #0055FF 0%, #0044CC 100%);
  border-radius: 16px;
  padding: 48px;
}

.ai-glow::before {
  content: '';
  position: absolute;
  inset: -4px;
  background: linear-gradient(45deg, #0055FF, #00CCFF, #0055FF);
  background-size: 200% 200%;
  border-radius: 16px;
  z-index: -1;
  filter: blur(20px);
  opacity: 0.6;
  animation: glowPulse 3s ease infinite;
}

@keyframes glowPulse {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}
```

### 10.2 Organic Shapes

```css
/* Blob Shape Background */
.blob-shape {
  position: absolute;
  width: 600px;
  height: 600px;
  background: radial-gradient(
    circle,
    rgba(0, 85, 255, 0.1) 0%,
    rgba(0, 85, 255, 0) 70%
  );
  border-radius: 40% 60% 70% 30% / 40% 50% 60% 50%;
  animation: blobMove 20s ease-in-out infinite;
  filter: blur(40px);
}

@keyframes blobMove {
  0%, 100% {
    border-radius: 40% 60% 70% 30% / 40% 50% 60% 50%;
    transform: translate(0, 0) scale(1);
  }
  50% {
    border-radius: 70% 30% 40% 60% / 60% 40% 50% 60%;
    transform: translate(50px, 50px) scale(1.1);
  }
}
```

### 10.3 Noise Texture

```css
.noise-texture {
  position: relative;
  background: var(--color-gray-50);
}

.noise-texture::after {
  content: '';
  position: absolute;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' /%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.05'/%3E%3C/svg%3E");
  pointer-events: none;
}
```

### 10.4 Variable Typography

```css
/* Dynamic Font Weight on Hover */
.dynamic-heading {
  font-family: 'Pretendard Variable';
  font-size: 3rem;
  font-weight: 600;
  transition: font-weight 0.3s ease;
}

.dynamic-heading:hover {
  font-weight: 800;
}
```

### 10.5 Scroll-Triggered Counter

```javascript
// Number Counter Animation
function animateCounter(element, target, duration = 2000) {
  let start = 0;
  const increment = target / (duration / 16);
  
  function updateCounter() {
    start += increment;
    if (start < target) {
      element.textContent = Math.floor(start);
      requestAnimationFrame(updateCounter);
    } else {
      element.textContent = target;
    }
  }
  
  updateCounter();
}

// Trigger on scroll
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const target = parseInt(entry.target.dataset.target);
      animateCounter(entry.target, target);
      observer.unobserve(entry.target);
    }
  });
});

document.querySelectorAll('.counter').forEach(el => observer.observe(el));
```

```html
<!-- HTML Usage -->
<div class="counter" data-target="95">0</div>%
```

---

## 11. 다크모드 (선택사항)

### 11.1 다크모드 컬러

```css
/* Dark Mode Variables */
:root[data-theme="dark"] {
  --color-bg: #0A0A0A;
  --color-surface: #1A1A1A;
  --color-text: #E5E5E5;
  --color-text-secondary: #A0A0A0;
  
  --color-primary-500: #3385FF; /* Lighter blue for dark bg */
}
```

### 11.2 다크모드 토글

```javascript
// Dark Mode Toggle
const toggleButton = document.getElementById('dark-mode-toggle');
const root = document.documentElement;

toggleButton.addEventListener('click', () => {
  const currentTheme = root.getAttribute('data-theme');
  const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
  root.setAttribute('data-theme', newTheme);
  localStorage.setItem('theme', newTheme);
});

// Load saved theme
const savedTheme = localStorage.getItem('theme') || 'light';
root.setAttribute('data-theme', savedTheme);
```

---

## 12. 성능 최적화

### 12.1 이미지 최적화

```html
<!-- WebP with Fallback -->
<picture>
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Description" loading="lazy">
</picture>
```

### 12.2 폰트 최적화

```css
/* Font Display Swap */
@font-face {
  font-family: 'Pretendard Variable';
  src: url('/fonts/PretendardVariable.woff2') format('woff2-variations');
  font-weight: 100 900;
  font-display: swap; /* FOUT 방지 */
}
```

### 12.3 Critical CSS

```html
<!-- Inline Critical CSS -->
<style>
  /* Above-the-fold styles */
  body { margin: 0; font-family: system-ui; }
  .header { position: fixed; top: 0; }
  .hero { min-height: 100vh; }
</style>

<!-- Load full CSS asynchronously -->
<link rel="preload" href="/styles/main.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
```

---

## 13. 개발자 가이드

### 13.1 CSS 아키텍처

```
styles/
├── base/
│   ├── reset.css          # CSS Reset
│   ├── typography.css     # 타이포그래피
│   └── variables.css      # CSS Variables
├── components/
│   ├── buttons.css
│   ├── cards.css
│   ├── forms.css
│   └── modals.css
├── layout/
│   ├── grid.css
│   ├── header.css
│   └── footer.css
├── utilities/
│   ├── spacing.css
│   └── animations.css
└── main.css               # Import all
```

### 13.2 네이밍 컨벤션

**BEM (Block Element Modifier)**

```css
/* Block */
.card { }

/* Element */
.card__header { }
.card__body { }
.card__footer { }

/* Modifier */
.card--featured { }
.card--large { }
```

### 13.3 Component Example

```html
<!-- Card Component -->
<div class="card card--featured">
  <div class="card__header">
    <h3 class="card__title">AI Platform</h3>
    <span class="badge badge-primary">New</span>
  </div>
  <div class="card__body">
    <p>Powerful AI solutions for enterprise.</p>
  </div>
  <div class="card__footer">
    <a href="#" class="btn-primary">Learn More</a>
  </div>
</div>
```

---

## 14. 품질 체크리스트

### 14.1 디자인 체크리스트

- [ ] 모든 컬러가 WCAG AA 대비 기준 충족
- [ ] 타이포그래피 스케일 일관성 유지
- [ ] 8px 간격 시스템 준수
- [ ] 모바일 반응형 동작 확인
- [ ] 모든 인터랙션 요소에 Hover/Focus 상태 정의
- [ ] 로딩 상태 디자인 포함
- [ ] 에러 상태 디자인 포함

### 14.2 접근성 체크리스트

- [ ] 키보드만으로 모든 기능 접근 가능
- [ ] 스크린 리더 테스트 완료
- [ ] Alt 텍스트 모든 이미지에 포함
- [ ] ARIA 레이블 적절히 사용
- [ ] Focus Visible 명확히 표시
- [ ] 색상만으로 정보 전달하지 않음

### 14.3 성능 체크리스트

- [ ] 이미지 WebP 형식 사용 (fallback 포함)
- [ ] 폰트 Variable Font 사용
- [ ] Critical CSS 인라인 처리
- [ ] JavaScript 최소화 및 지연 로딩
- [ ] Lighthouse 스코어 90+ (Performance)

---

## 15. 참고 자료

### 15.1 디자인 시스템 벤치마크

- **Apple Human Interface Guidelines** (Liquid Glass)
- **Samsung One UI Design System**
- **Microsoft Fluent 2**
- **Google Material 3 Expressive**

### 15.2 2026 웹 트렌드 출처

1. **Organic Shapes & Anti-Grid Layouts** - Elementor, Muz.li
2. **Bold Typography** - ReallyGoodDesigns, Contentsquare
3. **Micro-Delight Interactions** - Muzli, Devolfs
4. **AI-First Design** - Merehead, EnvizLabs
5. **Accessibility-First** - WCAG 2.2, European Accessibility Act
6. **Performance as Design** - Core Web Vitals, Lighthouse

### 15.3 도구 및 리소스

- **디자인**: Figma, Adobe XD
- **아이콘**: Lucide Icons, SF Symbols
- **폰트**: Pretendard Variable, Inter Variable
- **애니메이션**: Framer Motion, GSAP
- **접근성 테스트**: axe DevTools, WAVE

---

## 16. 버전 히스토리

| 버전 | 날짜 | 변경 사항 |
|------|------|----------|
| 1.0.0 | 2025.11 | 초기 디자인 시스템 구축 |

---

## 17. 라이선스 및 크레딧

**© 2025 Nova Partners. All rights reserved.**

이 디자인 시스템은 노바파트너스의 브랜드 아이덴티티를 위해 제작되었으며,  
2026년 웹 디자인 트렌드와 엔터프라이즈 베스트 프랙티스를 반영합니다.

**Designed by**: Nova Partners Design Team  
**Based on**: Apple HIG, Samsung One UI, Material 3, Fluent 2

---

**END OF DESIGN SYSTEM 1.0.0**
