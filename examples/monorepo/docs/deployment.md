# Deployment Architecture: Hybrid On-Prem + Cloud with Horizontal Split DNS

## Overview

The application runs simultaneously on **on-premises** infrastructure and **cloud** (Azure/AWS), with traffic routed via **horizontal split DNS** based on user location or network origin.

## What is Horizontal Split DNS?

Horizontal split DNS returns **different IP addresses** for the same domain name depending on where the DNS query originates:

```
┌─────────────────────────────────────────────────────────────┐
│                    app.company.com                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Internal Network (Office/VPN)     External Network         │
│  ┌─────────────────────┐          ┌─────────────────────┐  │
│  │ DNS resolves to:    │          │ DNS resolves to:    │  │
│  │ 10.0.1.100          │          │ 52.xxx.xxx.xxx      │  │
│  │ (On-Prem Server)    │          │ (Cloud LB)          │  │
│  └─────────┬───────────┘          └─────────┬───────────┘  │
│            │                                │               │
│            ▼                                ▼               │
│  ┌─────────────────────┐          ┌─────────────────────┐  │
│  │   ON-PREMISES       │          │   CLOUD (Azure)     │  │
│  │                     │          │                     │  │
│  │  ┌──────────────┐   │          │  ┌──────────────┐   │  │
│  │  │ Nginx/IIS    │   │          │  │ Azure Front  │   │  │
│  │  │ Reverse Proxy│   │          │  │ Door / ALB   │   │  │
│  │  └──────┬───────┘   │          │  └──────┬───────┘   │  │
│  │         │            │          │         │           │  │
│  │  ┌──────┴───────┐   │          │  ┌──────┴───────┐   │  │
│  │  │Next.js  │ API│   │          │  │Next.js  │ API│   │  │
│  │  │Container│    │   │          │  │App Svc  │    │   │  │
│  │  └──────┬───────┘   │          │  └──────┬───────┘   │  │
│  │         │            │          │         │           │  │
│  │  ┌──────┴───────┐   │          │  ┌──────┴───────┐   │  │
│  │  │  SQL Server  │   │◄────────►│  │ Azure SQL /  │   │  │
│  │  │  (Primary)   │   │  Sync    │  │ SQL Replica  │   │  │
│  │  └──────────────┘   │          │  └──────────────┘   │  │
│  └─────────────────────┘          └─────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## Why Horizontal Split DNS?

| Benefit | Description |
|---------|-------------|
| Low latency for internal users | Office users hit on-prem servers directly |
| Data sovereignty | Sensitive data stays on-prem |
| Cloud scalability | External/public traffic scales in cloud |
| Disaster recovery | If on-prem fails, DNS failover to cloud |
| Zero-downtime migration | Gradually shift traffic from on-prem to cloud |

## DNS Configuration

### Internal DNS Zone (On-Prem DNS Server / Active Directory)

```dns
; Zone: company.com (Internal View)
app.company.com.      A     10.0.1.100
api.company.com.      A     10.0.1.101
```

### External DNS Zone (Cloud DNS / Route53 / Azure DNS)

```dns
; Zone: company.com (External View)
app.company.com.      A     52.180.10.50      ; Cloud Load Balancer
api.company.com.      A     52.180.10.51      ; Cloud API Gateway
```

### Health Check & Failover

```dns
; Route53 / Azure Traffic Manager failover policy
app.company.com.      A     52.180.10.50    ; Primary: Cloud
                      A     203.0.113.10    ; Secondary: On-Prem public IP (failover)
```

## Deployment Targets

### On-Premises

| Component | Host | Technology |
|-----------|------|-----------|
| Frontend | `web-server-01` | Docker / IIS with Next.js standalone |
| API | `api-server-01` | Docker / IIS with .NET 8 |
| Database | `db-server-01` | SQL Server 2022 (Always On AG) |
| Reverse Proxy | `proxy-01` | Nginx / IIS ARR |

### Cloud (Azure Example)

| Component | Service | SKU |
|-----------|---------|-----|
| Frontend | Azure App Service / Container Apps | P1v3 |
| API | Azure App Service / Container Apps | P1v3 |
| Database | Azure SQL Database | S3 |
| CDN/Proxy | Azure Front Door | Standard |
| DNS | Azure DNS + Traffic Manager | — |

## Database Sync Strategy

```
On-Prem SQL Server (Primary)
        │
        ├── Always On Availability Group (sync within on-prem)
        │
        └── Azure SQL Managed Instance (async replica)
            └── Read replicas for cloud API
```

- **Writes** → Always route to on-prem primary
- **Reads** → Local replica (on-prem reads on-prem, cloud reads cloud replica)
- **Conflict resolution** → Primary wins (on-prem is source of truth)

## CI/CD Pipeline

```yaml
# Simplified flow
trigger: push to main

jobs:
  build:
    - Build Next.js → Docker image
    - Build .NET API → Docker image
    - Push to Container Registry

  deploy-cloud:
    - Deploy to Azure App Service / AKS
    - Run migrations on Azure SQL

  deploy-onprem:
    - SSH/Agent deploys to on-prem Docker host
    - Run migrations on on-prem SQL Server
```

## Network Requirements

| From | To | Port | Purpose |
|------|----|------|---------|
| Users (internal) | On-Prem Proxy | 443 | HTTPS traffic |
| Users (external) | Cloud LB | 443 | HTTPS traffic |
| On-Prem SQL | Azure SQL MI | 5022 | DB replication |
| CI/CD Runner | On-Prem | 22/443 | Deployment |
| CI/CD Runner | Cloud | 443 | Deployment |
