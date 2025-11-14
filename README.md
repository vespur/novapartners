# 식스샵 프로 커스텀 블록 개발 저장소

> 식스샵 프로(SixShop Pro) 웹빌더용 고품질 커스텀 블록 모음

---

## 📦 포함된 블록 (7개)

### 메인 페이지 블록

#### 1-1. 글로벌 헤더 (Global Header)
**파일:** `1-1-global-header.html`, `1-1-global-header.json`
**크기:** 79KB + 69KB
**설명:**
- 엔터프라이즈급 네비게이션 헤더
- 데스크톱/모바일 완전 분리 (햄버거+드로어 메뉴)
- 하위 메뉴 지원 (드롭다운/아코디언)
- 완벽한 디자인 커스터마이징

#### 1-2. 히어로 캐러셀 v3.3 (Hero Carousel)
**파일:** `1-2-hero-carousel-v3.html`, `1-2-hero-carousel-v3.json`
**크기:** 39KB + 36KB
**설명:**
- 비디오/이미지 슬라이드 캐러셀
- CSS 전역 패딩 완전 적용
- 진행 게이지, 화살표 네비게이션
- Glow 효과, 자동재생 제어

#### 1-3. 로고 배너 슬라이드 (Logo Banner Slide)
**파일:** `1-3-logo-banner-slide.html`, `1-3-logo-banner-slide.json`
**크기:** 4.3KB + 7.5KB
**설명:**
- 무한 스크롤 로고 배너
- 템플릿 6세트 복제 방식 (끊김 없음)
- 그레이스케일 + 호버 효과
- 파트너/고객사 로고 표시용

### 서브 페이지 콘텐츠 블록

#### 2-1. 서비스 카드 그리드 (Service Cards Grid)
**파일:** `2-1-service-cards-grid.html`, `2-1-service-cards-grid.json`
**설명:**
- 3단 그리드 반응형 카드 레이아웃
- 아이콘 + 제목 + 설명 + 버튼 구조
- 카드 호버 애니메이션 (lift, scale, glow)
- 스크롤 애니메이션 (fade-up, scale-in, fade-left)
- 완전한 데스크톱/모바일 분리

#### 2-2. CTA 섹션 (Call-to-Action Section)
**파일:** `2-2-cta-section.html`, `2-2-cta-section.json`
**설명:**
- 배경 이미지 + 그라디언트 오버레이 지원
- 주 버튼 + 보조 버튼 이중 구성
- 다양한 버튼 호버 애니메이션
- 스크롤 인입 애니메이션
- 중앙 정렬 콘텐츠 레이아웃

#### 2-3. 텍스트+이미지 섹션 (Text+Image Section)
**파일:** `2-3-text-image-section.html`, `2-3-text-image-section.json`
**설명:**
- 이미지 좌/우 위치 선택 가능
- 제목 + 부제목 + 설명 + 버튼 구성
- 이미지 호버 효과 (zoom, lift)
- 슬라이드 애니메이션 (좌우 분리)
- 수직 정렬 옵션 (상/중/하)

#### 2-4. 통계 카운터 (Statistics Counter)
**파일:** `2-4-stats-counter.html`, `2-4-stats-counter.json`
**설명:**
- 숫자 카운팅 애니메이션 효과
- 4단 그리드 통계 표시
- 아이콘 + 숫자 + 레이블 구조
- 그라디언트 배경 오버레이 옵션
- 아이템 호버 애니메이션

---

## 📚 개발 가이드

### 메인 가이드
**`SIXSHOP_BLOCK_DEVELOPMENT_GUIDE.md`** (38KB)
- **버전:** 2.2
- **최종 업데이트:** 2025-01-13
- **내용:**
  - Part 1: 핵심 규칙 (파일 구조, 제약사항, Handlebars, JSON, CSS, JavaScript API)
  - Part 2: 식스샵 프로 기준 (필수 요건, 커스텀 블록 점검, 헤더/푸터 기준)
  - Part 3: 개발 원칙 (사용자 요청 개발 원칙, 최종 체크리스트)

### 추가 레퍼런스
- **`BLOCK_MAKER_GUIDELINES.md`** (38KB): AI 에이전트 지침, CSS 디버깅 프로토콜
- **`BLOCKMAKER_DEVELOPMENT_GUIDELINES.md`** (18KB): Settings 타입별 상세 가이드
- **`HERO_V3.3_CSS_GLOBAL_PADDING.md`** (12KB): 히어로 v3.3 수정 문서

---

## 🎯 핵심 개발 원칙

### ⛔ 절대 금지 (13가지)
1. LIST 중첩
2. LIST 내 필드 5개 이상
3. Reserved ID 사용 (menus, products, collections, pages)
4. SECTION/DIVIDER 타입
5. description 100자 초과
6. 미지원 Handlebars 헬퍼 (div, mul, ne, gte, lte)
7. SVG 속성 CSS 조작
8. console.log 남기기
9. JavaScript 동적 복제
10. 반말 사용
11. 통이미지 사용
12. 단일 padding/margin (Y/X 분리 필수)
13. 제목과 항목 설정 공유

### ✅ 필수 준수 (17가지)
1. HTML/JSON 파일 분리
2. camelCase ID 명명
3. bm.onContextChange 구현
4. bm.onDestroy 구현 (이벤트 리스너 시)
5. bm.container 이벤트 위임
6. 테마 CSS 변수 사용
7. 빈 링크 클릭 방지 (헤더/푸터)
8. JSON-HTML 동기화
9. 템플릿 기반 정적 복제 (무한 스크롤)
10. 존댓말 + 친절한 설명
11. 텍스트 기본값 (제목 36/24px, 본문 20/16px)
12. 스크롤 인터렉션 (적합한 블록)
13. 데스크톱/모바일 완전 분리
14. 모든 텍스트에 크기/굵기/색상 3가지 제공
15. padding을 paddingY/paddingX로 분리
16. 계층 구조 시 제목/항목 완전 분리
17. TITLE로 섹션 구분

---

## 🚀 빠른 시작

### 1. 블록 사용 방법
1. `.html` 파일 내용 복사
2. 식스샵 프로 에디터 → 블록메이커 → HTML 탭에 붙여넣기
3. `.json` 파일 내용 복사
4. 블록메이커 → JSON 탭에 붙여넣기
5. 저장 및 미리보기

### 2. 새 블록 개발 시작
1. `SIXSHOP_BLOCK_DEVELOPMENT_GUIDE.md` 읽기
2. 기존 블록 중 하나를 템플릿으로 복사
3. 가이드 체크리스트 확인하며 개발
4. 필수 요건 모두 충족 확인

---

## 📊 품질 기준

### 식스샵 프로 필수 요건
- [ ] 에디터에서 모든 기능이 오류 없이 작동
- [ ] 사용자가 직접 편집 가능한 요소만 포함
- [ ] 통이미지 사용 금지
- [ ] 저작권 문제 없는 리소스만 사용
- [ ] 내 페이지 3개 이상 구성 가능
- [ ] 온전한 헤더/푸터 메뉴 구성

### 커스텀 블록 품질 체크
- [ ] 블록 이름 한글 작성
- [ ] 모든 문구 존댓말로 작성
- [ ] TITLE로 섹션 그룹핑
- [ ] isVisible 적절히 사용
- [ ] 설정 변경 시 즉시 반영
- [ ] 데스크톱/모바일 모두 정상 작동
- [ ] 테마 폰트 및 색상 변수 적용

---

## 🔧 개발 환경

### 지원 플랫폼
- 식스샵 프로 웹빌더

### 사용 기술
- Handlebars (템플릿 엔진)
- CSS3 (반응형 디자인)
- Vanilla JavaScript (ES6+)

### 브라우저 지원
- Chrome/Edge (최신 버전)
- Firefox (최신 버전)
- Safari (최신 버전)
- Mobile Safari (iOS)

---

## 📖 버전 이력

### v2.3 (2025-11-14)
- 서브 페이지 콘텐츠 블록 4개 추가
  - 서비스 카드 그리드 (3단 반응형)
  - CTA 섹션 (배경 그라디언트 + 이중 버튼)
  - 텍스트+이미지 섹션 (좌우 배치)
  - 통계 카운터 (애니메이션 카운팅)
- 모든 블록에 스크롤 애니메이션 적용
- 호버 효과 다양화 (lift, scale, glow, zoom)
- v2.2 가이드라인 완전 준수

### v2.2 (2025-01-13)
- 디자인 자유도 극대화 원칙 추가
- 데스크톱/모바일 완전 분리 원칙 추가
- 여백 분리 원칙 (paddingY/paddingX) 추가
- 통이미지 사용 금지 명시
- 계층 구조 제목/항목 완전 분리

### v2.1
- 비개발자를 위한 에디터 UX 가이드 추가
- 스크롤 인터렉션 구현 가이드 추가
- 텍스트 크기 기본값 정립

### v2.0
- 3개 세션 통합 (엔터프라이즈 헤더, 히어로 비디오, 로고 배너)
- 템플릿 납품 기준 추가

---

## 📞 문의 및 피드백

문제 발견 시 다음 정보와 함께 보고:
1. 사용한 블록 이름
2. 브라우저 및 버전
3. 설정 값 (JSON)
4. 예상 동작 vs 실제 동작
5. 브라우저 개발자 도구 콘솔 오류 (있는 경우)

---

## 📄 라이선스

이 프로젝트는 식스샵 프로 템플릿 납품용으로 개발되었습니다.

---

**Last Updated:** 2025-11-14
**Repository:** novapartners
**Branch:** claude/2025-11-14-custom-blocks-sixshop-01BSab2yU6kyFtHkDFm9ep8N
