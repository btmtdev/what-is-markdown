# Monorepo: Next.js + .NET Core WebAPI + SQL Server

Full-stack monorepo พร้อมการ deploy แบบ hybrid (on-premises + cloud) ด้วย horizontal split DNS

## Tech Stack

| Layer | เทคโนโลยี |
|-------|-----------|
| Frontend | Next.js 15 (React 19, TypeScript) |
| Backend API | .NET 8 (C# WebAPI) |
| Database | SQL Server 2022 |
| Containerization | Docker + Docker Compose |
| CI/CD | GitHub Actions |
| DNS | Horizontal Split DNS (on-prem ↔ cloud) |

## โครงสร้าง Monorepo

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
│   ├── docker-compose.yml      # สำหรับพัฒนาในเครื่อง
│   ├── docker-compose.prod.yml # Override สำหรับ production
│   ├── nginx/                  # Reverse proxy config
│   ├── dns/                    # Split DNS zone files
│   └── k8s/                    # Kubernetes manifests (ถ้าใช้)
├── docs/
│   └── deployment.md           # สถาปัตยกรรมการ deploy
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── deploy.yml
├── package.json                # Root workspace
├── turbo.json                  # Turborepo config
└── README.md
```

## เริ่มต้นใช้งาน

### สิ่งที่ต้องมี

- Node.js >= 20
- .NET SDK 8.0
- Docker Desktop
- SQL Server (หรือใช้ Docker)

### พัฒนาในเครื่อง

```bash
# เริ่ม services ทั้งหมด
docker compose -f infra/docker-compose.yml up -d

# Frontend (http://localhost:3000)
cd apps/web && npm run dev

# API (http://localhost:5000)
cd apps/api && dotnet run
```

### ตัวแปร Environment

```bash
# apps/web/.env.local
NEXT_PUBLIC_API_URL=http://localhost:5000/api

# apps/api/appsettings.Development.json
# ConnectionStrings__Default=Server=localhost;Database=AppDb;User=sa;Password=YourPass123!;TrustServerCertificate=true
```

## การ Deploy

ดู [docs/deployment.md](docs/deployment.md) สำหรับสถาปัตยกรรม hybrid deployment พร้อม horizontal split DNS

## คำสั่ง

| คำสั่ง | รายละเอียด |
|--------|-----------|
| `npm run dev` | เริ่ม frontend dev server |
| `npm run build` | Build ทุก apps |
| `dotnet run --project apps/api` | เริ่ม API |
| `docker compose up` | เริ่ม full stack ในเครื่อง |
