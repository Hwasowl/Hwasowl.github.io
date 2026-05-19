---
title: "[Tech Note] 시스템 설계 제목"
date: 2099-01-01 00:00:00 +0900
categories: [Tech Note]
tags: [system-design, architecture]
---

> **사용 방법**: 이 파일을 복사해 `_posts/YYYY-MM-DD-title.md` 로 저장하고,
> `date:` 값을 실제 작성 날짜로 변경하세요.

## Introduction & Goals

- **Context/Background:** 이 프로젝트가 왜 필요한지에 대한 배경 설명. 현재 시스템의 한계점이나 비즈니스 요구사항을 설명합니다.
- **Goals:** 이 설계를 통해 달성하고자 하는 구체적인 목표. (예: 읽기 레이턴시 50ms 이하로 단축)

## Detailed Design

아래 키워드들을 적절히 활용해서 Tech Note에서 강조하고자 하는 핵심 내용을 설명할 것.
면접관이 봤을 때 이런 기술적 역량과 깊이가 있구나를 확인할 수 있도록 작성하는 것이 중요.

- **System Architecture:** 전체적인 컴포넌트 구조도.
- **Data Models:** DB 스키마 변경점, 데이터 흐름, 캐싱 전략.
- **API Design:** 신규 추가되거나 변경되는 엔드포인트와 페이로드 형식.
- **Constraints:** 시스템의 제약 사항 (예: 특정 DB 엔진 사용 필수, 비용 제한 등).

## (Optional) Alternatives Considered

"왜 이 방법인가?"에 대한 답을 기술합니다.

- **Option A, B, C:** 고려했던 다른 설계 방향들.
- **Pros & Cons:** 각 대안의 장단점 비교.
- **Why we chose this:** 최종안을 선택한 기술적, 경제적 근거. (이 부분이 탄탄해야 시니어 리뷰를 통과할 수 있습니다.)

## (Optional) Cross-cutting Concerns

백엔드 엔지니어로서 실력을 증명할 수 있는 '비기능적 요구사항' 들입니다.

- **Scalability:** 트래픽 급증 시 어떻게 대응할 것인가?
- **Latency:** 응답 속도에 미치는 영향은?
- **Security & Privacy:** 데이터 암호화, 권한 제어, PII(개인 식별 정보) 처리 방식.
- **Observability:** 로깅, 모니터링 지표, 알람 설정 계획.
