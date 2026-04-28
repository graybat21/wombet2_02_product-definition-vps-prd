---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature/Query] F2-003: B2B 고객 맞춤형 핏 리스크 검증 API (/api/v1/fit/diagnosis) 제공 로직"
labels: 'feature, backend, priority:medium'
assignees: ''
---

## :dart: Summary
- 기능명: [F2-003] B2B 외부 연동용 핏 리스크 제공 API (Phase 2)
- 목적: B2B 파트너사(Shopify/ERP 등)가 서버 투 서버(S2S) 통신으로 자사 고객의 발 사이즈를 전송했을 때, 제품의 Match DNA와 대조하여 핏 리스크(반품 위험도)와 히트맵을 즉각 반환하는 API를 제공한다.

## :link: References (Spec & Context)
> :bulb: AI 기획 및 개발 가이드: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`3.3 API Overview, 6.3 시퀀스 다이어그램 4`](#)
- 비즈니스 모델: 월 $2,500 구독, 월 호출 10K 제한 연동

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] B2B 전용 인증 계층(API Key 등)을 적용한 `POST /api/b2b/v1/fit/diagnosis` 라우트 생성
- [ ] 기존 수동(PENDING) 방식과 달리, 이미 측정/저장된 제품의 `MATCH_DNA` 데이터와 유저의 사이즈를 수치적으로 비교하여 즉시 `risk_verdict`를 도출하는 자동 판정 로직(알고리즘) 개발
- [ ] B2B 파트너별 API 호출 횟수 기록 로직 및 Rate Limiting (월 10,000건 등) 기능 추가

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: B2B 인증 통과 및 핏 리스크 반환
- Given: 유효한 B2B API Key와 유저의 양발 실측 데이터 페이로드
- When: 파트너사 서버가 API를 호출함
- Then: 인증을 통과한 후, 기존 등록된 제품의 DNA 데이터와 실측치를 비교 계산하여 `{"fit_risk_level": "High", "heatmap_json": {...}}`를 동기식(Sync)으로 즉시 반환한다.

Scenario 2: 인증 실패 또는 권한 없음
- Given: 유효하지 않은 API Key
- When: API를 호출함
- Then: 401 Unauthorized 상태 코드를 반환하고 로직 수행을 거부한다.

## :gear: Technical & Non-Functional Constraints
- 보안: API Key 노출 방지를 위한 암호화 저장 및 HTTPS 통신 필수.
- 성능: S2S 동기 통신이므로 p95 응답시간 ≤ 1초 달성 필수.

## :checkered_flag: Definition of Done (DoD)
- [ ] B2B 전용 API Key 필터가 정상 동작하는가? (통합 테스트 검증)
- [ ] B2B 파트너용 Swagger 또는 연동 가이드 문서가 배포되었는가?

## :construction: Dependencies & Blockers
- Depends on: #F2-002, DB의 `MATCH_DNA` 자동 연산 로직 확정
- Blocks: None
