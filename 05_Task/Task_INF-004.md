---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature] INF-004: B2B 파일럿용 API 인증 체계 및 Rate Limiting 구축"
labels: 'feature, infra, priority:medium'
assignees: ''
---

## :dart: Summary
- 기능명: [INF-004] B2B 파일럿 매장용 API 인증(API Key) 체계 및 Rate Limiting(호출 제한) 미들웨어 구축
- 목적: B2B 제휴 매장에 핏 진단 API를 제공함에 있어, 무분별한 남용을 막고 SLA(월 1만 건) 기반의 인프라 비용 통제를 수행하기 위한 방어벽을 설치한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: `/docs/SRS_v0.3.md#3.3 API Overview` ("월 $2,500/매장, 호출 제한 10K건/월")

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] B2B 파트너사를 식별할 수 있는 API Key 발급 및 검증용 미들웨어(Interceptor/Guard) 로직 구현
- [ ] Redis 또는 인메모리 스토어를 활용하여 API Key별 호출 횟수를 카운팅하는 Rate Limiter 연동 로직
- [ ] 월 한도(10,000건) 도달 시 429 에러 반환 및 에러 포맷팅 처리
- [ ] Rate Limit 한도에 근접(예: 90%) 시 관리자(Slack)에게 경고를 보내는 모니터링 연계(옵션)

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 정상 API Key를 이용한 호출
- Given: 발급된 유효한 B2B API Key가 헤더(`X-API-Key`)에 포함되어 있음
- When: 핏 진단 API(`/api/v1/fit/diagnosis`)를 호출함
- Then: 인증을 통과하여 비즈니스 로직이 수행되며, 해당 키의 카운트가 1 증가한다.

Scenario 2: 월 단위 호출 한도 초과
- Given: 이번 달 호출 횟수 10,000회를 모두 소진한 API Key가 헤더에 포함됨
- When: API를 호출함
- Then: 비즈니스 로직 진입 전 미들웨어에서 차단되며, 429 Too Many Requests 상태 코드와 한도 초과 에러 메시지가 즉시 반환된다.

## :gear: Technical & Non-Functional Constraints
- 성능: 인증 및 카운팅 처리 과정이 원래의 API 응답 지연 시간(Latency)에 +10ms 이상 영향을 주지 않도록 경량화 필수 (Redis 캐시 권장).
- 보안: API Key는 DB 저장 시 평문이 아닌 안전한 해시 형태로 저장.

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 통합 테스트 시 Rate Limiting 초과 차단 로직이 검증되었는가?
- [ ] 파트너사에 제공할 API Key 연동 가이드(Swagger 문서 등)가 명확히 업데이트되었는가?

## :construction: Dependencies & Blockers
- Depends on: #F2-003 (B2B용 핏 진단 결과 제공 로직)
- Blocks: 본격적인 외부 B2B 시스템 연동
