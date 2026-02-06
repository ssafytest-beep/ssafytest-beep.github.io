---
title: Jekyll + Chirpy로 블로그 배포 자동화
date: 2026-02-06 15:39:01 +0900
categories:
- General
tags: []
---

# DevLog / TIL: Jekyll + Chirpy 배포 자동화 및 DevContainer 학습 기록

> 구조: 문제 인식 -> 탐색 -> 해결 -> 통찰

## 문제 인식 (Problem)
> Describe the 문제 상황을 간단히 적어두고, 이 글에서 다루려는 목표를 명시합니다.
> 예: Chirpy 테마를 사용하는 Jekyll 사이트를 GitHub Pages에 안정적으로 배포하고, 개발 환경의 일관성을 확보하는 방법을 기록합니다.

## 탐색 (Exploration)
> 어떤 방법들을 시도했는지, 각 시도에서 얻은 교훈과 실패 포인트를 정리합니다.
> - 시도 1: DevContainer를 이용한 로컬 개발 환경 구성
> - 시도 2: GitHub Actions를 활용한 CI/CD 파이프라인 구성
> - 시도 3: 포스트 메타데이터 자동화(마지막 수정 시간)와 HTML 검증 도구의 도입

## 해결 (Solution)
> 실제로 적용한 해결책과 구성의 핵심 요소를 구체적으로 기술합니다.
> - 배포: Jekyll 빌드 -> Pages 배포 워크플로우
> - 개발환경: DevContainer 설정과 기본 확장 설치
> - 메타데이터: 마지막 수정 시점 자동 채우기 플러그인, HTMLProofer 실행

## 통찰 (Insight)
> 이 경험에서 얻은 교훈과 앞으로의 개선 방향을 서술합니다.
> 예: CI/CD의 중요성, 팀 차원의 개발환경 표준화의 이점, 메타데이터의 정확성으로 인한 품질 개선

> 개인 생각을 여기에 자유롭게 기록합니다.

```yaml
name: Build and Deploy
on:
  push:
    branches: [ main, master ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.1'
      - run: gem install bundler
      - run: bundle install
      - run: bundle exec jekyll build
      - uses: peaceiris/actions-gh-pages@v3
        with:
          publish_dir: ./_site
          github_token: ${{ secrets.GITHUB_TOKEN }}
```