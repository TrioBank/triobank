# TrioBank Platform - Hiyerarşi Görselleştirmesi

Bu dokümant, TrioBank Platform'un tam dizin hiyerarşisini görsel olarak gösterir.

## 🌲 Tam Dizin Ağacı

```
triobank-platform/
│
├── 📁 services/                          # Backend Mikroservisler
│   │
│   ├── 📁 java/                         # Java/Spring Boot Servisleri
│   │   ├── 📁 account-service/          # Hesap Yönetimi
│   │   │   ├── src/
│   │   │   │   ├── main/
│   │   │   │   │   ├── java/com/triobank/account/
│   │   │   │   │   │   ├── controller/
│   │   │   │   │   │   ├── service/
│   │   │   │   │   │   ├── repository/
│   │   │   │   │   │   ├── model/
│   │   │   │   │   │   ├── dto/
│   │   │   │   │   │   ├── config/
│   │   │   │   │   │   └── AccountServiceApplication.java
│   │   │   │   │   └── resources/
│   │   │   │   │       ├── application.yml
│   │   │   │   │       ├── application-dev.yml
│   │   │   │   │       └── application-prod.yml
│   │   │   │   └── test/
│   │   │   ├── pom.xml
│   │   │   ├── Dockerfile
│   │   │   ├── .env.example
│   │   │   └── README.md
│   │   │
│   │   ├── 📁 transaction-service/      # İşlem Yönetimi
│   │   ├── 📁 auth-service/             # Kimlik Doğrulama
│   │   ├── 📁 customer-service/         # Müşteri Yönetimi
│   │   ├── 📁 payment-service/          # Ödeme İşlemleri
│   │   └── 📁 notification-service/     # Bildirim Servisi
│   │
│   └── 📁 go/                           # Go Servisleri
│       ├── 📁 analytics-service/        # Analitik Servisi
│       │   ├── cmd/
│       │   │   └── main.go
│       │   ├── internal/
│       │   │   ├── handler/
│       │   │   ├── service/
│       │   │   ├── repository/
│       │   │   ├── model/
│       │   │   └── config/
│       │   ├── pkg/
│       │   ├── go.mod
│       │   ├── go.sum
│       │   ├── Dockerfile
│       │   ├── .env.example
│       │   └── README.md
│       │
│       ├── 📁 reporting-service/        # Rapor Servisi
│       └── 📁 monitoring-service/       # İzleme Servisi
│
├── 📁 frontend/                          # Frontend Uygulamaları
│   │
│   ├── 📁 web-portal/                   # Müşteri Web Portalı
│   │   ├── public/
│   │   ├── src/
│   │   │   ├── components/
│   │   │   ├── pages/
│   │   │   ├── services/
│   │   │   ├── store/
│   │   │   ├── utils/
│   │   │   ├── types/
│   │   │   ├── App.tsx
│   │   │   └── index.tsx
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   ├── .env.example
│   │   ├── Dockerfile
│   │   └── README.md
│   │
│   ├── 📁 admin-portal/                 # Admin Yönetim Paneli
│   └── 📁 mobile-api/                   # Mobile BFF
│
├── 📁 infrastructure/                    # Altyapı Yapılandırmaları
│   │
│   ├── 📁 kubernetes/                   # Kubernetes Manifests
│   │   ├── base/                        # Base configurations
│   │   │   ├── namespace.yaml
│   │   │   ├── service-account.yaml
│   │   │   └── network-policy.yaml
│   │   │
│   │   ├── overlays/                    # Environment overlays
│   │   │   ├── dev/
│   │   │   │   ├── kustomization.yaml
│   │   │   │   └── patches/
│   │   │   ├── staging/
│   │   │   └── production/
│   │   │
│   │   ├── helm-charts/                 # Helm Charts
│   │   │   ├── triobank-services/
│   │   │   │   ├── Chart.yaml
│   │   │   │   ├── values.yaml
│   │   │   │   ├── values-dev.yaml
│   │   │   │   ├── values-prod.yaml
│   │   │   │   └── templates/
│   │   │   │       ├── deployment.yaml
│   │   │   │       ├── service.yaml
│   │   │   │       ├── ingress.yaml
│   │   │   │       └── hpa.yaml
│   │   │   └── triobank-data/
│   │   │
│   │   └── operators/                   # Custom Operators
│   │
│   ├── 📁 terraform/                    # Infrastructure as Code
│   │   ├── modules/
│   │   │   ├── eks/
│   │   │   ├── rds/
│   │   │   ├── elasticache/
│   │   │   ├── s3/
│   │   │   └── vpc/
│   │   │
│   │   ├── environments/
│   │   │   ├── dev/
│   │   │   │   ├── main.tf
│   │   │   │   ├── variables.tf
│   │   │   │   └── terraform.tfvars
│   │   │   ├── staging/
│   │   │   └── production/
│   │   │
│   │   └── README.md
│   │
│   └── 📁 docker/                       # Docker Configurations
│       ├── base/
│       │   ├── java.Dockerfile
│       │   ├── go.Dockerfile
│       │   └── node.Dockerfile
│       │
│       └── compose/
│           ├── docker-compose.yml
│           ├── docker-compose.dev.yml
│           ├── docker-compose.prod.yml
│           └── docker-compose.monitoring.yml
│
├── 📁 shared/                            # Paylaşılan Kaynaklar
│   │
│   ├── 📁 libraries/                    # Shared Libraries
│   │   ├── java/
│   │   │   ├── common-utils/
│   │   │   │   ├── src/
│   │   │   │   ├── pom.xml
│   │   │   │   └── README.md
│   │   │   ├── security/
│   │   │   └── messaging/
│   │   │
│   │   ├── go/
│   │   │   ├── logger/
│   │   │   │   ├── logger.go
│   │   │   │   ├── logger_test.go
│   │   │   │   ├── go.mod
│   │   │   │   └── README.md
│   │   │   ├── metrics/
│   │   │   └── httputil/
│   │   │
│   │   └── typescript/
│   │       ├── types/
│   │       ├── utils/
│   │       └── api-client/
│   │
│   ├── 📁 schemas/                      # Data Schemas
│   │   ├── avro/
│   │   │   ├── transaction-event.avsc
│   │   │   ├── account-event.avsc
│   │   │   └── customer-event.avsc
│   │   │
│   │   ├── protobuf/
│   │   │   ├── account.proto
│   │   │   ├── transaction.proto
│   │   │   └── customer.proto
│   │   │
│   │   └── json-schema/
│   │       └── api-schemas/
│   │
│   └── 📁 contracts/                    # API Contracts
│       ├── openapi/
│       │   ├── account-service.yaml
│       │   ├── transaction-service.yaml
│       │   └── auth-service.yaml
│       │
│       ├── grpc/
│       │   └── services.proto
│       │
│       └── graphql/
│           └── schema.graphql
│
├── 📁 docs/                              # Dokümantasyon
│   │
│   ├── 📁 architecture/                 # Mimari Dokümantasyon
│   │   ├── overview.md                  # Genel mimari
│   │   ├── microservices.md            # Mikroservis detayları
│   │   ├── data-flow.md                # Veri akış diyagramları
│   │   ├── security.md                 # Güvenlik mimarisi
│   │   ├── deployment.md               # Deployment stratejisi
│   │   └── diagrams/                   # C4, UML diyagramları
│   │       ├── system-context.puml
│   │       ├── container.puml
│   │       └── component.puml
│   │
│   ├── 📁 api/                          # API Dokümantasyonu
│   │   ├── rest/
│   │   │   ├── account-api.md
│   │   │   └── transaction-api.md
│   │   ├── graphql/
│   │   ├── grpc/
│   │   └── postman/
│   │       └── collections/
│   │
│   ├── 📁 guides/                       # Geliştirici Kılavuzları
│   │   ├── getting-started.md          # Başlangıç
│   │   ├── development.md              # Geliştirme
│   │   ├── testing.md                  # Test stratejileri
│   │   ├── deployment.md               # Deployment
│   │   ├── troubleshooting.md          # Sorun giderme
│   │   └── CONTRIBUTING.md             # Katkıda bulunma
│   │
│   └── README.md
│
├── 📁 scripts/                           # Otomasyon Scriptleri
│   │
│   ├── 📁 setup/                        # Kurulum Scriptleri
│   │   ├── setup-dev.sh
│   │   ├── setup-db.sh
│   │   └── install-deps.sh
│   │
│   ├── 📁 build/                        # Build Scriptleri
│   │   ├── build-all.sh
│   │   ├── build-java.sh
│   │   ├── build-go.sh
│   │   └── build-frontend.sh
│   │
│   ├── 📁 test/                         # Test Scriptleri
│   │   ├── test-all.sh
│   │   ├── test-unit.sh
│   │   ├── test-integration.sh
│   │   └── test-e2e.sh
│   │
│   ├── 📁 deploy/                       # Deployment Scriptleri
│   │   ├── deploy-dev.sh
│   │   ├── deploy-staging.sh
│   │   ├── deploy-prod.sh
│   │   └── rollback.sh
│   │
│   ├── 📁 db/                           # Database Scriptleri
│   │   ├── migrate.sh
│   │   ├── seed.sh
│   │   ├── backup.sh
│   │   └── restore.sh
│   │
│   └── 📁 utils/                        # Utility Scriptleri
│       ├── generate-swagger.sh
│       ├── check-health.sh
│       ├── logs.sh
│       └── cleanup.sh
│
├── 📁 tools/                             # Geliştirme Araçları
│   │
│   ├── 📁 cli/                          # CLI Tools
│   │   └── triobank-cli/
│   │       ├── cmd/
│   │       ├── pkg/
│   │       ├── main.go
│   │       └── README.md
│   │
│   ├── 📁 generators/                   # Code Generators
│   │   ├── service-generator/
│   │   │   ├── templates/
│   │   │   ├── generate-service.sh
│   │   │   └── README.md
│   │   ├── api-generator/
│   │   └── db-migration-generator/
│   │
│   ├── 📁 monitoring/                   # Monitoring Tools
│   │   ├── health-checker/
│   │   └── log-analyzer/
│   │
│   └── 📁 testing/                      # Test Utilities
│       ├── mock-data-generator/
│       ├── load-tester/
│       └── api-tester/
│
├── 📁 config/                            # Global Konfigürasyonlar
│   │
│   ├── 📁 environments/                 # Environment Configs
│   │   ├── dev.env
│   │   ├── staging.env
│   │   └── production.env
│   │
│   ├── 📁 docker/                       # Docker Configs
│   │   ├── docker-compose.yml
│   │   ├── docker-compose.dev.yml
│   │   └── docker-compose.prod.yml
│   │
│   ├── 📁 ci-cd/                        # CI/CD Configs
│   │   ├── github-actions/
│   │   ├── jenkins/
│   │   └── gitlab-ci/
│   │
│   ├── 📁 nginx/                        # Nginx Configs
│   │   ├── nginx.conf
│   │   └── sites-available/
│   │
│   ├── 📁 monitoring/                   # Monitoring Configs
│   │   ├── prometheus/
│   │   ├── grafana/
│   │   └── alertmanager/
│   │
│   └── 📁 logging/                      # Logging Configs
│       ├── logback.xml
│       └── fluentd/
│
├── 📄 .gitignore                        # Git ignore rules
├── 📄 LICENSE                           # MIT License
└── 📄 README.md                         # Ana README
```

## 🎯 Dizin Kategorileri

### 🔵 Birincil Dizinler (Primary)
Backend, Frontend ve Infrastructure gibi core sistemler.

### 🟢 Destek Dizinleri (Supporting)
Shared, docs, scripts gibi yardımcı sistemler.

### 🟡 Araç Dizinleri (Tools)
Tools ve config gibi geliştirme araçları.

## 📊 İstatistikler

```
Toplam Ana Dizin: 8
├── services/        (9 mikroservis)
├── frontend/        (3 uygulama)
├── infrastructure/  (3 kategori)
├── shared/          (3 alt kategori)
├── docs/            (3 bölüm)
├── scripts/         (5 kategori)
├── tools/           (4 kategori)
└── config/          (5 kategori)

Backend Servisleri: 9
├── Java: 6 servis
└── Go: 3 servis

Frontend Uygulamaları: 3
├── Web Portal (React/Vue)
├── Admin Portal
└── Mobile API (Node.js)

Dokümantasyon Sayfaları: 10+
Test Coverage Hedefi: >80%
```

## 🔄 Servis İletişim Haritası

```
┌─────────────┐
│   Client    │
└──────┬──────┘
       │
┌──────▼──────────┐
│  API Gateway    │
└──────┬──────────┘
       │
       ├──────────────────────────────────────┐
       │                                      │
┌──────▼──────┐                     ┌────────▼────────┐
│    Auth     │◄────────────────────┤  All Services   │
│   Service   │   Authentication    │                 │
└─────────────┘                     └─────────────────┘
                                              │
       ┌──────────────────────────────────────┤
       │                  │                   │
┌──────▼──────┐  ┌───────▼────────┐  ┌──────▼──────┐
│   Account   │  │  Transaction   │  │   Payment   │
│   Service   │  │    Service     │  │   Service   │
└──────┬──────┘  └───────┬────────┘  └──────┬──────┘
       │                  │                   │
       │         ┌────────▼─────────┐        │
       │         │      Kafka       │        │
       │         │  Message Broker  │        │
       │         └────────┬─────────┘        │
       │                  │                   │
       └──────────────────┼───────────────────┘
                          │
              ┌───────────┴────────────┐
              │                        │
      ┌───────▼────────┐     ┌────────▼────────┐
      │   Analytics    │     │  Notification   │
      │    Service     │     │     Service     │
      └────────────────┘     └─────────────────┘
```

## 📦 Teknoloji Dağılımı

```
Backend Languages:
├── Java (Spring Boot)    ████████████████████ 67%
└── Go                    ██████████           33%

Frontend:
└── TypeScript/React      ████████████████████ 100%

Infrastructure:
├── Kubernetes            ████████████████████ 40%
├── Terraform             ████████████████████ 30%
└── Docker                ███████████████      30%

Databases:
├── PostgreSQL            ████████████████████ 60%
├── Redis                 █████████████        30%
└── MongoDB               ███████              10%
```

## 🚀 Deployment Pipeline

```
Developer
    │
    ▼
┌───────────┐
│    Git    │ (push to branch)
└─────┬─────┘
      │
      ▼
┌───────────┐
│  GitHub   │ (PR created)
│  Actions  │
└─────┬─────┘
      │
      ├─► Build ────► Test ────► Security Scan
      │
      ▼
┌───────────┐
│  Docker   │ (build & push image)
│  Registry │
└─────┬─────┘
      │
      ▼
┌───────────┐
│ Kubernetes│ (deploy)
│  Cluster  │
└─────┬─────┘
      │
      ▼
┌───────────┐
│Production │
│Environment│
└───────────┘
```

## 📈 Gelecek Genişlemeler

Muhtemel eklenmesi gereken dizinler:

```
triobank-platform/
├── 📁 mobile/                  # Native mobile apps
│   ├── ios/
│   └── android/
│
├── 📁 ml/                      # Machine learning models
│   ├── fraud-detection/
│   └── recommendation/
│
└── 📁 data/                    # Data engineering
    ├── etl/
    └── analytics/
```

## 🔐 Güvenlik Katmanları

```
Layer 1: Network Security
    ├── WAF (Web Application Firewall)
    ├── DDoS Protection
    └── Rate Limiting

Layer 2: API Gateway
    ├── JWT Validation
    ├── API Key Management
    └── Request Throttling

Layer 3: Service Mesh
    ├── mTLS
    ├── Service-to-Service Auth
    └── Circuit Breaker

Layer 4: Application
    ├── Input Validation
    ├── RBAC
    └── Audit Logging

Layer 5: Data
    ├── Encryption at Rest
    ├── Encryption in Transit
    └── Database Access Control
```

Bu hiyerarşi, TrioBank Platform'un ölçeklenebilir, maintainable ve enterprise-ready olmasını sağlar.
