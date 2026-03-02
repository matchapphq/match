# GEMINI.md: Match Project

## Project Overview

"Match" is a reservation platform for sports fans that allows them to find and book tables at venues broadcasting sports matches. The project is a monorepo consisting of a high-performance backend API, a web partner dashboard, and a mobile application for fans.

### Components

-   **`match-api`**: The core backend API.
    -   **Runtime**: [Bun](https://bun.sh/)
    -   **Framework**: [Hono](https://hono.dev/)
    -   **Architecture**: Domain-driven, modular "Hono-Nest" style.
    -   **Database**: PostgreSQL with PostGIS, managed via [Drizzle ORM](https://orm.drizzle.team/).
    -   **Task Queue**: BullMQ with Redis for asynchronous jobs (Email, Notifications, Stripe).
    -   **Integrations**: Stripe (Subscriptions/Boosts), API-Sports (Real-time Football data), S3/R2 (Media Storage), LocationIQ (Geocoding).
-   **`match-frontend`**: The Partner Dashboard for venue owners.
    -   **Framework**: [React](https://react.dev/) + [Vite](https://vitejs.dev/)
    -   **UI Libraries**: Radix UI, Lucide React, Recharts, Tailwind CSS (Shadcn UI).
    -   **State Management**: TanStack Query (React Query).
-   **`match-mobile`**: The fan-facing mobile app.
    -   **Framework**: [React Native](https://reactnative.dev/) with [Expo](https://expo.dev/).
    -   **UI Libraries**: React Native Paper, Lucide.
    -   **State Management**: Zustand.

## Building and Running

### Prerequisites

-   **Bun** (Primary runtime and package manager)
-   **Docker** (For local PostgreSQL, Redis, and service orchestration)

### Development Commands

#### Backend (API)
```bash
cd match-api
bun install
bun run dev              # Starts the API in watch mode with .env.dev
bun run db:push          # Sync schema to database
bun run db:seed:sports-only # Seed real data from API-Sports
```

#### Web Frontend
```bash
cd match-frontend
bun install
bun run dev              # Starts the Vite development server
```

#### Mobile App
```bash
cd match-mobile
bun install
bun run start            # Starts the Expo development server
```

## Engineering Standards

### API Architecture (Hono-Nest Style)
The API is organized into feature modules under `src/modules/[module]`.

1.  **Logic Layer (`*.logic.ts`)**: Pure business logic. No Hono `Context`. Uses constructor-based dependency injection for Repositories.
2.  **Controller Layer (`*.controller.ts`)**: HTTP adapter. Validates input (Zod), calls Logic, and maps results to HTTP responses.
3.  **Router Layer (`*.routes.ts`)**: Wires Logic and Controller together, defines Hono routes, and applies middleware.
4.  **Repository Layer (`src/repository/`)**: Data access layer using Drizzle ORM. Shared across modules.

### Naming Conventions

| Element | Convention | ✅ Correct | ❌ Wrong |
|---------|------------|-----------|---------|
| **JSON Fields** | `snake_case` | `venue_id`, `user_email` | `venueId`, `userEmail` |
| **Functions** | `camelCase` | `getVenueById()`, `createBooking()` | `get_venue_by_id()` |
| **Classes** | `PascalCase` | `VenueLogic`, `BookingController` | `venueLogic` |
| **File names** | `kebab-case` | `venue-routes.ts`, `auth-middleware.ts` | `VenueRoutes.ts` |
| **Constants** | `UPPER_SNAKE_CASE` | `API_BASE_URL`, `MAX_RETRIES` | `apiBaseUrl` |

### TypeScript & REST Rules

-   **No `any` types**: Use `unknown` or Zod-inferred types.
-   **Explicit Returns**: All functions must have explicit return types.
-   **Zod Validation**: Mandatory for all external inputs (JSON payloads, Query params).
-   **REST Only**: Standard verbs (`GET`, `POST`, `PUT`, `DELETE`). Avoid `PATCH` unless for surgical updates.

## Directory Map

-   `match-api/`: Hono backend.
    -   `src/modules/`: Feature domains (Auth, Users, Venues, Matches, Fidelity, Boosts, etc.).
    -   `src/repository/`: Shared data access layer.
    -   `src/config/db/`: Drizzle table definitions and enums.
    -   `src/queue/`: BullMQ queue definitions.
    -   `src/workers/`: Background job processors.
    -   `services/`: Standalone microservices (e.g., `mail-service`).
    -   `docs/`: Detailed architectural and integration guides.
-   `match-frontend/`: React partner dashboard.
-   `match-mobile/`: Expo fan application.
-   `app-designs/`: UI/UX assets and Figma exports.
