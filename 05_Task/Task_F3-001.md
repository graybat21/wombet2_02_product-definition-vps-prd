---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature/Query] F3-001: 선택 종목 기반 범용 스펙 역산 변환 및 순위 반환 로직 구현"
labels: 'feature, backend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [F3-001] 상대성 태깅 역산 및 리스트 갱신
- 목적: 획일적 평점의 문제를 해결하기 위해, 사용자가 특정 운동 종목을 선택하면 해당 목적에 맞게 기존 제품 스펙의 점수를 역산(변환)하여 맞춤형 랭킹 리스트를 제공한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`4.1 REQ-FUNC-004`](#)
- 시퀀스 다이어그램: [`3.4 Interaction Sequences - 시퀀스 2`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] `RelativityTaggingEngine` 도메인 서비스 클래스 생성 및 스코어 역산 알고리즘(수식) 구현
- [ ] `ProductRepository`와 조인하여 `sport_type`에 매칭되는 `RELATIVITY_TAG`의 `converted_score` 기반 정렬 로직 구현
- [ ] API Controller (`GET /api/v1/products/tags?sport_type={type}`) 연결
- [ ] 이종 스펙(예: 역도인데 쿠셔닝 높은 제품)에 대한 플래그(`contamination_warning`) 맵핑

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 특정 종목(역도) 맞춤 리스트 조회
- Given: 쿠셔닝 점수가 높은 신발(A)과 딱딱한 신발(B)이 있음
- When: `sport_type=weightlifting`(역도) 파라미터로 제품 리스트를 요청함
- Then: 점수 역산 로직이 적용되어 딱딱한 신발(B)이 상위 랭킹에 표시된 응답 객체를 반환한다.

## :gear: Technical & Non-Functional Constraints
- 성능: 수많은 제품 데이터의 실시간 역산이 부하를 줄 수 있으므로, 종목별 변환 스코어 결과는 캐싱(Redis 등) 또는 사전에 배치 스크립트로 계산되어 `RELATIVITY_TAG`에 저장되어야 함 (응답 p95 ≤ 1,000ms 유지).

## :checkered_flag: Definition of Done (DoD)
- [ ] 변환 알고리즘 로직에 대한 단위 테스트가 작성되었는가? (수학적 정확도 검증)
- [ ] 종목을 변경함에 따라 반환되는 리스트의 정렬 순서가 동적으로 달라지는가?

## :construction: Dependencies & Blockers
- Depends on: #DB-002, #API-002
- Blocks: #F3-003, #F3-004
