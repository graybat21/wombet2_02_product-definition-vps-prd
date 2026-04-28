---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Infra] INF-001: LCP 및 API 변환 속도 지표 보장을 위한 부하 테스트(k6) 스크립트 작성"
labels: 'infra, test, priority:medium'
assignees: ''
---

## :dart: Summary
- 기능명: [INF-001] 성능 NFR 달성 검증 파이프라인
- 목적: 핵심 비기능 요구사항인 웹 LCP 속도(≤ 1.5초) 및 핵심 API 응답 속도(p95 ≤ 500ms) 목표를 달성할 수 있는지 확인하기 위한 자동화된 부하 테스트 환경을 구축한다.

## :link: References (Spec & Context)
> :bulb: AI 기획 및 개발 가이드: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`4.2 REQ-NF-001 ~ REQ-NF-003`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] k6 테스팅 도구 설치 및 프로젝트 내 `/load-test` 디렉토리 구성
- [ ] 핵심 API(1. 단점 조회 API, 2. 스펙 역산 리스트 API) 대상 Vus(Virtual Users) 시나리오 작성
- [ ] 타겟 성능 지표 Threshold 설정 (예: `http_req_duration: ['p(95)<500']`)
- [ ] GitHub Actions 또는 로컬 CI에서 k6 스크립트를 실행할 수 있는 파이프라인 스크립트 작성

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 스펙 역산 API 임계치 테스트
- Given: 초당 100건의 동시 접속을 가정하는 Vus 세팅
- When: k6 부하 테스트 스크립트를 실행함
- Then: F3 종목별 스펙 역산 API 통신 응답의 p95가 1000ms 이하를 기록하고 에러율이 0.5% 미만으로 도출되어 테스트 패스(Pass) 결과를 출력한다.

## :gear: Technical & Non-Functional Constraints
- 독립성: 로컬 테스트 시 타 시스템과 격리되거나, 별도의 스테이징 서버(Staging) 환경을 대상으로 테스트해야 함.
- 최적화: 기준 미달 시 DB 인덱스 조정, 캐시 적용 등 튜닝을 진행할 수 있는 명확한 리포트 출력 포맷 적용.

## :checkered_flag: Definition of Done (DoD)
- [ ] k6 스크립트 실행 명령어가 문서(`README.md`)에 정리되었는가?
- [ ] Threshold(임계치) 기반의 테스트 통과 여부가 자동 판별되는가?

## :construction: Dependencies & Blockers
- Depends on: #F1-001, #F3-001 (테스트 대상 API)
- Blocks: 프로덕션 배포
