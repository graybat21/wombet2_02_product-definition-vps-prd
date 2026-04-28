---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Test] F2-004: 저화질/인식 불가 사진 업로드 시 400 에러 및 실패 단위 테스트 작성"
labels: 'test, backend, priority:low'
assignees: ''
---

## :dart: Summary
- 기능명: [F2-004] 핏 진단 이미지 유효성 예외 테스트
- 목적: 진단이 불가능한 저해상도 이미지, 비정상 포맷, 초과 용량 파일이 업로드 되었을 때 백엔드가 즉시 400 Bad Request와 재촬영 가이드를 반환하는지 시스템적으로 검증한다.

## :link: References (Spec & Context)
- SRS 문서: [`4.1 REQ-FUNC-011`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] `FitDiagnosisControllerTest` 클래스에 Multipart 업로드 예외 시나리오 추가
- [ ] MockMvc를 이용하여 허용 용량(예: 5MB)을 초과하는 더미 파일 업로드 시뮬레이션
- [ ] 허용되지 않는 확장자(.exe, .txt 등)의 파일 업로드 시뮬레이션
- [ ] 해상도 감지(가로/세로 픽셀 제한 검증 로직이 있다면) 실패 시뮬레이션

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 용량 초과 예외
- Given: 10MB 크기의 더미 이미지 파일 요청
- When: 파일 업로드 API 호출
- Then: 400 Bad Request 상태와 "파일 용량이 너무 큽니다. 5MB 이하로 업로드해주세요." 메시지가 반환된다.

Scenario 2: 저화질/해상도 미달 예외
- Given: 100x100 픽셀의 저화질 이미지
- When: 파일 검증 로직 통과 시도
- Then: 400 Bad Request 상태와 "해상도가 낮아 핏 진단이 어렵습니다."라는 재촬영 가이드 에러 메시지를 반환한다.

## :gear: Technical & Non-Functional Constraints
- 안정성: 테스트 구동 시 실제 스토리지(S3 등)에 접근하지 않도록 Controller 계층에서 유효성 검사 단위 테스트만 격리 실행.

## :checkered_flag: Definition of Done (DoD)
- [ ] 파일 용량 및 확장자, 해상도 검증 브랜치 커버리지가 100% 충족되었는가?
- [ ] 클라이언트가 보여줄 구체적인 에러 메시지가 DTO에 명확히 담겨 반환되는가?

## :construction: Dependencies & Blockers
- Depends on: #F2-001
- Blocks: None
