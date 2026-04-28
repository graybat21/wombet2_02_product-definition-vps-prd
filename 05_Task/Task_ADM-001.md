---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature] ADM-001: 관리자 핏 진단 결과 수동 업로드 및 DB 관리 API 구현"
labels: 'feature, backend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [ADM-001] 관리자 핏 진단 결과 수동 업로드 및 단점 DB 관리(CRUD) API 구현
- 목적: 관리자가 사용자 발 사진을 보고 분석한 진단 결과(히트맵 JSON 및 Risk Verdict)를 시스템에 업데이트하고, 단점 DB를 관리할 수 있는 최소한의 백오피스 인터페이스를 제공한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: `/docs/SRS_v0.3.md#3.3 API Overview`
- SRS 문서: `/docs/SRS_v0.3.md#6.3 Detailed Interaction Models (시퀀스 3)`

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 진단 세션 ID 기반 진단 결과(히트맵 JSON, Risk 등급) 업데이트용 PATCH API 구현 (`/api/v1/admin/diagnosis/{id}`)
- [ ] 단점(Negative Filter) 데이터 및 제품 기본 정보 제어를 위한 Admin CRUD API 구현
- [ ] Admin 전용 API 접근을 보호하기 위한 단순 인증/인가(Admin Token 또는 Auth Middleware) 로직 연동
- [ ] 진단 상태 변경에 따른 B2C 조회 로직(F2-002) 트리거/연계 확인

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 관리자 인증 후 진단 결과 업데이트
- Given: 유효한 Admin Token과 대기 중인 진단 세션 ID가 주어짐
- When: 히트맵 데이터와 핏 위험도(Risk Verdict)를 PATCH 메서드로 전송함
- Then: DB의 `FIT_DIAGNOSIS` 데이터가 성공적으로 업데이트되고, 200 OK 상태 코드를 반환한다.

Scenario 2: 권한 없는 접근 차단
- Given: 일반 유저 토큰 또는 잘못된 토큰이 주어짐
- When: Admin 전용 API 경로로 데이터 수정을 요청함
- Then: 403 Forbidden 또는 401 Unauthorized 상태 코드를 반환하며 요청을 안전하게 거부한다.

## :gear: Technical & Non-Functional Constraints
- 보안: 초기 MVP 단계이므로 복잡한 롤(Role) 관리 대신, 하드코딩된 Secret Key 또는 심플한 JWT 인증 구조로 빠르게 구현할 것.
- 응답성: 관리자 작업 효율을 위해 응답 지연 시간(p95) ≤ 500ms 보장.

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 통합 테스트를 통해 Auth Middleware 정상 작동을 확인했는가?
- [ ] 해당 API 명세가 Swagger 등에 문서화되었는가?

## :construction: Dependencies & Blockers
- Depends on: #DB-003 (FIT_DIAGNOSIS 테이블)
- Blocks: #F2-002 (진단 결과 조회 로직 연동 시 테스트 필요)
