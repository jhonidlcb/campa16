# Overview

This is a political campaign website for a municipal council candidate (Concejal Municipal) running in an internal party election for the Colorado Party (ANR) in Paraguay. The site is designed to be modern, visually impactful, and territorially focused — showcasing the candidate's proposals, campaign activities, and a supporter signup form. It features a public-facing landing page and a basic admin page for creating campaign activity posts.

# User Preferences

Preferred communication style: Simple, everyday language.

# System Architecture

## Frontend
- **Framework**: React 18 with TypeScript, bundled by Vite
- **Routing**: Wouter (lightweight client-side router) with two main routes: `/` (Home) and `/admin` (Admin)
- **UI Components**: Shadcn/ui (new-york style) built on Radix UI primitives, styled with Tailwind CSS
- **State Management**: TanStack React Query for server state (data fetching, mutations, caching)
- **Forms**: React Hook Form with Zod validation via `@hookform/resolvers`
- **Animations**: Framer Motion for scroll-based entry animations
- **Styling**: Tailwind CSS with CSS custom properties for theming (red-dominant Colorado Party color scheme), fonts Inter and Montserrat via Google Fonts
- **Path aliases**: `@/` maps to `client/src/`, `@shared/` maps to `shared/`

## Backend
- **Runtime**: Node.js with Express 5
- **Language**: TypeScript, executed via `tsx` in development
- **API Pattern**: RESTful JSON endpoints under `/api/` prefix, route definitions shared between client and server via `shared/routes.ts`
- **Build**: Vite builds the client to `dist/public/`, esbuild bundles the server to `dist/index.cjs` for production

## Data Layer
- **Database**: PostgreSQL (required, connection via `DATABASE_URL` environment variable)
- **ORM**: Drizzle ORM with `drizzle-zod` for automatic Zod schema generation from table definitions
- **Schema location**: `shared/schema.ts` — contains two tables:
  - `supporters`: id, name, neighborhood, phone, message, createdAt
  - `activities`: id, title, description, date, imageUrl
- **Migrations**: Managed via `drizzle-kit push` (schema push approach, no migration files)
- **Storage layer**: `server/storage.ts` implements `IStorage` interface with `DatabaseStorage` class using Drizzle queries

## API Endpoints
- `POST /api/supporters` — Create a new campaign supporter (signup form)
- `GET /api/supporters/count` — Get total supporter count (displayed live on homepage, refreshes every 30 seconds)
- `GET /api/activities` — List all campaign activities
- `POST /api/activities` — Create a new campaign activity (used from admin page)

## Shared Code
The `shared/` directory contains code used by both client and server:
- `schema.ts` — Drizzle table definitions, Zod insert schemas, and TypeScript types
- `routes.ts` — API route contract definitions (paths, methods, input/output schemas)

## Development vs Production
- **Development**: Vite dev server runs as middleware on the Express server with HMR
- **Production**: Client is pre-built to static files, served by Express static middleware with SPA fallback

# External Dependencies

- **PostgreSQL**: Primary database, required via `DATABASE_URL` environment variable. Uses `pg` (node-postgres) driver with `connect-pg-simple` for session storage capability
- **Google Fonts**: Inter and Montserrat fonts loaded from `fonts.googleapis.com`
- **Unsplash**: Seed data for activities uses placeholder images from `images.unsplash.com`
- **Replit Plugins**: Development-only Vite plugins (`@replit/vite-plugin-runtime-error-modal`, `@replit/vite-plugin-cartographer`, `@replit/vite-plugin-dev-banner`) for enhanced Replit IDE experience