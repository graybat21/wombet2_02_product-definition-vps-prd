---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Mock] API-004: 프론트엔드 UI 개발용 F1, F2, F3 Mocking 데이터 및 가짜 엔드포인트 세팅"
labels: 'mock, frontend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [API-004] 프론트엔드 개발용 Mock 서버 구축
- 목적: 백엔드 API가 개발 완료되기 전이라도, 프론트엔드 에이전트가 지연 없이 UI/UX 컴포넌트(F1 단점 경고, F2 히트맵, F3 리스트)를 연동하고 상태를 관리할 수 있도록 가짜 통신 환경을 구축한다.

## :link: References (Spec & Context)
> :bulb: AI Agent & Dev Note: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- 연관 태스크: [`#API-001, #API-002, #API-003`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] MSW(Mock Service Worker) 또는 json-server 등 프로젝트 환경에 맞는 Mocking 라이브러리 설치 및 세팅
- [ ] F1: 네거티브 필터 응답 Mock JSON 생성 (정상 상태, "분석 중" 상태, 지연 상태)
- [ ] F3: 스펙 역산 리스트 Mock JSON 생성 (정상 종목 변환 결과, 미지원 종목 Fallback 플래그 포함)
- [ ] F2: 핏 진단 응답 Mock JSON 생성 (PENDING 딜레이 응답 1회 후, COMPLETED 및 히트맵 좌표 데이터 반환 시뮬레이션)
- [ ] 프론트엔드 로컬 개발 환경 실행 시 자동으로 Mock Server가 동작하도록 스크립트(`package.json`) 구성

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: F1 네거티브 필터 가짜 데이터 로드
- Given: MSW가 구동된 로컬 환경
- When: 프론트엔드에서 `/api/v1/products/1/penalties`를 호출함
- Then: 300ms 딜레이 후 API-001 규약에 맞는 정상 JSON 데이터를 반환한다.

Scenario 2: F2 비동기 폴링 시뮬레이션
- Given: 진단 결과 폴링 API 요청 환경
- When: 최초 호출 시도
- Then: `{"status": "PENDING"}`을 반환하고, 3초 뒤 재호출 시 `{"status": "COMPLETED", "heatmap": [...]}`을 반환하여 비동기 UI 테스트를 가능하게 한다.

## :gear: Technical & Non-Functional Constraints
- 독립성: 로컬 개발 환경(`NODE_ENV=development`)에서만 활성화되어야 하며, 프로덕션 빌드 시 포함되지 않아야 함.

## :checkered_flag: Definition of Done (DoD)
- [ ] F1, F2, F3의 모든 엣지 케이스(정상, 예외, 폴링, 분석중 등)에 대한 Mock Handler가 등록되었는가?
- [ ] 프론트엔드 개발 시 네트워크 탭에서 Mock 응답이 정상적으로 인셉트(Intercept) 되는가?

## :construction: Dependencies & Blockers
- Depends on: #API-001, #API-002, #API-003 (규약 확정 후 진행)
- Blocks: #F1-004, #F3-004, #F2-005 (프론트엔드 UI 태스크)
