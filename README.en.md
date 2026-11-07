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

**[Construction Task CRM](https://github.com/NikolayYaroslavcev/construction-task-crm)**: a mini-CRM for construction sites. A manager assigns tasks per site, workers take them on and submit them for review, and every action lands in the task history.
Permissions are enforced by the database, not the UI: RLS on every table, and status changes only through a SQL function that checks the transition, locks the row and writes the history in the same transaction. Nobody can edit the history, the role is not kept in metadata the client writes itself, and accounts are created by an Edge Function. A separate integration test fires forbidden requests straight at the live API.
`React 19` `Supabase` `PostgreSQL` `TanStack Query` `Zod` `Tailwind CSS 4`

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

**[Chicken Crossing](https://github.com/NikolayYaroslavcev/chicken-crossing-pixi)** · [play](https://nikolayyaroslavcev.github.io/chicken-crossing-pixi/): a turn-based crash game in the spirit of Chicken Road.
The engine is plain TypeScript: it decides the round outcome with a seeded RNG and knows nothing about rendering, which an ESLint rule enforces. The PixiJS scene only animates what the engine has already decided, and a store connects them through a narrow interface. The mock engine implements the same `GameEngine` as the future server-side one.
`PixiJS 8` `GSAP` `React 19` `Zustand` `Vitest` `Playwright`

**[Pirate's Fortune / pixi-slot-sdk](https://github.com/NikolayYaroslavcev/pixi-slot-sdk)** · [play](https://nikolayyaroslavcev.github.io/pixi-slot-sdk/): a 5×4 video slot and the slot SDK it is built on.
The SDK holds everything shared: loading, landscape and portrait layout, reels, the round state machine, win presentation. The game keeps its symbols, math and its own mechanics, like the octopus "Grab". Dependencies point one way, from game to SDK, and ESLint rejects the reverse import. The server (a seeded-RNG mock for now) computes the whole round and sends a list of steps for the client to play back. I check RTP with a headless simulator over 200,000 rounds. A new game is scaffolded from a template with one command.
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

**[Currency Exchange Widget](https://github.com/NikolayYaroslavcev/raison)**: a currency exchange form with rates from an API.
The data layer is built on Effect-TS: a stale rate request is not filtered out with an `ignore` flag but interrupted together with its Fiber. The amount is a branded type, the request state is a discriminated union, and transport and validation errors collapse into one typed `RateFetchError`.
`React 19` `Effect-TS` `TypeScript` `Vitest`

## Product frontend

**[Subscription Usage Widget](https://github.com/NikolayYaroslavcev/intera)** · [demo](https://nikolayyaroslavcev.github.io/intera/): a widget that tracks telecom subscription limit usage for a dashboard platform.
One card in four grid sizes, from full width down to a quarter, with container queries, light and dark themes, and loading, refreshing, stale, error and empty states. Untyped params are read defensively, and the computation is a pure function with no React in it. 153 tests; the demo deploys from CI after typecheck and tests pass.
`React 19` `TypeScript` `Vite` `Vitest`

**[snapbuild](https://github.com/NikolayYaroslavcev/snap)** · [demo](https://nikolayyaroslavcev.github.io/snap/): the snapbuild.ru landing page rebuilt in Next.js. All 11 original sections plus five new ones (pricing, testimonials, use cases, integrations, a contact form), woven into the page flow.
`Next.js 16` `React 19` `Tailwind CSS` `Vitest`

**[VRental](https://github.com/NikolayYaroslavcev/vr-rental)** · [demo](https://nikolayyaroslavcev.github.io/vr-rental/): the website of my former VR headset rental business plus a Telegram bot for bookings. Next.js static export, SEO with JSON-LD, a catalog of 80+ games. The bot uses plain `fetch` and long polling, with no dependencies.
`Next.js 16` `Tailwind CSS 4` `Framer Motion`

**[AI Overlay](https://github.com/NikolayYaroslavcev/ai-overlay-test)**: a desktop chat overlay on top of all windows, with AI responses streamed over WebSocket.
`Tauri 2` `Rust` `Vue 3` `Pinia`

Everything else is in the [Repositories](https://github.com/NikolayYaroslavcev?tab=repositories) tab.

## Stack

**Frontend:** React, Next.js, TypeScript, Redux Toolkit / RTK Query, TanStack Query, Zustand, Vue 3, PixiJS, Tailwind CSS, Electron, Tauri

**Backend:** Node.js, NestJS, Fastify, Express, FastAPI, PostgreSQL, Supabase, Prisma, Redis, BullMQ, GraphQL, REST, WebSocket / Socket.IO

**Quality and infrastructure:** Vitest, Jest, Playwright, GitHub Actions, Docker, Turborepo, pnpm workspaces
