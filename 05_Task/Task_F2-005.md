---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[UI/UX] F2-005: 발 사진 업로드 프롬프트 및 산출된 히트맵 사진 위 오버레이 렌더링 UI"
labels: 'feature, frontend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [F2-005] Match DNA 발 사진 업로드 및 히트맵 렌더링 뷰
- 목적: 사용자가 발 사진과 사이즈를 직관적으로 업로드하고, 진단 중에는 폴링 UI(로딩)를, 완료 후에는 자신의 발 사진 위에 정확히 히트맵(압박 지점)을 렌더링하여 보여주는 프론트엔드 경험을 구축한다.

## :link: References (Spec & Context)
- SRS 문서: [`4.1 REQ-FUNC-008`](#)
- Mock API: [`#API-004`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] 파일 드래그 앤 드롭 및 모바일 갤러리 업로드 폼 마크업 (좌우 사이즈 입력창 포함)
- [ ] 파일 업로드 API 통신 및 업로드 중 프로그레스바 구현
- [ ] `PENDING` 상태일 때 백그라운드 폴링(예: 3초 주기)을 도는 커스텀 훅(`usePolling`) 또는 React Query 셋팅
- [ ] `COMPLETED` 수신 시 반환받은 원본 사진 위에 히트맵 JSON(좌표, 강도 색상)을 매핑하는 Canvas API 또는 SVG 오버레이 컴포넌트 개발
- [ ] 에러 상황(400 Bad Request)에 대한 "재촬영 안내" 팝업 마크업

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 진단 완료 및 렌더링
- Given: 사용자가 사진을 업로드하고 폴링 대기 중임
- When: API-004(Mock)에서 `COMPLETED` 상태와 히트맵 데이터를 반환함
- Then: 로딩 스피너가 사라지고, 업로드했던 발 사진 위에 붉은색/노란색 등의 시각적 압박 히트맵 오버레이가 페이드인 애니메이션과 함께 렌더링된다.

Scenario 2: 폴링 로딩 UI 유지
- Given: API가 계속 `PENDING`을 반환함
- When: 화면 렌더링 상태
- Then: 뷰포트 내에 "관리자가 정밀 핏 진단 중입니다"라는 안내와 애니메이션 로딩 컴포넌트가 유지된다.

## :gear: Technical & Non-Functional Constraints
- 성능: 캔버스 렌더링 시 브라우저 메모리 릭이 발생하지 않도록 컴포넌트 언마운트 시 클리어 처리 철저.
- 디자인: 모바일 기기에서의 사진 비율 및 오버레이 좌표 스케일링이 디바이스 해상도에 구애받지 않도록 상대 좌표 비율 계산 로직 적용.

## :checkered_flag: Definition of Done (DoD)
- [ ] Storybook에 폼 입력, 폴링 로딩, 히트맵 렌더링 완료 상태가 각각 구현 및 등록되었는가?
- [ ] 오버레이된 색상이나 레이어가 사진 비율 변경(Resizing)에 맞춰 유동적으로 잘 스케일링되는가?

## :construction: Dependencies & Blockers
- Depends on: #API-004
- Blocks: #F4-001 (경고 오버레이)
