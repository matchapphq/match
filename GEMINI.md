# GEMINI.md: Match Project

## Project Overview

**Match** is a reservation platform for sports fans, enabling them to find and book tables at venues (bars, pubs, etc.) broadcasting live sports matches. The project is managed as a monorepo containing a high-performance backend, a partner dashboard for venue owners, and a mobile app for fans.

### Key Components

-   **`match-api`**: Core backend service.
    -   **Runtime**: [Bun](https://bun.sh/)
    -   **Framework**: [Hono](https://hono.dev/)
    -   **Database**: PostgreSQL with PostGIS, managed via [Drizzle ORM](https://orm.drizzle.team/).
    -   **Task Queue**: BullMQ + Redis for background jobs (Email, Notifications, Stripe).
    -   **Integrations**: Stripe (Subscriptions/Boosts), API-Sports (Real-time data), S3/R2 (Media), LocationIQ (Geocoding).
-   **`match-frontend`**: Partner Dashboard for venue owners.
    -   **Framework**: [React](https://react.dev/) + [Vite](https://vitejs.dev/)
    -   **UI**: Radix UI, Tailwind CSS (Shadcn UI), Lucide React.
    -   **State**: TanStack Query (React Query).
-   **`match-mobile`**: Fan-facing mobile application.
    -   **Framework**: [React Native](https://reactnative.dev/) with [Expo](https://expo.dev/).
    -   **UI**: React Native Paper.
    -   **State**: Zustand.

## Building and Running

### Prerequisites
-   **Bun** (Primary runtime and package manager)
-   **Docker** (For PostgreSQL, Redis, and service orchestration)

### Local Development Commands

#### Backend (API)
```bash
cd match-api
bun install
bun run dev              # Starts the API in watch mode (uses .env.dev)
bun run db:push          # Sync schema to database
bun run db:seed:sports-only # Seed real data from API-Sports
```

#### Web Frontend (Partner Dashboard)
```bash
cd match-frontend
bun install
bun run dev              # Starts the Vite development server
```

#### Mobile App (Expo)
```bash
cd match-mobile
bun install
bun run start            # Starts the Expo development server
```

#### Infrastructure (Docker)
```bash
docker-compose up -d     # Starts PostgreSQL, Redis, and Mail Worker
```

## Engineering Standards

### API Architecture (Hono-Nest Style)
The API follows a domain-driven, modular structure located in `match-api/src/modules/`.

1.  **Logic Layer (`*.logic.ts`)**: Contains pure business logic. Uses dependency injection for Repositories.
2.  **Controller Layer (`*.controller.ts`)**: Handles HTTP requests/responses and input validation via Zod.
3.  **Router Layer (`*.routes.ts`)**: Defines routes and applies middleware.
4.  **Repository Layer (`src/repository/`)**: Shared data access layer using Drizzle ORM.

### Naming Conventions

| Element | Convention | Example |
|---------|------------|---------|
| **JSON Fields** | `snake_case` | `venue_id`, `user_email` |
| **Functions** | `camelCase` | `createBooking()` |
| **Classes** | `PascalCase` | `VenueLogic` |
| **Files** | `kebab-case` | `venue-routes.ts` |
| **Constants** | `UPPER_SNAKE_CASE` | `API_BASE_URL` |

### TypeScript & REST Rules
-   **Strict Typing**: Avoid `any`; use `unknown` or Zod-inferred types.
-   **Zod Validation**: Mandatory for all external inputs (Payloads, Params).
-   **REST Verbs**: Use `GET`, `POST`, `PUT`, `DELETE`. Avoid `PATCH` unless for surgical updates.

## Directory Map

-   `match-api/`: Hono backend.
    -   `src/modules/`: Feature domains (Auth, Venues, Matches, etc.).
    -   `src/repository/`: Shared data access.
    -   `src/config/db/`: Drizzle schema and enums.
    -   `services/`: Microservices (e.g., `mail-service`).
    -   `docs/`: Detailed architectural guides.
-   `match-frontend/`: React partner dashboard.
-   `match-mobile/`: Expo mobile app.
-   `app-designs/`: UI/UX assets and designs.
-   `docker-compose.yml`: Local infrastructure setup.
