# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Flowise is an AI agent builder platform with a visual interface for creating LLM-powered workflows. This is a monorepo using PNPM workspaces and Turbo for build orchestration.

## Architecture

The project consists of 4 main modules:

- **`packages/server`**: Node.js/Express backend serving APIs and handling the core logic
- **`packages/ui`**: React frontend built with Vite, Material-UI, and React Flow
- **`packages/components`**: Reusable component nodes for third-party integrations (LangChain, various AI providers, databases)
- **`packages/api-documentation`**: Auto-generated Swagger UI API documentation

## Common Development Commands

### Setup & Installation
```bash
# Install dependencies
pnpm install

# Build all packages
pnpm build

# Clean all build artifacts
pnpm clean
```

### Development
```bash
# Start development servers (all packages in parallel)
pnpm dev

# Start production build locally
pnpm start

# Start worker process
pnpm start-worker

# Start user management CLI
pnpm user
```

### Testing & Quality
```bash
# Run tests across all packages
pnpm test

# Run linting
pnpm lint

# Fix linting issues
pnpm lint-fix

# Format code
pnpm format
```

### Database Operations
```bash
# Create new migration
pnpm migration:create

# Run database migrations
cd packages/server && pnpm typeorm:migration-run

# Generate migration from entity changes
cd packages/server && pnpm typeorm:migration-generate
```

## Development Environment

- **Node.js**: >=18.15.0 <19.0.0 || ^20
- **Package Manager**: PNPM (required, v9+)
- **Build System**: Turbo (monorepo orchestration)
- **Bundling**: Vite (UI), TypeScript (server/components)

## Key Architecture Patterns

### Server Structure
- **Controllers**: REST API endpoints in `packages/server/src/controllers/`
- **Services**: Business logic and external integrations
- **Database**: TypeORM with configurable databases (SQLite, PostgreSQL, MySQL)
- **Authentication**: Passport.js with multiple providers (local, Google, GitHub, Auth0)
- **Queue System**: BullMQ for background job processing

### UI Structure
- **Components**: React components with Material-UI design system
- **State Management**: Redux Toolkit
- **Routing**: React Router v6
- **Flow Editor**: React Flow for visual workflow building
- **Code Editor**: CodeMirror with various language support

### Components Package
- **Node Categories**: LLMs, document loaders, text splitters, embeddings, vector stores, memory, tools
- **LangChain Integration**: Extensive use of LangChain components and abstractions
- **AI Providers**: OpenAI, Anthropic, Google, Cohere, and many others
- **Database Integrations**: Pinecone, Chroma, Weaviate, Qdrant, etc.

## Environment Configuration

Development requires `.env` files:
- `packages/server/.env`: Server configuration (PORT, database, API keys)
- `packages/ui/.env`: UI configuration (VITE_PORT, API endpoints)

Refer to `packages/server/.env.example` and `packages/ui/.env.example` for available options.

## Testing Strategy

- **Server**: Jest with Supertest for API testing
- **UI**: React Testing Library
- **Components**: Jest for unit testing
- **E2E**: Cypress for full application testing

## Build Process

1. **Dependencies**: Use `pnpm install` to install workspace dependencies
2. **TypeScript**: All packages use TypeScript with strict settings
3. **Build Order**: Components → Server → UI (handled by Turbo)
4. **Outputs**: `dist/` directories in each package

## Memory Constraints

The project may require increased Node.js heap size for builds:
```bash
export NODE_OPTIONS="--max-old-space-size=4096"
```

## Common Gotchas

- Always use PNPM, not npm or yarn
- Run `pnpm build` before `pnpm start` for production mode
- Database migrations must be run manually in development
- The server uses OCLIF for CLI commands
- Frontend runs on port 8080 in development, 3000 in production
- Components package has extensive external dependencies for AI integrations