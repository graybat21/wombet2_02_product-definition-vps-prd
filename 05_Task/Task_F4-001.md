---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[UI/UX] F4-001: 진단 고위험군 결제 진입 시 시각적 안전 진단 경고 팝업(Overlay) 렌더링"
labels: 'feature, frontend, priority:medium'
assignees: ''
---

## :dart: Summary
- 기능명: [F4-001] 결제 직전 안전 진단 경고 팝업
- 목적: 핏 진단(F2) 결과가 '고위험(High Risk)'으로 판정되었음에도 사용자가 구매(제휴 링크 클릭)를 시도할 경우, 다시 한 번 경고 모달을 띄워 인지를 강화하고 핏 기인 반품을 사전에 차단한다.

## :link: References (Spec & Context)
> :bulb: AI 기획 및 개발 가이드: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`4.1 REQ-FUNC-010`](#)
- 시퀀스 다이어그램: [`6.3 시퀀스 다이어그램 3 (F4 결제 직전 영역)`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 구매하기(제휴 링크) 버튼 클릭 이벤트 인터셉트 로직 작성
- [ ] 현재 클라이언트 전역 상태(State)의 `risk_verdict` 값 확인 (High일 경우 모달 트리거)
- [ ] "이 신발은 고객님의 발에 부상을 유발할 위험이 매우 높습니다. 그래도 구매하시겠습니까?" 경고 팝업(Modal) UI 마크업
- [ ] '위험 감수하고 구매 계속' 버튼 클릭 시 F4-002(리다이렉트 API) 호출 및 이동 처리
- [ ] '취소하고 대안 찾기' 버튼 클릭 시 모달 닫기 및 F3(리스트)로 포커스 이동

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 고위험 제품 구매 시도
- Given: 진단 결과가 `High` 상태인 제품 상세 페이지
- When: 제휴몰로 이동하는 '구매하기' 버튼을 클릭함
- Then: 즉시 이동하지 않고 화면 중앙에 붉은색 톤의 안전 진단 경고 모달 팝업이 노출된다.

Scenario 2: 저위험 제품 구매 시도
- Given: 진단 결과가 `Low` 상태인 제품 상세 페이지
- When: '구매하기' 버튼을 클릭함
- Then: 경고 모달 없이 즉시 서버(F4-002)로 리다이렉트 URL을 요청하여 이동한다.

## :gear: Technical & Non-Functional Constraints
- 성능: 팝업 렌더링은 클라이언트 사이드에서 즉각(≤ 100ms) 반응해야 함.
- 사용성: 모달 팝업이 띄워졌을 때 배경(Body) 스크롤이 고정(Lock)되어야 함.

## :checkered_flag: Definition of Done (DoD)
- [ ] 고위험군일 때만 선택적으로 모달이 렌더링 되는지 단위 테스트(Jest/RTL)가 통과하는가?
- [ ] Storybook에 경고 모달 상태가 구현되어 있는가?

## :construction: Dependencies & Blockers
- Depends on: #F2-005 (상태 관리 연동)
- Blocks: None
