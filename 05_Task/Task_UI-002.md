---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature] UI-002: 페이크도어 랜딩페이지 UI 및 이메일 수집 폼 구현"
labels: 'feature, frontend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [UI-002] 페이크도어 랜딩페이지(Landing Page) UI 및 이메일 수집 폼 구현
- 목적: MVP Phase 0 가설 검증을 위해 '부상 방지' 가치를 소구하는 랜딩페이지를 띄우고, 잠재 고객의 이메일을 수집하여 CPA 및 CVR 지표를 측정한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: `/docs/SRS_v0.3.md#6.4 Phase 0: 페이크도어 검증`

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 히어로 섹션(Hero Section) 카피라이팅 및 배경 비주얼 컴포넌트 렌더링
- [ ] 문제 제기(Pain Point) 및 해결책(WOMBET2 Solution) 소개 뷰 구현
- [ ] 이메일 입력 폼(Form) 컴포넌트 및 정규식 기반 유효성 검사 로직 구현
- [ ] 이메일 제출 시 임시 API 연동(또는 Firebase 등 연동) 및 성공 모달(Thank you) 구현
- [ ] Meta Pixel 및 Google Analytics 이벤트 트래킹 코드 삽입 (CPA 측정용)

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 유효한 이메일 제출
- Given: 사용자가 랜딩페이지 폼에 유효한 이메일(`test@runner.com`)을 입력함
- When: '사전 알림 받기' 버튼을 클릭함
- Then: 이메일 데이터가 서버에 전송되고, 완료 모달창이 표시된다.

Scenario 2: 잘못된 이메일 형식 입력
- Given: 사용자가 폼에 잘못된 형식(`testrunner.com`)을 입력함
- When: 폼을 제출하거나 포커스를 잃음(blur)
- Then: 인라인 에러 메시지("올바른 이메일 형식을 입력해주세요")가 붉은색으로 노출되며 전송이 차단된다.

## :gear: Technical & Non-Functional Constraints
- 보안: 봇(Bot) 스팸 방지를 위한 단순 허니팟(Honeypot) 필드 또는 쓰로틀링 적용
- 성능: 모바일 환경에서 이탈을 방지하기 위해 TTI(Time To Interactive) ≤ 2초 보장

## :checkered_flag: Definition of Done (DoD)
- [ ] 모든 Acceptance Criteria를 충족하는가?
- [ ] 입력 폼에 대한 단위 테스트가 작성되었는가?
- [ ] 이벤트 트래킹(CPA 트래킹용)이 정상적으로 트리거되는지 개발자 도구로 확인했는가?

## :construction: Dependencies & Blockers
- Depends on: #UI-001 (기본 디자인 토큰)
- Blocks: 없음 (단독 검증 파이프라인)
