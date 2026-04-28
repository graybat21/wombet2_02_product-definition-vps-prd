---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Monitoring] INF-003: 5xx 에러(> 0.5%) 및 제휴 CVR(< 3%) 저하 감지 시 Slack/PagerDuty 알림 연동"
labels: 'monitoring, infra, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [INF-003] 운영 지표 모니터링 및 Alert 연동
- 목적: 시스템 신뢰도 및 북극성 비즈니스 지표(CVR)의 치명적인 저하를 즉시 감지하고, 관련 엔지니어와 기획자에게 알림을 발송하여 신속한 대응을 돕는다.

## :link: References (Spec & Context)
> :bulb: AI 기획 및 개발 가이드: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`4.2 REQ-NF-006, REQ-NF-012, REQ-NF-013`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 스프링 부트 Actuator/Prometheus 또는 Datadog/Sentry 등 모니터링 툴 통합(Integration) 설정
- [ ] 에러율 감지 룰셋 구성: 최근 5분 내 5xx HTTP 에러율이 전체 요청의 0.5%를 초과할 경우 Alert 트리거
- [ ] CVR 감지 룰셋 구성: 제휴 클릭 로깅 데이터 집계 기반 CVR이 3% 미만으로 2일 연속 유지될 경우 Alert 트리거(배치 작업)
- [ ] Slack Webhook 및 PagerDuty API를 통한 경고 발송 파이프라인 구현

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 서버 에러율 폭증 알림
- Given: 모니터링 메트릭 상 5xx 에러가 임계치 0.5%를 초과하는 데이터가 인입됨
- When: Alert Manager가 해당 룰셋을 감지함
- Then: 지정된 Slack 채널로 `[CRITICAL] 5xx 에러율 0.5% 초과 감지` 메시지와 함께 에러 로그 링크를 발송한다.

Scenario 2: 비즈니스 KPI 미달 경고
- Given: 배치 통계 쿼리 실행 결과, 48시간 기준 Affiliate CVR이 2.5%로 계산됨
- When: CVR 스케줄러가 검증 로직을 실행함
- Then: Slack의 기획/비즈니스 채널로 "비즈니스 지표 경고: CVR 3% 미만으로 하락" 알림을 전송한다.

## :gear: Technical & Non-Functional Constraints
- 안정성: 모니터링과 알림 발송 로직 자체가 비즈니스 메인 서버에 부하를 주지 않도록 시스템 자원을 분리/격리 구성 권장.

## :checkered_flag: Definition of Done (DoD)
- [ ] 슬랙 Webhook 연동을 통한 정상 발송 테스트가 성공했는가?
- [ ] 임계치 기반의 룰셋 조건(5xx > 0.5%)이 설정 파일에 명확히 기재되어 있는가?

## :construction: Dependencies & Blockers
- Depends on: #F4-003 (CVR 산출용 로그)
- Blocks: 시스템 운영 안정화
