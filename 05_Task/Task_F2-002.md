---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature/Query] F2-002: 관리자 진단 완료(Risk Verdict, Heatmap JSON) 상태 폴링 및 결과 조회 로직"
labels: 'feature, backend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [F2-002] 핏 진단 상태 조회 및 폴링 API
- 목적: 클라이언트가 비동기적으로 등록된 핏 진단 요청의 진행 상태를 조회하고, 진단이 완료되었을 경우 Risk Verdict와 오버레이 렌더링을 위한 Heatmap JSON 데이터를 안전하게 가져올 수 있도록 한다.

## :link: References (Spec & Context)
> :bulb: AI 기획 및 개발 가이드: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`4.1 REQ-FUNC-008, 6.3 시퀀스 다이어그램 3`](#)
- 데이터 모델: [`6.2 FIT_DIAGNOSIS 테이블`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] `GET /api/v1/fit/diagnosis/{id}` 엔드포인트 구현
- [ ] DB 조회 후 상태가 `PENDING`일 경우, `status: PENDING` 만 반환하는 로직 구현
- [ ] DB 조회 후 상태가 `COMPLETED`일 경우, `risk_verdict` (예: High, Low) 및 `heatmap_overlay` JSON 데이터를 함께 반환
- [ ] 관리자가 수동 진단 결과를 업데이트할 수 있는 내부용 어드민 기능(Service 로직) 또는 API 연동 고려

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 진단 진행 중 폴링
- Given: `PENDING` 상태의 진단 ID(`DIAG-123`)
- When: 클라이언트가 결과 조회 API를 호출함
- Then: 200 OK 상태코드와 함께 `{"status": "PENDING"}` 페이로드를 반환한다.

Scenario 2: 진단 완료 후 결과 반환
- Given: 관리자가 진단 결과를 `COMPLETED`로 변경하고 Heatmap JSON을 저장함
- When: 클라이언트가 결과 조회 API를 호출함
- Then: `{"status": "COMPLETED", "risk_verdict": "High", "heatmap_overlay": {...}}` 의 완전한 페이로드를 반환한다.

## :gear: Technical & Non-Functional Constraints
- 성능: 초당 여러 번의 폴링 요청이 들어올 수 있으므로, 해당 조회 쿼리는 인덱스를 타고 최적화되어야 하며 필요 시 응답 지연(Long Polling) 또는 캐싱 계층 도입을 염두에 둘 것.

## :checkered_flag: Definition of Done (DoD)
- [ ] PENDING과 COMPLETED 상태를 정확히 구분하여 반환하는 단위 테스트가 추가되었는가?
- [ ] 히트맵 JSON 데이터가 클라이언트 요구 규격(API-003)과 일치하게 반환되는가?

## :construction: Dependencies & Blockers
- Depends on: #F2-001
- Blocks: #F2-003, #F2-005
