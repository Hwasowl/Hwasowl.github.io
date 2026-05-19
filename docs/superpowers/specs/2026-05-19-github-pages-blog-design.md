# GitHub Pages 기술 블로그 설계 스펙

**날짜**: 2026-05-19  
**레포**: https://github.com/Hwasowl/Hwasowl.github.io  
**상태**: 승인됨

---

## 목표

다크/라이트 모드를 지원하고, 구조화된 테크니컬 라이팅에 최적화된 개인 기술 블로그를 GitHub Pages에 배포한다.

---

## 기술 스택

| 항목 | 선택 | 이유 |
|------|------|------|
| SSG | Jekyll | GitHub Pages 네이티브 지원, 별도 서버 불필요 |
| 테마 | chirpy-starter | 다크/라이트 토글 내장, 코드 하이라이팅·TOC·카테고리 기본 제공 |
| 댓글 | Giscus | GitHub Discussions 기반, 무료, 다크/라이트 자동 연동 |
| 배포 | GitHub Actions | main 브랜치 push → 자동 빌드 → gh-pages 배포 |

---

## 아키텍처

```
Hwasowl.github.io/
├── _config.yml              # 사이트 전체 설정 (언어·테마·Giscus·프로필)
├── _posts/                  # 블로그 글 (YYYY-MM-DD-title.md)
│   └── 0000-00-00-template.md  # 테크니컬 라이팅 포스트 템플릿
├── _tabs/                   # About·Archives·Categories·Tags 고정 페이지
├── assets/                  # 이미지·파일 업로드
├── Gemfile                  # jekyll-theme-chirpy gem 버전 관리
└── .github/
    └── workflows/
        └── pages-deploy.yml # 자동 빌드·배포 워크플로
```

**빌드 흐름**:  
`main` push → GitHub Actions 감지 → `bundle exec jekyll build` → `gh-pages` 브랜치 배포 → `hwasowl.github.io` 서빙

---

## `_config.yml` 핵심 설정

```yaml
# 사이트 기본
title: Hwasowl
tagline: "백엔드 개발자의 기술 노트"
url: "https://hwasowl.github.io"
lang: ko-KR
timezone: Asia/Seoul

# 프로필 (사이드바)
github:
  username: Hwasowl

# 다크/라이트 모드
# 비워두면 OS 설정 자동 감지, 우측 상단 토글로 수동 전환 가능
theme_mode:

# 마크다운
markdown: kramdown
highlighter: rouge

# 댓글
comments:
  provider: giscus
  giscus:
    repo: Hwasowl/Hwasowl.github.io
    repo_id:          # giscus.app에서 발급
    category: Announcements
    category_id:      # giscus.app에서 발급
    mapping: pathname
    reactions_enabled: 1
    theme: preferred_color_scheme
```

---

## Giscus 댓글 활성화 절차

1. 레포 **Settings → Features → Discussions** 체크
2. [giscus.app](https://giscus.app) 접속 → 레포명 입력 → `repo_id` · `category_id` 복사
3. `_config.yml`의 해당 필드에 붙여넣기

`theme: preferred_color_scheme` 으로 블로그 다크/라이트 토글과 댓글창 테마 자동 동기화.

---

## 언어 및 타이포그래피

- **작성 언어**: 한국어 기반, 기술 용어는 영어 혼용
- `lang: ko-KR` 로 Chirpy UI 문자열(날짜 포맷, 버튼 레이블 등) 한국어 자동 적용
- `timezone: Asia/Seoul` 로 포스트 날짜 한국 시간 기준 표시

---

## 포스트 템플릿 (`_posts/0000-00-00-template.md`)

테크니컬 라이팅 전용 골격. 불필요한 섹션은 삭제하고 사용.

```markdown
---
title: "[Retrospective] 제목 (N주차 · K팀 · 이름)"
date: YYYY-MM-DD HH:MM:SS +0900
categories: [Retrospective]
tags: [키워드1, 키워드2]
---

## TL;DR
5초 안에 핵심을 전하는 1~2줄.

## 개요 (Overview)
- **목표**:
- **중점 사항**:

## 핵심 기술 및 결정 (Technical Decisions)

| 기술/설계 항목 | 선택한 대안 | 선택 이유 (Rationale) |
|---|---|---|
|  |  |  |

## 트러블슈팅 (Troubleshooting)

### 문제 현상

### 원인 분석
-

### 해결 방안
-

## 회고 (Retrospective)
- **Keep**:
- **Problem**:
- **Try**:
```

---

## 성공 기준

- [ ] `https://hwasowl.github.io` 접속 시 블로그 렌더링
- [ ] 다크/라이트 모드 토글 동작
- [ ] 포스트 작성 시 코드 하이라이팅·TOC 표시
- [ ] Giscus 댓글창 노출 및 다크/라이트 자동 연동
- [ ] 포스트 템플릿 파일 제공

---

## 범위 외 (Out of Scope)

- 커스텀 도메인 설정
- 구글 애널리틱스 연동
- 다국어(i18n) 지원
- 테마 HTML/CSS 커스터마이징
