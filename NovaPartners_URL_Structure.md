# 노바파트너스 웹사이트 URL 구조 설계

## 📌 기본 설정

**Domain**: `novapartners.com` (예시)

**URL 설계 원칙**:
- ✅ 영문 소문자 사용 (SEO 최적화)
- ✅ 하이픈(-) 사용 (언더스코어 X)
- ✅ 3depth 이상 지양 (사용자 경험 최적화)
- ✅ 직관적 구조 (페이지 목적이 URL로부터 명확)
- ✅ 카테고리별 그룹핑

---

## 🏗️ 완전한 URL 맵 (URL Map)

### **Root Level (Depth 0)**

```
/ (HOME)
```

---

### **주메뉴 1: 솔루션 (Solutions)**

#### **1-1. AI 플랫폼 (Depth 1-2)**

| 페이지 ID | 페이지명 | URL | Breadcrumb |
|---------|--------|-----|-----------|
| 1-1-1 | AI 플랫폼 개요 | `/solutions/ai-platform/` | Solutions > AI Platform |
| 1-1-2 | 주요기능 | `/solutions/ai-platform/features` | Solutions > AI Platform > Features |
| 1-1-3 | 가격정책 | `/solutions/ai-platform/pricing` | Solutions > AI Platform > Pricing |
| 1-1-4 | 성공사례 | `/solutions/ai-platform/case-studies` | Solutions > AI Platform > Case Studies |
| 1-1-5 | 데모신청 | `/solutions/ai-platform/demo` | Solutions > AI Platform > Demo |

#### **1-2. 산업별 솔루션 (Depth 1-2)**

| 페이지 ID | 페이지명 | URL | Breadcrumb |
|---------|--------|-----|-----------|
| 1-2-1 | 금융 산업 | `/solutions/industries/finance` | Solutions > Industries > Finance |
| 1-2-2 | 제조 산업 | `/solutions/industries/manufacturing` | Solutions > Industries > Manufacturing |
| 1-2-3 | 의료 산업 | `/solutions/industries/healthcare` | Solutions > Industries > Healthcare |
| 1-2-4 | 소매 산업 | `/solutions/industries/retail` | Solutions > Industries > Retail |

**참고**: `/solutions/industries/` 자체는 산업별 솔루션 랜딩 페이지 역할

---

### **주메뉴 2: 노바파트너스의 가치 (Nova Value)**

| 페이지 ID | 페이지명 | URL | Breadcrumb |
|---------|--------|-----|-----------|
| 2-1 | 노바파트너스의 가치 | `/value/` | Nova Value |
| 2-2 | 보안 & 규정준수 | `/value/security-compliance` | Nova Value > Security & Compliance |

---

### **주메뉴 3: 고객지원 (Support)**

| 페이지 ID | 페이지명 | URL | Breadcrumb |
|---------|--------|-----|-----------|
| 3-1 | 기술지원 | `/support/technical` | Support > Technical Support |
| 3-2 | 다운로드 | `/support/downloads` | Support > Downloads |
| 3-3 | FAQ | `/support/faq` | Support > FAQ |
| 3-4 | 공지사항 | `/support/news` | Support > News |

**참고**: 기본 랜딩 페이지 `/support/` 도 필요 (또는 리다이렉트)

---

### **주메뉴 4: 회사소개 (Company)**

| 페이지 ID | 페이지명 | URL | Breadcrumb |
|---------|--------|-----|-----------|
| 4-1 | 노바파트너스 (회사소개) | `/company/` | Company |
| 4-2 | CEO | `/company/ceo` | Company > CEO |
| 4-3 | 연혁 | `/company/history` | Company > History |
| 4-4 | 윤리경영 | `/company/ethics` | Company > Ethics & Values |
| 4-5 | 채용 | `/company/careers` | Company > Careers |
| 4-6 | 찾아오시는 길 | `/company/locations` | Company > Locations |

**대안 URL**: 
- `/about/`, `/team/`, `/join/` 등으로 구성 가능
- `/office/`, `/contact-us/` 등

---

### **유틸리티 (Utilities)**

| 페이지 ID | 페이지명 | URL | 위치 |
|---------|--------|-----|------|
| U-1 | 문의하기 | `/contact/` | Header/Footer |
| U-2 | 기술지원 | `/support/technical` | Header (또는 Support 참고) |

---

## 📊 URL 계층 구조 다이어그램

```
novapartners.com
│
├─ / (HOME)
│
├─ /solutions/
│  ├─ /solutions/ai-platform/
│  │  ├─ /solutions/ai-platform/features
│  │  ├─ /solutions/ai-platform/pricing
│  │  ├─ /solutions/ai-platform/case-studies
│  │  └─ /solutions/ai-platform/demo
│  │
│  └─ /solutions/industries/
│     ├─ /solutions/industries/finance
│     ├─ /solutions/industries/manufacturing
│     ├─ /solutions/industries/healthcare
│     └─ /solutions/industries/retail
│
├─ /value/
│  ├─ /value/ (메인 - 차별점)
│  └─ /value/security-compliance
│
├─ /support/
│  ├─ /support/technical
│  ├─ /support/downloads
│  ├─ /support/faq
│  └─ /support/news
│
├─ /company/
│  ├─ /company/ (메인 - 회사소개)
│  ├─ /company/ceo
│  ├─ /company/history
│  ├─ /company/ethics
│  ├─ /company/careers
│  └─ /company/locations
│
└─ /contact/
```

---

## 🔍 URL 패턴 정리

### **카테고리별 URL Prefix**

| Prefix | 의미 | 페이지 수 | 예시 |
|--------|------|---------|------|
| `/solutions/` | 솔루션 & 상품 | 9 | `/solutions/ai-platform/pricing` |
| `/value/` | 차별점 & 신뢰 | 2 | `/value/security-compliance` |
| `/support/` | 고객 지원 | 4 | `/support/faq` |
| `/company/` | 회사 정보 | 6 | `/company/ceo` |
| `/contact/` | 문의 | 1 | `/contact/` |

### **URL 명명 규칙 (Naming Convention)**

| 유형 | 형식 | 예시 |
|------|------|------|
| **카테고리 페이지** | `/category/` | `/support/`, `/company/` |
| **세부 페이지** | `/category/page-name` | `/support/faq`, `/company/ceo` |
| **다단계** (가능한 경우만) | `/category/subcategory/item` | `/solutions/industries/finance` |

---

## 🚀 고급 URL 옵션 (대안)

### **옵션 A: 더 간단한 구조**

```
/
├─ /solutions/ (AI Platform 개요)
├─ /features/
├─ /pricing/
├─ /case-studies/
├─ /finance-solution/
├─ /manufacturing-solution/
├─ /value/
├─ /security/
├─ /support/
├─ /faq/
├─ /about/
├─ /contact/
```

**장점**: 깔끔, 직관적  
**단점**: 카테고리별 구분이 덜함, 페이지 증가 시 복잡도 증가

---

### **옵션 B: 더 상세한 구조 (권장 X)**

```
/solutions/
├─ /ai-platform/
│  ├─ /overview/
│  ├─ /features/
│  └─ ...
└─ /industries/
   ├─ /finance/
   └─ ...
```

**장점**: 매우 명확한 계층  
**단점**: URL이 너무 길어짐, 3depth 이상 (SEO에 불리)

---

## 📄 특수 페이지 URL

### **동적 페이지 (Dynamic Pages)**

```
# 공지사항 상세 페이지
/support/news/{post-id}/ 또는 /support/news/{post-slug}/
예: /support/news/product-update-v2-0/

# 산업별 성공사례 (선택사항)
/solutions/ai-platform/case-studies/{case-id}/
예: /solutions/ai-platform/case-studies/financial-bank-korea/
```

---

## ✅ SEO 최적화 체크리스트

- [x] 영문 소문자 사용
- [x] 하이픈(-) 구분자 사용
- [x] 3depth 이상 지양
- [x] 카테고리별 명확한 구분
- [x] 페이지 목적이 URL에서 명확
- [ ] **필요**: 301 리다이렉트 규칙 정의 (도메인 변경 시)
- [ ] **필요**: robots.txt 설정
- [ ] **필요**: sitemap.xml 자동 생성

---

## 🔗 내부 링크 전략 (Internal Linking Strategy)

### **주요 Link Flow**

```
HOME
 ├─> /solutions/ai-platform/
 ├─> /solutions/industries/finance
 ├─> /value/
 ├─> /support/faq
 ├─> /company/
 └─> /contact/

/solutions/ai-platform/
 ├─> /solutions/ai-platform/features
 ├─> /solutions/ai-platform/pricing
 ├─> /solutions/ai-platform/case-studies
 ├─> /solutions/ai-platform/demo
 ├─> /solutions/industries/ (산업별 솔루션)
 ├─> /contact/ (CTA)
 └─> /support/technical (기술지원)

/solutions/industries/finance/
 ├─> /solutions/ai-platform/features
 ├─> /solutions/ai-platform/pricing
 ├─> /solutions/ai-platform/case-studies
 └─> /solutions/ai-platform/demo

/value/
 ├─> /value/security-compliance
 └─> /contact/ (CTA)

/support/
 ├─> /support/technical
 ├─> /support/downloads
 ├─> /support/faq
 └─> /support/news
```

---

## 📋 최종 URL 목록 (Quick Reference)

```
1. HOME: /
2. AI Platform Overview: /solutions/ai-platform/
3. Features: /solutions/ai-platform/features
4. Pricing: /solutions/ai-platform/pricing
5. Case Studies: /solutions/ai-platform/case-studies
6. Demo: /solutions/ai-platform/demo
7. Finance Solution: /solutions/industries/finance
8. Manufacturing Solution: /solutions/industries/manufacturing
9. Healthcare Solution: /solutions/industries/healthcare
10. Retail Solution: /solutions/industries/retail
11. Nova Value: /value/
12. Security & Compliance: /value/security-compliance
13. Technical Support: /support/technical
14. Downloads: /support/downloads
15. FAQ: /support/faq
16. News: /support/news
17. Company: /company/
18. CEO: /company/ceo
19. History: /company/history
20. Ethics: /company/ethics
21. Careers: /company/careers
22. Locations: /company/locations
23. Contact: /contact/
```

**총 23개 페이지** (HOME 제외시 22개)

---

## 🛠️ 구현 시 고려사항

### **1. 404 페이지**
- URL: `/404/` 또는 별도 처리
- 구글 웹마스터도구 등록 필요

### **2. 사이트맵 (Sitemap)**
- 파일: `/sitemap.xml`
- 모든 페이지 URL 포함
- 주기적 업데이트 (robots.txt에 명시)

### **3. Robots.txt**
- 파일: `/robots.txt`
- Sitemap 위치 명시
- 크롤링 규칙 설정

### **4. 301 리다이렉트**
- 구 URL → 신 URL 매핑 필요
- 예: `/solutions/ → /solutions/ai-platform/` (선택사항)

### **5. 언어별 URL (다국어 지원 시)**
- 한국: `/ko/`, `/en/` 별도 구조
- 또는: `.kr`, `.com` 별도 도메인
- hreflang 태그 설정 필요

---

## 📊 URL 구조 비교 분석

| 항목 | 추천 구조 | 옵션 A | 옵션 B |
|-----|---------|--------|--------|
| Depth | 2-3 | 1-2 | 3+ |
| 복잡도 | 중간 | 낮음 | 높음 |
| 확장성 | 좋음 | 중간 | 낮음 |
| SEO | 좋음 | 중간 | 낮음 |
| 사용자 경험 | 좋음 | 중간 | 낮음 |
| 권장 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐ |

