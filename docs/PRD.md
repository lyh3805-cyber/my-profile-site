# PRD: 개인 개발 블로그 (Notion CMS)

## 1. 프로젝트 개요

| 항목       | 내용                                   |
| ---------- | -------------------------------------- |
| 프로젝트명 | 개인 개발 블로그                       |
| 목적       | Notion을 CMS로 활용한 개인 기술 블로그 |
| 배포 환경  | Vercel                                 |

### CMS로 Notion을 선택한 이유

Notion에서 글을 작성하면 별도의 배포 없이 자동으로 블로그에 반영됩니다. 익숙한 에디터 환경에서 콘텐츠를 관리할 수 있어 운영 부담이 낮습니다.

---

## 2. 기술 스택

| 분류       | 기술                            |
| ---------- | ------------------------------- |
| Frontend   | Next.js 15, TypeScript          |
| CMS        | Notion API (`@notionhq/client`) |
| Styling    | Tailwind CSS, shadcn/ui         |
| Icons      | Lucide React                    |
| Deployment | Vercel                          |

---

## 3. Notion 데이터베이스 구조

| 필드명    | 타입         | 설명                                      |
| --------- | ------------ | ----------------------------------------- |
| Title     | title        | 글 제목                                   |
| Category  | select       | 카테고리 (예: React, Next.js, TypeScript) |
| Tags      | multi_select | 태그 (복수 선택 가능)                     |
| Published | date         | 발행일                                    |
| Status    | select       | 상태 — `초안` / `발행됨`                  |
| Content   | page content | 본문 (Notion 페이지 블록)                 |

> **필터 조건**: Status가 `발행됨`인 글만 블로그에 노출합니다.

---

## 4. 주요 기능

### 4.1 글 목록 조회

- Notion 데이터베이스 API로 발행된 글 목록을 가져옵니다.
- 최신 발행일 기준 내림차순 정렬

### 4.2 글 상세 페이지

- Notion 페이지 블록을 HTML로 변환하여 렌더링
- 제목, 발행일, 카테고리, 태그 메타 정보 표시

### 4.3 카테고리 필터링

- 카테고리별 글 목록 페이지 제공
- URL 패턴: `/category/[slug]`

### 4.4 검색 기능

- 글 제목 및 태그 기반 클라이언트 사이드 검색

### 4.5 반응형 디자인

- 모바일 / 태블릿 / 데스크탑 뷰포트 대응

---

## 5. 화면 구성

```
/                       → 홈: 최근 글 목록
/posts/[slug]           → 글 상세: 개별 글 내용
/category/[category]    → 카테고리: 카테고리별 글 목록
```

### 홈 (`/`)

- 최근 발행 글 카드 목록
- 카테고리 필터 탭
- 검색 인풋

### 글 상세 (`/posts/[slug]`)

- 제목, 발행일, 카테고리 배지, 태그
- 본문 콘텐츠 (Notion 블록 렌더링)
- 목록으로 돌아가기 버튼

### 카테고리 (`/category/[category]`)

- 해당 카테고리에 속한 글 카드 목록

---

## 6. MVP 범위

| 기능                     | MVP 포함 여부 |
| ------------------------ | :-----------: |
| Notion API 연동          |       O       |
| 글 목록 페이지           |       O       |
| 글 상세 페이지           |       O       |
| 기본 스타일링 (Tailwind) |       O       |
| 반응형 디자인            |       O       |
| 카테고리 필터            |       O       |
| 검색 기능                |       O       |
| RSS 피드                 |       X       |
| 댓글 기능                |       X       |
| 다크 모드                |       X       |

---

## 7. 구현 단계

### 1단계 — 환경 설정

- Next.js 15 프로젝트 생성 (`create-next-app`)
- 패키지 설치: `@notionhq/client`, `tailwindcss`, `shadcn/ui`, `lucide-react`
- `.env.local` 설정: `NOTION_API_KEY`, `NOTION_DATABASE_ID`

### 2단계 — Notion 설정

- Notion Integration 생성 및 API 키 발급
- 데이터베이스 생성 (위 구조 적용)
- Integration을 데이터베이스에 연결 (Share → Invite)

### 3단계 — 글 목록 페이지 구현

- `lib/notion.ts`: Notion 클라이언트 및 데이터 fetching 함수 작성
- `app/page.tsx`: 글 카드 목록 렌더링
- ISR 적용 (`revalidate` 설정)

### 4단계 — 글 상세 페이지 구현

- `app/posts/[slug]/page.tsx`: 동적 라우트 생성
- Notion 블록 → JSX 변환 렌더러 구현
- `generateStaticParams`로 정적 생성

### 5단계 — 스타일링 및 최적화

- shadcn/ui 컴포넌트 적용 (Card, Badge, Input 등)
- 반응형 레이아웃 완성
- Vercel 배포 및 환경 변수 설정

---

## 8. 환경 변수

```env
# .env.local
NOTION_API_KEY=secret_xxxxxxxxxxxxxxxxxxxx
NOTION_DATABASE_ID=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

---

## 9. 주요 API 호출 패턴

```typescript
// 글 목록 조회
notion.databases.query({
  database_id: process.env.NOTION_DATABASE_ID,
  filter: {
    property: 'Status',
    select: { equals: '발행됨' },
  },
  sorts: [{ property: 'Published', direction: 'descending' }],
});

// 글 상세 조회
notion.pages.retrieve({ page_id: pageId });
notion.blocks.children.list({ block_id: pageId });
```
