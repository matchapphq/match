# Match Project Architecture

## 🌟 Overview
Match is a free reservation platform for sports fans to book tables at venues broadcasting their favorite matches.

- **Venue Owners**: Pay a subscription to list their venue and matches.
- **Users**: Book tables for free, receive a QR code for verification.

## 🏗️ System Architecture

The project is structured as a monorepo-style repository with the following main components:

### 1. Match API (`/match-api`)
The backend service handling all business logic, data persistence, and external integrations.
- **Runtime**: [Bun](https://bun.sh/)
- **Framework**: [Hono](https://hono.dev/)
- **Database**: PostgreSQL with [Drizzle ORM](https://orm.drizzle.team/)
- **Key Files**:
    - [ARCHITECTURE.md](file:///Users/rafaelsapaloesteves/Developer/Match-Project/match/match-api/ARCHITECTURE.md): Detailed backend architecture.
    - [API_ROUTES.md](file:///Users/rafaelsapaloesteves/Developer/Match-Project/match/match-api/API_ROUTES.md): Complete API documentation.

### 2. Match Mobile (`/match-mobile`)
The mobile application for both users (discovery & booking) and venue owners (scanning & check-ins).
- **Framework**: [React Native](https://reactnative.dev/) with [Expo](https://expo.dev/)
- **Structure**:
    - `src/screens/`: Application screens (Auth, Discovery, Bookings, etc.)
    - `src/components/`: Reusable UI components.
    - `src/services/`: API integration layer.
    - `src/navigation/`: Expo Router / React Navigation setup.

### 3. Match Frontend (`/match-frontend`)
The web application, primarily serving as the landing page and partner dashboard.
- **Framework**: [React](https://react.dev/) + [Vite](https://vitejs.dev/)
- **Styling**: Likely Vanilla CSS or similar (based on `index.css`).

### 4. Infrastructure
- **Docker**: Managed via [docker-compose.yml](file:///Users/rafaelsapaloesteves/Developer/Match-Project/match/docker-compose.yml).
    - `match-api`: Port 8008
    - `match-frontend`: Port 3001

## 🛠️ Development Workflow

### Prerequisites
- [Bun](https://bun.sh/) installed locally.
- Docker for running the database/infrastructure.

### Running Locally
1. **API**: `cd match-api && bun run dev`
2. **Frontend**: `cd match-frontend && bun run dev`
3. **Mobile**: `cd match-mobile && bun run start`

## 📂 Root Directory Map
- `match-api/`: Backend API.
- `match-mobile/`: Mobile app.
- `match-frontend/`: Web frontend.
- `app-designs/`: Design assets.
- `docker-compose.yml`: Infrastructure orchestration.
