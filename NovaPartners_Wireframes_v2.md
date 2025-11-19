# 🎨 노바파트너스 와이어프레임 설계

**버전**: 2.0  
**작성일**: 2025.11  
**총 페이지**: 23개  
**디자인 시스템**: NovaPartners_DesignSystem_2026.md 참조

---

## 📚 문서 구조

1. **공통 템플릿** (Header, Footer)
2. **HOME** - 메인 페이지
3. **솔루션** (9개 페이지)
   - AI 플랫폼 (5개): 개요, 주요기능, 가격정책, 성공사례, 데모신청
   - 산업별 솔루션 (4개): 금융, 제조, 의료, 소매
4. **노바파트너스의 가치** (2개): 보안·규정준수, 전문성
5. **고객지원** (4개): FAQ, 기술문서, 튜토리얼, 커뮤니티
6. **회사소개** (6개): 회사개요, 연혁, 팀, 뉴스룸, 채용, 오시는길
7. **유틸리티** (2개): 문의하기, 기술지원

---

## 🎯 공통 템플릿

### Header (고정, 80px)
```
┌─────────────────────────────────────────────────┐
│ [LOGO] 솔루션▼ 가치▼ 고객지원▼ 회사소개▼ [검색][문의][지원] │
└─────────────────────────────────────────────────┘
```
- 로고: 200×60px (좌측)
- GNB: 중앙, 드롭다운
- 검색: 아이콘 버튼
- 문의/지원: Primary/Secondary 버튼

### Footer (300px)
```
┌───────────────────────────────────────────┐
│ [회사소개]  [솔루션]  [고객지원]  [SNS]   │
│ - 소개      - AI     - FAQ      [F][T]   │
│ - 연혁      - 산업   - 문서     [L][Y]   │
│ © 2025 Nova | 개인정보 | 이용약관        │
└───────────────────────────────────────────┘
```

---

## 1️⃣ HOME (메인 페이지)

### 구조 (총 높이: ~3500px)
1. **Hero** (100vh): 좌측 텍스트 + 우측 비디오
2. **Features** (400px): 3개 카드 (신뢰, 최적화, 효과)
3. **Industries** (1000px): 4개 산업 (금융/제조/의료/소매)
4. **Stats** (400px): 숫자 통계 (150+ 고객사, 98% 만족도)
5. **Logos** (300px): 고객사 로고 캐러셀
6. **CTA** (500px): 배경 비디오 + 버튼 2개

### Hero Section
```
┌─────────────────────────────────────────┐
│ [좌측 50%]            [우측 50%]        │
│ H1: 메인 카피         [배경 비디오]     │
│ P: 서브 카피          + 패럴렉스        │
│ [CTA 1][CTA 2]                          │
└─────────────────────────────────────────┘
```
- 인터랙션: 마우스 움직임 → 패럴렉스, 버튼 Hover → Lift

### Features (3 Column)
```
[카드1: 신뢰]  [카드2: 최적화]  [카드3: 효과]
```
- Hover: 배경색+그림자+스케일 1.02

### Industries (좌우 교대 레이아웃)
```
[금융]
[이미지] [텍스트+통계] → Hover: 이미지 확대

[제조]
[텍스트+통계] [이미지] → 반대 레이아웃

(의료, 소매 동일)
```

---

## 2️⃣ 솔루션 (9개 페이지)

### 2-1. AI 플랫폼 개요 (`/solutions/ai-platform/`)
- Hero + 3단계 프로세스 + 4개 특징 + 산업 탭 + 사례 캐러셀 + CTA

### 2-2. 주요기능 (`/features`)
- Hero + 5개 탭 (데이터관리/AI분석/리포팅/통합/보안) + 기술스펙 테이블 + 통합 로고

### 2-3. 가격정책 (`/pricing`)
- Hero + 4개 플랜 카드 (Starter/Pro/Enterprise/Custom) + 비교 테이블 + 요금 계산기 + FAQ

### 2-4. 성공사례 (`/case-studies`)
- Hero + 필터 (금융/제조/의료/소매) + 3 Column 그리드 + Pagination

#### 상세 사례 페이지 (`/case-studies/{id}`)
- Hero + 고객소개 + 문제점 + 솔루션 + 결과 (그래프) + 후기 (비디오) + Before/After 슬라이더

### 2-5. 데모 신청 (`/demo`)
- Hero + 폼 (좌측 50%) + 정보 (우측 50%) + 연락처

### 2-6~9. 산업별 솔루션 (금융/제조/의료/소매)
**공통 구조:**
- Hero (산업별 배경)
- 주요 문제 (4개 카드)
- 솔루션 (2-4개, 이미지+설명)
- 성과 (3개 통계, 카운팅 애니메이션)
- 사례 (3개 카드)
- CTA

---

## 3️⃣ 노바파트너스의 가치 (2개)

### 3-1. 보안·규정준수 (`/value/security-compliance`)
- Hero + 보안 인증 배지 (ISO/SOC2/GDPR/HIPAA/CCPA)
- 보안 기능 (4개: 암호화/접근제어/감사/백업)
- 규정 준수 탭 (금융/의료/개인정보/글로벌)
- 보안 프로세스 (타임라인)

### 3-2. 전문성 (`/value/expertise`)
- Hero + 전문성 지표 (10년+ 경력, 100+ 프로젝트, 98% 성공률)
- 전문 분야 (3개: ML/AI, 데이터엔지니어링, 클라우드)
- 팀 구성 (경영진 3명 + 부서별 팀원들)
- 파트너십 (AWS/GCP/MS/IBM)

---

## 4️⃣ 고객지원 (4개)

### 4-1. FAQ (`/support/faq`)
- Hero + 검색창
- 카테고리 필터 (전체/일반/기술/가격/보안/계정)
- 아코디언 리스트 (20-30개 질문)
- 문제 해결 안됨 CTA

### 4-2. 기술 문서 (`/support/documentation`)
- Hero + 검색
- 사이드바 (250px, 트리 메뉴) + 메인 콘텐츠
- 목차 (Sticky), 코드 하이라이팅, 복사 버튼

### 4-3. 튜토리얼 (`/support/tutorials`)
- Hero + 필터 (초급/중급/고급, 비디오/텍스트)
- 3 Column 그리드 (썸네일+제목+난이도+시간)

#### 튜토리얼 상세 (`/tutorials/{id}`)
- Hero (비디오 플레이어)
- 진행 바 (1→2→3→4 단계)
- 콘텐츠 (텍스트+코드+이미지)
- 관련 튜토리얼

### 4-4. 커뮤니티 (`/support/community`)
- Hero + 통계 (5000+ 회원, 10000+ 게시글)
- 카테고리 (전체/Q&A/공지/팁&트릭/쇼케이스)
- 게시글 목록 (댓글수+좋아요+태그+작성자)
- Pagination

---

## 5️⃣ 회사소개 (6개)

### 5-1. 회사 개요 (`/company/about`)
- Hero (회사 슬로건)
- 회사 소개 (이미지+텍스트: 설립/직원/고객/글로벌)
- 미션·비전·가치 (3개 카드)
- 주요 성과 (5개 통계)
- 글로벌 오피스 (인터랙티브 지도, 5개 마커)

### 5-2. 연혁 (`/company/history`)
- Hero
- 세로 타임라인 (2015→2020→2023→2024→2025)
- 각 연도: 이미지+주요 이벤트
- 스크롤 애니메이션

### 5-3. 팀 (`/company/team`)
- Hero (팀 사진)
- 경영진 (3명: CEO/CTO/CFO, 사진+LinkedIn)
- 부서별 팀 탭 (전체/AI/엔지니어링/디자인/영업)
- 팀 문화 (3개: 유연근무/학습지원/글로벌)

### 5-4. 뉴스룸 (`/company/newsroom`)
- Hero + 필터 (전체/보도자료/블로그/행사/수상)
- 주요 뉴스 (대형 카드 1개)
- 뉴스 목록 (3 Column 그리드)
- 언론 보도 (리스트)

### 5-5. 채용 (`/company/careers`)
- Hero (채용 슬로건)
- Why Nova? (6개: 성장/혁신/보상/워라밸/복지/문화)
- 채용 프로세스 (6단계 플로우)
- 채용 공고 (필터+카드 리스트)

### 5-6. 오시는 길 (`/company/location`)
- Hero
- 본사 정보 (주소/전화/이메일/운영시간)
- 카카오맵 임베드 (500px)
- 교통편 탭 (지하철/버스/자가용)
- 주차 안내
- 해외 오피스 리스트

---

## 6️⃣ 유틸리티 (2개)

### 6-1. 문의하기 (`/contact/`)
- Hero
- 폼 (좌측 50%): 이름/이메일/회사/산업/문의유형/메시지/동의
- 정보 (우측 50%): 전화/이메일/채팅/운영시간/FAQ/지도
- 제출 성공 모달

### 6-2. 기술지원 (`/support/technical`)
- Hero + 빠른 지원 (3개: 채팅/문서/비디오)
- 지원 요청 폼: 이름/이메일/계정ID/문제유형/우선순위/설명/첨부/환경정보
- 지원 정책 (응답 시간)

---

## 🎭 인터랙션 가이드

### 공통 인터랙션
| 요소 | 인터랙션 | 효과 |
|------|----------|------|
| **Button** | Hover | 배경색 진하게, 그림자 추가, Lift 2px |
| **Card** | Hover | 배경색 변화, 그림자 상승, 스케일 1.02 |
| **Link** | Hover | 언더라인, 색상 진하게 |
| **Input** | Focus | 테두리 파란색, 그림자 추가 |
| **Image** | Hover | 스케일 1.05, 밝기 증가 |

### 특수 인터랙션
| 컴포넌트 | 동작 |
|----------|------|
| **Hero 비디오** | 자동재생, 무음, 루프, 패럴렉스 |
| **숫자 통계** | 스크롤 시 0부터 카운팅 애니메이션 |
| **캐러셀** | 자동 회전 5초, 수동 제어 (◄►), Hover 일시정지 |
| **탭 메뉴** | 클릭 시 콘텐츠 전환 (슬라이드) |
| **아코디언** | 클릭 시 열기/닫기 (슬라이드 다운/업) |
| **모달** | Fade-in 0.2s, 배경 blur + 반투명 |
| **슬라이더** | 드래그 시 실시간 값 업데이트 |
| **스크롤 애니메이션** | Fade-in, Slide-up (Intersection Observer) |

---

## 📱 반응형 Breakpoints

### Desktop (≥1200px)
- Container: 1280px
- Layout: 2-3 columns
- Nav: Full menu
- Font: 기본 크기

### Tablet (768px - 1199px)
- Container: 100% - 48px padding
- Layout: 1-2 columns
- Nav: 햄버거 메뉴
- Font: -1 size

### Mobile (<768px)
- Container: 100% - 24px padding
- Layout: 1 column stack
- Nav: 햄버거 메뉴
- Font: -2 size
- Touch: 최소 44px

---

## 🎨 디자인 토큰 Quick Reference

```css
/* Colors */
--primary: #0055FF;
--primary-hover: #0044CC;
--text: #343A40;
--bg: #FFFFFF;
--bg-light: #F8F9FA;

/* Typography */
--font-primary: 'Pretendard Variable';
--h1: 48px/700;
--h2: 36px/600;
--body: 16px/400;

/* Spacing */
--space-base: 8px;
--space-2: 16px;
--space-4: 32px;
--space-8: 64px;

/* Border Radius */
--radius-sm: 8px;
--radius-md: 12px;
--radius-lg: 16px;

/* Shadow */
--shadow-sm: 0 2px 8px rgba(0,0,0,0.08);
--shadow-md: 0 4px 16px rgba(0,0,0,0.12);
--shadow-lg: 0 8px 32px rgba(0,0,0,0.16);

/* Transition */
--duration-fast: 0.2s;
--duration-normal: 0.3s;
--ease: cubic-bezier(0.4, 0, 0.2, 1);
```

---

## ✅ 개발 체크리스트

### 필수 구현
- [ ] 모든 페이지 Header/Footer 포함
- [ ] Breadcrumb (Depth 2 이상)
- [ ] Hover/Focus 상태
- [ ] 로딩 상태 (Skeleton/Spinner)
- [ ] 에러 상태 (404/500)
- [ ] 모바일 반응형
- [ ] 키보드 네비게이션
- [ ] ARIA labels
- [ ] 색상 대비 4.5:1 이상

### 성능
- [ ] 이미지 WebP + Lazy loading
- [ ] 비디오 최적화 (<10MB)
- [ ] Critical CSS 인라인
- [ ] JavaScript 지연 로딩
- [ ] Lighthouse 스코어 90+

### 인터랙션
- [ ] Scroll 애니메이션
- [ ] 숫자 카운팅
- [ ] 캐러셀 자동/수동
- [ ] 탭/아코디언
- [ ] 모달/팝업
- [ ] 폼 검증
- [ ] Before/After 슬라이더

---

## 📦 컴포넌트 라이브러리

### 재사용 컴포넌트
1. **Button** (Primary, Secondary, Ghost)
2. **Card** (Default, Hover, Featured)
3. **Input** (Text, Email, Textarea, Select)
4. **Modal** (Small, Medium, Large)
5. **Tab** (Horizontal, Vertical)
6. **Accordion** (Single, Multiple)
7. **Carousel** (Auto, Manual, Dots)
8. **Badge** (Success, Warning, Error, Info)
9. **Breadcrumb**
10. **Pagination**

---

## 🚀 다음 단계

1. **디자인**: Figma에서 고해상도 목업 제작
2. **프로토타입**: 인터랙티브 프로토타입 (Figma/Framer)
3. **개발**: React/Next.js로 구현
4. **테스트**: Lighthouse, axe DevTools
5. **배포**: Vercel/Netlify

---

**END OF WIREFRAME DOCUMENT v2.0**

**총 23개 페이지 와이어프레임 설계 완료**

📄 관련 문서:
- `NovaPartners_IA.md` - 정보 아키텍처
- `NovaPartners_URL_Structure.md` - URL 구조
- `NovaPartners_Content_Outline.md` - 콘텐츠 아웃라인
- `NovaPartners_DesignSystem_2026.md` - 디자인 시스템
