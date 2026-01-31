# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

UIGen is an AI-powered React component generator with live preview. Users describe components in a chat interface, and Claude generates React+Tailwind code that renders in a sandboxed iframe preview. The app supports anonymous usage and authenticated users with persistent projects.

## Commands

- `npm run setup` — Install dependencies, generate Prisma client, and run migrations
- `npm run dev` — Start dev server with Turbopack (http://localhost:3000)
- `npm run build` — Production build
- `npm run lint` — ESLint
- `npm test` — Run all tests (vitest, jsdom environment)
- `npx vitest run src/lib/__tests__/file-system.test.ts` — Run a single test file
- `npx prisma generate` — Regenerate Prisma client (after schema changes)
- `npx prisma migrate dev` — Run database migrations
- `npm run db:reset` — Reset the database

## Architecture

### AI Chat → Tool Calls → Virtual File System → Live Preview

The core flow is:

1. **Chat** (`src/lib/contexts/chat-context.tsx`) — Uses Vercel AI SDK's `useChat` hook, sends messages to `/api/chat` with the serialized virtual file system state
2. **API Route** (`src/app/api/chat/route.ts`) — Streams responses from Claude (Haiku 4.5) using `streamText` with two tools: `str_replace_editor` and `file_manager`
3. **Tool Execution** — Tools operate on a server-side `VirtualFileSystem` instance; tool calls are also forwarded to the client-side `FileSystemContext` via `onToolCall` to keep both in sync
4. **Preview** (`src/components/preview/PreviewFrame.tsx`) — Transforms all virtual files with Babel (`src/lib/transform/jsx-transformer.ts`), creates blob URLs, builds an import map pointing `@/` aliases to blob URLs, and renders in an iframe with Tailwind CDN and React from esm.sh

### Virtual File System

`src/lib/file-system.ts` — In-memory tree structure (`VirtualFileSystem` class) with `Map<string, FileNode>`. No files are written to disk. The file system serializes to/from JSON for persistence and API transport. It includes text-editor operations (view, create, str_replace, insert) used by the AI tools.

### Provider & Mock Mode

`src/lib/provider.ts` — When `ANTHROPIC_API_KEY` is not set, a `MockLanguageModel` provides static responses (counter/form/card components) so the app runs without an API key. The mock implements `LanguageModelV1` and simulates tool call steps.

### Auth & Persistence

- JWT-based sessions (`src/lib/auth.ts`) using `jose`, stored in httpOnly cookies
- Server actions in `src/actions/` handle sign-up/sign-in (bcrypt), project CRUD
- Prisma with SQLite (`prisma/schema.prisma`) — two models: `User` and `Project`
- Projects store messages and file system data as JSON strings
- Middleware (`src/middleware.ts`) protects `/api/projects` and `/api/filesystem` routes

### Routing

- `/` — Anonymous users get `MainContent` directly; authenticated users redirect to their most recent project
- `/[projectId]` — Authenticated project page; loads saved messages and file system data

### Key Contexts

- `FileSystemProvider` — Wraps `VirtualFileSystem`, provides CRUD operations and handles AI tool calls on the client side
- `ChatProvider` — Wraps Vercel AI SDK's `useChat`, wired to `FileSystemProvider` for tool call handling

### UI Components

- Uses shadcn/ui (New York style) with Radix primitives
- `@/` path alias maps to `./src/*`
- Monaco editor for code editing
- `react-resizable-panels` for the split-pane layout (chat | preview/code)
- Prisma client is generated to `src/generated/prisma`

### JSX Transformer

`src/lib/transform/jsx-transformer.ts` — Client-side Babel transformation that:
- Transpiles JSX/TSX files to JS
- Resolves `@/` import aliases to blob URLs
- Maps third-party imports to esm.sh URLs
- Creates placeholder modules for missing imports
- Collects CSS imports and injects them as style tags
