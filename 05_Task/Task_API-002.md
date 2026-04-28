---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[API Spec] API-002: 상대성 태깅 (F3) 스펙 역산 Request/Response DTO 및 예외 코드 정의"
labels: 'api, backend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [API-002] 스펙 역산 API Contract 정의
- 목적: 사용자가 특정 종목(예: 역도)을 선택했을 때, 이종 스펙 오염을 배제하고 역산된 제품 리스트를 반환하기 위한 데이터 통신 규약을 확정한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`6.1 API Endpoint List`](#)
- 요구사항: [`4.1 REQ-FUNC-004, REQ-FUNC-005, REQ-FUNC-007`](#)
- 시퀀스 다이어그램: [`3.4 Interaction Sequences - 시퀀스 2`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 엔드포인트 `/api/v1/products/tags` Request DTO 정의 (`sport_type`, `category` 파라미터 등)
- [ ] Response DTO 정의 (제품 리스트 배열, `converted_score`, `contamination_warning` 플래그 등)
- [ ] 미지원 종목 요청 시의 예외 규약(예: 200 OK + `is_fallback=true` 플래그 및 기본 리스트 반환) 정의
- [ ] 이종 스펙 혼입 경고(Filter Out) 시 반환할 메타데이터 구조 정의
- [ ] OpenAPI(Swagger) 또는 API Blueprint 문서화 작성

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: API 문서화 확인
- Given: DTO 및 쿼리 파라미터 스키마가 작성됨
- When: Swagger UI에 접속하여 API 명세를 확인 시
- Then: `sport_type` 쿼리 파라미터가 필수값인지, 미지원 시 어떻게 응답하는지가 명확한 JSON 스키마로 노출된다.

## :gear: Technical & Non-Functional Constraints
- 성능: 프론트엔드에서 리스트를 렌더링할 때 지연이 없도록 페이로드 구조를 플랫(Flat)하게 설계할 것.
- 컨벤션: RESTful API 원칙 준수

## :checkered_flag: Definition of Done (DoD)
- [ ] 프론트엔드 개발자가 혼동 없이 UI를 그릴 수 있는 수준의 상세한 JSON Mock 구조가 정의되었는가?
- [ ] 지원/미지원 종목에 따른 응답 플래그(`is_fallback` 등)가 명확히 포함되어 있는가?

## :construction: Dependencies & Blockers
- Depends on: None
- Blocks: #API-004, #F3-001, #F3-005
