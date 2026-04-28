---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[DB] DB-003: USER_PROFILE, FIT_DIAGNOSIS 테이블 스키마 및 마이그레이션 스크립트 작성"
labels: 'database, backend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [DB-003] 유저 프로필 및 핏 진단 기록 테이블 마이그레이션
- 목적: 고객의 발 사이즈 실측치 데이터와 히트맵 핏 진단(F2) 요청/결과 기록을 저장하기 위한 테이블을 구축한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`6.2 Entity & Data Model`](#)
- 데이터 모델 (ERD): [`6.2 Table Schema - USER_PROFILE, FIT_DIAGNOSIS`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] `USER_PROFILE` 테이블(user_id, left_foot_mm, right_foot_mm, primary_sport 등) DDL 스크립트 작성
- [ ] `FIT_DIAGNOSIS` 테이블(diagnosis_id, user_id, product_id, heatmap_overlay, risk_verdict 등) DDL 스크립트 작성
- [ ] `FIT_DIAGNOSIS`의 외래키 설정 (`user_id` -> USER_PROFILE, `product_id` -> PRODUCT)
- [ ] 처리 SLA 추적을 위한 `created_at` 및 `response_time_hours` 필드 타입(Datetime, Float) 정의
- [ ] Flyway / Prisma 등을 이용한 Up/Down 마이그레이션 스크립트 작성

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 스키마 생성 및 JSON 필드 검증
- Given: `PRODUCT` 테이블이 세팅된 데이터베이스 환경
- When: 마이그레이션 Up 스크립트를 실행함
- Then: `USER_PROFILE`과 `FIT_DIAGNOSIS` 테이블이 정상 생성되며, 히트맵 오버레이 필드가 JSON 객체를 수용할 수 있다.

Scenario 2: 마이그레이션 롤백
- Given: 모든 테이블이 최신 상태로 적용됨
- When: 마이그레이션 Down 스크립트를 실행함
- Then: 외래키 충돌 없이 `FIT_DIAGNOSIS`가 먼저 지워지고 `USER_PROFILE`이 삭제된다.

## :gear: Technical & Non-Functional Constraints
- 보안: `USER_PROFILE`에 저장되는 발 사이즈 등 PII 데이터는 향후 폐기 및 마스킹을 고려한 독립적 관리를 염두에 둘 것.
- 컨벤션: snake_case 컬럼명 준수

## :checkered_flag: Definition of Done (DoD)
- [ ] ERD에 명시된 테이블 구조 및 제약조건(Not Null 등)이 완벽히 반영되었는가?
- [ ] 로컬 DB에서 마이그레이션 Up/Down이 에러 없이 실행되는가?

## :construction: Dependencies & Blockers
- Depends on: #DB-001 (PRODUCT 테이블 필요)
- Blocks: #F2-001, #F2-002
