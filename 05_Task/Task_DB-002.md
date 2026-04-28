---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[DB] DB-002: RELATIVITY_TAG, MATCH_DNA 테이블 스키마 및 마이그레이션 스크립트 작성"
labels: 'database, backend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [DB-002] 상대성 태깅 및 Match DNA 테이블 마이그레이션
- 목적: 상대성 태깅(F3)에 활용되는 종목별 스펙 역산 스코어 및 Match DNA(F2)의 제품별 핏 DNA 데이터를 저장할 테이블을 구축한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`6.2 Entity & Data Model`](#)
- 데이터 모델 (ERD): [`6.2 Table Schema - RELATIVITY_TAG, MATCH_DNA`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] `RELATIVITY_TAG` 테이블(tag_id, product_id, sport_type, original_score, converted_score 등) DDL 스크립트 작성
- [ ] `MATCH_DNA` 테이블(dna_id, product_id, upper_elasticity_grade, toebox_pressure_index, seam_collision_heatmap 등) DDL 작성
- [ ] 두 테이블의 `product_id`를 `PRODUCT` 테이블과 참조하도록 Foreign Key 및 Cascade 룰 설정
- [ ] JSON 타입 필드(seam_collision_heatmap 등)의 데이터베이스 타입 매핑(PostgreSQL JSONB 등) 적용
- [ ] Flyway / Prisma 등을 이용한 Up/Down 마이그레이션 스크립트 구성

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 스키마 생성 및 타입 검증
- Given: `PRODUCT` 테이블이 존재하는 데이터베이스가 주어짐
- When: 마이그레이션 Up 스크립트를 실행함
- Then: `RELATIVITY_TAG`와 `MATCH_DNA` 테이블이 생성되며, `PRODUCT`를 정상 참조하고 JSON 필드가 올바른 타입으로 적용된다.

Scenario 2: FK 무결성 검증
- Given: 두 테이블에 특정 `product_id`의 데이터가 등록되어 있음
- When: 부모 테이블인 `PRODUCT`에서 해당 제품을 삭제함
- Then: `RELATIVITY_TAG`와 `MATCH_DNA`의 관련 레코드가 Cascade 설정에 의해 정상적으로 함께 삭제된다.

## :gear: Technical & Non-Functional Constraints
- 안정성: 테이블 간 FK 참조 무결성 보장
- 성능: `product_id` 및 `sport_type` 복합 인덱스 고려(Read 성능 개선)

## :checkered_flag: Definition of Done (DoD)
- [ ] ERD 및 테이블 스키마 명세와 데이터 타입이 100% 일치하는가?
- [ ] 로컬 DB에서 마이그레이션 스크립트가 정상적으로 구동 및 롤백되는가?

## :construction: Dependencies & Blockers
- Depends on: #DB-001 (PRODUCT 테이블)
- Blocks: #F3-001, #F2-005
