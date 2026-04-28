---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature] UI-006: 핏 진단 비동기 상태 표시 UI (대기 중/완료)"
labels: 'feature, frontend, priority:medium'
assignees: ''
---

## :dart: Summary
- 기능명: [UI-006] 핏 진단 수동 처리에 따른 비동기 상태 표시 UI
- 목적: 관리자가 수동으로 처리하여 최대 2시간이 소요되는 핏 진단(Match DNA) 과정에서, 사용자가 혼란을 겪지 않도록 직관적인 상태(대기/완료) UI를 제공한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev 시퀀스
- SRS 문서: `/docs/SRS_v0.3.md#6.3 Detailed Interaction Models`

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 사진 업로드 직후 사진 썸네일 위에 "진단 대기 중 (최대 2시간 소요)" 상태 배지 및 딤(Dim) 처리 UI 구현
- [ ] 상태 체크용 서버 폴링(Polling) Hook 또는 컴포넌트 렌더링 로직 연동
- [ ] 진단 상태가 '완료(Completed)'로 변경될 시, 상태 배지를 제거하고 "결과 확인하기" 활성 버튼 렌더링
- [ ] 카카오톡/SMS 알림 발송 전, 웹 내에서의 상태값 갱신 테스트

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 진단 대기 중 상태 렌더링
- Given: 사용자가 핏 진단용 발 사진 업로드를 완료함
- When: API가 201 Created를 반환함
- Then: 즉시 해당 영역에 "전문가 진단 중(SLA 2시간)"이라는 시각적 배지가 노출된다.

Scenario 2: 진단 완료 상태 변경
- Given: 상태가 '대기 중'인 화면
- When: 서버로부터 '완료' 상태 응답을 수신함
- Then: 화면이 리렌더링되어 결과(히트맵) 오버레이를 볼 수 있는 버튼이 활성화된다.

## :gear: Technical & Non-Functional Constraints
- 최적화: 폴링(Polling) 주기는 서버 부하를 고려해 MVP 단계에서는 30초~1분 간격으로 설정하거나, 수동 새로고침 기반으로 동작하도록 유연하게 설계.

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 대기 상태 UI가 사용자에게 충분히 인지될 수 있도록 디자인되었는가?

## :construction: Dependencies & Blockers
- Depends on: #UI-004, #F2-001
- Blocks: 없음
