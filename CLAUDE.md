# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Running the Application
```bash
npm run dev              # Start development server with Turbopack
npm run build           # Build for production
npm run start           # Start production server
```

### Code Quality
```bash
npm run lint            # Run ESLint
npm run type-check      # TypeScript type checking
npm run format          # Format code with Prettier
```

### Testing
```bash
npm test                # Run unit tests with Vitest
npm test -- <file>      # Run specific test file
npm test -- --watch     # Run tests in watch mode
npm run test:e2e        # Run E2E tests with Playwright
npm run test:e2e:ui     # Run E2E tests with Playwright UI
```

### Git Workflow
```bash
npm run prepare         # Install Husky hooks
git commit              # Commitizen automatically runs via Husky hook
npx cz                  # Manually run Commitizen for conventional commits
```

## Architecture Overview

This is a Next.js 15 starter template using the App Router with React 19, implementing a server-first architecture where all components are React Server Components by default.

### Key Directories
- `/src/app/` - App Router pages and API routes
- `/src/components/` - React components, with `/ui/` for reusable primitives
- `/src/lib/` - Core libraries and services
- `/src/utils/` - Utility functions and helpers
- `/src/types/` - TypeScript type definitions
- `/tests/` - Test files (unit tests in `/unit/`, E2E in `/e2e/`)

### Core Architecture Patterns

1. **Server-First Rendering**: All components are React Server Components by default. Only add `"use client"` when client-side interactivity is needed
2. **Client Wrapper Pattern**: Use the `ClientWrapper` component when server components need client-side features
3. **Path Aliases**: Use `@/` to import from the `src/` directory
4. **Styling System**:
   - Tailwind CSS with semantic CSS variables for theming (primary, secondary, muted, accent, destructive)
   - Dark mode support via CSS variables in `src/styles/globals.css`
   - Use `cn()` utility from `@/utils/cn` for conditional class merging
5. **Component Library**: shadcn/ui configured (New York style, RSC enabled) - components go in `/src/components/ui/`

### Testing Strategy
- Unit tests use Vitest with React Testing Library
- E2E tests use Playwright with automatic dev server startup
- Test files should be placed in the `/tests/` directory, not colocated with source files

### Code Standards
- TypeScript strict mode is enabled - ensure all code is properly typed
- ESLint and Prettier are configured - code must pass linting and type-checking before commits
- Conventional commits are enforced via Commitlint and Commitizen
- Pre-commit hooks run `npm run lint && npm run type-check` via Husky

### Environment Variables
Environment variables should be managed in `.env.local` (not committed to repository). No environment files are currently present, which is a security best practice.

## Implementation Patterns

### Server Actions and API Routes
- **Server Actions**: Use `"use server"` directive for form mutations and server-side operations
- **API Routes**: Create in `/src/app/api/[route]/route.ts` with GET/POST/PUT/DELETE exports
- **Route Handlers**: Return `NextResponse.json()` for JSON responses

### Component Development
- **Server Components**: Default for data fetching and static rendering
- **Client Components**: Add `"use client"` only when you need:
  - Event handlers (onClick, onChange, etc.)
  - Browser APIs (localStorage, window, etc.)
  - React hooks (useState, useEffect, etc.)
  - Interactive third-party libraries

### File Organization
- **Utilities**: Separate by concern - `formatters.ts`, `validators.ts`, `utils.ts`
- **Constants**: Global values in `/src/lib/constants.ts`
- **Types**: Shared TypeScript types in `/src/types/types.ts`

### Fonts and Assets
- **Fonts**: Geist Sans and Geist Mono are configured via next/font/google
- **Public Assets**: Static files in `/public/` directory

### Planned Technologies
Based on TODO.md, these libraries are selected for future implementation:
- **Database**: Drizzle ORM (chosen over Prisma)
- **Validation**: Zod for schema validation
- **State Management**: Zustand (chosen over Jotai)