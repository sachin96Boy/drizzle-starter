# NestJS + Drizzle Starter - AI Agent Instructions

## Architecture Overview

This is a **NestJS backend starter** integrated with **Drizzle ORM** for type-safe database operations. The structure follows NestJS conventions:

- **src/** - Application source code (controllers, services, modules)
- **test/** - E2E tests
- **dist/** - Compiled JavaScript output (generated)
- **docker-compose.yaml** - Local PostgreSQL database setup

Key flow: `HTTP Request → Controller → Service → Drizzle ORM → PostgreSQL`

## Module & Dependency Injection Patterns

NestJS uses TypeScript decorators and modules for dependency management:

- **@Module()** - Declares a feature module with imports, controllers, and providers
- **@Injectable()** - Marks classes (Services, Repositories) for dependency injection
- **@Controller()** - Defines route handlers; services injected via constructor
- **AppModule** ([src/app.module.ts](src/app.module.ts)) - Root module; register new features here

**Pattern:** Always create feature modules grouping related controllers + services, then import into AppModule.

## Development Workflows

```bash
# Start watching for changes
pnpm run start:dev

# Format & lint before commits
pnpm run lint
pnpm run format

# Test locally
pnpm run test:watch
pnpm run test:e2e

# Production build
pnpm run build && pnpm run start:prod
```

Database container must run: `docker-compose up -d` sets up PostgreSQL on localhost:5432 (user: postgres, password: drizzledb).

## Code Style & Conventions

- **ESLint + Prettier** configured; use `pnpm run lint` to auto-fix
- **Single quotes, trailing commas** ([.prettierrc](.prettierrc))
- **TypeScript strict mode** enabled; always specify return types
- **Test naming:** `*.spec.ts` for unit tests, `app.e2e-spec.ts` for E2E
- **Jest config** ([package.json](package.json#L30)): rootDir=src, coverage reports in /coverage

## Drizzle Integration Notes

- **Drizzle ORM** 0.45.1 configured for PostgreSQL
- Schema definitions and migrations scaffolding not yet implemented (starter template)
- When adding models: Define Drizzle schemas in `src/database/schema.ts`, create migrations, then services consume via Drizzle client
- Use `drizzle-orm/pg` for query building; favor prepared statements for security

## Key Files Reference

| File | Purpose |
|------|---------|
| [src/main.ts](src/main.ts) | App bootstrap; configures port (env: PORT, default 3000) |
| [src/app.module.ts](src/app.module.ts) | Root module; import new feature modules here |
| [src/app.controller.ts](src/app.controller.ts) | Route handler example |
| [nest-cli.json](nest-cli.json) | NestJS generator config |

## Common Tasks for AI Agents

1. **Adding a new feature:** Create module → controller + service → register in AppModule
2. **Database changes:** Add Drizzle schema definition → run migration → update service to query
3. **Testing:** Unit tests in same directory as source; E2E tests in test/ folder
4. **Debugging:** Use `pnpm run start:debug` for Node debugger; leverage NestJS Devtools for visualization

---

**Node version:** 22.x+ | **Package manager:** pnpm | **Database:** PostgreSQL 15+
