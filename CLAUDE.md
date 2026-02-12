# Next.js 16 + React 19 + Tailwind v4 Starter

이 프로젝트는 Next.js 16 App Router와 Tailwind CSS v4를 사용하는 현대적인 스타터 템플릿입니다.

## 개발 명령어

```bash
npm run dev      # Turbopack 개발 서버 (http://localhost:3000)
npm run build    # 프로덕션 빌드
npm run start    # 프로덕션 서버 시작
npm run lint     # ESLint 실행
```

## 기술 스택

- **Next.js 16.1.6** - App Router 기반
- **React 19.2.3** - 최신 React 기능
- **TypeScript 5** - strict mode 활성화
- **Tailwind CSS v4** - 새로운 CSS 우선 설정 방식
- **next/font/google** - Geist 폰트 최적화

## 프로젝트 구조

```
app/
├── layout.tsx      # Root Layout (폰트, 메타데이터, HTML 구조)
├── page.tsx        # 홈 페이지
└── globals.css     # 글로벌 스타일 (Tailwind v4 설정)
```

## 아키텍처

### Tailwind CSS v4

이 프로젝트는 Tailwind CSS v4를 사용합니다. v3와 다른 점:

- **설정 파일 없음**: `tailwind.config.js` 대신 `app/globals.css`에서 `@theme inline` 사용
- **CSS 우선**: 모든 테마 설정이 CSS 파일 내에서 직접 정의됨
- **자동 다크 모드**: CSS 변수(`--background`, `--foreground` 등) 기반으로 미디어 쿼리 없이 자동 지원

### 폰트 시스템

`next/font/google`으로 Geist 폰트를 최적화:

```tsx
const geistSans = Geist_Font({
  variable: "--font-geist-sans",
});
```

CSS 변수로 폰트 적용:
```css
font-family: var(--font-geist-sans);
```

### TypeScript 설정

`tsconfig.json`에서 경로 별칭 설정:
- `@/*` → 프로젝트 루트 (`./`)

## 중요 파일

### `app/layout.tsx`
- 루트 레이아웃 컴포넌트
- Geist 폰트 설정 및 CSS 변수 정의
- 메타데이터 (타이틀, 설명)
- 자동 다크 모드를 위한 `className` 설정

### `app/globals.css`
- Tailwind v4 설정 (`@import "tailwindcss";`)
- `@theme inline` 블록에서 테마 색상 정의
- CSS 변수를 통한 다크 모드 자동 지원

### `app/page.tsx`
- 홈 페이지 (Server Component)

## 개발 가이드

### 새로운 컴포넌트 추가

기본적으로 모든 컴포넌트는 **Server Component**입니다:

```tsx
// Server Component (기본)
export default function MyComponent() {
  return <div>Hello</div>;
}
```

클라이언트 상태(`useState`, `useEffect` 등)가 필요한 경우:

```tsx
'use client';

import { useState } from 'react';

export default function InteractiveComponent() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```

### 스타일 추가

Tailwind 클래스를 직접 사용:

```tsx
<div className="flex items-center gap-4 bg-background text-foreground">
```

사용자 정의 색상은 `app/globals.css`의 `@theme inline`에 추가:

```css
@theme inline {
  --color-my-custom: #hexcode;
}
```

## 주의사항

1. **Tailwind v4 문법**: 기존 v3 프로젝트와 설정 방식이 다름
2. **Server Component 우선**: 클라이언트 기능이 필요한 경우에만 `'use client'` 추가
3. **폰트 변수**: `var(--font-geist-sans)` 또는 `var(--font-geist-mono)` 사용
4. **Turbopack**: 개발 서버는 Turbopack 사용으로 더 빠른 핫 리로드
