# Nikolay Yaroslavtsev

[Русский](README.md) · **English**

Senior Full-Stack Developer with a frontend focus. 7+ years of commercial development: SaaS, fintech, enterprise.
React, Next.js, TypeScript on the frontend; Node.js/NestJS, PostgreSQL, Redis, WebSocket, Docker on the backend.

I enjoy work where the UI runs into a hard backend problem: realtime and concurrent edits, idempotent payments, offline queues, large data volumes. I write architecture decisions down (ADRs, READMEs that explain the trade-offs) and run tests in CI as well as locally.

My commercial code is under NDA, so this profile holds pet projects and take-home assignments.

Telegram: [@aquariumlifee](https://t.me/aquariumlifee) · [LinkedIn](https://www.linkedin.com/in/nikolay-yaroslavtsev-a6248a241/)

## Main project

### [CareerOS](https://github.com/NikolayYaroslavcev/career-os)

An AI career workspace, built solo. It collects vacancies from job boards and ATSs, deduplicates them, matches them against a resume with AI and shows ranked recommendations in a dashboard and in Telegram.

- 28 job sources (HH.ru, Greenhouse, Lever, Ashby, Workday, LinkedIn, Telegram channels and more), each with its own fetcher, mapper, normalizer and test suite behind a shared `Provider` interface
- AI matching through five interchangeable providers (OpenAI, Anthropic, Groq, Gemini, OpenRouter) with fallback chains
- The API, worker and dashboard never call each other directly; they communicate through Postgres and BullMQ queues, so syncing and AI analysis never block the UI
- 39 ADRs and 363+ tests (unit, integration, contract, e2e) that Turborepo runs per package

`TypeScript` `Fastify` `Next.js` `BullMQ` `PostgreSQL` `Redis` `Turborepo` `Docker`

## Engineering problems

Test assignments and pet projects where the interesting part is under the UI. Each README explains the decisions behind it.

**[Collaborative Todo List](https://github.com/NikolayYaroslavcev/collaborative-todo-list)**: a realtime task list for multiple users.
Edit conflicts are resolved by a single atomic `UPDATE ... WHERE version = ?` instead of client timestamps. Task order is stored as fractional-index strings, concurrent reorders are serialized by a Postgres advisory lock, and this holds for any number of backend instances. Client-side offline queue, idempotency by `operationId` persisted in the DB, presence, and permissions checked on the server at every REST and WS entry point.
`NestJS` `Prisma` `PostgreSQL` `Socket.IO` `Next.js` `dnd-kit`

**[Million Items Manager](https://github.com/NikolayYaroslavcev/million-items-manager)** · [demo](https://million-items-manager.onrender.com): two lists over a million items with filtering, chunked loading and drag-and-drop sorting.
State lives on the server and is shared by all open tabs; changes fan out over SSE. CI runs lint, typecheck, unit and integration tests, tests against the full million-item set, Playwright against the production build, a one-minute load test with 200 connections, and a Docker image smoke test that checks graceful shutdown.
`Express` `React` `Vite` `zod` `Vitest` `Playwright` `pnpm workspaces`

**[Telegram Desktop Client](https://github.com/NikolayYaroslavcev/telegram-desktop-client)**: an independent Telegram desktop client built on TDLib.
Phone login with 2FA, private chats, quoted replies, attachments, realtime updates, recovery after network drops. Messages are cached in SQLite, and deleted ones stay visible with a "deleted" mark.
`Electron` `React` `TypeScript` `TDLib` `SQLite`

**[ChaChat Funnel](https://github.com/NikolayYaroslavcev/chachat-funnel)**: a quiz → email → paywall → payment → install funnel for an AI chat app.
Most of the work went into correctness: user identification, attribution, event analytics and payments that are never duplicated under retried or concurrent requests. Tests run against a real Postgres, with no DB mocks.
`Next.js 16` `React 19` `PostgreSQL` `Prisma` `Docker Compose` `Vitest`

**[Durak Multiplayer](https://github.com/NikolayYaroslavcev/durak-multiplayer)**: the skeleton of a multiplayer game platform (lobby → matchmaking → room → game) with two-player Durak.
The rules live in a `game-core` package of pure deterministic functions with no dependency on React, Phaser or NestJS. The server validates moves, each client sees only its own part of the state, and the session is restored after a reconnect.
`NestJS` `Socket.IO` `Next.js` `Phaser` `pnpm monorepo`

**[Chicken Crossing](https://github.com/NikolayYaroslavcev/chicken-crossing-pixi)** · [play](https://NikolayYaroslavcev.github.io/chicken-crossing-pixi/): a turn-based crash game in the spirit of Chicken Road.
The engine is plain TypeScript: it decides the round outcome with a seeded RNG and knows nothing about rendering, which an ESLint rule enforces. The PixiJS scene only animates what the engine has already decided, and a store connects them through a narrow interface. The mock engine implements the same `GameEngine` as the future server-side one.
`PixiJS 8` `GSAP` `React 19` `Zustand` `Vitest` `Playwright`

**[Pirate's Fortune / pixi-slot-sdk](https://github.com/NikolayYaroslavcev/pixi-slot-sdk)** · [play](https://nikolayyaroslavcev.github.io/pixi-slot-sdk/): a 5×4 video slot and the reusable slot SDK it is built on.
The SDK owns the shared parts (loading, responsive layout, reels, the round state machine, win presentation), so a game only adds its symbols, math and mechanics. The game imports only the SDK and the SDK knows nothing about games, which ESLint enforces. The server (a seeded-RNG mock for now) computes the whole round and returns it as a list of steps that the client just plays back in order. A headless simulator checks the math over 200,000 rounds, and a new game is scaffolded from a template with one command.
`PixiJS 8` `TypeScript` `@pixi/sound` `Vite` `Vitest` `npm workspaces`

**[Task Manager](https://github.com/NikolayYaroslavcev/act-comp)**: a multi-user task manager with Kanban, task dependencies, timers that respect a working calendar, version rollback and CSV/PDF/Excel export.
Layered as entities / features / widgets: business logic lives in pure functions, API routes stay thin (auth → feature with permission check → JSON). 2000+ tests.
`Next.js 16` `Redux Toolkit` `RTK Query` `Zod` `shadcn/ui`

**[foundation](https://github.com/NikolayYaroslavcev/foundation)**: payment-provider webhooks become subscriptions, and retried or duplicate deliveries break nothing.
The payment and the subscription are written in one transaction via `INSERT ... ON CONFLICT`, so "payment recorded, subscription not extended" is impossible by construction.
`Python` `FastAPI` `async SQLAlchemy 2` `Alembic` `PostgreSQL`

**[MedChat](https://github.com/NikolayYaroslavcev/MedChat)**: a medical support dashboard.
The WebSocket chat with optimistic sends, guaranteed message order, an offline queue and auto-reconnect is written by hand, without client libraries. The meetings list is prefetched on the server and hydrated on the client.
`Next.js` `TanStack Query` `WebSocket` `Tailwind CSS 4`

## Product frontend

**[VRental](https://github.com/NikolayYaroslavcev/vr-rental)** · [demo](https://nikolayyaroslavcev.github.io/vr-rental/): the website of my former VR headset rental business plus a Telegram bot for bookings. Next.js static export, SEO with JSON-LD, a catalog of 80+ games. The bot uses plain `fetch` and long polling, with no dependencies.
`Next.js 16` `Tailwind CSS 4` `Framer Motion`

**[AI Overlay](https://github.com/NikolayYaroslavcev/ai-overlay-test)**: a desktop chat overlay on top of all windows, with AI responses streamed over WebSocket.
`Tauri 2` `Rust` `Vue 3` `Pinia`

Everything else is in the [Repositories](https://github.com/NikolayYaroslavcev?tab=repositories) tab.

## Stack

**Frontend:** React, Next.js, TypeScript, Redux Toolkit / RTK Query, TanStack Query, Zustand, Vue 3, PixiJS, Tailwind CSS

**Backend:** Node.js, NestJS, Fastify, Express, FastAPI, PostgreSQL, Prisma, Redis, BullMQ, GraphQL, REST, WebSocket / Socket.IO

**Quality and infrastructure:** Vitest, Jest, Playwright, GitHub Actions, Docker, Turborepo, pnpm workspaces
