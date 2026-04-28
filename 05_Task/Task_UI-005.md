---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature] UI-005: 제휴 구매 전환(CTA) 버튼 및 안전 리다이렉션 로딩 화면 구현"
labels: 'feature, frontend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [UI-005] 제휴 구매 전환(CTA) 버튼 및 안전 리다이렉션 로딩 화면 구현
- 목적: 아마존 등 제휴 몰로 전환되는 핵심 CTA 버튼을 배치하고, 서버 통신(안전 링크 발급 등) 대기 시간 동안 사용자가 이탈하지 않도록 시각적 피드백을 제공한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev 시퀀스
- SRS 문서: `/docs/SRS_v0.3.md#3.4 시퀀스 다이어그램 1` (제휴 URL 발급 요청 파트)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 모바일 하단 고정(Sticky Bottom) 구매/최저가 확인 버튼(CTA) 컴포넌트 구현
- [ ] 버튼 클릭 시 제휴 링크 API 응답을 대기하는 동안의 Spinner/Loading 오버레이 뷰 구현
- [ ] F4-001(안전 진단 경고 팝업)을 트리거하기 위한 클릭 이벤트 인터셉터 로직 연동 지점 마련
- [ ] 링크 발급 실패 시(예외 처리) 에러 토스트(Toast) 알림 구현

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 리다이렉션 대기 상태
- Given: 제품 상세 페이지에서 구매 전환 CTA가 활성화됨
- When: 사용자가 버튼을 클릭함 (안전 경고 조건 없음 가정)
- Then: 화면 중앙에 "안전한 제휴 링크를 생성 중입니다..." 로딩 UI가 렌더링되며, 응답 후 외부로 리다이렉트 된다.

## :gear: Technical & Non-Functional Constraints
- UX: 로딩 뷰는 기존 화면을 딤(Dim) 처리하고 화면 중앙을 점유하여 중복 클릭을 방지해야 함.

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 중복 클릭(Debounce/Throttle 방어) 방지 처리가 되었는가?

## :construction: Dependencies & Blockers
- Depends on: #UI-004
- Blocks: #F4-001 (경고 오버레이)
