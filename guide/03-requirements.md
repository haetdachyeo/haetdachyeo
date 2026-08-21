# 블로그 서비스 요구사항

[한국어](03-requirements.md) | [English](03-requirements.en.md) | [日本語](03-requirements.ja.md)

← 이전: [02. 블로그 서비스 범위와 우선순위](02-scope.md)

## 적용

| 항목 | 값 |
| --- | --- |
| 기준 | Stage 01 제품 목표, Stage 02 첫 번째 배포 범위 |
| 대상 | Stage 03 요구사항 문서 모음의 모든 기능·비기능 요구사항 |
| 우선순위 | 전부 필수(Mandatory), 선택 가능한 하위 우선순위 없음 |

## 역할

| 역할 | 식별 | 허용 범위 |
| --- | --- | --- |
| 방문자(Visitor) | 로그인 여부 무관 | 공개 포스트 목록·상세 조회. 비로그인 상태에서는 회원가입·로그인 외 회원 전용 동작 금지 |
| 회원(Member) | 고유 이메일·비밀번호로 가입 완료, 현재 로그인 상태 | 방문자와 같은 공개 조회. 로그인 중 포스트 작성, 자신의 기존 공개 포스트 수정·영구 삭제, 로그아웃 |

## 요구사항 문서

1. [계정 요구사항](requirements/01-account.md)
2. [포스트 요구사항](requirements/02-posts.md)
3. [소유권 및 권한 요구사항](requirements/03-authorization.md)
4. [품질 요구사항](requirements/04-quality.md)

## 요구사항 색인

| 요구사항 ID | 기능·품질 | 역할·대상 | 우선순위 |
| --- | --- | --- | --- |
| `FR-AUTH-001` | [회원가입](requirements/01-account/FR-AUTH-001.md) | 방문자 | 필수(Mandatory) |
| `FR-AUTH-002` | [로그인](requirements/01-account/FR-AUTH-002.md) | 방문자 | 필수(Mandatory) |
| `FR-AUTH-003` | [로그아웃](requirements/01-account/FR-AUTH-003.md) | 회원 | 필수(Mandatory) |
| `FR-POST-001` | [포스트 작성](requirements/02-posts/FR-POST-001.md) | 회원 | 필수(Mandatory) |
| `FR-POST-002` | [공개 포스트 목록 조회](requirements/02-posts/FR-POST-002.md) | 방문자, 회원 | 필수(Mandatory) |
| `FR-POST-003` | [공개 포스트 상세 조회](requirements/02-posts/FR-POST-003.md) | 방문자, 회원 | 필수(Mandatory) |
| `FR-POST-004` | [포스트 수정](requirements/02-posts/FR-POST-004.md) | 회원 | 필수(Mandatory) |
| `FR-POST-005` | [포스트 삭제](requirements/02-posts/FR-POST-005.md) | 회원 | 필수(Mandatory) |
| `FR-AUTHZ-001` | [소유권·권한 제한](requirements/03-authorization/FR-AUTHZ-001.md) | 방문자, 회원 | 필수(Mandatory) |
| `NFR-PERF-001` | [첫 번째 배포 기능 응답시간](requirements/04-quality/NFR-PERF-001.md) | 주요 동작 | 필수(Mandatory) |
| `NFR-AVAIL-001` | [첫 번째 배포 기능 월간 가용성](requirements/04-quality/NFR-AVAIL-001.md) | 주요 동작 | 필수(Mandatory) |

## Stage 경계 및 유예 결정

| Stage | 소유 결정 | 고정 조건 |
| --- | --- | --- |
| Stage 02 | 제외 범위 | 명시된 제외 기능 유지, Stage 03 요구사항 문서 모음으로 추가 금지 |
| Stage 04 | 정상·대체·실패 흐름 순서, 사용자-시스템 상호작용, 경계 상황 | Stage 03 요구사항 문서 모음의 요구사항 변경 금지 |
| Stage 05 | 예상 사용량, 트래픽 구성, 데이터 증가량, 품질 목표 측정 조건, 동시 사용자 100명 동작 구성·측정 환경·부하 산정 근거 | 동시 사용자 100명, p95 2초 이하, 월간 가용성 99.5% 이상 변경 금지 |
| Stage 08 | 요청·응답 구조, API 형식, 오류 형식·코드, 공개 목록의 정확한 후속 결과 조회 방식 | 공개 목록은 결과당 최대 20개, 원래 생성 시각 내림차순, 모든 대상 포스트의 후속 결과 조회 가능 |
| Stage 09 | 아키텍처, 모듈 책임 | 이 문서에서는 미선택 |
| Stage 10 | 데이터베이스 스키마, 무결성 규칙, 영구 삭제의 저장·제거 방식 | 이 문서에서는 미선택 |
| Stage 12 | 인증, 세션, 비밀번호 보호, 인가의 구체적 보안 메커니즘 | 이 문서에서는 미선택 |

| 유예 결정 | 상태 |
| --- | --- |
| HTTP 방식 | 미선택 |
| API 스키마 | 미선택 |
| 상태·오류 코드 | 미선택, Stage 08 |
| 인증 토큰·세션 방식 | 미선택, Stage 12 |
| 비밀번호 해싱 방식 | 미선택, Stage 12 |
| 데이터베이스 구조 | 미선택, Stage 10 |
| 영구 삭제의 저장·제거 방식 | 미선택, Stage 10 |
| 아키텍처 | 미선택, Stage 09 |
| UI | 미선택 |
| 공개 목록의 정확한 후속 조회 방식 | 미선택, Stage 08 |
