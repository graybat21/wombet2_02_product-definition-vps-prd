---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature/Command] F2-001: 발 사진 업로드/데이터 검증 및 수동 진단용 큐(Queue) 등록 API 구현"
labels: 'feature, backend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [F2-001] 발 사진 업로드 및 진단 큐 등록
- 목적: 사용자가 업로드한 발 사진 및 실측 정보를 검증하고, MVP 단계에서 시스템 관리자가 수동으로 진단(SLA 2시간)을 진행할 수 있도록 작업 큐에 등록(PENDING 상태)한다.

## :link: References (Spec & Context)
> :bulb: AI 기획 및 개발 가이드: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`4.1 REQ-FUNC-008, 6.3 시퀀스 다이어그램 3`](#)
- DTO 명세: [`#API-003`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 클라이언트로부터 `multipart/form-data` 형식으로 이미지 및 사이즈(left_foot_mm 등) 데이터를 수신하는 컨트롤러 구현
- [ ] 파일 확장자(JPEG/PNG) 및 용량(예: 5MB 이하) 제한 검증 로직 작성
- [ ] 이미지를 클라우드 스토리지(AWS S3 등) 또는 로컬 경로에 안전하게 저장 후 URL 확보
- [ ] `FIT_DIAGNOSIS` 테이블에 상태가 `PENDING`인 새 레코드를 생성 (transaction 처리)
- [ ] 관리자 채널(Slack 또는 SMS 등)로 새로운 진단 요청이 도착했음을 알리는 알림 전송 로직 구현

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 정상적인 진단 요청 등록
- Given: 올바른 형식의 발 사진 이미지와 좌우 발 사이즈 정보가 주어짐
- When: `/api/v1/fit/diagnosis` 로 POST 요청함
- Then: 파일이 성공적으로 저장되고, `FIT_DIAGNOSIS` DB에 레코드가 생성되며, `202 Accepted` 상태 코드와 함께 진단 조회용 `diagnosis_id`를 반환한다.

## :gear: Technical & Non-Functional Constraints
- 성능: S3 이미지 업로드 시 I/O 병목이 발생하지 않도록 스트림 처리(Streaming) 적용.
- 보안: 업로드된 이미지 파일 내부의 악성 스크립트 실행 방지 (MIME Type 이중 검증).

## :checkered_flag: Definition of Done (DoD)
- [ ] DB 레코드 생성 후 202 응답이 정상 반환되는가?
- [ ] 업로드된 사진이 지정된 저장소에서 접근 가능한가? (단위/통합 테스트 완료)

## :construction: Dependencies & Blockers
- Depends on: #DB-003, #API-003
- Blocks: #F2-002, #F2-004, #INF-002
