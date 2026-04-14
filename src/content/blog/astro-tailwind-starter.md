---
title: "Astro + Tailwind로 포트폴리오 만들기"
description: "기존 HTML 사이트를 Astro 컴포넌트로 마이그레이션한 과정을 공유합니다."
pubDate: 2026-04-14
tags: ["Astro", "Tailwind", "포트폴리오"]
---

## 왜 Astro를 선택했나?

기존에 HTML/CSS/JS로 만든 포트폴리오 사이트가 있었지만, 점점 관리가 어려워지는 문제가 있었습니다.
컴포넌트를 재사용하고 싶고, 블로그 기능도 추가하고 싶어서 **Astro**를 선택했습니다.

Astro의 핵심 장점:

- **Zero JS by default** — 기본적으로 JavaScript를 클라이언트에 보내지 않아 빠릅니다.
- **Component Islands** — 필요한 부분만 인터랙티브하게 만들 수 있습니다.
- **Content Collections** — 마크다운 기반 블로그를 타입 안전하게 관리합니다.
- **Tailwind 통합** — `@astrojs/tailwind` 플러그인 하나로 즉시 사용 가능합니다.

## 프로젝트 구조

```
src/
├── components/   # 재사용 가능한 .astro 컴포넌트
├── content/      # 블로그 포스트 (.md 파일)
├── layouts/      # 페이지 레이아웃
├── pages/        # 라우팅 (파일 기반)
└── styles/       # 글로벌 CSS
```

## Tailwind 설정

`astro.config.mjs`에 통합을 추가하면 끝입니다:

```js
import tailwind from '@astrojs/tailwind';

export default defineConfig({
  integrations: [tailwind()],
});
```

## 마치며

HTML 파일 하나에서 시작한 사이트가 이제 컴포넌트 기반으로 정리되었고,
블로그 포스트를 마크다운으로 관리할 수 있게 되었습니다.

다음 포스트에서는 **Vercel 배포** 방법을 다뤄보겠습니다!
