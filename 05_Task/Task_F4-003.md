---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature/Command] F4-003: 리다이렉트 구매 건의 핏 불일치 반품 추적용 로깅 데이터 적재 (B2B 연동용)"
labels: 'feature, backend, priority:medium'
assignees: ''
---

## :dart: Summary
- 기능명: [F4-003] 제휴 클릭 및 반품 추적용 로그 적재
- 목적: 고객이 특정 핏 진단 결과를 확인한 후 제휴몰로 이동한 이력을 DB에 로깅하여, 핏 불일치로 인한 반품률(KPI)이 실제로 감소하는지 향후 측정할 기반 데이터를 마련한다.

## :link: References (Spec & Context)
> :bulb: AI 기획 및 개발 가이드: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`4.1 REQ-FUNC-009`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] F4-002의 URL 발급 API 로직 내부에 로깅 서비스(`AffiliateLogService`) 연동
- [ ] 로깅 데이터 스키마 정의 (`AFFILIATE_CLICK_LOG` 테이블 생성 혹은 ELK 연동)
- [ ] 데이터 컬럼 구성: `session_id`, `product_id`, `diagnosis_id`(있을 경우), `risk_verdict_viewed`, `clicked_at`
- [ ] 사용자의 페이지 전환 속도에 영향을 주지 않도록 로깅 로직을 비동기 이벤트 큐(Spring Event, @Async 등)로 분리 처리

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 구매 리다이렉션 로그 비동기 적재
- Given: 진단 결과(`High`)를 본 사용자가 구매 버튼을 클릭하여 URL 발급을 요청함
- When: 컨트롤러가 리다이렉트 URL을 반환하기 직전 이벤트를 발행함
- Then: 클라이언트는 지연 없이 URL을 응답받고, 백그라운드 스레드에서 DB에 클릭 로그(`risk_verdict_viewed: High`)가 정상 적재된다.

## :gear: Technical & Non-Functional Constraints
- 성능: DB 쓰기 작업이 API 응답을 지연(Blocking)시켜서는 안 됨.
- 확장성: 향후 Google Analytics 4 / Meta Pixel 서버사이드 연동을 위한 이벤트 구조로 확장 가능하도록 설계.

## :checkered_flag: Definition of Done (DoD)
- [ ] 비동기 이벤트 발행 시 메인 로직과 분리되어 실패하더라도 URL 발급에는 영향을 주지 않는가? (단위 테스트 작성)
- [ ] `AFFILIATE_CLICK_LOG` 적재 테스트가 통과하는가?

## :construction: Dependencies & Blockers
- Depends on: #F4-002
- Blocks: None
