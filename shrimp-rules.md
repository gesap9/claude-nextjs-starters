# Development Guidelines

## Project Overview

Next.js 16 + React 19 + Tailwind CSS v4 기반 스타터 템플릿 프로젝트.

**기술 스택:**
- Next.js 16.1.6 (App Router)
- React 19.2.3
- TypeScript 5 (strict mode)
- Tailwind CSS v4
- next/font/google (Geist 폰트)

## Project Architecture

```
app/
├── layout.tsx      # Root Layout - 폰트, 메타데이터, HTML 구조 정의
├── page.tsx        # 홈 페이지 (Server Component)
└── globals.css     # 글로벌 스타일 + Tailwind v4 테마 설정
```

**핵심 파일:**
- `app/layout.tsx` - 루트 레이아웃, 폰트 설정, 메타데이터
- `app/globals.css` - Tailwind v4 설정, 테마 색상, CSS 변수
- `app/page.tsx` - 홈 페이지 (Server Component 참조용)

## Code Standards

### TypeScript
- strict mode 활성화 상태 유지
- 경로 별칭: `@/*` → 프로젝트 루트 (`./`)

### 명명 규칙
- 컴포넌트 파일: PascalCase (예: `MyComponent.tsx`)
- 유틸리티 파일: camelCase (예: `formatDate.ts`)
- 타입 정의: PascalCase (예: `UserProps`)

## Functionality Implementation Standards

### Server Component vs Client Component

**기본 규칙:** 모든 컴포넌트는 Server Component로 작성.

**'use client' 지시문 추가 기준:**
다음 중 하나라도 필요한 경우에만 추가:

1. React Hooks 사용 (`useState`, `useEffect`, `useRef` 등)
2. 이벤트 핸들러 직접 정의 (`onClick`, `onChange` 등)
3. 브라우저 API 직접 사용 (`window`, `document` 등)

**의사결정 트리:**
```
새 컴포넌트 생성
  ↓
useState/useEffect 필요?
  → 예: 'use client' 추가
  → 아니오: 이벤트 핸들러 필요?
      → 예: 'use client' 추가
      → 아니오: Server Component로 작성
```

**예시:**
```tsx
// ✅ 올바른 Server Component
export default function MyComponent() {
  return <div>Hello</div>;
}

// ✅ 올바른 Client Component
'use client';
import { useState } from 'react';
export default function InteractiveComponent() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}

// ❌ 잘못된 예: 불필요한 'use client'
'use client';
export default function StaticContent() {
  return <div>정적 콘텐츠</div>;
}
```

## Tailwind CSS v4 Usage Standards

### 핵심 규칙

**Tailwind CSS v4는 v3와 설정 방식이 다름:**

1. **설정 파일 없음** - `tailwind.config.js` 또는 `tailwind.config.ts` 파일을 절대 생성하지 말 것
2. **CSS 우선 설정** - 모든 테마 설정은 `app/globals.css`의 `@theme inline` 블록 내에서만 수정

### 색상 추가 방법

새로운 색상이 필요한 경우 `app/globals.css`의 `@theme inline` 블록에 추가:

```css
@theme inline {
  --color-my-custom: #hexcode;
}
```

### 스타일 적용 방법

```tsx
// ✅ Tailwind 클래스 직접 사용
<div className="flex items-center gap-4 bg-background text-foreground">

// ❌ tailwind.config.js로 색상 추가 금지
```

### 스타일 변경 의사결정 트리

```
스타일 변경 필요
  ↓
기존 Tailwind 클래스로 가능?
  → 예: className에 클래스 추가
  → 아니오: globals.css의 @theme에 CSS 변수 추가
```

## Font System Standards

### 폰트 추가 절차

1. `app/layout.tsx`에서 `next/font/google`로 폰트 로드
2. CSS 변수 형식으로 정의 (`--font-*`)
3. `globals.css`에서 해당 변수 사용
4. 필요한 컴포넌트에서 `className`으로 적용

**예시:**
```tsx
// app/layout.tsx
import { GeistSans } from 'next/font/google';

const geistSans = GeistSans({
  variable: '--font-geist-sans',
});
```

```css
/* globals.css */
font-family: var(--font-geist-sans);
```

### 폰트 변경 의사결정 트리

```
폰트 변경 필요
  ↓
next/font/google로 폰트 로드
  ↓
CSS 변수 정의 (--font-*)
  ↓
layout.tsx에 적용
```

## Key File Interaction Standards

### 파일 간 의존관계

| 수정 파일 | 연관 파일 | 동시 수정 필요 여부 |
|-----------|-----------|-------------------|
| `app/layout.tsx` | 없음 | 독립적 |
| `app/globals.css` | `app/layout.tsx` | 색상 변수 변경 시 className 영향 확인 |
| 새 페이지 (`app/*/page.tsx`) | `app/page.tsx` | Server Component 패턴 참조 |

### 새로운 페이지 추가

새 페이지 추가 시 `app/page.tsx`를 참조하여 Server Component 패턴을 따르십시오.

## AI Decision-Making Standards

### 의사결정 우선순위

1. **불필요한 'use client' 방지** - 기본적으로 Server Component 우선
2. **Tailwind v4 규칙 준수** - 설정 파일 생성 금지
3. **최소한의 변경** - 요청된 기능만 구현, 과도한 리팩토링 금지

### 모호한 상황 처리

```
기능 추가 요청
  ↓
Server Component로 가능?
  → 예: Server Component로 구현
  → 아니오: 'use client' 추가 후 구현
```

## Prohibited Actions

### 절대 금지 사항

1. **Tailwind 설정 파일 생성 금지**
   - ❌ `tailwind.config.js` 생성
   - ❌ `tailwind.config.ts` 생성
   - ✅ `app/globals.css`의 `@theme inline`만 사용

2. **Pages Router 사용 금지**
   - ❌ `pages/` 디렉토리 사용
   - ❌ `_app.tsx`, `_document.tsx` 파일 생성/수정
   - ✅ `app/layout.tsx`만 사용

3. **불필요한 'use client' 추가 금지**
   - ❌ 단순 HTML 반환 컴포넌트에 'use client' 추가
   - ✅ React Hooks/이벤트 핸들러 필요 시만 추가

4. **자동 코드 포맷팅 금지**
   - ❌ 사용자 요청 없는 Prettier 실행
   - ❌ 사용자 요청 없는 ESLint --fix 실행
   - ✅ TypeScript/JS/CSS 변경 작업만 수행

5. **다크 모드 별도 구현 금지**
   - ❌ 수동 다크 모드 스위치 구현
   - ✅ CSS 변수(`--background`, `--foreground`) 기반 자동 처리 의존

6. **일반 지식 포함 금지**
   - ❌ LLM이 이미 알고 있는 일반 React/Next.js 지식 설명
   - ✅ 프로젝트 특화 규칙만 명시

### 허용되는 작업

- ✅ `app/` 디렉토리 내 새로운 route/page.tsx 추가
- ✅ `app/globals.css`의 `@theme inline` 블록에 색상 변수 추가
- ✅ `app/layout.tsx`에서 next/font/google으로 폰트 로드
- ✅ TypeScript 경로 별칭(`@/*`) 사용
- ✅ Server Component 패턴 따르는 컴포넌트 생성
