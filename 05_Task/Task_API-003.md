---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[API Spec] API-003: Match DNA 핏 진단 (F2) Request/Response DTO 정의"
labels: 'api, backend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [API-003] Match DNA 핏 진단 API Contract 정의
- 목적: 사용자 발 사진 업로드(요청) 및 관리자의 수동 핏 진단 결과(응답)를 처리하는 비동기/폴링 방식의 API 통신 규약을 확정한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`6.1 API Endpoint List`](#)
- 요구사항: [`4.1 REQ-FUNC-008`](#)
- 시퀀스 다이어그램: [`6.3 Detailed Interaction Models - 시퀀스 3`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 진단 요청 POST `/api/v1/fit/diagnosis` Request DTO 정의 (`product_id`, `foot_image_url` 또는 `multipart/form-data`, 발 실측 데이터 등)
- [ ] 진단 요청의 Response DTO (202 Accepted + `diagnosis_id`) 규약 정의
- [ ] 결과 조회 GET `/api/v1/fit/diagnosis/{id}` Response DTO (상태 플래그: `PENDING`, `COMPLETED`, `risk_verdict`, `heatmap_overlay` JSON) 정의
- [ ] 저화질/인식 불가 이미지에 대한 에러 규약(400 Bad Request + 가이드 메시지) 정의
- [ ] OpenAPI(Swagger) 또는 API Blueprint 문서화 작성

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 비동기 처리 API 명세 확인
- Given: 정의된 DTO 및 에러 코드 스키마
- When: Swagger UI에 접속 시
- Then: POST 요청 시 202 응답과 식별자를 반환하고, GET 요청 시 폴링 처리를 위한 상태코드(PENDING/COMPLETED)가 정의되어 있음이 노출된다.

## :gear: Technical & Non-Functional Constraints
- 보안: 이미지 업로드 시 Content-Type 검증 및 파일 사이즈 제약 사항을 명세서 상에 반드시 기록할 것.
- 통신 패턴: 완전 비동기 폴링(Polling) 방식을 가정한 상태값 설계를 적용할 것.

## :checkered_flag: Definition of Done (DoD)
- [ ] 비동기 프로세스(요청 -> 대기 -> 완료)를 프론트엔드가 구현할 수 있도록 응답 상태 플래그가 명확히 정의되었는가?
- [ ] Swagger 문서가 최신화되었는가?

## :construction: Dependencies & Blockers
- Depends on: None
- Blocks: #API-004, #F2-001, #F2-002, #F2-003
