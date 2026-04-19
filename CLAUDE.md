# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev       # 개발 서버 (http://localhost:4321)
npm run build     # 정적 사이트 빌드 → dist/
npm run preview   # 빌드 결과 미리보기
npx prettier --write src/  # 포맷 (Write/Edit 후 훅이 자동 실행)
```

테스트 러너 없음. 빌드 오류는 `npm run build`로 확인한다.

## Architecture

**Astro 4 + Tailwind CSS 정적 사이트.** 싱글페이지 포트폴리오(`/`) + 마크다운 블로그(`/blog`)로 구성된다.

### 라우팅 구조

```
src/pages/index.astro          → 포트폴리오 메인 (SPA 스타일, 섹션 앵커)
src/pages/blog/index.astro     → 블로그 목록
src/pages/blog/[...slug].astro → 개별 포스트 (getStaticPaths + Content Collections)
```

### 레이아웃 계층

- `BaseLayout.astro` — `<html>` 뼈대, 다크 배경(`bg-[#1a1730]`), SEO 메타
- `BlogLayout.astro` — BaseLayout 확장, 포스트 헤더(제목·날짜·태그) + `prose-dark` 아티클 래퍼

### 컴포넌트 구성 (포트폴리오)

`index.astro`가 순서대로 조합: `Navbar → Hero → About → Skills → Projects → Contact → Footer`  
각 컴포넌트는 독립적이며, 섹션 id(`#hero`, `#about` 등)로 앵커 링크가 연결된다.

### 스타일 시스템

모든 공유 클래스는 `src/styles/global.css`의 `@layer components / utilities`에 정의된다.  
새 컴포넌트 작성 시 아래 커스텀 클래스를 **우선 재사용**한다:

| 클래스                | 용도                                          |
| --------------------- | --------------------------------------------- |
| `gradient-text`       | 보라→핑크→황금 그라디언트 텍스트              |
| `btn-glow`            | 그라디언트 배경 + 보라 글로우 버튼            |
| `project-card`        | 유리형 카드 (호버 시 상단 그라디언트 바 노출) |
| `contact-card`        | 글로우 테두리 카드                            |
| `section-divider`     | 파랑→보라→그린 구분선                         |
| `fade-in` / `visible` | IntersectionObserver 기반 진입 애니메이션     |
| `prose-dark`          | 블로그 본문 다크 모드 타이포그래피            |

### Content Collections (블로그)

스키마는 `src/content/config.ts`에 정의:

```ts
{ title: string; description?: string; pubDate: Date; tags: string[] }
```

새 포스트는 `src/content/blog/*.md`에 위 frontmatter를 포함해 추가한다.  
`/new-post` 커스텀 커맨드로 포스트 파일을 자동 생성할 수 있다.

### 브랜드 색상 (tailwind.config.mjs)

```js
brand.blue   '#3b82f6'
brand.purple '#8b5cf6'
brand.green  '#34d399'
```

### 인라인 스크립트 패턴

Astro 컴포넌트 내 `<script>` 블록은 Astro가 번들링한다.  
Hero의 타이핑 애니메이션, Skills의 스킬바 등 인터랙션은 각 컴포넌트 하단 `<script>`에 위치한다.

## Prettier 훅

Write/Edit 도구 실행 후 Prettier가 자동으로 포맷을 적용한다 (`.claude/settings.json` 훅).  
`.prettierrc`: singleQuote, semi, tabWidth 2, printWidth 100, `prettier-plugin-astro`.
