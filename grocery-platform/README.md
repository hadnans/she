# Grocery Platform Monorepo

Welcome to the Grocery Platform! This is a modern, full-stack monorepo designed for scale, structured exactly as per the master architecture specification.

## Table of Contents
1. [Project Structure (The Map)](#project-structure-the-map)
2. [Prerequisites](#prerequisites)
3. [Local Development Setup](#local-development-setup)
4. [Where to Add Features](#where-to-add-features)
5. [Common Commands](#common-commands)

---

## Project Structure (The Map)

This repository uses **npm workspaces** to manage multiple applications and shared packages in one place.

```text
grocery-platform/
├── apps/
│   ├── api/          # Backend API (Node.js, Express, TypeScript, Prisma)
│   │   ├── prisma/   # Database schema (schema.prisma)
│   │   └── src/      # Controllers, services, and routes go here
│   ├── admin/        # Admin Dashboard (React, Vite, TypeScript)
│   │   └── src/      # Admin UI components and pages
│   └── web/          # Customer Storefront (React, Vite, TypeScript)
│       └── src/      # Storefront UI components and pages
├── packages/
│   ├── config/       # Shared configuration (ESLint, Prettier, tsconfig)
│   ├── types/        # Shared TypeScript types / DTOs
│   ├── ui/           # Shared React component library
│   └── utils/        # Shared helper functions
├── docker-compose.yml # PostgreSQL and Redis setup for local dev
└── package.json       # Root monorepo configuration
```

---

## Prerequisites

Before you start, make sure you have the following installed on your local machine:
- [Node.js](https://nodejs.org/en/) (v18 or higher recommended)
- [npm](https://www.npmjs.com/) (usually comes with Node.js)
- [Docker](https://www.docker.com/) and Docker Compose (required to run PostgreSQL and Redis locally)

---

## Local Development Setup

Follow these steps to get the platform running locally:

### 1. Start the Database
The project includes a `docker-compose.yml` file to easily spin up a PostgreSQL database and a Redis instance.
```bash
# In the root of the grocery-platform directory
docker-compose up -d
```

### 2. Install Dependencies
Install the dependencies for all applications and packages from the root directory.
```bash
npm install
```

### 3. Setup the Database Schema
Navigate to the API folder, configure your environment, and apply the Prisma schema to your database.
```bash
cd apps/api

# Create an .env file
echo 'DATABASE_URL="postgresql://user:password@localhost:5432/grocery"' > .env

# Generate Prisma client and push the schema to the database
npx prisma generate
npx prisma db push
```

### 4. Run the Development Servers
From the root directory, you can start all development servers simultaneously.
```bash
# Go back to the root of the monorepo
cd ../../

# Run the dev script (starts Vite for web/admin and nodemon for API)
npm run dev
```

- **Customer Storefront:** Usually runs on `http://localhost:5173`
- **Admin Dashboard:** Usually runs on `http://localhost:5174`
- **Backend API:** Set to run on your configured PORT (e.g., `http://localhost:4000`)

---

## Where to Add Features

If you want to build upon this platform, here is a quick guide on where to make your changes:

### 1. Database & Schema Changes
- **File:** `apps/api/prisma/schema.prisma`
- **Process:** Add or modify your models here. After saving, always run `npx prisma db push` (or `prisma migrate dev`) and `npx prisma generate` to update the TypeScript client.

### 2. Backend Logic (API)
- **Directory:** `apps/api/src/modules/`
- **Structure:** For a new feature (e.g., `coupons`), create a new folder:
  - `coupons.controller.ts` (Handles incoming HTTP requests)
  - `coupons.service.ts` (Core business logic and database interactions)
  - `coupons.routes.ts` (Express routing definitions)
  - `coupons.validation.ts` (Zod validation schemas)

### 3. Shared Components
- **Directory:** `packages/ui/`
- **Usage:** If you build a generic button, modal, or input field that both the admin dashboard and the web storefront will use, build it here and export it.

### 4. Shared Types
- **Directory:** `packages/types/`
- **Usage:** Define your API payload types, shared enums, and response shapes here so that the backend and frontend are strictly typed and always in sync.

### 5. Frontend Pages (Admin or Web)
- **Directory:** `apps/admin/src/pages/` or `apps/web/src/pages/`
- **Usage:** Build your actual screens here. Connect them to the API using React Query or standard `fetch`/`axios` calls.

---

## Common Commands (from the root directory)

| Command | Description |
|---|---|
| `npm install` | Installs dependencies across the entire monorepo |
| `npm run dev` | Starts all apps in development mode |
| `npm run build` | Builds all apps and packages for production |
| `npm run test` | Runs tests for all workspaces |
| `docker-compose up -d` | Starts local databases (Postgres, Redis) |
| `docker-compose down` | Stops local databases |
