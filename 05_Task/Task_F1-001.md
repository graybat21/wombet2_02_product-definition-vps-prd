---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature/Query] F1-001: 제품 식별자 기반 최우선 단점(Penalty) 3가지 조회 로직 구현"
labels: 'feature, backend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [F1-001] 네거티브 필터 데이터 조회 (Read)
- 목적: 클라이언트의 요청 시 DB에서 특정 제품의 치명적인 단점 데이터를 빠르게 조회하여 반환한다.

## :link: References (Spec & Context)
- SRS 문서: [`4.1 REQ-FUNC-001`](#)
- 연관 API: `/api/v1/products/{id}/penalties` (API-001)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] `NegativeFilterRepository`에 `findPenaltiesByProductId` 쿼리 메서드 구현
- [ ] `NegativeFilterService` 비즈니스 로직 작성 (데이터 가공 및 DTO 맵핑)
- [ ] `ProductController`에 GET 엔드포인트 연동
- [ ] 데이터베이스 인덱스(`product_id`) 설정 검토 (성능 최적화용)

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 존재하는 제품의 단점 조회
- Given: DB에 단점 데이터가 등록된 제품 ID(`PROD-123`)가 주어짐
- When: `/api/v1/products/PROD-123/penalties`로 GET 요청함
- Then: 200 OK 상태와 함께 치명적 단점 3가지가 포함된 JSON을 반환한다.

Scenario 2: 등록되지 않은 제품 조회
- Given: DB에 존재하지 않는 제품 ID(`PROD-999`)가 주어짐
- When: 해당 ID로 조회 요청함
- Then: 404 Not Found 상태와 명세된 에러 메시지를 반환한다.

## :gear: Technical & Non-Functional Constraints
- 성능: 해당 API의 응답 속도 p95 ≤ 500ms 충족 (DB 조회 쿼리 최적화)
- 보안: SQL 인젝션 방어 처리 (ORM 또는 Parameterized Query 사용 필수)

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 단위/통합 테스트가 작성되었고 성공하는가?
- [ ] 응답 지연 발생을 방지하는 쿼리(인덱스 등)가 적용되었는가?

## :construction: Dependencies & Blockers
- Depends on: #DB-001, #API-001
- Blocks: #F1-003, #F1-004
