# Hybrid Monorepo MVP

A modern full-stack monorepo starter using:

- **Next.js 14** (App Router) → frontend
- **NestJS** (Fastify) → backend API
- **Prisma + PostgreSQL** → database
- **Redis** → caching / realtime (future use)
- **Shared Packages** → database client, UI components
- **npm Workspaces** → monorepo management

---

## 📂 Project Structure

```
.
├── package.json            # root workspace + scripts
├── tsconfig.base.json      # shared TS config
├── docker-compose.yml      # Postgres + Redis
├── .env.example            # example environment variables
├── apps/
│   ├── web/                # Next.js app (frontend)
│   └── api/                # NestJS app (backend API)
├── packages/
│   ├── database/           # Prisma schema + client
│   └── ui/                 # Shared UI components
├── docs/                   # documentation
│   ├── api.md
│   ├── architecture.md
│   └── deployment.md
└── README.md
```

---

## 🚀 Getting Started

### 1. Install Dependencies

```bash
npm install
```

> Uses **npm workspaces** to install deps for root, apps, and packages together.

---

### 2. Environment Variables

Copy the example file:

```bash
cp .env.example .env
cp .env.example apps/web/.env
cp .env.example apps/api/.env
```

Minimum required vars:

```env
DATABASE_URL="postgresql://hybrid:hybrid@localhost:5432/hybrid?schema=public"
REDIS_URL="redis://localhost:6379"
JWT_SECRET="change_me"
NEXTAUTH_SECRET="change_me"
NEXTAUTH_URL="http://localhost:3000"
```

---

### 3. Start Services (Postgres + Redis)

```bash
docker compose up -d
```

Check:

```bash
docker ps
```

You should see **Postgres (5432)** and **Redis (6379)**.

---

### 4. Database Setup (Prisma)

Generate Prisma client + run migrations:

```bash
npm run prisma:generate
npm run prisma:migrate
```

---

### 5. Start the Monorepo

Runs **Next.js (port 3000)** and **NestJS API (port 4000)** concurrently:

```bash
npm run dev
```

- Web → http://localhost:3000  
- API → http://localhost:4000/v1  

---

## 📖 Usage

### API (NestJS)

Base URL: `http://localhost:4000/v1`

Examples:

```bash
# Register
curl -X POST http://localhost:4000/v1/auth/register   -H "Content-Type: application/json"   -d '{"email":"user@example.com","password":"secret"}'

# Login
curl -X POST http://localhost:4000/v1/auth/login   -H "Content-Type: application/json"   -d '{"email":"user@example.com","password":"secret"}'

# Products
curl http://localhost:4000/v1/products
```

---

### Web (Next.js)

Base URL: `http://localhost:3000`

- **Landing** → Trending products  
- **Marketplace** → Search/browse  
- **Product page** → Details + (future checkout)  
- **Dashboard** → Basic analytics  
- **Auth demo** → Calls Nest API `/auth/*`  

---

## 🛠️ Scripts

From the project root:

| Command                  | Description                                  |
|--------------------------|----------------------------------------------|
| `npm run dev`            | Run web + api in dev mode concurrently       |
| `npm run build`          | Build both apps                              |
| `npm run dev:web`        | Run Next.js only                             |
| `npm run dev:api`        | Run NestJS only                              |
| `npm run prisma:generate`| Generate Prisma client                       |
| `npm run prisma:migrate` | Run DB migrations (local dev)                |

---

## 🐳 Docker Notes

- **Postgres** → username: `hybrid`, password: `hybrid`, db: `hybrid`  
- **Redis** → default no password  
- Data persists as long as containers are not removed (`docker compose down -v` wipes volumes).

---

## 📚 Documentation

See [`docs/`](./docs) for:

- [API Docs](./docs/api.md)  
- [Architecture](./docs/architecture.md)  
- [Deployment](./docs/deployment.md)  

---
