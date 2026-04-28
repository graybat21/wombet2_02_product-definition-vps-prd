---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature/Command] F3-002: 종목 리스트 부정확 피드백 신고 접수 및 보정 검증 트리거 연동"
labels: 'feature, backend, priority:medium'
assignees: ''
---

## :dart: Summary
- 기능명: [F3-002] 역산 데이터 피드백 접수 API
- 목적: 상대성 태깅 데이터(F3)에 대해 전문/일반 유저가 '부정확' 피드백을 보낼 경우 이를 접수하고, 시스템 관리자가 48시간 내 검증하도록 트리거를 발생시킨다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`4.1 REQ-FUNC-006`](#)
- 시퀀스 다이어그램: [`3.4 Interaction Sequences - 시퀀스 2 (opt 영역)`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 피드백 접수를 위한 엔드포인트 `POST /api/v1/feedbacks/tags` 컨트롤러 구현
- [ ] Request DTO 정의 (`product_id`, `sport_type`, `reason_text` 등)
- [ ] 피드백 데이터를 저장할 테이블(예: `TAG_FEEDBACK_LOG`) 마이그레이션 스크립트 작성 (DB-001~003에 없었으므로 여기에서 소규모 스키마 추가)
- [ ] 검증 SLA 48시간 체크를 위한 어드민 알림 발송 모듈(Slack Webhook 등) 연동

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 부정확 피드백 접수
- Given: 유저가 랭킹 리스트에서 특정 신발의 점수가 부정확하다는 피드백 내용을 작성함
- When: 피드백 접수 API를 호출함
- Then: DB에 피드백 상태가 `PENDING`으로 저장되며 201 Created 응답이 내려가고, 관리자 채널(Slack 등)로 검증 트리거 알림이 발송된다.

## :gear: Technical & Non-Functional Constraints
- 남용 방지: 동일 사용자의 연속된 무의미한 신고를 방지하기 위해 간단한 Rate Limiting(예: 1시간당 3회 제한)을 적용.

## :checkered_flag: Definition of Done (DoD)
- [ ] 피드백 데이터가 테이블에 적재되고 알림 모듈(Mock 가능)이 호출되는 단위/통합 테스트가 통과하는가?
- [ ] 에러 발생 시 사용자에게 명확한 에러 코드(429 Too Many Requests 등)가 반환되는가?

## :construction: Dependencies & Blockers
- Depends on: #API-002
- Blocks: None
