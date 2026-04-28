---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[UI/UX] F3-004: 종목 맞춤형 리스트 뷰 렌더링 및 신규 종목 팝업 UI 컴포넌트 개발"
labels: 'feature, frontend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [F3-004] 종목별 제품 리스트 뷰 UI 및 상호작용 구현
- 목적: 프론트엔드에서 사용자가 종목 드롭다운을 통해 목적을 선택하면, 역산된 데이터에 기반해 UI가 동적으로 갱신되며, 이종 스펙 경고나 미지원 종목 안내를 시각적으로 명확히 전달한다.

## :link: References (Spec & Context)
- SRS 문서: [`4.1 REQ-FUNC-004, REQ-FUNC-005, REQ-FUNC-007`](#)
- Mock API: [`#API-004`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 종목 선택용 Dropdown / Filter 모달 컴포넌트 마크업
- [ ] API-004 Mock 데이터 통신을 연결하여 카드를 렌더링하는 `ProductRankingList` 컴포넌트 개발
- [ ] 카드 내 `contamination_warning` 플래그가 `true`일 경우 표시될 "이종 스펙 혼입 주의" 빨간 딱지 UI 추가
- [ ] `is_fallback=true` 플래그가 수신될 경우 화면에 띄울 "해당 종목 분석 중입니다. 기본 리스트를 표시합니다." 팝업(Toast/Modal) 컴포넌트 개발
- [ ] 부정확 피드백 제출 버튼(F3-002 연동) 마크업

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 동적 리스트 갱신
- Given: 종목 미선택(범용) 리스트가 렌더링 된 상태
- When: 사용자가 드롭다운에서 '역도'를 선택함
- Then: 로딩 스피너(또는 Skeleton)가 잠깐 표시된 후, 역도 종목 스코어로 정렬이 변경된 제품 리스트 카드들이 렌더링된다.

Scenario 2: 신규/기타 종목 Fallback 노출
- Given: 범용 리스트가 렌더링 된 상태
- When: 사용자가 아직 지원되지 않는 종목(예: 테니스)을 선택함
- Then: "해당 종목 분석 중"이라는 토스트 팝업이 발생하며, 앱이 크래시되지 않고 원래의 범용 리스트 뷰를 유지한다.

## :gear: Technical & Non-Functional Constraints
- 성능: DOM 리렌더링 최적화(React.memo 등)를 통해 60FPS 이상의 스크롤 및 전환 부드러움을 유지.
- 디자인: 모바일 뷰어(Max-width 768px) 기준 카드가 시원하게 스크롤될 수 있도록 반응형 설계.

## :checkered_flag: Definition of Done (DoD)
- [ ] Storybook에 각 예외/정상 상태의 Product Card 컴포넌트 및 Toast 팝업이 등록되었는가?
- [ ] 사용자의 인터랙션 시 오류(크래시) 없이 상태가 안전하게 렌더링되는지 브라우저에서 검증되었는가?

## :construction: Dependencies & Blockers
- Depends on: #API-004
- Blocks: None
