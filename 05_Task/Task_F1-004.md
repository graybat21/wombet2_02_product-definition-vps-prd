---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[UI/UX] F1-004: 네거티브 필터 최상단 붉은색 경고 UI 및 데이터 누락 대체 UI 개발"
labels: 'feature, frontend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [F1-004] 네거티브 경고 오버레이 UI 구현
- 목적: 제품 상세 페이지 진입 시 가장 치명적인 단점을 사용자가 즉시 인지하도록 붉은색 톤의 직관적인 UI를 구현한다.

## :link: References (Spec & Context)
- SRS 문서: [`4.1 REQ-FUNC-001, REQ-FUNC-003`](#)
- Mock API: [`/mocks/api/v1/products/{id}/penalties`](#) (API-004)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 최상단 고정 붉은색 경고 박스 컴포넌트 마크업 (Tailwind CSS 활용)
- [ ] 제품 단점 배열 렌더링 로직 및 폰트/아이콘 시각화 연동
- [ ] 데이터를 불러오는 동안 보여줄 Skeleton UI 작성
- [ ] 데이터가 누락되었거나 지연될 경우 표시할 "현재 단점 분석 중" Fallback UI 컴포넌트 작성
- [ ] API-004를 활용한 클라이언트 측 상태 관리(SWR/React Query 등) 연동

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 정상 데이터 렌더링
- Given: Mock API가 3개의 단점 데이터를 200ms 내에 반환함
- When: 모바일 브라우저로 상세 페이지 로드
- Then: 뷰포트 최상단 상위 30% 영역 이내에 붉은색 경고 박스와 단점 텍스트가 노출된다.

Scenario 2: 데이터 누락/분석 중 렌더링
- Given: Mock API가 "분석 중" 상태 플래그를 반환함
- When: 페이지 로드
- Then: 붉은색 경고 박스 대신 회색 톤의 "현재 단점 분석 중" 안내 박스가 부드러운 애니메이션과 함께 렌더링된다.

## :gear: Technical & Non-Functional Constraints
- 성능: LCP(가장 큰 콘텐츠 렌더링) 지표 ≤ 1.5초 이내 달성 필요 (CSS 최적화)
- 호환성: 모바일 반응형 웹 화면(320px ~ 768px)에 최적화하여 구현

## :checkered_flag: Definition of Done (DoD)
- [ ] Storybook에 컴포넌트 상태별(로딩, 정상, Fallback) UI가 등록되었는가?
- [ ] 화면 로드 시 LCP 성능 저하를 유발하는 불필요한 렌더링 지연이 없는가?

## :construction: Dependencies & Blockers
- Depends on: #API-004 (Mock Data)
- Blocks: None
