# WOMBET2 개발 태스크(Task) 추출 리스트

본 문서는 `SRS_v0.3_by_gemini.md`를 기반으로 `개발 TASK 추출 절차.md`의 핵심 원칙 4단계(1. Contract/Data 선추출, 2. Query/Command 분리, 3. Test 전환, 4. NFR 도출)에 따라 분해된 실행 가능한 개발 태스크 리스트입니다. 프론트엔드 UI/UX 작업과 백엔드/인프라 기능을 분리하여 도출하였습니다.

## 전체 개발 태스크 목록 (Task Breakdown)

| Task ID | Epic (도메인) | Feature (기능명) | 관련 SRS 섹션 | 선행 태스크 (Dependencies) | 복잡도 (H/M/L) |
|---|---|---|---|---|---|
| **DB-001** | Data & Contract | `[DB]` PRODUCT, NEGATIVE_FILTER 테이블 스키마 및 마이그레이션 스크립트 작성 | 6.2 Table Schema | None | L |
| **DB-002** | Data & Contract | `[DB]` RELATIVITY_TAG, MATCH_DNA 테이블 스키마 및 마이그레이션 스크립트 작성 | 6.2 Table Schema | DB-001 | L |
| **DB-003** | Data & Contract | `[DB]` USER_PROFILE, FIT_DIAGNOSIS 테이블 스키마 및 마이그레이션 스크립트 작성 | 6.2 Table Schema | None | L |
| **API-001** | Data & Contract | `[API Spec]` 네거티브 필터 (F1) 단점 조회 Request/Response DTO 및 예외 코드 정의 | 6.1 API Endpoint, 4.1 | None | L |
| **API-002** | Data & Contract | `[API Spec]` 상대성 태깅 (F3) 스펙 역산 Request/Response DTO 정의 | 6.1 API Endpoint, 4.1 | None | L |
| **API-003** | Data & Contract | `[API Spec]` Match DNA 핏 진단 (F2) Request/Response DTO 정의 | 6.1 API Endpoint, 4.1 | None | L |
| **API-004** | Data & Contract | `[Mock]` 프론트엔드 UI 개발용 F1, F2, F3 Mocking 데이터 및 가짜 엔드포인트 세팅 | 6.1 API Endpoint | API-001, API-002, API-003 | L |
| **F1-001** | Negative Filter (F1) | `[Feature/Query]` 제품 식별자 기반 최우선 단점(Penalty) 3가지 조회 로직 구현 | 4.1 REQ-FUNC-001 | DB-001, API-001 | M |
| **F1-002** | Negative Filter (F1) | `[Feature/Command]` 단점 경고 박스 인터랙션 시 체류 시간 로깅 API 구현 | 4.1 REQ-FUNC-002 | API-001 | L |
| **F1-003** | Negative Filter (F1) | `[Test]` 단점 데이터 누락/응답 지연 시 예외 처리(Fallback) 단위 테스트 작성 | 4.1 REQ-FUNC-003 | F1-001 | M |
| **F1-004** | Negative Filter (F1) | `[UI/UX]` 네거티브 필터 최상단 붉은색 경고 UI 및 데이터 누락 대체 UI 개발 | 4.1 REQ-FUNC-001, 003 | API-004 | M |
| **F3-001** | Relativity Tagging (F3) | `[Feature/Query]` 선택 종목(예: 역도) 기반 범용 스펙 역산 변환 및 순위 반환 로직 구현 | 4.1 REQ-FUNC-004 | DB-002, API-002 | H |
| **F3-002** | Relativity Tagging (F3) | `[Feature/Command]` 종목 리스트 부정확 피드백 신고 접수 및 보정 검증 트리거 연동 | 4.1 REQ-FUNC-006 | API-002 | M |
| **F3-003** | Relativity Tagging (F3) | `[Test]` 이종 스펙 혼입 시 오염 차단(Filter Out) 및 예외 처리(신규 종목) 단위 테스트 | 4.1 REQ-FUNC-005, 007| F3-001 | M |
| **F3-004** | Relativity Tagging (F3) | `[UI/UX]` 종목 맞춤형 리스트 뷰 렌더링 및 신규 종목 팝업 UI 컴포넌트 개발 | 4.1 REQ-FUNC-004, 007| API-004 | M |
| **F2-001** | Match DNA (F2) | `[Feature/Command]` 발 사진 업로드/데이터 검증 및 수동 진단용 큐(Queue) 등록 API 구현 | 4.1 REQ-FUNC-008, 6.3 | DB-003, API-003 | H |
| **F2-002** | Match DNA (F2) | `[Feature/Query]` 관리자 진단 완료(Risk Verdict, Heatmap JSON) 상태 폴링 및 결과 조회 로직 | 4.1 REQ-FUNC-008, 6.3 | DB-003, F2-001 | M |
| **F2-003** | Match DNA (F2) | `[Feature/Query]` B2B 고객 맞춤형 핏 리스크 검증 API (`/api/v1/fit/diagnosis`) 제공 로직 | 3.3, 6.3 Phase 2 | F2-002 | M |
| **F2-004** | Match DNA (F2) | `[Test]` 저화질/인식 불가 사진 업로드 시 400 에러 및 실패 단위 테스트 작성 | 4.1 REQ-FUNC-011 | F2-001 | L |
| **F2-005** | Match DNA (F2) | `[UI/UX]` 발 사진 업로드 프롬프트 및 산출된 히트맵 사진 위 오버레이 렌더링 UI | 4.1 REQ-FUNC-008 | API-004 | H |
| **F4-001** | Safety & Checkout (F4)| `[UI/UX]` 진단 고위험군 결제 진입 시 시각적 안전 진단 경고 팝업(Overlay) 렌더링 | 4.1 REQ-FUNC-010 | F2-005 | M |
| **F4-002** | Safety & Checkout (F4)| `[Feature/Query]` 쿠키 하이재킹이 방어된 Amazon 제휴 리다이렉트 URL 서버사이드 발급 API | 4.1 REQ-NF-010, 6.1 | None | M |
| **F4-003** | Safety & Checkout (F4)| `[Feature/Command]` 리다이렉트 구매 건의 핏 불일치 반품 추적용 로깅 데이터 적재 (B2B 연동용) | 4.1 REQ-FUNC-009 | F2-003, F4-002 | M |
| **INF-001**| Infra & DevOps (NFR) | `[Infra]` LCP 및 API 변환 속도 지표(p95 ≤ 1초) 보장을 위한 부하 테스트(k6) 스크립트 작성 | 4.2 REQ-NF-001~004| F1-001, F3-001 | M |
| **INF-002**| Infra & DevOps (NFR) | `[Sec]` 발 사진 PII 처리(CCPA/GDPR 준수) 및 원본 이미지 즉시 파기/난독화 파이프라인 구축 | 4.2 REQ-NF-009 | F2-001 | H |
| **INF-003**| Infra & DevOps (NFR) | `[Monitoring]` 5xx 에러(> 0.5%) 및 제휴 CVR(< 3%) 저하 감지 시 Slack/PagerDuty 알림 연동 | 4.2 REQ-NF-006, 012| F4-002 | M |
