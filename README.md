# Next.js 16 + React 19 + Tailwind v4 Starter

최신 Next.js 16 App Router와 React 19, Tailwind CSS v4를 사용하는 현대적인 스타터 템플릿입니다.

## 기술 스택

- **Next.js 16.1.6** - App Router + Turbopack
- **React 19.2.3** - 최신 React 기능
- **TypeScript 5** - Strict 모드 활성화
- **Tailwind CSS v4** - CSS 우선 설정 방식
- **shadcn/ui** - Radix UI 기반 컴포넌트 라이브러리
- **Prettier** - 코드 포맷팅

## 개발 명령어

```bash
npm run dev          # Turbopack 개발 서버 (http://localhost:3000)
npm run build        # 프로덕션 빌드
npm run start        # 프로덕션 서버 시작
npm run lint         # ESLint 실행
npm run format       # Prettier 포맷팅
npm run format:check # 포맷팅 검증
```

## 프로젝트 구조

```
src/
├── app/
│   ├── layout.tsx      # Root Layout (폰트, 메타데이터, HTML 구조)
│   ├── page.tsx        # 홈 페이지
│   └── globals.css     # 글로벌 스타일 (Tailwind v4 설정)
lib/
└── utils.ts            # 유틸리티 함수 (cn, clsx)
```

## Claude Code 설정

이 프로젝트는 Claude Code 개발 경험을 최적화하기 위한 설정이 포함되어 있습니다.

### MCP Servers

프로젝트 레벨에서 구성된 MCP 서버들 (`.mcp.json`):

| 서버                    | 설명                                         |
| ----------------------- | -------------------------------------------- |
| **shrimp-task-manager** | 태스크 계획, 분석, 실행, 검증 관리           |
| **shadcn**              | shadcn/ui 컴포넌트 검색 및 추가              |
| **context7**            | 최신 라이브러리 문서 및 코드 예제 검색       |
| **sequential-thinking** | 복잡한 문제 해결을 위한 순차적 사고 프로세스 |

### Agents

전문화된 에이전트들 (`.claude/agents/`):

- **code-reviewer** - 코드 변경사항 리뷰 및 피드백 제공
- **security-vulnerability-scanner** - 보안 취약점 스캔 및 분석

### Skills

재사용 가능한 스킬들 (`.claude/skills/`):

- **git-commit** - 변경사항을 자동 분류하여 논리적인 커밋 생성
- **playwright-cli** - 브라우저 자동화, 스크린샷, 폼 작성, 데이터 추출

### Hooks

자동화된 작업 훅 (`.claude/settings.json`):

- **PostToolUse** - `Edit`, `Write` 툴 사용 후 자동 Prettier 포맷팅 실행

## 주요 기능

### Tailwind CSS v4

- `@import "tailwindcss"` 방식 사용
- `@theme inline` 블록에서 테마 설정
- CSS 변수 기반 자동 다크 모드 지원

### shadcn/ui 구성

- Radix UI 컴포넌트 기반
- Lucide React 아이콘
- `class-variance-authority`로 변형 관리
- `tailwind-merge`로 클래스 병합

### 폰트

- Geist Sans (본문)
- Geist Mono (코드)

## 추가 설정

### ESLint

`eslint.config.mjs` - Next.js 권장 설정

### Prettier

- `.prettierrc` - Prettier 설정
- `prettier-plugin-tailwindcss` - Tailwind 클래스 정렬
- PostToolUse 훅으로 자동 포맷팅

### TypeScript

- `tsconfig.json` - 경로 별칭 (`@/*` → `./src`)

## Claude Code 사용 팁

```bash
# 태스크 관리 (shrimp-task-manager MCP 사용)
# /context에서 MCP 도구로 태스크 생성, 관리

# shadcn 컴포넌트 추가
# /component-add [컴포넌트명]

# 코드 리뷰 요청
# 코드 변경 후 자동으로 code-reviewer agent 실행

# 커밋 생성
# /git:commit 으로 태스크별/기능별 자동 커밋
```

## 라이선스

MIT
