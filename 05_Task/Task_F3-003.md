---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Test] F3-003: 이종 스펙 혼입 시 오염 차단(Filter Out) 및 예외 처리(신규 종목) 단위 테스트"
labels: 'test, backend, priority:medium'
assignees: ''
---

## :dart: Summary
- 기능명: [F3-003] 이종 스펙 필터링 및 예외 테스트 작성
- 목적: 상대성 태깅(F3) 로직이 기획 의도대로 이종 스펙 오염을 차단하고, 지원되지 않는 종목 요청 시 안정적으로 예외 처리(Fallback)를 하는지 검증한다.

## :link: References (Spec & Context)
- SRS 문서: [`4.1 REQ-FUNC-005, REQ-FUNC-007`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] `RelativityTaggingEngineTest` 테스트 클래스에 시나리오 케이스 추가 작성
- [ ] 이종 스펙(예: 역도 리스트에 쿠셔닝 스펙만 존재하는 제품) 데이터 Mocking 주입
- [ ] 해당 제품이 결과 리스트에서 누락되거나(Filter Out), 경고 플래그(`contamination_warning = true`)가 맵핑되는지 검증 로직 구현
- [ ] DB에 맵핑 로직이 없는 신규/기타 종목 파라미터(`sport_type=unknown`)로 조회 요청을 시뮬레이션
- [ ] 예외가 발생하지 않고 범용 스펙 리스트가 `is_fallback=true` 속성과 함께 반환되는지 검증

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 이종 스펙 오염 차단
- Given: 특정 종목(역도)에 절대 적합하지 않은 상극 스펙을 지닌 제품 DTO를 Mocking
- When: 스펙 역산 엔진으로 리스트를 처리함
- Then: 반환되는 리스트 크기에서 해당 제품이 제거(0%)되거나 오염 경고 필드가 명시되어 있어야 한다.

Scenario 2: 미지원 종목 조회 예외 처리
- Given: 미지원 종목명(예: `sport_type=climbing`)으로 데이터가 주어짐
- When: 리스트 조회 API 로직을 호출함
- Then: 500 에러를 뱉지 않고 200 상태 코드로 범용 리스트 원본 데이터와 "분석 중" 대체 UI를 위한 상태 플래그를 반환한다.

## :gear: Technical & Non-Functional Constraints
- 독립성: 모든 비즈니스 로직 테스트는 스프링 컨텍스트(또는 프레임워크 런타임) 없이 순수 단위 테스트(Unit Test)로 고속 실행되어야 한다.

## :checkered_flag: Definition of Done (DoD)
- [ ] 오염 차단 로직(Filter Out) 브랜치 커버리지가 100%인가?
- [ ] Fallback 리스트 반환 단위 테스트가 CI 환경에서 정상적으로 Green을 나타내는가?

## :construction: Dependencies & Blockers
- Depends on: #F3-001
- Blocks: None
