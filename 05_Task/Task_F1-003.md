---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Test] F1-003: 단점 데이터 누락/응답 지연 시 예외 처리(Fallback) 단위 테스트 작성"
labels: 'test, backend, priority:medium'
assignees: ''
---

## :dart: Summary
- 기능명: [F1-003] 네거티브 필터 예외/지연 테스트
- 목적: 제품의 단점 데이터가 없거나, DB 장애로 인해 응답이 지연될 경우 시스템이 안전하게 Fallback 처리되는지 시스템적으로 검증한다.

## :link: References (Spec & Context)
- SRS 문서: [`4.1 REQ-FUNC-003`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] `NegativeFilterServiceTest` 클래스 생성 및 Mocking 프레임워크 설정
- [ ] DB 타임아웃 상황을 시뮬레이션하는 단위 테스트 로직 작성
- [ ] 데이터 누락 시 빈 배열 혹은 "분석 중" 상태 코드를 반환하는지 테스트 작성
- [ ] 예외 발생 시 시스템 어드민에게 알림을 보내는 서비스가 1회 호출되는지 Verify(Spy) 테스트 작성

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 데이터베이스 응답 타임아웃
- Given: DB 쿼리가 3초 이상 지연되는 Mock 환경
- When: 서비스 로직에서 단점 데이터를 요청함
- Then: 타임아웃 예외가 Catch되어 200 OK와 함께 "현재 단점 분석 중" 상태를 의미하는 Fallback DTO를 반환한다. (장애 전파 방지)

Scenario 2: 어드민 알림 전송 로직 트리거
- Given: 단점 데이터가 누락된 제품 조회 요청
- When: 서비스 로직이 실행됨
- Then: `NotificationService.sendAdminAlert()` 메서드가 백그라운드에서 정확히 1번 호출되었음을 검증한다.

## :gear: Technical & Non-Functional Constraints
- 안정성: 테스트 실행 시 외부 DB에 의존하지 않고 100% Mocking 되어야 함.

## :checkered_flag: Definition of Done (DoD)
- [ ] 서비스 로직의 예외 처리(Circuit Breaker 또는 Try-Catch) 브랜치 커버리지가 100%인가?
- [ ] CI 파이프라인에서 해당 테스트가 Green 상태를 유지하는가?

## :construction: Dependencies & Blockers
- Depends on: #F1-001
- Blocks: None
