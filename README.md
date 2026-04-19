# 개인 개발 블로그 — Notion CMS

> Notion을 CMS로 활용한 개인 기술 블로그입니다.  
> Notion에서 글을 작성하면 별도 배포 없이 자동으로 블로그에 반영됩니다.

**GitHub 저장소:** [새로운 GitHub 저장소 링크]  
**배포 URL:** Vercel 배포 후 업데이트 예정

---

## 프로젝트 개요

| 항목       | 내용                                   |
| ---------- | -------------------------------------- |
| 프로젝트명 | 개인 개발 블로그                       |
| 목적       | Notion을 CMS로 활용한 개인 기술 블로그 |
| CMS        | Notion API                             |
| 배포       | Vercel                                 |

### Notion을 CMS로 선택한 이유

- 별도 관리자 페이지 없이 익숙한 Notion 에디터로 글 작성 가능
- 글 발행/수정이 즉시 블로그에 반영
- 카테고리, 태그, 발행 상태를 데이터베이스 속성으로 간편 관리

---

## 기술 스택

| 분류       | 기술                            |
| ---------- | ------------------------------- |
| Frontend   | Next.js 15, TypeScript          |
| CMS        | Notion API (`@notionhq/client`) |
| Styling    | Tailwind CSS, shadcn/ui         |
| Icons      | Lucide React                    |
| Deployment | Vercel                          |

---

## 주요 기능

| 기능            | 설명                                               |
| --------------- | -------------------------------------------------- |
| 글 목록 조회    | Notion DB에서 `발행됨` 상태의 글을 최신순으로 표시 |
| 글 상세 페이지  | Notion 페이지 블록을 HTML로 변환하여 렌더링        |
| 카테고리 필터링 | 카테고리별 글 목록 페이지 (`/category/[slug]`)     |
| 검색 기능       | 제목 및 태그 기반 클라이언트 사이드 검색           |
| 반응형 디자인   | 모바일 / 태블릿 / 데스크탑 대응                    |

---

## 화면 구성

```
/                       → 홈: 최근 글 목록 + 검색 + 카테고리 탭
/posts/[slug]           → 글 상세: 본문 + 메타 정보
/category/[category]    → 카테고리별 글 목록
```

---

## 시작하기

### 1. 저장소 클론

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
```

### 2. 패키지 설치

```bash
npm install
```

### 3. 환경 변수 설정

`.env.local` 파일을 생성하고 아래 값을 입력합니다.

```env
NOTION_API_KEY=secret_xxxxxxxxxxxxxxxxxxxx
NOTION_DATABASE_ID=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

> Notion Integration 생성 방법: [Notion 개발자 문서](https://developers.notion.com/docs/getting-started)

### 4. 개발 서버 실행

```bash
npm run dev
```

브라우저에서 [http://localhost:3000](http://localhost:3000)을 엽니다.

---

## Notion 데이터베이스 구조

| 필드      | 타입         | 설명                                         |
| --------- | ------------ | -------------------------------------------- |
| Title     | title        | 글 제목                                      |
| Category  | select       | 카테고리 (예: React, Next.js, TypeScript)    |
| Tags      | multi_select | 태그 (복수 선택 가능)                        |
| Published | date         | 발행일                                       |
| Status    | select       | `초안` / `발행됨` — `발행됨`만 블로그에 노출 |
| Content   | page content | 본문 (Notion 페이지 블록)                    |

---

## 프로젝트 구조

```
├── docs/
│   └── PRD.md                  # 제품 요구사항 문서
├── src/
│   ├── app/
│   │   ├── page.tsx             # 홈 (글 목록)
│   │   ├── posts/[slug]/
│   │   │   └── page.tsx         # 글 상세
│   │   └── category/[category]/
│   │       └── page.tsx         # 카테고리별 목록
│   ├── components/              # UI 컴포넌트 (shadcn/ui 기반)
│   └── lib/
│       └── notion.ts            # Notion API 클라이언트 및 fetching 함수
├── public/
├── .env.local                   # 환경 변수 (git 제외)
└── README.md
```

---

## MVP 범위

| 기능            | 포함 |
| --------------- | :--: |
| Notion API 연동 |  ✅  |
| 글 목록 페이지  |  ✅  |
| 글 상세 페이지  |  ✅  |
| 카테고리 필터   |  ✅  |
| 검색 기능       |  ✅  |
| 반응형 디자인   |  ✅  |
| RSS 피드        |  ❌  |
| 댓글 기능       |  ❌  |
| 다크 모드       |  ❌  |

---

## 구현 단계

1. **환경 설정** — Next.js 15 프로젝트 생성 및 패키지 설치
2. **Notion 설정** — Integration 생성, DB 구조 설정, API 키 연결
3. **글 목록 페이지** — `lib/notion.ts` 작성, ISR 적용
4. **글 상세 페이지** — 동적 라우트 및 Notion 블록 렌더러 구현
5. **스타일링 및 최적화** — shadcn/ui 적용, 반응형 완성, Vercel 배포

자세한 내용은 [docs/PRD.md](./docs/PRD.md)를 참고하세요.

---

## 배포

Vercel 배포 시 환경 변수를 프로젝트 설정에 추가합니다.

```
NOTION_API_KEY
NOTION_DATABASE_ID
```

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new)

---

## 라이선스

MIT
