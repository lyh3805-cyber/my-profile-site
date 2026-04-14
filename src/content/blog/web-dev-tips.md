---
title: "프론트엔드 개발자가 알아야 할 CSS 팁 5가지"
description: "실무에서 자주 쓰는 CSS 테크닉을 정리했습니다."
pubDate: 2026-04-10
tags: ["CSS", "Frontend", "팁"]
---

## 1. `clamp()`으로 반응형 폰트 크기

미디어 쿼리 없이 화면 크기에 따라 자동으로 폰트 크기를 조절합니다.

```css
h1 {
  font-size: clamp(1.5rem, 5vw, 3rem);
}
```

## 2. CSS Grid `subgrid`

중첩된 그리드 아이템을 부모 그리드에 맞출 수 있습니다.

```css
.card-grid { display: grid; grid-template-columns: repeat(3, 1fr); }
.card { display: grid; grid-row: span 3; grid-template-rows: subgrid; }
```

## 3. `@layer`로 CSS 우선순위 관리

Tailwind와 함께 쓸 때 특히 유용합니다.

```css
@layer base, components, utilities;

@layer components {
  .btn { @apply px-4 py-2 rounded-lg font-semibold; }
}
```

## 4. `container` 쿼리

부모 요소의 크기에 반응하는 컴포넌트를 만들 수 있습니다.

```css
.card-wrapper { container-type: inline-size; }

@container (min-width: 400px) {
  .card { flex-direction: row; }
}
```

## 5. `scroll-driven` 애니메이션

JavaScript 없이 스크롤 기반 애니메이션을 구현합니다.

```css
@keyframes fade-in {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}

.reveal {
  animation: fade-in linear both;
  animation-timeline: view();
  animation-range: entry 0% cover 30%;
}
```

---

CSS는 계속 진화하고 있습니다. 최신 기능을 잘 활용하면 JavaScript에 의존하지 않고도
풍부한 인터랙션을 구현할 수 있습니다.
