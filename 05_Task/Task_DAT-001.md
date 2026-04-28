---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature] DAT-001: MVP 론칭용 초기 데이터 시딩(Seeding) 스크립트 작성"
labels: 'feature, data, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [DAT-001] MVP 론칭용 초기 데이터 시딩(Seeding) 스크립트 작성
- 목적: MVP 단계에서 정의된 타겟 제품 30~50종과 단점/태깅 데이터를 환경(Dev/Prod) 배포 시 일괄 주입하여, 빈 화면 없이 즉시 테스트 및 서비스가 구동되도록 한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: `/docs/SRS_v0.3.md#1.2 Scope` ("수동 DB 30~50종" 관련)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] MVP 타겟 제품 30여 종의 기본 스펙 명세 (JSON 또는 CSV 형태 포맷팅)
- [ ] 상기 제품에 대응하는 초기 네거티브 필터(단점 3요소) 및 상대성 태깅 기본값 데이터 파일 구성
- [ ] ORM 또는 DB 드라이버를 활용한 Seeding 스크립트 작성 (예: `npm run seed`)
- [ ] 스크립트 재실행 시 데이터 중복 방지를 위한 멱등성(Idempotency) 보장 로직 구현 (Upsert 또는 Clear 후 Insert)

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 빈 데이터베이스에 초기 시딩 실행
- Given: 데이터베이스 스키마만 존재하고 데이터가 비어 있는 환경
- When: 데이터 시딩 스크립트를 실행함
- Then: 지정된 30여 종의 제품(`PRODUCT`), 단점(`NEGATIVE_FILTER`), 태깅(`RELATIVITY_TAG`) 데이터가 정상 삽입된다.

Scenario 2: 데이터가 존재하는 환경에서 스크립트 재실행
- Given: 이미 초기 데이터가 삽입된 데이터베이스 환경
- When: 시딩 스크립트를 다시 한 번 실행함
- Then: 오류 없이 완료되며, 데이터가 중복으로 쌓이지 않고 최신 상태(Upsert)로 유지된다.

## :gear: Technical & Non-Functional Constraints
- 안정성: 시딩 중 일부 레코드 실패 시 전체를 롤백(Rollback)하는 트랜잭션 처리가 되어야 함.

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 스크립트 실행 명령어 및 방법이 README에 문서화되었는가?
- [ ] Local 환경에서 시딩된 데이터로 프론트엔드 UI 화면 구동이 확인되는가?

## :construction: Dependencies & Blockers
- Depends on: #DB-001, #DB-002 (테이블 스키마 생성 완비)
- Blocks: 전체 UI 테스트 및 QA
