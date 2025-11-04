# 미라콤(Miracom) 글로벌 네비게이션 구조 분석

## 1. 전체 구조 개요

### 1.1 헤더 구성요소
```
<header id="header">
  ├── <div class="header">
  │   ├── 로고 (h1.logo)
  │   ├── 뒤로가기 버튼 (a.history-back)
  │   ├── 글로벌 네비게이션 (nav.header-gnb)
  │   └── 유틸리티 메뉴 (div.top-util)
```

---

## 2. 메인 네비게이션 구조 (GNB - Global Navigation Bar)

### 2.1 네비게이션 계층 구조

#### **3단계 계층 구조 (Depth 1 → Depth 2 → Depth 3)**

```
nav#gnb.header-gnb
├── div.gnb-depth2-bg (배경 오버레이)
└── ul.gnb-depth1
    ├── li (1차 메뉴)
    │   ├── a (1차 메뉴 링크)
    │   └── div.gnb-depth1-hover
    │       └── ul.gnb-depth2
    │           └── li (2차 메뉴)
    │               ├── a (2차 메뉴 링크)
    │               └── ul.gnb-depth3 (3차 메뉴 - 선택적)
```

---

## 3. 상세 메뉴 구조

### 3.1 1단계 메뉴 (Depth 1) - 4개 주요 카테고리

1. **미라콤의 가치**
2. **서비스 및 솔루션**
3. **고객지원**
4. **회사소개**

---

### 3.2 메뉴별 상세 구조

#### **① 미라콤의 가치**
```
미라콤의 가치
└── 스마트팩토리 역량
```

**특징:**
- 단순 구조 (1 depth → 2 depth)
- 하위 메뉴 1개만 존재

---

#### **② 서비스 및 솔루션** (가장 복잡한 구조)

**클래스:** `div.gnb-depth1-hover.wide` (넓은 드롭다운)

**3개 카테고리로 구분:**

##### **A. 서비스 (9개 항목)**
```
서비스
├── 스마트팩토리 컨설팅
├── MES 구축 (3단계 메뉴)
│   ├── On-Premise MES
│   └── Cloud MES
├── ERP 구축
├── 설비 자동화
├── 제조물류 자동화
├── 에너지(FEMS) / 탄소 관리
├── IT 인프라
├── 스마트팩토리 운영
└── IT 운영 (ITO)
```

##### **B. 솔루션 (3개 제품군)**
```
솔루션
├── Nexplant MESplus (4개 하위 모듈)
│   ├── MES (플랫폼 기반 통합생산관리) ★ icon-area 클래스
│   ├── QMS (품질 개선을 통한 생산성 혁신)
│   ├── EES (실시간 설비데이터 분석 및 엔지니어링 솔루션)
│   └── MC (양방향 설비 온라인/자동화 실현)
├── Nexplant MCS (물류설비 연동을 통한 반송/보관 제어)
└── Highway101 (다양한 시스템간 빠르고 쉬운 연계/통합)
```

**솔루션 항목 특징:**
- 각 항목마다 `<span class="descrip">` 로 설명문구 제공
- MES 항목만 특별히 `class="icon-area"` 적용

##### **C. 업종 (7개 산업군)**
```
업종
├── 반도체/태양광
├── 전기/전자
├── 오토모티브
├── 배터리
├── 식음료/제약
├── 화학/소재
└── 기계/금속
```

**구조적 특징:**
- `div.gnb-depth1-hover-category` 사용하여 3개 컬럼으로 분류
- 각 컬럼은 `<dl><dt><dd>` 구조 사용 (Definition List)

---

#### **③ 고객지원**
```
고객지원
├── 인사이트 리포트 (/user/Board/comm_notice.do?BD_NO=6)
├── 라이브러리 (/user/Board/comm_notice.do?BD_NO=5)
├── 언론보도 (/user/Board/comm_notice.do?BD_NO=2)
├── 공지사항 (/user/Board/comm_notice.do?BD_NO=1&LANG_CH=KO)
└── M-FOCUS (/user/Board/comm_notice.do?BD_NO=14)
```

**특징:**
- 모든 메뉴가 게시판 시스템으로 연결 (BD_NO 파라미터)
- 공지사항만 언어 파라미터 포함 (LANG_CH=KO)

---

#### **④ 회사소개**
```
회사소개
├── 미라콤아이앤씨
├── CEO
├── 연혁
├── 윤리경영
├── 보안제보
├── 채용
├── 복리후생
├── IR (게시판: BD_NO=8)
└── 찾아오시는 길
```

**특징:**
- 9개 하위 메뉴
- IR만 게시판 링크, 나머지는 일반 페이지

---

## 4. CSS 클래스 시스템

### 4.1 네비게이션 관련 클래스

| 클래스명 | 용도 | 적용 위치 |
|---------|------|----------|
| `header-gnb` | 글로벌 네비게이션 컨테이너 | `<nav>` |
| `gnb-depth1` | 1단계 메뉴 목록 | `<ul>` |
| `gnb-depth2` | 2단계 메뉴 목록 | `<ul>` |
| `gnb-depth3` | 3단계 메뉴 목록 | `<ul>` |
| `gnb-depth1-hover` | 드롭다운 컨테이너 | `<div>` |
| `gnb-depth1-hover.wide` | 넓은 드롭다운 (서비스 및 솔루션) | `<div>` |
| `gnb-depth2-bg` | 드롭다운 배경 오버레이 | `<div>` |
| `gnb-depth1-hover-category` | 카테고리 구분 컨테이너 | `<div>` |
| `icon-area` | 아이콘 영역 (MES 메뉴) | `<a>` |
| `descrip` | 설명 텍스트 | `<span>` |

---

## 5. 유틸리티 메뉴 구조 (div.top-util)

### 5.1 우측 상단 유틸리티 영역

```
div.top-util
├── 문의하기 (팝업 윈도우)
├── 고객포탈 (외부 링크: cust.miracom-inc.com)
├── Cloud MES (외부 링크: mespluscloud.com)
├── 스마트 팩토리 수준진단 (새창)
├── 언어 선택 (ul.language)
│   ├── KO (현재 선택: class="on")
│   └── EN
└── 전체메뉴 버튼 (button.top-menu.ml40)
```

### 5.2 특수 기능

#### **언어 선택기**
```html
<ul class="language">
  <li class="on" data-value="ko">KO</li>
  <li data-value="en">EN</li>
</ul>
```
- `data-value` 속성으로 언어 코드 관리
- `class="on"` 으로 현재 선택 언어 표시

#### **문의하기 팝업**
```javascript
window.open(
  'https://cust.miracom-inc.com/custportal/rMiracomInquiryKo',
  '_blank',
  'toolbar=no,location=no,directories=no,status=yes,memubar=no,scrollbars=yes,resizable=yes,width=870,height=800,left=50,top=30'
)
```
- 870x800 크기의 팝업 윈도우
- 위치: left=50, top=30

---

## 6. 접근성 (Accessibility) 요소

### 6.1 시맨틱 마크업
- `<nav>` 태그 사용
- `<h1>`, `<h2>` 태그로 제목 구조화
- `<dl>`, `<dt>`, `<dd>` 태그로 정의 목록 구조화

### 6.2 스크린 리더 지원
```html
<span class="hidden">뒤로가기</span>
<span class="hidden">전체메뉴</span>
```
- `class="hidden"` 으로 시각적으로 숨기되 스크린 리더는 읽을 수 있도록 처리

### 6.3 타이틀 속성
- 모든 링크에 `title` 속성 제공
- "바로가기", "새창 열림" 등의 명확한 설명

### 6.4 ARIA 속성
```html
<button type="button" class="top-menu ml40" aria-expanded="false">
```
- `aria-expanded` 속성으로 메뉴 펼침 상태 표시

---

## 7. 라우팅 패턴 분석

### 7.1 URL 구조 패턴

#### **A. 일반 페이지**
```
/common/index.do?jpath=/kor/{category}/{page}
/smartfactory/{section}/index.do?jpath=/kor/{category}/{page}
```

**예시:**
- `/common/index.do?jpath=/kor/company/about`
- `/smartfactory/consulting/index.do?jpath=/kor/offering/consulting`

#### **B. 게시판 페이지**
```
/user/Board/comm_notice.do?BD_NO={board_id}&LANG_CH={language}
```

**예시:**
- 인사이트 리포트: `BD_NO=6`
- 라이브러리: `BD_NO=5`
- 언론보도: `BD_NO=2`
- 공지사항: `BD_NO=1&LANG_CH=KO`
- M-FOCUS: `BD_NO=14`
- IR: `BD_NO=8`

#### **C. 특수 URL**
- Cloud MES: `https://www.mespluscloud.com/` (외부 도메인)
- 고객포탈: `https://cust.miracom-inc.com` (외부 도메인)
- 스마트팩토리 수준진단: `/common/index.do?jpath=/online/service/summary`

---

## 8. 인터랙션 설계

### 8.1 호버 효과
- `div.gnb-depth2-bg`: 메뉴 호버 시 전체 배경 오버레이
- `div.gnb-depth1-hover`: 각 1차 메뉴의 드롭다운 컨테이너

### 8.2 레이아웃 변형
- 일반 드롭다운: `div.gnb-depth1-hover`
- 넓은 드롭다운: `div.gnb-depth1-hover.wide` (서비스 및 솔루션 메뉴)

### 8.3 3단계 메뉴 처리
"MES 구축" 메뉴만 3단계 구조:
```html
<li>
  <span>MES 구축</span>
  <ul class="gnb-depth3">
    <li><a>On-Premise MES</a></li>
    <li><a>Cloud MES</a></li>
  </ul>
</li>
```
- 2단계는 `<span>` (클릭 불가)
- 3단계가 실제 링크

---

## 9. 기술적 특징

### 9.1 JavaScript void 링크
```html
<a href="javascript:void(0);">미라콤의 가치</a>
```
- 1차 메뉴는 직접 링크가 아닌 드롭다운 트리거

### 9.2 이미지 경로
```html
<img src="/assets/images/common/header-logo-black.svg" alt="Miracom">
```
- SVG 형식의 로고 사용

### 9.3 마진 유틸리티 클래스
```html
<button class="top-menu ml40">
```
- `ml40`: margin-left: 40px (추정)

---

## 10. 구조적 장단점 분석

### 10.1 장점
✅ **명확한 계층 구조**: depth1/depth2/depth3 클래스로 직관적 구분
✅ **접근성 고려**: title, hidden, aria 속성 활용
✅ **시맨틱 마크업**: nav, dl/dt/dd 등 적절한 태그 사용
✅ **모듈화된 CSS**: BEM 유사 네이밍 컨벤션
✅ **설명적 UI**: descrip 클래스로 솔루션 설명 제공

### 10.2 개선 가능 영역
⚠️ **불필요한 void(0)**: 최신 표준에서는 `#` 또는 버튼 사용 권장
⚠️ **인라인 JavaScript**: 문의하기 링크의 window.open()
⚠️ **깊은 중첩**: 일부 메뉴의 3단계 구조는 UX 복잡도 증가
⚠️ **다양한 라우팅 패턴**: /common, /smartfactory, /user 등 일관성 부족

---

## 11. 데이터 구조 맵핑

### 11.1 JSON 형태로 표현한 네비게이션 구조

```json
{
  "navigation": {
    "main_menu": [
      {
        "id": 1,
        "title": "미라콤의 가치",
        "link": "javascript:void(0);",
        "sub_menu": [
          {
            "title": "스마트팩토리 역량",
            "link": "/smartfactory/index.do?jpath=/kor/value/ability"
          }
        ]
      },
      {
        "id": 2,
        "title": "서비스 및 솔루션",
        "link": "javascript:void(0);",
        "wide": true,
        "categories": [
          {
            "category": "서비스",
            "items": [
              {
                "title": "스마트팩토리 컨설팅",
                "link": "/smartfactory/consulting/index.do?jpath=/kor/offering/consulting"
              },
              {
                "title": "MES 구축",
                "sub_items": [
                  {
                    "title": "On-Premise MES",
                    "link": "/smartfactory/mes/index.do?jpath=/kor/offering/smart"
                  },
                  {
                    "title": "Cloud MES",
                    "link": "/smartfactory/cloudmes/index.do?jpath=/kor/offering/cloud"
                  }
                ]
              }
              // ... 나머지 서비스 항목
            ]
          },
          {
            "category": "솔루션",
            "items": [
              {
                "title": "Nexplant MESplus",
                "modules": [
                  {
                    "name": "MES",
                    "description": "플랫폼 기반 통합생산관리",
                    "link": "/smartfactory/mes/index.do?jpath=/kor/solution/mes",
                    "icon": true
                  }
                  // ... 나머지 모듈
                ]
              }
              // ... 나머지 솔루션
            ]
          },
          {
            "category": "업종",
            "items": [
              {
                "title": "반도체/태양광",
                "link": "/common/index.do?jpath=/kor/industry/semiconductor"
              }
              // ... 나머지 업종
            ]
          }
        ]
      },
      {
        "id": 3,
        "title": "고객지원",
        "link": "javascript:void(0);",
        "sub_menu": [
          {
            "title": "인사이트 리포트",
            "link": "/user/Board/comm_notice.do?BD_NO=6"
          }
          // ... 나머지 항목
        ]
      },
      {
        "id": 4,
        "title": "회사소개",
        "link": "javascript:void(0);",
        "sub_menu": [
          {
            "title": "미라콤아이앤씨",
            "link": "/common/index.do?jpath=/kor/company/about"
          }
          // ... 나머지 항목
        ]
      }
    ],
    "utility": [
      {
        "title": "문의하기",
        "type": "popup",
        "link": "https://cust.miracom-inc.com/custportal/rMiracomInquiryKo",
        "window_specs": "width=870,height=800"
      },
      {
        "title": "고객포탈",
        "type": "external",
        "link": "https://cust.miracom-inc.com"
      },
      {
        "title": "Cloud MES",
        "type": "external",
        "link": "https://www.mespluscloud.com/"
      },
      {
        "title": "스마트 팩토리 수준진단",
        "type": "new_window",
        "link": "/common/index.do?jpath=/online/service/summary"
      }
    ],
    "language": ["ko", "en"],
    "current_language": "ko"
  }
}
```

---

## 12. 반응형 디자인 고려사항

### 12.1 모바일 대응 요소
- `button.top-menu`: 전체메뉴 버튼 (햄버거 메뉴로 추정)
- `a.history-back`: 뒤로가기 버튼 (모바일 네비게이션)

### 12.2 추정되는 브레이크포인트 동작
- 데스크톱: 호버 방식 드롭다운
- 모바일: 전체메뉴 버튼 클릭 → 사이드 패널 또는 풀스크린 메뉴

---

## 13. 요약

### 핵심 구조
- **4개 주요 카테고리** (1depth)
- **최대 3단계 메뉴 구조** (depth1 → depth2 → depth3)
- **특수 wide 레이아웃**: 서비스 및 솔루션 메뉴
- **카테고리 분류**: dl/dt/dd 구조 사용

### 기술 스택
- 순수 HTML/CSS 구조 (프레임워크 미사용)
- JavaScript 이벤트 핸들링 (별도 파일로 추정)
- SVG 아이콘/로고
- 접근성 표준 준수 (ARIA, title, hidden)

### 라우팅 시스템
- JSP 기반 서버 사이드 라우팅 (.do 확장자)
- jpath 파라미터로 페이지 구분
- 게시판은 BD_NO 파라미터 사용

이 네비게이션은 **전통적인 기업형 웹사이트** 구조로, 명확한 계층과 접근성을 중시한 설계입니다.
