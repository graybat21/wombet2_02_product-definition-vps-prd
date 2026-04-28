# 보완된 UI/UX 태스크 상세 명세서 (GitHub Issue 형식)

본 문서는 `02_보완된_UI_TASK_리스트.md`에서 도출된 6개의 UI/UX 프론트엔드 태스크를 실제 개발에 착수할 수 있도록 구체적인 GitHub Issue 형태로 작성한 명세서입니다.

---

```markdown
---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature] UI-001: 공통 디자인 시스템 및 기본 레이아웃 구축"
labels: 'feature, frontend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [UI-001] 공통 디자인 시스템 및 기본 레이아웃 구축
- 목적: WOMBET2 웹 애플리케이션 전체의 일관된 사용자 경험(UX)을 보장하기 위해 컬러/타이포그래피 토큰을 정의하고, GNB 및 Footer 등 공통 골격을 구성한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: `/docs/SRS_v0.3.md#3.2 Client Applications & UseCase Diagram`
- 보완 태스크: `/docs/02_보완된_UI_TASK_리스트.md`

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 글로벌 스타일(Global Styles) 및 테마(Theme) 설정 (경고용 붉은색 등 브랜드 컬러 토큰화)
- [ ] 모바일 반응형 GNB(Global Navigation Bar) 컴포넌트 구현
- [ ] Footer 컴포넌트 구현 (약관, 고객센터 등 기본 정보 포함)
- [ ] 공통 에러 바운더리(Error Boundary) 및 404/500 Fallback 페이지 골격 구현

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 기본 레이아웃 렌더링
- Given: 사용자가 웹사이트 루트('/')에 접속함
- When: 페이지가 로드됨
- Then: 뷰포트 크기(Mobile/Desktop)에 맞는 GNB와 Footer가 깨짐 없이 정상적으로 렌더링된다.

Scenario 2: 존재하지 않는 페이지 접근
- Given: 사용자가 정의되지 않은 URL('/unknown-path')로 접근함
- When: 라우팅이 시도됨
- Then: 공통 404 Not Found 에러 페이지가 렌더링되어 홈으로 돌아가는 링크를 제공한다.

## :gear: Technical & Non-Functional Constraints
- 성능: 전체 공통 레이아웃의 LCP(Largest Contentful Paint) ≤ 1.5초 달성
- 안정성: React Strict Mode 환경에서 Warning 및 에러 없이 동작
- 스택: Tailwind CSS 권장 (또는 Vanilla CSS 모듈)

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 단위 테스트(Unit Test)를 통해 GNB/Footer 마운트 여부가 검증되었는가?
- [ ] SonarQube / Linter 등의 정적 분석 도구 경고가 없는가?
- [ ] 모바일 해상도(375px)에서 레이아웃 깨짐이 없는가?

## :construction: Dependencies & Blockers
- Depends on: 없음 (기반 작업)
- Blocks: #UI-002, #UI-003, #UI-004
```

---

```markdown
---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature] UI-002: 페이크도어 랜딩페이지 UI 및 이메일 수집 폼 구현"
labels: 'feature, frontend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [UI-002] 페이크도어 랜딩페이지(Landing Page) UI 및 이메일 수집 폼 구현
- 목적: MVP Phase 0 가설 검증을 위해 '부상 방지' 가치를 소구하는 랜딩페이지를 띄우고, 잠재 고객의 이메일을 수집하여 CPA 및 CVR 지표를 측정한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: `/docs/SRS_v0.3.md#6.4 Phase 0: 페이크도어 검증`

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 히어로 섹션(Hero Section) 카피라이팅 및 배경 비주얼 컴포넌트 렌더링
- [ ] 문제 제기(Pain Point) 및 해결책(WOMBET2 Solution) 소개 뷰 구현
- [ ] 이메일 입력 폼(Form) 컴포넌트 및 정규식 기반 유효성 검사 로직 구현
- [ ] 이메일 제출 시 임시 API 연동(또는 Firebase 등 연동) 및 성공 모달(Thank you) 구현
- [ ] Meta Pixel 및 Google Analytics 이벤트 트래킹 코드 삽입 (CPA 측정용)

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 유효한 이메일 제출
- Given: 사용자가 랜딩페이지 폼에 유효한 이메일(`test@runner.com`)을 입력함
- When: '사전 알림 받기' 버튼을 클릭함
- Then: 이메일 데이터가 서버에 전송되고, 완료 모달창이 표시된다.

Scenario 2: 잘못된 이메일 형식 입력
- Given: 사용자가 폼에 잘못된 형식(`testrunner.com`)을 입력함
- When: 폼을 제출하거나 포커스를 잃음(blur)
- Then: 인라인 에러 메시지("올바른 이메일 형식을 입력해주세요")가 붉은색으로 노출되며 전송이 차단된다.

## :gear: Technical & Non-Functional Constraints
- 보안: 봇(Bot) 스팸 방지를 위한 단순 허니팟(Honeypot) 필드 또는 쓰로틀링 적용
- 성능: 모바일 환경에서 이탈을 방지하기 위해 TTI(Time To Interactive) ≤ 2초 보장

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 입력 폼에 대한 단위 테스트가 작성되었는가?
- [ ] 이벤트 트래킹(CPA 트래킹용)이 정상적으로 트리거되는지 개발자 도구로 확인했는가?

## :construction: Dependencies & Blockers
- Depends on: #UI-001 (기본 디자인 토큰)
- Blocks: 없음 (단독 검증 파이프라인)
```

---

```markdown
---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature] UI-003: 제품 리스트 페이지(PLP) 기본 골격 및 검색/카테고리 뼈대 구현"
labels: 'feature, frontend, priority:medium'
assignees: ''
---

## :dart: Summary
- 기능명: [UI-003] 제품 리스트 페이지(PLP) 기본 골격 구현
- 목적: 사용자가 목적에 맞는 신발을 찾을 수 있도록 제품 목록 그리드와 카테고리 필터링 골격을 제공하여 향후 '상대성 태깅(F3)' 기능이 연동될 기반을 마련한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: `/docs/SRS_v0.3.md#3.4 시퀀스 다이어그램 2`
- 관련 기능: `[F3] 상대성 태깅`

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 스포츠 종목/카테고리 선택을 위한 필터 바(Dropdown / Tabs) 컴포넌트 구현
- [ ] 개별 제품의 썸네일 카드 컴포넌트(이미지, 브랜드, 모델명, 기본 평점 Placeholder) 구현
- [ ] 반응형 제품 목록 그리드 레이아웃(CSS Grid) 구축
- [ ] 상태(목록 로딩 중, 데이터 없음) 표시에 대한 UI 구현

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 리스트 렌더링
- Given: 카테고리 페이지에 접근함
- When: 상품 목록 API가 성공적으로 데이터를 반환함
- Then: 모바일에서는 1열 또는 2열, 데스크톱에서는 3열 이상의 그리드로 상품 카드가 노출된다.

Scenario 2: 카테고리 필터 선택
- Given: 리스트가 렌더링된 상태
- When: 사용자가 상단 필터에서 '역도'를 선택함
- Then: 쿼리 파라미터(URL)가 업데이트되며 데이터 로딩 스피너가 표시된다.

## :gear: Technical & Non-Functional Constraints
- 성능: 리스트 렌더링 시 이미지 레이지 로딩(Lazy Loading) 필수 적용

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 썸네일 카드의 Storybook UI 테스트 또는 단위 테스트가 추가되었는가?
- [ ] 반응형 그리드가 화면 크기에 맞게 정상 동작하는가?

## :construction: Dependencies & Blockers
- Depends on: #UI-001
- Blocks: #F3-004 (종목 맞춤형 뷰 렌더링 연동)
```

---

```markdown
---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature] UI-004: 제품 상세 페이지(PDP) 기본 정보 영역 렌더링"
labels: 'feature, frontend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [UI-004] 제품 상세 페이지(PDP) 기본 정보 영역 렌더링
- 목적: 개별 제품의 상세 정보(이미지, 가격, 스펙)를 표시하고, 핵심 MVP 기능인 네거티브 필터(F1)와 핏 진단(F2)이 마운트될 컨테이너 슬롯(Slot)을 확보한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: `/docs/SRS_v0.3.md#3.4 시퀀스 다이어그램 1`

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 상단 제품 이미지 슬라이더/갤러리 컴포넌트 구현
- [ ] 브랜드 명, 제품 명, 가격 정보 렌더링
- [ ] [중요] 최상단 붉은색 경고 박스(F1-004)가 주입될 Slot 영역 예약 및 배치
- [ ] 제품 스펙 테이블(무게, 드롭 등) 및 상세 설명 영역 레이아웃 구현
- [ ] 핏 진단 결과(F2) 오버레이가 렌더링 될 이미지 컨테이너 영역 구성

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 상세 페이지 렌더링
- Given: 유효한 `product_id`로 상세 페이지에 접근함
- When: 페이지가 렌더링됨
- Then: 제품의 이미지, 브랜드, 가격, 스펙이 화면에 정상적으로 표시된다.

Scenario 2: 데이터 로딩 상태
- Given: 상세 데이터 패칭 중임
- When: 데이터를 기다리는 동안
- Then: 이미지와 텍스트 영역에 Skeleton UI가 표시되어 레이아웃 시프트를 방지한다.

## :gear: Technical & Non-Functional Constraints
- 성능: CLS(Cumulative Layout Shift) 점수 0.1 이하 유지 (이미지 및 Slot 영역 높이 사전 할당)

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 제품 정보가 없는 경우 404 페이지로 리다이렉트 되는가?
- [ ] 기능 결합을 위한 Children 컴포넌트 Slot 구조가 잘 설계되었는가?

## :construction: Dependencies & Blockers
- Depends on: #UI-001
- Blocks: #F1-004 (네거티브 경고 UI), #F2-005 (핏 히트맵 오버레이), #UI-005, #UI-006
```

---

```markdown
---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature] UI-005: 제휴 구매 전환(CTA) 버튼 및 안전 리다이렉션 로딩 화면 구현"
labels: 'feature, frontend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [UI-005] 제휴 구매 전환(CTA) 버튼 및 안전 리다이렉션 로딩 화면 구현
- 목적: 아마존 등 제휴 몰로 전환되는 핵심 CTA 버튼을 배치하고, 서버 통신(안전 링크 발급 등) 대기 시간 동안 사용자가 이탈하지 않도록 시각적 피드백을 제공한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev 시퀀스
- SRS 문서: `/docs/SRS_v0.3.md#3.4 시퀀스 다이어그램 1` (제휴 URL 발급 요청 파트)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 모바일 하단 고정(Sticky Bottom) 구매/최저가 확인 버튼(CTA) 컴포넌트 구현
- [ ] 버튼 클릭 시 제휴 링크 API 응답을 대기하는 동안의 Spinner/Loading 오버레이 뷰 구현
- [ ] F4-001(안전 진단 경고 팝업)을 트리거하기 위한 클릭 이벤트 인터셉터 로직 연동 지점 마련
- [ ] 링크 발급 실패 시(예외 처리) 에러 토스트(Toast) 알림 구현

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 리다이렉션 대기 상태
- Given: 제품 상세 페이지에서 구매 전환 CTA가 활성화됨
- When: 사용자가 버튼을 클릭함 (안전 경고 조건 없음 가정)
- Then: 화면 중앙에 "안전한 제휴 링크를 생성 중입니다..." 로딩 UI가 렌더링되며, 응답 후 외부로 리다이렉트 된다.

## :gear: Technical & Non-Functional Constraints
- UX: 로딩 뷰는 기존 화면을 딤(Dim) 처리하고 화면 중앙을 점유하여 중복 클릭을 방지해야 함.

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 중복 클릭(Debounce/Throttle 방어) 방지 처리가 되었는가?

## :construction: Dependencies & Blockers
- Depends on: #UI-004
- Blocks: #F4-001 (경고 오버레이)
```

---

```markdown
---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature] UI-006: 핏 진단 비동기 상태 표시 UI (대기 중/완료)"
labels: 'feature, frontend, priority:medium'
assignees: ''
---

## :dart: Summary
- 기능명: [UI-006] 핏 진단 수동 처리에 따른 비동기 상태 표시 UI
- 목적: 관리자가 수동으로 처리하여 최대 2시간이 소요되는 핏 진단(Match DNA) 과정에서, 사용자가 혼란을 겪지 않도록 직관적인 상태(대기/완료) UI를 제공한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev 시퀀스
- SRS 문서: `/docs/SRS_v0.3.md#6.3 Detailed Interaction Models`

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 사진 업로드 직후 사진 썸네일 위에 "진단 대기 중 (최대 2시간 소요)" 상태 배지 및 딤(Dim) 처리 UI 구현
- [ ] 상태 체크용 서버 폴링(Polling) Hook 또는 컴포넌트 렌더링 로직 연동
- [ ] 진단 상태가 '완료(Completed)'로 변경될 시, 상태 배지를 제거하고 "결과 확인하기" 활성 버튼 렌더링
- [ ] 카카오톡/SMS 알림 발송 전, 웹 내에서의 상태값 갱신 테스트

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 진단 대기 중 상태 렌더링
- Given: 사용자가 핏 진단용 발 사진 업로드를 완료함
- When: API가 201 Created를 반환함
- Then: 즉시 해당 영역에 "전문가 진단 중(SLA 2시간)"이라는 시각적 배지가 노출된다.

Scenario 2: 진단 완료 상태 변경
- Given: 상태가 '대기 중'인 화면
- When: 서버로부터 '완료' 상태 응답을 수신함
- Then: 화면이 리렌더링되어 결과(히트맵) 오버레이를 볼 수 있는 버튼이 활성화된다.

## :gear: Technical & Non-Functional Constraints
- 최적화: 폴링(Polling) 주기는 서버 부하를 고려해 MVP 단계에서는 30초~1분 간격으로 설정하거나, 수동 새로고침 기반으로 동작하도록 유연하게 설계.

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 대기 상태 UI가 사용자에게 충분히 인지될 수 있도록 디자인되었는가?

## :construction: Dependencies & Blockers
- Depends on: #UI-004, #F2-001
- Blocks: 없음
```
