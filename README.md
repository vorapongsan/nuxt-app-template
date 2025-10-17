
# tpqi — monorepo

This repository is an Nx monorepo containing a Nuxt 4 frontend app and its end-to-end tests. The README summarizes what frameworks, languages, tools and libraries are used across the workspace, the repo layout, and quick start instructions.

## Prerequisites

Before you run or develop the projects in this workspace, make sure you have the following installed and enabled on your machine:

- Node.js 22.20.0 (the workspace is tested against Node 22.x). You can install Node.js from the official website or use a version manager such as nvm-windows.
- Corepack enabled to manage Yarn (the repository uses Yarn v4 / Berry). Corepack comes bundled with Node 16.10+ but must be enabled for some setups.

Example PowerShell commands (run in an elevated shell if required):

```powershell
# Verify the Node.js version:
node -v

# Install Corepack:
npm install -g corepack

# Download and install Yarn:
corepack enable yarn

# Verify Yarn version:
yarn -v
```

## What this project uses

- Frameworks
	- Nuxt (Nuxt 4) — server + client framework for Vue 3 applications
	- Nx — monorepo tool for managing projects and tasks across the workspace

- Languages
	- TypeScript — primary language for application source and configuration
	- JavaScript — used where tooling or scripts require it

- Build & Dev Tools
	- Vite — dev server and bundler (used with Nuxt and Nx)
	- Nuxi — Nuxt CLI used to prepare and run Nuxt apps
	- Yarn (Berry) — workspace package manager (project sets `packageManager: "yarn@4.x"`)
	- Nx CLI — run targets, builds, and workspace tasks

- Styling
	- Tailwind CSS — utility-first CSS framework

- Testing
	- Vitest — unit/integration testing runner
	- Cypress — end-to-end (E2E) testing
	- @vue/test-utils — component test utilities for Vue

- Linting & Formatting
	- ESLint — linting
	- Prettier — code formatting

- Server / Backend related libraries present in root dependencies (may be used by other apps or backends in the workspace)
	- TypeORM — ORM for working with relational databases
	- Prisma (via @prisma/nuxt) — Prisma integration for Nuxt (optional)
	- Database drivers: `pg` (Postgres) and `mysql2`

## Notable packages (from repository manifests)

- Root dependencies include (high level): `@prisma/nuxt`, `typeorm`, `pg`, `mysql2`, `mssql`, `tailwindcss`.
 - Root dependencies include (high level): `@prisma/nuxt`, `typeorm`, `pg`, `mysql2`, `mssql`, `tailwindcss`.
- Root devDependencies include (high level): `nuxt`, `nx`, `vite`, `typescript`, `vitest`, `cypress`, `eslint`, `prettier`, `@vitejs/plugin-vue`, `@vue/test-utils`, `@nx/*` plugins for Nuxt/Vite/Cypress.

> See the root `package.json` for the full and exact list of packages and versions.

## Workspace layout

- `apps/nuxt-app/` — main Nuxt application (source in `apps/nuxt-app/app/`)
	- `app/` — application source (Vue pages, components, assets)
	- `server/api/` — server API routes handled by Nuxt (where present)
	- `nuxt.config.ts` — Nuxt configuration (uses Nx/Vite plugins + Tailwind)

- `apps/nuxt-app-e2e/` — Cypress-based end-to-end tests for the Nuxt app

The repo is configured as a Yarn workspace (workspaces: `apps/*`) and uses Nx to wire up project targets and tasks.

## Databases

This project includes database-related dependencies in the root `package.json` and supports multiple relational databases depending on which driver and ORM you choose to use. The workspace contains both TypeORM and Prisma-related packages; choose one workflow for your app or use them in separate services as needed.

Possible databases you can use with this project:

- PostgreSQL (`pg`) — widely used; pair with TypeORM or Prisma.
- MySQL / MariaDB (`mysql2`) — pair with TypeORM or Prisma.
- Microsoft SQL Server (`mssql`) — available in root dependencies and can be used with TypeORM.

Notes:

- TypeORM is included (`typeorm`) and supports most relational databases via drivers. Configure connections in your app's server code or using env variables.
- Prisma support is available via `@prisma/nuxt` (if you prefer Prisma as your ORM inside the Nuxt app). Prisma has its own schema and migration workflow.
- Environment variables: keep database credentials and connection URLs out of source control. Typical env keys:

	- DATABASE_URL (Prisma style URL)
	- TYPEORM_CONNECTION, TYPEORM_HOST, TYPEORM_PORT, TYPEORM_USERNAME, TYPEORM_PASSWORD, TYPEORM_DATABASE (TypeORM-style envs) — or another config approach used by your service.

- Migrations: use the migration tooling for the ORM you pick (TypeORM CLI or Prisma migrate).

If you want, I can add example connection snippets for TypeORM and Prisma and a sample `.env.example` to the repo.

## Quick start

Open a shell (PowerShell is used by default in this repository context) and run:

```powershell
# Install dependencies
yarn install

# Recommended: run using Nx (if Nx targets are available)
yarn nx serve nuxt-app

# Alternatively run Nuxt directly from the nuxt app folder
cd apps/nuxt-app
npx nuxi dev
```

Notes:
- This workspace uses Yarn v4 (the project contains a `.yarnrc.yml` pointing to Yarn 4). Use the repository yarn wrapper if present (the repo contains `.yarn/releases` configured in `.yarnrc.yml`).
- Many Nx projects expose additional targets (build, preview, test, e2e). Use `npx nx show projects` or `npx nx list` to inspect available targets or open the `project.json` / workspace config for each app.

## Testing

- Unit tests (Vitest) are configured in the workspace. Run via Nx if targets exist, e.g.:

```powershell
npx nx test nuxt-app
```

- E2E tests (Cypress) are in `apps/nuxt-app-e2e/`. Run them via Nx or the Cypress CLI:

```powershell
npx nx e2e nuxt-app-e2e
# or
cd apps/nuxt-app-e2e
npx cypress open
```

## Lint & format

```powershell
# Lint (ESLint)
npx nx lint nuxt-app

# Format (Prettier)
npx prettier --write "**/*.{ts,js,vue,json,css,md}"
```

## Contributing

- Follow existing repository conventions (TypeScript, ESLint rules, Prettier formatting).
- Open an issue or a PR for larger changes. Use Nx targets to add or modify projects.

## License

This repository is licensed under the MIT license (see root `package.json`).

