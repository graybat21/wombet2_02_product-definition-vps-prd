---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Sec] INF-002: 발 사진 PII 처리(CCPA/GDPR 준수) 및 원본 이미지 즉시 파기/난독화 파이프라인 구축"
labels: 'security, infra, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [INF-002] 개인정보(PII) 이미지 보안 처리 파이프라인
- 목적: 사용자의 발 사이즈 및 사진 데이터는 민감한 신체 정보(PII)이므로, CCPA 및 GDPR 규정에 따라 핏 진단이 끝난 직후 원본 이미지를 안전하게 파기하거나 비식별화(난독화)하여 보관하는 체계를 구축한다.

## :link: References (Spec & Context)
> :bulb: AI 기획 및 개발 가이드: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`4.2 REQ-NF-009`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 진단 상태가 `COMPLETED` 로 변경된 시점을 감지하는 스케줄러(Cron) 또는 이벤트 리스너 작성
- [ ] 진단 완료 N시간(예: 24시간) 경과 후, 스토리지(S3 등)에 저장된 원본 이미지 객체 삭제 API 연동 로직
- [ ] DB의 `USER_PROFILE` 테이블 내 PII(발 실측 mm 등) 데이터를 사용자가 계정 삭제/조회 요청 시 대응할 수 있는 처리 파이프라인 검토 및 문서화
- [ ] 로그 스토리지에서 삭제 성공/실패 여부를 감사(Audit)하기 위한 보안 로깅

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 진단 완료 후 자동 파기
- Given: 진단 상태가 `COMPLETED`로 변경된 지 24시간이 경과한 데이터 레코드
- When: 보안 스케줄러가 백그라운드에서 주기적으로 실행됨
- Then: 연결된 클라우드 스토리지의 원본 이미지 파일을 물리적으로 완전 삭제하고 DB에 삭제 완료 Audit 로그를 남긴다.

## :gear: Technical & Non-Functional Constraints
- 보안: 이미지 삭제 실패 시 민감 정보 유출 방지를 위해 알림(Slack 등)을 전송하여 수동 파기를 유도해야 함.
- 컴플라이언스: "삭제 요청 시 즉시 파기"라는 GDPR 권리 보장 프로세스를 충족하도록 설계.

## :checkered_flag: Definition of Done (DoD)
- [ ] 스케줄러 작동 단위 테스트 및 스토리지 삭제 Mock 테스트가 작성되고 성공하는가?
- [ ] 원본 데이터가 확실히 파기되는 아키텍처가 시스템 내재화 되었는가?

## :construction: Dependencies & Blockers
- Depends on: #F2-001, #F2-002
- Blocks: 프로덕션(실 서버) 런칭
