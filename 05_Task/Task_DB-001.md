---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[DB] DB-001: PRODUCT, NEGATIVE_FILTER 테이블 스키마 및 마이그레이션 스크립트 작성"
labels: 'database, backend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [DB-001] 핵심 도메인 초기 DB 테이블 마이그레이션
- 목적: 애플리케이션의 핵심이 되는 제품 정보 및 네거티브 필터 데이터 저장을 위한 데이터베이스 스키마를 구성한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`6.2 Entity & Data Model`](#)
- 데이터 모델 (ERD): [`6.2 Table Schema - PRODUCT, NEGATIVE_FILTER`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] `PRODUCT` 테이블(id, brand, model_name, price_usd, category 등) DDL 스크립트 작성
- [ ] `NEGATIVE_FILTER` 테이블(id, product_id, penalty_1~3, severity_level 등) DDL 스크립트 작성
- [ ] `NEGATIVE_FILTER`의 `product_id`에 대한 Foreign Key 및 Cascade 룰 설정
- [ ] Flyway / Prisma 등의 도구를 사용한 Up/Down 마이그레이션 스크립트 구성

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 스키마 생성 및 초기화
- Given: 빈 데이터베이스가 주어짐
- When: 마이그레이션 Up 스크립트를 실행함
- Then: PRODUCT 및 NEGATIVE_FILTER 테이블이 정의된 제약조건(PK, FK, Not Null 등)과 함께 정상 생성된다.

Scenario 2: 마이그레이션 롤백
- Given: 두 테이블이 생성된 데이터베이스가 주어짐
- When: 마이그레이션 Down(롤백) 스크립트를 실행함
- Then: 테이블이 외래키 충돌 없이 안전하게 삭제된다.

## :gear: Technical & Non-Functional Constraints
- 안정성: 테이블 간 FK 참조 무결성 보장
- 컨벤션: 테이블명 및 컬럼명은 snake_case 규칙을 엄격히 준수할 것.

## :checkered_flag: Definition of Done (DoD)
- [ ] ERD 및 테이블 스키마 명세와 데이터 타입이 100% 일치하는가?
- [ ] 로컬 DB에서 마이그레이션 스크립트가 정상적으로 구동 및 롤백되는가?

## :construction: Dependencies & Blockers
- Depends on: None
- Blocks: #DB-002, #F1-001
