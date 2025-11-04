# 식스샵 프로 - 미라콤 스타일 헤더 설치 가이드

## 📋 목차
1. [소개](#소개)
2. [빠른 시작](#빠른-시작)
3. [상세 설치 방법](#상세-설치-방법)
4. [커스터마이징 가이드](#커스터마이징-가이드)
5. [구조 설명](#구조-설명)
6. [문제 해결](#문제-해결)

---

## 소개

이 헤더는 미라콤(Miracom) 웹사이트의 글로벌 네비게이션 구조를 식스샵 프로에 최적화하여 제작한 커스텀 블록입니다.

### ✨ 주요 기능

- **3단계 네비게이션**: Depth 1 → Depth 2 → Depth 3 계층 구조
- **메가 메뉴**: "서비스 및 솔루션" 메뉴에 3컬럼 레이아웃
- **반응형 디자인**: PC/태블릿/모바일 완벽 대응
- **부드러운 애니메이션**: 호버 효과 및 드롭다운 전환
- **접근성**: 키보드 네비게이션 지원
- **모바일 메뉴**: 햄버거 메뉴와 슬라이드 네비게이션

### 🎨 디자인 특징

- 깔끔한 미니멀 디자인
- 호버 시 배경 오버레이 효과
- 솔루션 항목에 설명 텍스트 표시
- CSS 변수를 통한 쉬운 색상 커스터마이징

---

## 빠른 시작

### 1단계: 코드 복사

`sixshop-pro-header-code.html` 파일의 전체 코드를 복사하세요.

### 2단계: 식스샵 프로에 추가

1. 식스샵 프로 에디터 접속
2. 헤더 영역 선택
3. **"HTML 섹션"** 추가
4. 복사한 코드 전체를 붙여넣기
5. 저장

### 3단계: 로고 이미지 업로드

1. 로고 이미지를 식스샵에 업로드
2. 코드에서 다음 부분 수정:

```html
<!-- 변경 전 -->
<img src="/path/to/your-logo.svg" alt="Your Company Logo">

<!-- 변경 후 (업로드한 이미지 URL로) -->
<img src="https://your-sixshop-domain.com/uploaded/logo.png" alt="회사명">
```

### 4단계: 링크 수정

모든 `href="#"` 를 실제 페이지 URL로 변경하세요.

```html
<!-- 변경 전 -->
<li><a href="#" title="회사 개요">회사 개요</a></li>

<!-- 변경 후 -->
<li><a href="/pages/about" title="회사 개요">회사 개요</a></li>
```

---

## 상세 설치 방법

### Step 1: 식스샵 프로 에디터 열기

1. 식스샵 프로 대시보드 로그인
2. 편집하고자 하는 사이트 선택
3. 상단의 **"에디터"** 버튼 클릭

### Step 2: HTML 섹션 추가

1. 에디터 좌측 패널에서 **"섹션 추가"** 클릭
2. **"HTML 섹션"** 선택
3. 헤더 영역으로 드래그 앤 드롭

### Step 3: 코드 삽입

1. HTML 섹션 클릭하여 선택
2. 우측 패널의 **"코드 편집"** 클릭
3. `sixshop-pro-header-code.html` 파일의 전체 내용 복사
4. 코드 편집기에 붙여넣기
5. **"적용"** 버튼 클릭

### Step 4: 미리보기 및 확인

1. 에디터 상단의 **"미리보기"** 버튼 클릭
2. PC/태블릿/모바일 모드에서 각각 확인
3. 드롭다운 메뉴 동작 테스트
4. 문제없으면 **"저장"** 및 **"게시"**

---

## 커스터마이징 가이드

### 🎨 색상 변경

CSS 상단의 `:root` 변수를 수정하세요:

```css
:root {
  --header-height: 80px;          /* 헤더 높이 */
  --header-bg: #ffffff;            /* 배경색 */
  --header-text: #000000;          /* 텍스트 색상 */
  --header-hover: #0066cc;         /* 호버 색상 (파란색) */
  --dropdown-bg: #ffffff;          /* 드롭다운 배경 */
  --dropdown-shadow: rgba(0, 0, 0, 0.1); /* 그림자 */
  --transition-speed: 0.3s;        /* 애니메이션 속도 */
  --font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Malgun Gothic", sans-serif;
}
```

#### 예시: 다크 모드로 변경

```css
:root {
  --header-bg: #1a1a1a;
  --header-text: #ffffff;
  --header-hover: #00d4ff;
  --dropdown-bg: #2a2a2a;
}
```

### 📏 로고 크기 조정

```css
.miracom-logo img {
  height: 32px;  /* 기본값: 32px */
  width: auto;
}
```

- **작게**: `height: 24px;`
- **중간**: `height: 32px;`
- **크게**: `height: 40px;`

### 📐 헤더 높이 조정

```css
:root {
  --header-height: 80px;  /* 기본값: 80px */
}

/* PC용 */
.miracom-header-inner {
  height: var(--header-height);
}

/* 모바일용 (별도 설정) */
@media (max-width: 768px) {
  .miracom-header-inner {
    height: 60px;  /* 모바일 높이 */
  }

  :root {
    --header-height: 60px;
  }
}
```

### 🔤 폰트 변경

```css
:root {
  --font-family: "Noto Sans KR", "Malgun Gothic", sans-serif;
}
```

또는 특정 요소만:

```css
.gnb-depth1 > li > a {
  font-family: "Montserrat", sans-serif;
  font-weight: 600;
}
```

### 📱 모바일 브레이크포인트 조정

기본값은 768px이지만, 변경 가능합니다:

```css
/* 기본: 768px */
@media (max-width: 768px) { ... }

/* 변경: 992px */
@media (max-width: 992px) { ... }
```

### 🎯 메뉴 간격 조정

```css
/* 1depth 메뉴 간격 */
.gnb-depth1 {
  gap: 50px;  /* 기본값: 50px */
}

/* 2depth 메뉴 패딩 */
.gnb-depth2 > li > a {
  padding: 12px 24px;  /* 상하: 12px, 좌우: 24px */
}
```

---

## 구조 설명

### 전체 구조

```
<header class="miracom-header">
  └── <div class="miracom-header-inner">
      ├── 로고 (.miracom-logo)
      ├── 네비게이션 (.miracom-gnb)
      │   ├── 배경 오버레이 (.gnb-depth2-bg)
      │   └── 1depth 메뉴 (.gnb-depth1)
      │       └── 2depth 드롭다운 (.gnb-depth1-hover)
      │           └── 3depth 서브메뉴 (.gnb-depth3)
      └── 유틸리티 (.miracom-top-util)
          ├── 링크들
          ├── 언어 선택 (.miracom-language)
          └── 모바일 버튼 (.miracom-mobile-menu-btn)
```

### CSS 클래스 설명

| 클래스명 | 용도 |
|---------|------|
| `.miracom-header` | 전체 헤더 컨테이너 |
| `.miracom-header-inner` | 내부 콘텐츠 래퍼 (max-width 적용) |
| `.miracom-logo` | 로고 영역 |
| `.miracom-gnb` | 글로벌 네비게이션 바 |
| `.gnb-depth1` | 1단계 메뉴 목록 |
| `.gnb-depth2` | 2단계 메뉴 목록 |
| `.gnb-depth3` | 3단계 메뉴 목록 |
| `.gnb-depth1-hover` | 드롭다운 컨테이너 |
| `.gnb-depth1-hover.wide` | 넓은 드롭다운 (메가 메뉴) |
| `.gnb-depth1-hover-category` | 카테고리 3컬럼 레이아웃 |
| `.gnb-depth2-bg` | 배경 오버레이 |
| `.miracom-top-util` | 유틸리티 메뉴 영역 |
| `.miracom-language` | 언어 선택기 |
| `.miracom-mobile-menu-btn` | 모바일 햄버거 버튼 |

### 메뉴 항목 추가하기

#### 1depth 메뉴 추가 (단순형)

```html
<li>
  <a href="/new-page" title="새 메뉴">새 메뉴</a>
  <div class="gnb-depth1-hover">
    <ul class="gnb-depth2">
      <li><a href="/sub-1" title="서브메뉴 1">서브메뉴 1</a></li>
      <li><a href="/sub-2" title="서브메뉴 2">서브메뉴 2</a></li>
    </ul>
  </div>
</li>
```

#### 3depth 메뉴 추가

```html
<li>
  <span>카테고리명</span>
  <ul class="gnb-depth3">
    <li><a href="/sub-1" title="하위메뉴 1"><span>하위메뉴 1</span></a></li>
    <li><a href="/sub-2" title="하위메뉴 2"><span>하위메뉴 2</span></a></li>
  </ul>
</li>
```

**주의**: 2depth는 `<span>` (클릭 불가), 3depth는 `<a>` (클릭 가능)

#### 메가 메뉴 형식 (wide)

```html
<li>
  <a href="#" title="메가 메뉴">메가 메뉴</a>
  <div class="gnb-depth1-hover wide">
    <div class="gnb-depth1-hover-category">

      <!-- 첫 번째 컬럼 -->
      <dl>
        <dt>카테고리 1</dt>
        <dd>
          <ul class="gnb-depth2">
            <li><a href="#">항목 1</a></li>
            <li><a href="#">항목 2</a></li>
          </ul>
        </dd>
      </dl>

      <!-- 두 번째 컬럼 -->
      <dl>
        <dt>카테고리 2</dt>
        <dd>
          <ul class="gnb-depth2">
            <li><a href="#">항목 3</a></li>
            <li><a href="#">항목 4</a></li>
          </ul>
        </dd>
      </dl>

    </div>
  </div>
</li>
```

### 메뉴 항목 삭제하기

불필요한 `<li>...</li>` 블록 전체를 삭제하면 됩니다.

```html
<!-- 이 전체 블록을 삭제 -->
<li>
  <a href="#" title="삭제할 메뉴">삭제할 메뉴</a>
  <div class="gnb-depth1-hover">
    ...
  </div>
</li>
```

---

## JavaScript 기능 설명

### 1. 모바일 메뉴 토글

```javascript
// 햄버거 버튼 클릭 시
mobileMenuBtn.addEventListener('click', function() {
  this.classList.toggle('active');      // 버튼 애니메이션
  gnbNav.classList.toggle('active');    // 메뉴 슬라이드

  // 스크롤 방지
  if (gnbNav.classList.contains('active')) {
    document.body.style.overflow = 'hidden';
  } else {
    document.body.style.overflow = '';
  }
});
```

### 2. 모바일 아코디언

```javascript
// 모바일에서 1depth 클릭 시 서브메뉴 펼치기/접기
if (window.innerWidth <= 768) {
  parentLi.classList.toggle('active');
}
```

### 3. 반응형 초기화

```javascript
// 화면 크기 변경 시 모바일 메뉴 자동 닫기
window.addEventListener('resize', function() {
  if (window.innerWidth > 768) {
    // 데스크톱으로 전환 시 모든 활성 상태 제거
  }
});
```

---

## 문제 해결

### ❓ 헤더가 표시되지 않아요

**원인**: HTML 섹션이 제대로 추가되지 않음

**해결**:
1. HTML 섹션이 올바른 위치에 있는지 확인
2. 코드가 완전히 복사되었는지 확인 (마지막 `</script>` 태그까지)
3. 에디터에서 "적용" 버튼을 클릭했는지 확인

### ❓ 드롭다운 메뉴가 작동하지 않아요

**원인**: JavaScript가 로드되지 않음

**해결**:
1. 코드 하단의 `<script>` 태그가 포함되어 있는지 확인
2. 브라우저 콘솔(F12)에서 에러 메시지 확인
3. 다른 JavaScript와 충돌 가능성 확인

### ❓ 모바일에서 메뉴가 이상해요

**원인**: 반응형 CSS가 제대로 적용되지 않음

**해결**:
1. `@media (max-width: 768px)` 부분이 모두 포함되었는지 확인
2. 식스샵 프로의 기본 CSS와 충돌 가능성 확인
3. CSS 클래스명이 중복되지 않았는지 확인

### ❓ 로고가 너무 크거나 작아요

**해결**:

```css
.miracom-logo img {
  height: 32px;  /* 이 값을 조정하세요 */
  width: auto;
}
```

### ❓ 링크 클릭이 안 돼요

**원인**: `href="#"` 또는 `href="javascript:void(0);"` 상태

**해결**:
- 모든 `href="#"` 를 실제 URL로 변경
- 1depth 메뉴는 `javascript:void(0);` 유지 (드롭다운 표시용)

### ❓ 색상을 변경했는데 적용이 안 돼요

**해결**:
1. `:root` 변수를 올바르게 수정했는지 확인
2. CSS 캐시 문제일 수 있으므로 강력 새로고침 (Ctrl + Shift + R)
3. 특정 요소만 변경하고 싶다면 해당 클래스의 CSS를 직접 수정

### ❓ 다른 섹션과 겹쳐요

**원인**: z-index 또는 position 충돌

**해결**:

```css
.miracom-header {
  position: relative;  /* 또는 sticky */
  z-index: 1000;  /* 값을 더 높게 조정 */
}
```

### ❓ 모바일에서 스크롤이 안 돼요

**원인**: 메뉴 열렸을 때 body overflow가 hidden으로 설정됨

**해결**:
- 모바일 메뉴 버튼 다시 클릭하여 메뉴 닫기
- JavaScript 코드에서 `overflow: 'hidden'` 부분 제거 (단, 배경 스크롤 방지 기능 사라짐)

---

## 고급 커스터마이징

### Sticky 헤더 (스크롤 시 고정)

```css
.miracom-header {
  position: sticky;
  top: 0;
  z-index: 1000;
}

/* 스크롤 시 그림자 추가 */
.miracom-header.scrolled {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}
```

JavaScript 추가:

```javascript
window.addEventListener('scroll', function() {
  const header = document.querySelector('.miracom-header');
  if (window.scrollY > 50) {
    header.classList.add('scrolled');
  } else {
    header.classList.remove('scrolled');
  }
});
```

### 메뉴 호버 딜레이 추가

```css
.gnb-depth1-hover {
  transition: opacity 0.3s 0.2s, visibility 0.3s 0.2s; /* 0.2초 딜레이 */
}
```

### 드롭다운 애니메이션 변경

```css
.gnb-depth1-hover {
  transform: translateY(10px);  /* 시작 위치 */
  transition: opacity 0.3s, visibility 0.3s, transform 0.3s;
}

.gnb-depth1 > li:hover .gnb-depth1-hover {
  transform: translateY(0);  /* 최종 위치 */
}
```

### 메가 메뉴 4컬럼으로 확장

```css
.gnb-depth1-hover.wide {
  min-width: 1200px;
  max-width: 1400px;
}

.gnb-depth1-hover-category {
  display: grid;
  grid-template-columns: repeat(4, 1fr);  /* 4컬럼 */
  gap: 40px;
}
```

---

## 체크리스트

설치 전 확인사항:

- [ ] 식스샵 프로 플랜 가입 확인
- [ ] 로고 이미지 준비 (SVG 또는 PNG, 투명 배경 권장)
- [ ] 메뉴 구조 설계 완료
- [ ] 각 메뉴의 링크 URL 준비

설치 후 확인사항:

- [ ] PC에서 모든 드롭다운 메뉴 정상 작동
- [ ] 태블릿 화면에서 레이아웃 확인
- [ ] 모바일에서 햄버거 메뉴 작동
- [ ] 모든 링크 클릭 시 올바른 페이지 이동
- [ ] 로고 이미지 정상 표시
- [ ] 색상 및 폰트가 브랜드 가이드와 일치
- [ ] 여러 브라우저에서 테스트 (Chrome, Safari, Firefox)

---

## 추가 리소스

### 식스샵 프로 공식 문서
- [식스샵 프로 고객센터](https://help.pro.sixshop.com)
- [커스텀 섹션 가이드](https://www.sixshop.com/official-blog)

### CSS Flexbox 참고자료
- [CSS Flexbox 완전 가이드](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [Flexbox Froggy (학습 게임)](https://flexboxfroggy.com/)

### 디자인 인스피레이션
- [Dribbble - Navigation Designs](https://dribbble.com/tags/navigation)
- [CodePen - Mega Menu Examples](https://codepen.io/search/pens?q=mega+menu)

---

## 지원 및 문의

이 헤더 코드에 대한 추가 지원이 필요하시면:

1. 코드에 포함된 주석을 먼저 확인하세요
2. 이 가이드의 "문제 해결" 섹션을 참조하세요
3. 식스샵 프로 고객센터에 문의하세요

---

## 버전 정보

- **버전**: 1.0.0
- **최종 업데이트**: 2025-01-04
- **호환성**: 식스샵 프로 전체 버전
- **브라우저 지원**: Chrome, Safari, Firefox, Edge (최신 버전)

---

## 라이선스

이 코드는 미라콤 웹사이트의 구조를 참고하여 제작되었으며, 식스샵 프로 사용자를 위한 교육 및 상업적 사용이 가능합니다.

**제공**: NovaPartners
**분석 기반**: miracom-inc.com

---

**즐거운 웹사이트 제작 되세요! 🚀**
