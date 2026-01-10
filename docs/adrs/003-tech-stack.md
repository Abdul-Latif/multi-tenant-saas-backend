# ADR-003: Technology Stack

## Context

Need to choose technologies that support building a production-grade multi-tenant SaaS backend while optimizing for learning.

## Decision

| Component | Choice | Version |
|-----------|--------|---------|
| Runtime | Node.js | 18+ |
| Framework | NestJS | 10+ |
| Language | TypeScript | 5+ |
| ORM | TypeORM | 0.3+ |
| Database | PostgreSQL | 14+ |
| Authentication | JWT | |
| Validation | class-validator | |

## Rationale

### NestJS:
- **Built for enterprise**: Opinionated structure, dependency injection
- **Perfect for multi-tenancy**: Guards, interceptors, middleware out-of-the-box
- **TypeScript-first**: Type safety helps prevent bugs
- **Scalable architecture**: Modular design supports growth
- **Great documentation**: Easy to learn

### TypeORM:
- **PostgreSQL support**: Excellent integration
- **Migrations**: Database version control
- **QueryBuilder**: Flexible query construction
- **Repository pattern**: Clean data access layer
- **TypeScript support**: Type-safe queries

### PostgreSQL:
- **Row-Level Security**: Critical for our multi-tenant strategy
- **JSONB**: Flexible data when needed
- **Mature**: Battle-tested in production
- **Performance**: Handles millions of rows efficiently
- **Open source**: No licensing costs

## Consequences

### Learning curve:
- Must learn NestJS concepts (modules, providers, guards)
- TypeORM patterns and best practices
- PostgreSQL RLS syntax and policies

### Benefits:
- Industry-relevant skills
- Type safety prevents many bugs
- Clean, maintainable architecture
- Great for portfolio

### Trade-offs:
- More boilerplate than simpler frameworks
- Steeper learning curve initially
- More abstraction layers to understand
```
