# Николай Ярославцев

**Русский** · [English](README.en.md)

Senior Full-Stack Developer, упор на фронтенд. 7+ лет в коммерческой разработке: SaaS, финтех, enterprise.
React, Next.js, TypeScript на фронте; Node.js/NestJS, PostgreSQL, Redis, WebSocket, Docker на бэке.

Больше всего люблю задачи, где UI упирается в сложный бэкенд: реалтайм и конкурентные правки, идемпотентные платежи, офлайн-очереди, большие объёмы данных. Архитектурные решения записываю (ADR, README с разбором компромиссов), тесты гоняю в CI, а не только локально.

Telegram: [@aquariumlifee](https://t.me/aquariumlifee) · [LinkedIn](https://www.linkedin.com/in/nikolay-yaroslavtsev-a6248a241/)

## Главный проект

### [CareerOS](https://github.com/NikolayYaroslavcev/career-os)

Карьерный AI-воркспейс, сделан в одиночку. Собирает вакансии с джоб-бордов и ATS, дедуплицирует, матчит с резюме через AI и показывает ранжированные рекомендации в дашборде и Telegram.

- 28 источников вакансий (HH.ru, Greenhouse, Lever, Ashby, Workday, LinkedIn, Telegram-каналы и другие), у каждого свой fetcher, маппер, нормализатор и набор тестов за общим интерфейсом `Provider`
- AI-матчинг через пять взаимозаменяемых провайдеров (OpenAI, Anthropic, Groq, Gemini, OpenRouter) с цепочками фолбэков
- API, воркер и дашборд не вызывают друг друга напрямую, а общаются через Postgres и очереди BullMQ, поэтому синхронизация и AI-анализ не блокируют UI
- 39 ADR и 363+ тестов (unit, integration, contract, e2e), которые Turborepo прогоняет по каждому пакету

`TypeScript` `Fastify` `Next.js` `BullMQ` `PostgreSQL` `Redis` `Turborepo` `Docker`

## Инженерные задачи

Тестовые задания и pet-проекты, в которых самое интересное спрятано под UI. В каждом README есть разбор решений.

**[Collaborative Todo List](https://github.com/NikolayYaroslavcev/collaborative-todo-list)**: реалтайм-список задач для нескольких пользователей.
Конфликт правок решает один атомарный `UPDATE ... WHERE version = ?`, а не клиентские таймстемпы. Порядок задач хранится в fractional-index строках, параллельные перестановки сериализует advisory lock в Postgres, и это работает при любом числе инстансов бэкенда. Офлайн-очередь на клиенте, идемпотентность по `operationId` с записью в БД, presence, права проверяются на сервере на каждом REST- и WS-входе.
`NestJS` `Prisma` `PostgreSQL` `Socket.IO` `Next.js` `dnd-kit`

**[Million Items Manager](https://github.com/NikolayYaroslavcev/million-items-manager)** · [демо](https://million-items-manager.onrender.com): два списка поверх миллиона элементов с фильтром, подгрузкой порциями и drag-and-drop сортировкой.
Состояние живёт на сервере и общее для всех открытых вкладок, изменения расходятся по SSE. В CI проходят lint, typecheck, unit и integration, тесты на полном наборе в миллион элементов, Playwright против production-сборки, минутная нагрузка на 200 соединений и smoke Docker-образа с проверкой корректной остановки.
`Express` `React` `Vite` `zod` `Vitest` `Playwright` `pnpm workspaces`

**[Telegram Desktop Client](https://github.com/NikolayYaroslavcev/telegram-desktop-client)**: независимый десктопный клиент Telegram на TDLib.
Вход по телефону с 2FA, личные чаты, ответы с цитатой, вложения, реалтайм-обновления, восстановление после обрыва сети. Сообщения кэшируются в SQLite, а удалённые остаются видны с пометкой «удалено».
`Electron` `React` `TypeScript` `TDLib` `SQLite`

**[ChaChat Funnel](https://github.com/NikolayYaroslavcev/chachat-funnel)**: воронка квиз → email → пейволл → оплата → установка для AI-чата.
Главное здесь корректность, а не вёрстка: идентификация пользователя, атрибуция, аналитика событий и платежи, которые не дублируются при повторных и конкурентных запросах. Тесты идут на настоящем Postgres, без моков БД.
`Next.js 16` `React 19` `PostgreSQL` `Prisma` `Docker Compose` `Vitest`

**[Durak Multiplayer](https://github.com/NikolayYaroslavcev/durak-multiplayer)**: каркас мультиплеерной игровой платформы (лобби → матчмейкинг → комната → игра) с «Дураком» на двоих.
Правила вынесены в пакет `game-core` из чистых детерминированных функций без зависимостей от React, Phaser или NestJS. Ходы валидирует сервер, клиент видит только свою часть состояния, после переподключения сессия восстанавливается.
`NestJS` `Socket.IO` `Next.js` `Phaser` `pnpm monorepo`

**[Chicken Crossing](https://github.com/NikolayYaroslavcev/chicken-crossing-pixi)** · [играть](https://NikolayYaroslavcev.github.io/chicken-crossing-pixi/): пошаговая crash-игра в духе Chicken Road.
Движок написан на чистом TypeScript: он решает исход раунда по seeded RNG и ничего не знает о рендере, за чем следит правило ESLint. Сцена на PixiJS только анимирует то, что уже решил движок, а store связывает их через узкий интерфейс. Mock-движок реализует тот же `GameEngine`, что и будущий серверный.
`PixiJS 8` `GSAP` `React 19` `Zustand` `Vitest` `Playwright`

**[Task Manager](https://github.com/NikolayYaroslavcev/act-comp)**: многопользовательский менеджер задач с Kanban, зависимостями между задачами, таймерами с учётом рабочего календаря, откатом версий и экспортом в CSV/PDF/Excel.
Слои entities / features / widgets: бизнес-логика вынесена в чистые функции, API-роуты тонкие (auth → фича с проверкой прав → JSON). 2000+ тестов.
`Next.js 16` `Redux Toolkit` `RTK Query` `Zod` `shadcn/ui`

**[foundation](https://github.com/NikolayYaroslavcev/foundation)**: вебхуки платёжного провайдера превращаются в подписки, и повторные или дублирующиеся доставки ничего не ломают.
Платёж и подписка пишутся в одной транзакции через `INSERT ... ON CONFLICT`, так что ситуация «платёж записан, подписка не продлена» невозможна по построению.
`Python` `FastAPI` `async SQLAlchemy 2` `Alembic` `PostgreSQL`

**[MedChat](https://github.com/NikolayYaroslavcev/MedChat)**: дашборд медицинской поддержки.
WebSocket-чат с оптимистичной отправкой, гарантией порядка сообщений, офлайн-очередью и автопереподключением написан вручную, без клиентских библиотек. Список встреч префетчится на сервере и гидрируется на клиенте.
`Next.js` `TanStack Query` `WebSocket` `Tailwind CSS 4`

## Продуктовый фронтенд

**[VRental](https://github.com/NikolayYaroslavcev/vr-rental)** · [демо](https://nikolayyaroslavcev.github.io/vr-rental/): сайт моего бывшего проката VR-шлемов и Telegram-бот для заявок. Статический экспорт Next.js, SEO с JSON-LD, каталог из 80+ игр. Бот на голом `fetch` и long polling, без зависимостей.
`Next.js 16` `Tailwind CSS 4` `Framer Motion`

**[AI Overlay](https://github.com/NikolayYaroslavcev/ai-overlay-test)**: десктопный оверлей-чат поверх всех окон, AI-ответы приходят стримом по WebSocket.
`Tauri 2` `Rust` `Vue 3` `Pinia`

Остальное лежит во вкладке [Repositories](https://github.com/NikolayYaroslavcev?tab=repositories).

## Стек

**Frontend:** React, Next.js, TypeScript, Redux Toolkit / RTK Query, TanStack Query, Zustand, Vue 3, PixiJS, Tailwind CSS

**Backend:** Node.js, NestJS, Fastify, Express, FastAPI, PostgreSQL, Prisma, Redis, BullMQ, GraphQL, REST, WebSocket / Socket.IO

**Качество и инфраструктура:** Vitest, Jest, Playwright, GitHub Actions, Docker, Turborepo, pnpm workspaces
