---
name: Feature Task
about: SRS 기반의 구체적인 개발 태스크 명세
title: "[Feature/Query] F4-002: 쿠키 하이재킹이 방어된 Amazon 제휴 리다이렉트 URL 서버사이드 발급 API"
labels: 'feature, backend, priority:high'
assignees: ''
---

## :dart: Summary
- 기능명: [F4-002] 제휴 리다이렉트 URL 안전 발급
- 목적: 클라이언트에서 악의적인 스크립트에 의해 제휴 태그(Affiliate Tag)가 변조되거나 쿠키 하이재킹이 발생하는 것을 막기 위해, 서버에서 직접 검증된 리다이렉트 URL을 조합하여 반환한다.

## :link: References (Spec & Context)
> :bulb: AI 기획 및 개발 가이드: 작업 시작 전 아래 문서를 반드시 먼저 Read/Evaluate 할 것.
- SRS 문서: [`4.2 REQ-NF-010, 6.1 API Endpoint`](#)

## :white_check_mark: Task Breakdown (실행 계획)
- [ ] `GET /api/v1/affiliate/link?product_id={id}` 엔드포인트 구현
- [ ] 컨트롤러 단에서 유효한 `product_id`인지 1차 검증
- [ ] 백엔드 환경 변수(`ENV`)에 저장된 Amazon Associates Tag(예: `wombet2-20`)를 조합하여 최종 Affiliate URL 구성
- [ ] 클릭 해싱 또는 1회용 토큰 등 어뷰징 방지를 위한 보안 체계 도입(선택사항)
- [ ] 구성된 URL을 302 Found 또는 JSON 포맷으로 클라이언트에 반환

## :test_tube: Acceptance Criteria (BDD/GWT)
Scenario 1: 제휴 링크 정상 발급
- Given: 등록된 제품 ID 파라미터가 주어짐
- When: URL 발급 API를 호출함
- Then: 서버 환경변수의 제휴 태그가 정상 포함된 Amazon URL(예: `https://www.amazon.com/dp/.../?tag=wombet2-20`)이 반환된다.

Scenario 2: 악의적 파라미터 방어
- Given: 비정상적인 파라미터 또는 XSS를 유도하는 특수문자 조합
- When: URL 발급 API를 호출함
- Then: 400 Bad Request 에러를 반환하며 조합 로직이 실행되지 않는다.

## :gear: Technical & Non-Functional Constraints
- 보안: 제휴 태그(`tag=`)는 클라이언트 측 소스코드에 절대 하드코딩되지 않아야 하며 백엔드에서만 관리해야 함.
- 성능: 해당 API는 단순 문자열 조합이므로 응답시간 p95 ≤ 100ms 보장.

## :checkered_flag: Definition of Done (DoD)
- [ ] 반환된 URL에 서버 환경변수 설정값이 올바르게 바인딩되는지 통합 테스트가 통과하는가?
- [ ] 파라미터 검증 로직이 적용되어 있는가?

## :construction: Dependencies & Blockers
- Depends on: None
- Blocks: #F4-003
