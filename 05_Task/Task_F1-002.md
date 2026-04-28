---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature/Command] F1-002: 단점 경고 박스 인터랙션 시 체류 시간 로깅 API 구현"
labels: 'feature, backend, priority:medium'
assignees: ''
---

## :dart: Summary
- 기능명: [F1-002] 단점 경고 체류 시간 기록
- 목적: 단점 최우선 노출(F1)이 사용자의 탐색 행동에 긍정적인 영향(체류 시간 증가)을 미치는지 정량적으로 검증하기 위해, UI 인터랙션 체류 시간을 기록한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`4.1 REQ-FUNC-002`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 클라이언트의 로깅 이벤트를 수신할 `POST /api/v1/analytics/dwell-time` API 엔드포인트 구현
- [ ] DTO 정의 (`product_id`, `session_id`, `dwell_time_ms`, `event_type` 등)
- [ ] 전달받은 로그 데이터를 비동기적으로 DB(`USER_ACTIVITY_LOG` 등) 또는 로그스태시/분석 파일로 저장하는 Service 계층 로직 작성
- [ ] 대량의 트래픽을 고려하여 로깅 시 메인 스레드를 블로킹하지 않도록 이벤트 비동기 처리(Message Queue 또는 `@Async`) 적용

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 정상적인 체류 시간 로깅
- Given: 클라이언트가 8000ms(8초) 동안 단점 박스에 머물렀다는 데이터 페이로드가 주어짐
- When: 로깅 API로 POST 요청을 보냄
- Then: 202 Accepted 응답이 즉시 반환되며, 비동기적으로 데이터베이스나 분석 로그에 해당 체류 시간이 정확히 기록된다.

## :gear: Technical & Non-Functional Constraints
- 성능: 로깅 API 응답 시간은 비즈니스 로직에 영향을 주지 않아야 하므로 극도로 짧아야 함 (p95 ≤ 50ms 권장).
- 통신: 프론트엔드의 `sendBeacon()` API 요청 포맷을 고려한 호환성 확보.

## :checkered_flag: Definition of Done (DoD)
- [ ] 로깅 데이터가 DB(혹은 지정된 로깅 시스템)에 유실 없이 저장되는가?
- [ ] 동시성 테스트(Concurrency Test) 시 메인 DB 커넥션 풀을 고갈시키지 않는가?

## :construction: Dependencies & Blockers
- Depends on: #API-001
- Blocks: None
