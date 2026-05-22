# สถาปัตยกรรมการ Deploy: Hybrid On-Prem + Cloud ด้วย Horizontal Split DNS

## ภาพรวม

แอปพลิเคชันทำงานพร้อมกันบน **on-premises** และ **cloud** (Azure/AWS) โดยใช้ **horizontal split DNS** กำหนดเส้นทาง traffic ตามตำแหน่งหรือเครือข่ายของผู้ใช้

## Horizontal Split DNS คืออะไร?

Horizontal split DNS คืนค่า **IP address ที่แตกต่างกัน** สำหรับ domain name เดียวกัน ขึ้นอยู่กับว่า DNS query มาจากที่ไหน:

```
┌─────────────────────────────────────────────────────────┐
│                    app.company.com                       │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  เครือข่ายภายใน (Office/VPN)    เครือข่ายภายนอก          │
│  ┌─────────────────────┐       ┌─────────────────────┐  │
│  │ DNS ชี้ไปที่:        │       │ DNS ชี้ไปที่:        │  │
│  │ 10.0.1.100          │       │ 52.xxx.xxx.xxx      │  │
│  │ (เซิร์ฟเวอร์ On-Prem)│       │ (Cloud LB)          │  │
│  └─────────┬───────────┘       └─────────┬───────────┘  │
│            │                             │              │
│            ▼                             ▼              │
│  ┌─────────────────────┐       ┌─────────────────────┐  │
│  │   ON-PREMISES       │       │   CLOUD (Azure)     │  │
│  │                     │       │                     │  │
│  │  ┌──────────────┐   │       │  ┌──────────────┐   │  │
│  │  │ Nginx/IIS    │   │       │  │ Azure Front  │   │  │
│  │  │ Reverse Proxy│   │       │  │ Door / ALB   │   │  │
│  │  └──────┬───────┘   │       │  └──────┬───────┘   │  │
│  │         │            │       │         │           │  │
│  │  ┌──────┴───────┐   │       │  ┌──────┴───────┐   │  │
│  │  │Next.js  │ API│   │       │  │Next.js  │ API│   │  │
│  │  │Container│    │   │       │  │App Svc  │    │   │  │
│  │  └──────┬───────┘   │       │  └──────┬───────┘   │  │
│  │         │            │       │         │           │  │
│  │  ┌──────┴───────┐   │       │  ┌──────┴───────┐   │  │
│  │  │  SQL Server  │   │◄─────►│  │ Azure SQL /  │   │  │
│  │  │  (Primary)   │   │ Sync  │  │ SQL Replica  │   │  │
│  │  └──────────────┘   │       │  └──────────────┘   │  │
│  └─────────────────────┘       └─────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

## ทำไมต้องใช้ Horizontal Split DNS?

| ข้อดี | รายละเอียด |
|-------|-----------|
| Latency ต่ำสำหรับผู้ใช้ภายใน | ผู้ใช้ในออฟฟิศเข้าถึงเซิร์ฟเวอร์ on-prem โดยตรง |
| Data sovereignty | ข้อมูลสำคัญอยู่ใน on-prem |
| Cloud scalability | Traffic ภายนอก/สาธารณะ scale ได้ใน cloud |
| Disaster recovery | ถ้า on-prem ล่ม DNS failover ไป cloud |
| Zero-downtime migration | ค่อยๆ ย้าย traffic จาก on-prem ไป cloud |

## การตั้งค่า DNS

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

### Health Check และ Failover

```dns
; Route53 / Azure Traffic Manager failover policy
app.company.com.      A     52.180.10.50    ; Primary: Cloud
                      A     203.0.113.10    ; Secondary: On-Prem public IP (failover)
```

## เป้าหมายการ Deploy

### On-Premises

| Component | Host | เทคโนโลยี |
|-----------|------|-----------|
| Frontend | `web-server-01` | Docker / IIS กับ Next.js standalone |
| API | `api-server-01` | Docker / IIS กับ .NET 8 |
| Database | `db-server-01` | SQL Server 2022 (Always On AG) |
| Reverse Proxy | `proxy-01` | Nginx / IIS ARR |

### Cloud (ตัวอย่าง Azure)

| Component | Service | SKU |
|-----------|---------|-----|
| Frontend | Azure App Service / Container Apps | P1v3 |
| API | Azure App Service / Container Apps | P1v3 |
| Database | Azure SQL Database | S3 |
| CDN/Proxy | Azure Front Door | Standard |
| DNS | Azure DNS + Traffic Manager | — |

## กลยุทธ์การ Sync ฐานข้อมูล

```
On-Prem SQL Server (Primary)
        │
        ├── Always On Availability Group (sync ภายใน on-prem)
        │
        └── Azure SQL Managed Instance (async replica)
            └── Read replicas สำหรับ cloud API
```

- **Writes** → route ไปที่ on-prem primary เสมอ
- **Reads** → ใช้ replica ในเครื่อง (on-prem อ่าน on-prem, cloud อ่าน cloud replica)
- **Conflict resolution** → Primary ชนะ (on-prem เป็น source of truth)

## CI/CD Pipeline

```yaml
# Flow แบบย่อ
trigger: push to main

jobs:
  build:
    - Build Next.js → Docker image
    - Build .NET API → Docker image
    - Push ไปที่ Container Registry

  deploy-cloud:
    - Deploy ไปที่ Azure App Service / AKS
    - รัน migrations บน Azure SQL

  deploy-onprem:
    - SSH/Agent deploy ไปที่ on-prem Docker host
    - รัน migrations บน on-prem SQL Server
```

## ข้อกำหนดเครือข่าย

| จาก | ไป | Port | วัตถุประสงค์ |
|-----|-----|------|-------------|
| ผู้ใช้ (ภายใน) | On-Prem Proxy | 443 | HTTPS traffic |
| ผู้ใช้ (ภายนอก) | Cloud LB | 443 | HTTPS traffic |
| On-Prem SQL | Azure SQL MI | 5022 | DB replication |
| CI/CD Runner | On-Prem | 22/443 | Deployment |
| CI/CD Runner | Cloud | 443 | Deployment |
