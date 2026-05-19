# GitHub Pages 기술 블로그 구현 계획

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Jekyll + Chirpy Starter 기반으로 다크/라이트 모드·Giscus 댓글·테크니컬 라이팅 템플릿을 갖춘 기술 블로그를 `hwasowl.github.io`에 배포한다.

**Architecture:** 빈 GitHub Pages 레포에 Chirpy Starter 파일을 수동으로 구성한다. 테마는 gem으로 분리되어 있어 레포에는 설정·콘텐츠 파일만 존재한다. `main` push 시 GitHub Actions가 자동 빌드·배포한다.

**Tech Stack:** Jekyll 4.x, jekyll-theme-chirpy ~> 7.3, Ruby 3.x, Bundler, GitHub Actions, Giscus

---

## 파일 맵

| 파일 | 역할 |
|------|------|
| `Gemfile` | jekyll-theme-chirpy gem 의존성 |
| `.gitignore` | _site, .bundle, vendor 등 제외 |
| `index.html` | 루트 홈 페이지 (Chirpy 필수) |
| `_config.yml` | 사이트 전체 설정 |
| `.github/workflows/pages-deploy.yml` | CI/CD 자동 빌드·배포 |
| `_tabs/about.md` | About 탭 |
| `_tabs/archives.md` | Archives 탭 |
| `_tabs/categories.md` | Categories 탭 |
| `_tabs/tags.md` | Tags 탭 |
| `_posts/0000-00-00-template.md` | 테크니컬 라이팅 포스트 템플릿 |

---

## Task 1: Ruby 환경 확인 및 Gemfile 생성

**Files:**
- Create: `Gemfile`

- [ ] **Step 1: Ruby 및 Bundler 설치 확인**

  PowerShell에서 실행:
  ```powershell
  ruby -v
  gem -v
  bundler -v
  ```
  Expected: Ruby 3.x, gem, bundler 버전 출력.
  
  **Ruby가 없으면:** https://rubyinstaller.org/downloads/ 에서 `Ruby+Devkit 3.3.x (x64)` 설치. 설치 시 "Add Ruby executables to your PATH" 체크.

- [ ] **Step 2: Gemfile 생성**

  `C:/Users/hwaso/Hwasowl.github.io/Gemfile` 파일 생성:
  ```ruby
  source "https://rubygems.org"

  gem "jekyll-theme-chirpy", "~> 7.3"
  ```

- [ ] **Step 3: bundle install 실행**

  레포 루트에서:
  ```powershell
  cd C:\Users\hwaso\Hwasowl.github.io
  bundle install
  ```
  Expected: `Bundle complete!` 메시지, `Gemfile.lock` 생성.

- [ ] **Step 4: 커밋**

  ```bash
  git add Gemfile Gemfile.lock
  git commit -m "chore: Chirpy gem 의존성 추가"
  ```

---

## Task 2: 기본 파일 구조 생성

**Files:**
- Create: `.gitignore`
- Create: `index.html`

- [ ] **Step 1: .gitignore 생성**

  `C:/Users/hwaso/Hwasowl.github.io/.gitignore` 파일 생성:
  ```
  # Bundler
  .bundle
  vendor
  Gemfile.lock

  # Jekyll
  .jekyll-cache
  .jekyll-metadata
  _site

  # Node
  node_modules
  package-lock.json

  # Misc
  .sass-cache
  ```

- [ ] **Step 2: index.html 생성**

  `C:/Users/hwaso/Hwasowl.github.io/index.html` 파일 생성:
  ```html
  ---
  layout: home
  # Index page
  ---
  ```

- [ ] **Step 3: 커밋**

  ```bash
  git add .gitignore index.html
  git commit -m "chore: 기본 파일 구조 추가"
  ```

---

## Task 3: GitHub Actions 배포 워크플로 설정

**Files:**
- Create: `.github/workflows/pages-deploy.yml`

- [ ] **Step 1: 워크플로 디렉토리 생성 및 파일 작성**

  `C:/Users/hwaso/Hwasowl.github.io/.github/workflows/pages-deploy.yml` 파일 생성:
  ```yaml
  name: "Build and Deploy"
  on:
    push:
      branches:
        - main
        - master
      paths-ignore:
        - .gitignore
        - README.md
        - LICENSE
    workflow_dispatch:

  permissions:
    contents: read
    pages: write
    id-token: write

  concurrency:
    group: "pages"
    cancel-in-progress: true

  jobs:
    build:
      runs-on: ubuntu-latest
      steps:
        - name: Checkout
          uses: actions/checkout@v4
          with:
            fetch-depth: 0
        - name: Setup Pages
          id: pages
          uses: actions/configure-pages@v4
        - name: Setup Ruby
          uses: ruby/setup-ruby@v1
          with:
            ruby-version: 3.3
            bundler-cache: true
        - name: Build site
          run: bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
          env:
            JEKYLL_ENV: "production"
        - name: Upload site artifact
          uses: actions/upload-pages-artifact@v3
          with:
            path: "_site"

    deploy:
      environment:
        name: github-pages
        url: ${{ steps.deployment.outputs.page_url }}
      runs-on: ubuntu-latest
      needs: build
      steps:
        - name: Deploy to GitHub Pages
          id: deployment
          uses: actions/deploy-pages@v4
  ```

- [ ] **Step 2: 커밋**

  ```bash
  git add .github/workflows/pages-deploy.yml
  git commit -m "ci: GitHub Actions 자동 배포 워크플로 추가"
  ```

---

## Task 4: _config.yml 작성

**Files:**
- Create: `_config.yml`

- [ ] **Step 1: _config.yml 생성**

  `C:/Users/hwaso/Hwasowl.github.io/_config.yml` 파일 생성:
  ```yaml
  theme: jekyll-theme-chirpy

  lang: ko-KR
  timezone: Asia/Seoul

  title: Hwasowl
  tagline: 백엔드 개발자의 기술 노트
  description: >-
    Java, Spring Boot 백엔드 개발 블로그. 트러블슈팅, 기술 결정, 회고를 기록합니다.

  url: "https://hwasowl.github.io"
  baseurl: ""

  github:
    username: Hwasowl

  social:
    name: Hwasowl
    email: hwasowl598@gmail.com
    links:
      - https://github.com/Hwasowl

  theme_mode: # 비워두면 OS 설정 자동 감지, 우측 상단 토글로 수동 전환 가능

  avatar: # /assets/img/avatar.jpg — 프로필 사진 추가 시 주석 해제

  toc: true

  comments:
    provider: giscus
    giscus:
      repo: Hwasowl/Hwasowl.github.io
      repo_id:       # Task 8에서 giscus.app에서 발급 후 입력
      category: Announcements
      category_id:   # Task 8에서 giscus.app에서 발급 후 입력
      mapping: pathname
      strict: 0
      input_position: bottom
      lang: ko
      reactions_enabled: 1

  paginate: 10

  kramdown:
    footnote_backlink: "&#8617;&#xfe0e;"
    syntax_highlighter: rouge
    syntax_highlighter_opts:
      css_class: highlight
      block:
        line_numbers: true
        start_line: 1

  collections:
    tabs:
      output: true
      sort_by: order

  defaults:
    - scope:
        path: ""
        type: posts
      values:
        layout: post
        comments: true
        toc: true
        permalink: /posts/:title/
    - scope:
        path: _drafts
      values:
        comments: false
    - scope:
        path: ""
        type: tabs
      values:
        layout: page
        permalink: /:title/

  sass:
    style: compressed

  compress_html:
    clippings: all
    comments: all
    endings: all
    profile: false
    blanklines: false
    ignore:
      envs: [development]

  exclude:
    - "*.gem"
    - "*.gemspec"
    - docs
    - tools
    - README.md
    - LICENSE
    - "*.config.js"
    - package-lock.json

  jekyll-archives:
    enabled: [categories, tags]
    layouts:
      category: category
      tag: tag
    permalinks:
      tag: /tags/:name/
      category: /categories/:name/
  ```

- [ ] **Step 2: 커밋**

  ```bash
  git add _config.yml
  git commit -m "feat: 사이트 기본 설정 추가 (한국어, 다크/라이트 모드)"
  ```

---

## Task 5: 탭 페이지 생성

**Files:**
- Create: `_tabs/about.md`
- Create: `_tabs/archives.md`
- Create: `_tabs/categories.md`
- Create: `_tabs/tags.md`

- [ ] **Step 1: _tabs 디렉토리 생성 및 about.md 작성**

  `C:/Users/hwaso/Hwasowl.github.io/_tabs/about.md`:
  ```markdown
  ---
  icon: fas fa-info-circle
  order: 4
  ---

  백엔드 개발자 Hwasowl의 기술 블로그입니다.

  Java, Spring Boot를 주로 다루며, 개발 회고와 트러블슈팅 경험을 기록합니다.

  - **GitHub**: [Hwasowl](https://github.com/Hwasowl)
  - **Email**: hwasowl598@gmail.com
  ```

- [ ] **Step 2: archives.md 작성**

  `C:/Users/hwaso/Hwasowl.github.io/_tabs/archives.md`:
  ```markdown
  ---
  layout: archives
  icon: fas fa-archive
  order: 2
  ---
  ```

- [ ] **Step 3: categories.md 작성**

  `C:/Users/hwaso/Hwasowl.github.io/_tabs/categories.md`:
  ```markdown
  ---
  layout: categories
  icon: far fa-folder-open
  order: 1
  ---
  ```

- [ ] **Step 4: tags.md 작성**

  `C:/Users/hwaso/Hwasowl.github.io/_tabs/tags.md`:
  ```markdown
  ---
  layout: tags
  icon: fas fa-tags
  order: 3
  ---
  ```

- [ ] **Step 5: 커밋**

  ```bash
  git add _tabs/
  git commit -m "feat: 탭 페이지 추가 (About, Archives, Categories, Tags)"
  ```

---

## Task 6: 포스트 템플릿 작성

**Files:**
- Create: `_posts/0000-00-00-template.md`

- [ ] **Step 1: _posts 디렉토리 생성 및 템플릿 작성**

  `C:/Users/hwaso/Hwasowl.github.io/_posts/0000-00-00-template.md`:
  ```markdown
  ---
  title: "[Retrospective] 제목 (N주차 · K팀 · 이름)"
  date: YYYY-MM-DD HH:MM:SS +0900
  categories: [Retrospective]
  tags: [키워드1, 키워드2]
  ---

  ## TL;DR

  5초 안에 핵심을 전하는 1~2줄.

  예) Redisson 분산 락으로 동시성 문제를 해결했고, 정합성 실패율 3% → 0%로 개선.

  ---

  ## 개요 (Overview)

  - **목표**:
  - **중점 사항**:

  ---

  ## 핵심 기술 및 결정 (Technical Decisions)

  | 기술/설계 항목 | 선택한 대안 | 선택 이유 (Rationale) |
  |---|---|---|
  |  |  |  |

  작성 팁: 기술 결정은 "왜 그 선택을 했는가"가 핵심.

  ---

  ## 트러블슈팅 (Troubleshooting)

  ### 문제 현상

  ### 원인 분석

  -

  ### 해결 방안

  -

  작성 팁: 수치를 곁들이면 강력해진다.

  ---

  ## 회고 (Retrospective)

  - **Keep**:
  - **Problem**:
  - **Try**:
  ```

- [ ] **Step 2: 커밋**

  ```bash
  git add _posts/0000-00-00-template.md
  git commit -m "docs: 테크니컬 라이팅 포스트 템플릿 추가"
  ```

---

## Task 7: 로컬 빌드 검증

- [ ] **Step 1: 로컬 서버 실행**

  ```powershell
  cd C:\Users\hwaso\Hwasowl.github.io
  bundle exec jekyll serve
  ```
  Expected: `Server running... press ctrl-c to stop.` 출력, `http://127.0.0.1:4000` 접속 가능.

  **오류 발생 시 체크:**
  - `bundle install` 재실행
  - Ruby 버전 확인 (`ruby -v`)
  - `_config.yml` YAML 문법 오류 확인

- [ ] **Step 2: 브라우저 확인 항목**

  `http://127.0.0.1:4000` 에서 확인:
  - [ ] 사이드바에 "Hwasowl" 이름 표시
  - [ ] 우측 상단 다크/라이트 토글 버튼 존재
  - [ ] 상단 탭 메뉴 (Categories, Archives, Tags, About) 표시
  - [ ] About 페이지 접속 시 소개 내용 렌더링

---

## Task 8: GitHub Pages 활성화 및 첫 배포

- [ ] **Step 1: GitHub Pages Source 설정 (수동 — 브라우저)**

  1. https://github.com/Hwasowl/Hwasowl.github.io → **Settings** 탭
  2. 좌측 메뉴 **Pages** 클릭
  3. **Build and deployment** → Source → **"GitHub Actions"** 선택
  4. 저장

- [ ] **Step 2: 전체 변경사항 push**

  ```bash
  cd C:\Users\hwaso\Hwasowl.github.io
  git push origin main
  ```

- [ ] **Step 3: GitHub Actions 빌드 확인**

  https://github.com/Hwasowl/Hwasowl.github.io/actions 에서:
  - "Build and Deploy" 워크플로 실행 확인
  - 빌드·배포 단계 모두 녹색(✓) 확인

  **빌드 실패 시:** Actions 로그에서 오류 메시지 확인 후 수정.

- [ ] **Step 4: 라이브 사이트 확인**

  https://hwasowl.github.io 접속 후 확인:
  - [ ] 블로그 홈 렌더링
  - [ ] 다크/라이트 토글 동작
  - [ ] About 페이지 정상 표시

---

## Task 9: Giscus 댓글 연동

- [ ] **Step 1: GitHub Discussions 활성화 (수동 — 브라우저)**

  1. https://github.com/Hwasowl/Hwasowl.github.io → **Settings** 탭
  2. **Features** 섹션 → **Discussions** 체크박스 활성화

- [ ] **Step 2: Giscus ID 발급 (수동 — 브라우저)**

  1. https://giscus.app 접속
  2. **Repository** 필드에 `Hwasowl/Hwasowl.github.io` 입력
  3. **Discussion Category** → `Announcements` 선택
  4. **Page ↔️ Discussions Mapping** → `pathname` 선택
  5. 하단 **Enable giscus** 섹션에서 `data-repo-id` 값 복사 → `repo_id`
  6. `data-category-id` 값 복사 → `category_id`

- [ ] **Step 3: _config.yml에 ID 입력**

  `_config.yml`의 giscus 블록 수정:
  ```yaml
  giscus:
    repo: Hwasowl/Hwasowl.github.io
    repo_id: "발급받은_repo_id_값"       # ← 여기
    category: Announcements
    category_id: "발급받은_category_id_값"  # ← 여기
    mapping: pathname
    strict: 0
    input_position: bottom
    lang: ko
    reactions_enabled: 1
  ```

- [ ] **Step 4: 커밋 및 배포**

  ```bash
  git add _config.yml
  git commit -m "feat: Giscus 댓글 연동"
  git push origin main
  ```

- [ ] **Step 5: 댓글창 확인**

  배포 완료(약 2~3분) 후 https://hwasowl.github.io 에서 포스트 페이지 하단에 댓글창 렌더링 확인.
  - [ ] 다크 모드 전환 시 댓글창 테마도 자동 전환 확인

---

## 성공 기준 체크리스트

- [ ] `https://hwasowl.github.io` 접속 시 블로그 렌더링
- [ ] 다크/라이트 모드 토글 동작
- [ ] 포스트 코드 하이라이팅·TOC 표시
- [ ] Giscus 댓글창 노출 및 다크/라이트 자동 연동
- [ ] `_posts/0000-00-00-template.md` 포스트 템플릿 제공
