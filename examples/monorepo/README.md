# Monorepo: Next.js + .NET Core WebAPI + SQL Server

A full-stack monorepo with hybrid deployment (on-premises + cloud) using horizontal split DNS.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 15 (React 19, TypeScript) |
| Backend API | .NET 8 (C# WebAPI) |
| Database | SQL Server 2022 |
| Containerization | Docker + Docker Compose |
| CI/CD | GitHub Actions |
| DNS | Horizontal Split DNS (on-prem ↔ cloud) |

## Monorepo Structure

```
monorepo/
├── apps/
│   ├── web/                    # Next.js frontend
│   │   ├── src/
│   │   ├── public/
│   │   ├── next.config.ts
│   │   ├── package.json
│   │   └── Dockerfile
│   └── api/                    # .NET Core WebAPI
│       ├── Controllers/
│       ├── Services/
│       ├── Models/
│       ├── Program.cs
│       ├── Api.csproj
│       └── Dockerfile
├── packages/
│   └── shared-types/           # Shared TypeScript types (API contracts)
├── infra/
│   ├── docker-compose.yml      # Local development
│   ├── docker-compose.prod.yml # Production override
│   ├── nginx/                  # Reverse proxy config
│   ├── dns/                    # Split DNS zone files
│   └── k8s/                    # Kubernetes manifests (optional)
├── docs/
│   └── deployment.md           # Deployment architecture
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── deploy.yml
├── package.json                # Root workspace
├── turbo.json                  # Turborepo config
└── README.md
```

## Getting Started

### Prerequisites

- Node.js >= 20
- .NET SDK 8.0
- Docker Desktop
- SQL Server (or use Docker)

### Local Development

```bash
# Start all services
docker compose -f infra/docker-compose.yml up -d

# Frontend (http://localhost:3000)
cd apps/web && npm run dev

# API (http://localhost:5000)
cd apps/api && dotnet run
```

### Environment Variables

```bash
# apps/web/.env.local
NEXT_PUBLIC_API_URL=http://localhost:5000/api

# apps/api/appsettings.Development.json
# ConnectionStrings__Default=Server=localhost;Database=AppDb;User=sa;Password=YourPass123!;TrustServerCertificate=true
```

## Deployment

See [docs/deployment.md](docs/deployment.md) for the full hybrid deployment architecture with horizontal split DNS.

## Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start frontend dev server |
| `npm run build` | Build all apps |
| `dotnet run --project apps/api` | Start API |
| `docker compose up` | Start full stack locally |
