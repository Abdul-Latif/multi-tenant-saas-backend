# Multi-Tenant SaaS Backend - Learning Project

> A production-grade multi-tenant SaaS backend built with NestJS, TypeORM, and PostgreSQL to deeply understand tenant isolation, security, and scalability patterns.

##  Project Goal

This is a **self-learning project** where I'm building a multi-tenant SaaS backend from scratch. The focus is on understanding the architectural patterns, security considerations, and implementation challenges of serving multiple customers (tenants) from a single application instance.

##  What I'm Learning

- ✅ Multi-tenant architecture patterns
- ✅ Data isolation strategies (shared database with RLS)
- ✅ JWT-based tenant identification and routing
- ✅ Row-Level Security in PostgreSQL
- ✅ Rate limiting per tenant
- ✅ Secure API design for multi-tenant systems
- ✅ TypeORM patterns for tenant-scoped queries
- ✅ NestJS middleware and guards for tenant context

##  Architecture Overview

### Multi-Tenancy Strategy

**Approach**: Shared Database with Shared Schema + Row-Level Security (RLS)

**Why this approach?**
- Most cost-effective for learning and scaling
- Teaches the most challenging aspects of multi-tenancy
- Forces disciplined coding practices
- Real-world pattern used by many SaaS companies

**Data Isolation:**
- Every table includes a `tenant_id` column
- PostgreSQL RLS policies enforce tenant boundaries at the database level
- Application-level filtering in all queries
- Double-layer security: app code + database policies

### Request Flow

![Multi-Tenant Request Flow](./docs/diagrams/request-flow.png)

1. **Authentication**: JWT validation and tenant_id extraction
2. **Rate Limiting**: Per-tenant throttling to prevent abuse
3. **Tenant Context**: Inject tenant_id into request context
4. **Isolation Layer**: Middleware ensures all queries are tenant-scoped
5. **Business Logic**: Controllers and services process request
6. **Database**: RLS enforces tenant isolation as final safeguard

##  Tech Stack

| Layer | Technology | Why? |
|-------|-----------|------|
| **Runtime** | Node.js | JavaScript ecosystem, async-first |
| **Framework** | NestJS | Enterprise-grade architecture, dependency injection, built-in guards/interceptors |
| **Language** | TypeScript | Type safety, better developer experience |
| **ORM** | TypeORM | PostgreSQL support, migration management, query builder |
| **Database** | PostgreSQL | Row-Level Security (RLS), JSONB support, battle-tested for multi-tenancy |
| **Authentication** | JWT | Stateless, includes tenant_id in payload |
| **Validation** | class-validator | Request validation |

##  Project Structure
```
multi-tenant-saas-backend/
├── docs/
│   ├── diagrams/           # Architecture diagrams
│   ├── adrs/               # Architecture Decision Records
│   ├── LEARNING_NOTES.md   # My learning journey
│   └── CASE_STUDIES.md     # Real-world examples analyzed
├── src/
│   ├── auth/               # Authentication & JWT
│   ├── tenants/            # Tenant management
│   ├── common/
│   │   ├── guards/         # Tenant guards
│   │   ├── interceptors/   # Tenant context injection
│   │   ├── decorators/     # Custom decorators
│   │   └── filters/        # Exception filters
│   ├── database/
│   │   ├── migrations/     # Database migrations
│   │   └── seeds/          # Seed data
│   └── modules/            # Feature modules
├── test/                   # E2E and integration tests
├── .env.example
├── .gitignore
├── package.json
├── tsconfig.json
├── nest-cli.json
└── README.md
```

##  Getting Started

### Prerequisites

- Node.js >= 18.2
- PostgreSQL >= 14.1
- npm or yarn

### Installation
```bash
# Clone the repository
git clone https://github.com/Abdul-Latif/multi-tenant-saas-backend.git
cd multi-tenant-saas-backend

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env
# Edit .env with your database credentials

# Run database migrations
npm run migration:run

# Seed initial data (optional)
npm run seed

# Start development server
npm run start:dev
```

### Environment Variables
```env
# Database
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_USER=postgres
DATABASE_PASSWORD=your_password
DATABASE_NAME=multitenant_saas

# JWT
JWT_SECRET=your-secret-key
JWT_EXPIRES_IN=24h

# App
PORT=3000
NODE_ENV=development
```

##  Documentation

- [Learning Notes](./docs/LEARNING_NOTES.md) - My research and understanding of multi-tenancy
- [Architecture Decisions](./docs/adrs/) - ADRs documenting key design choices
- [Case Studies](./docs/CASE_STUDIES.md) - Real-world examples I analyzed
- [API Documentation](./docs/API.md) - Endpoints and usage (TBD)

##  Testing
```bash
# Unit tests
npm run test

# E2E tests
npm run test:e2e

# Test coverage
npm run test:cov
```

##  Development Timeline

###  Week 1: Research & Architecture (Completed)
- [x] Researched multi-tenant architecture patterns
- [x] Analyzed real-world case studies
- [x] Made architectural decisions (ADRs)
- [x] Set up project documentation

###  Week 2: Foundation Setup (In Progress)
- [ ] Initialize NestJS project with TypeScript
- [ ] Set up PostgreSQL with TypeORM
- [ ] Configure environment and project structure
- [ ] Set up basic testing infrastructure

###  Week 3+: Implementation (Planned)
- [ ] Implement authentication system
- [ ] Build tenant management
- [ ] Add RLS policies
- [ ] Implement tenant-scoped queries
- [ ] Add rate limiting
- [ ] Build sample feature module

##  Security Considerations

This project implements multiple layers of security:

1. **JWT Authentication**: Validates user identity and tenant membership
2. **Tenant Context Guards**: Ensures every request has valid tenant context
3. **Query-Level Filtering**: All TypeORM queries automatically scoped to tenant
4. **Row-Level Security**: PostgreSQL RLS as the final safety net
5. **Rate Limiting**: Per-tenant request throttling
6. **Input Validation**: class-validator on all DTOs

##  Key Challenges I'm Solving

1. **How do I prevent Tenant A from accessing Tenant B's data?**
   - Multi-layer approach: JWT validation → Guards → Query scoping → RLS

2. **How do I identify which tenant is making a request?**
   - JWT payload contains `tenant_id`, extracted in authentication middleware

3. **What if I forget to filter by tenant_id in a query?**
   - PostgreSQL RLS will block the query at database level

4. **How do I handle different usage levels per tenant?**
   - Per-tenant rate limiting with configurable thresholds

5. **How do I test that tenant isolation actually works?**
   - E2E tests that attempt to access other tenants' data

##  Learning Resources

Resources that helped me understand multi-tenancy:
- i will list them as soon as i finish the project

##  Contributing

This is a personal learning project, but feedback and suggestions are welcome! Feel free to:
- Open an issue if you spot a security concern
- Suggest improvements to the architecture
- Share resources that helped you learn multi-tenancy

##  License

MIT - This is a learning project, feel free to learn from it!

---

**Note**: This is an educational project built to understand multi-tenant architecture. It's not production-ready but implements production-grade patterns.

**Progress**: Currently in Week 2 - Foundation Setup
**Last Updated**: 10/01/2026
