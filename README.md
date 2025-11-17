# TrioBank Platform

TrioBank projesinin ana kod deposu. Tüm mikroservisleri (Java/Go), frontend (Web) kodunu ve altyapı yapılandırmalarını içeren monorepo mimarisi.

## 📋 İçindekiler

- [Proje Hakkında](#proje-hakkında)
- [Hiyerarşi](#hiyerarşi)
- [Teknoloji Stack](#teknoloji-stack)
- [Başlangıç](#başlangıç)
- [Geliştirme](#geliştirme)
- [Deployment](#deployment)
- [Katkıda Bulunma](#katkıda-bulunma)

## 🏦 Proje Hakkında

TrioBank Platform, modern bankacılık ihtiyaçlarını karşılamak için tasarlanmış, mikroservis mimarisine dayalı, ölçeklenebilir bir dijital bankacılık platformudur.

### Temel Özellikler

- **Mikroservis Mimarisi**: Bağımsız deploy edilebilir, ölçeklenebilir servisler
- **Çoklu Teknoloji Desteği**: Java (Spring Boot) ve Go servisleri
- **Modern Frontend**: React/Vue.js tabanlı web portalları
- **Cloud-Native**: Kubernetes üzerinde çalışmaya hazır
- **API-First Design**: RESTful ve GraphQL API'ler
- **Event-Driven**: Kafka tabanlı asenkron iletişim
- **Güvenlik**: OAuth2, JWT, mTLS desteği
- **Observability**: Prometheus, Grafana, Jaeger entegrasyonu

## 📁 Hiyerarşi

```
triobank-platform/
│
├── services/                   # Backend Mikroservisler
│   ├── java/                  # Java/Spring Boot servisleri
│   │   ├── account-service/           # Hesap yönetimi
│   │   ├── transaction-service/       # İşlem yönetimi
│   │   ├── auth-service/              # Kimlik doğrulama
│   │   ├── customer-service/          # Müşteri yönetimi
│   │   ├── payment-service/           # Ödeme işlemleri
│   │   └── notification-service/      # Bildirim servisi
│   │
│   └── go/                    # Go servisleri
│       ├── analytics-service/         # Analitik servisi
│       ├── reporting-service/         # Rapor servisi
│       └── monitoring-service/        # İzleme servisi
│
├── frontend/                   # Frontend Uygulamaları
│   ├── web-portal/            # Müşteri web portalı
│   ├── admin-portal/          # Admin yönetim paneli
│   └── mobile-api/            # Mobile BFF (Backend for Frontend)
│
├── infrastructure/            # Altyapı Yapılandırmaları
│   ├── kubernetes/            # K8s manifests, Helm charts
│   ├── terraform/             # Infrastructure as Code
│   └── docker/                # Docker configs, Compose files
│
├── shared/                    # Paylaşılan Kaynaklar
│   ├── libraries/             # Paylaşılan kütüphaneler
│   │   ├── java/              # Java shared libs
│   │   ├── go/                # Go shared packages
│   │   └── typescript/        # TypeScript shared libs
│   ├── schemas/               # Avro, Protobuf, JSON schemas
│   └── contracts/             # API contracts (OpenAPI, gRPC)
│
├── docs/                      # Dokümantasyon
│   ├── architecture/          # Mimari dökümanlar
│   ├── api/                   # API dokümantasyonu
│   └── guides/                # Geliştirici kılavuzları
│
├── scripts/                   # Otomasyon Scriptleri
│   ├── setup/                 # Kurulum scriptleri
│   ├── build/                 # Build scriptleri
│   ├── test/                  # Test scriptleri
│   ├── deploy/                # Deployment scriptleri
│   └── utils/                 # Utility scriptleri
│
├── tools/                     # Geliştirme Araçları
│   ├── cli/                   # CLI araçları
│   ├── generators/            # Code generators
│   ├── monitoring/            # Monitoring tools
│   └── testing/               # Test utilities
│
├── config/                    # Konfigürasyonlar
│   ├── environments/          # Environment configs
│   ├── docker/                # Docker Compose files
│   ├── ci-cd/                 # CI/CD pipeline configs
│   ├── nginx/                 # Nginx configs
│   ├── monitoring/            # Prometheus, Grafana configs
│   └── logging/               # Logging configs
│
├── .gitignore
├── LICENSE
└── README.md
```

### Dizin Açıklamaları

Her ana dizin kendi README.md dosyasına sahiptir ve detaylı bilgi içerir:

- **[services/](services/README.md)**: Backend mikroservisler ve teknoloji standartları
- **[frontend/](frontend/README.md)**: Frontend uygulamaları ve UI framework'leri
- **[infrastructure/](infrastructure/README.md)**: Kubernetes, Terraform, Docker yapılandırmaları
- **[shared/](shared/README.md)**: Paylaşılan kütüphaneler, schemas, API contracts
- **[docs/](docs/README.md)**: Mimari dokümantasyon ve geliştirici kılavuzları
- **[scripts/](scripts/README.md)**: Otomasyon scriptleri ve utilities
- **[tools/](tools/README.md)**: Geliştirme araçları, code generators
- **[config/](config/README.md)**: Global konfigürasyon dosyaları

## 🛠 Teknoloji Stack

### Backend
- **Java**: OpenJDK 17+, Spring Boot 3.x, Maven/Gradle
- **Go**: Go 1.21+, Gin/Echo framework
- **Databases**: PostgreSQL 15+, Redis 7+, MongoDB (optional)
- **Messaging**: Apache Kafka, RabbitMQ
- **API**: REST, GraphQL, gRPC

### Frontend
- **Frameworks**: React 18+ / Vue 3, Next.js
- **Language**: TypeScript
- **UI Libraries**: Material-UI, Ant Design, Tailwind CSS
- **State Management**: Redux Toolkit, Zustand
- **Build Tools**: Vite, Webpack

### Infrastructure
- **Container**: Docker, Docker Compose
- **Orchestration**: Kubernetes (EKS/AKS/GKE)
- **IaC**: Terraform, Helm
- **CI/CD**: GitHub Actions, Jenkins, GitLab CI
- **Cloud**: AWS (primary), Azure, GCP

### Monitoring & Observability
- **Metrics**: Prometheus, Grafana
- **Logging**: ELK Stack, Loki
- **Tracing**: Jaeger, Tempo
- **APM**: Dynatrace, New Relic (optional)

## 🚀 Başlangıç

### Gereksinimler

- **Java**: JDK 17 veya üzeri
- **Go**: 1.21 veya üzeri
- **Node.js**: 20 LTS veya üzeri
- **Docker**: 24.x veya üzeri
- **Kubernetes**: 1.28+ (production için)
- **Git**: 2.40+

### Hızlı Kurulum

```bash
# Repository'yi clone et
git clone https://github.com/TrioBank/triobank-platform.git
cd triobank-platform

# Development ortamını kur
./scripts/setup/setup-dev.sh

# Tüm servisleri build et
./scripts/build/build-all.sh

# Local environment'ı başlat (Docker Compose)
docker-compose -f config/docker/docker-compose.yml \
               -f config/docker/docker-compose.dev.yml up -d

# Servislerin health check'ini yap
./scripts/utils/check-health.sh
```

### Manuel Kurulum

#### 1. Java Servisleri

```bash
cd services/java/account-service
mvn clean install
mvn spring-boot:run
```

#### 2. Go Servisleri

```bash
cd services/go/analytics-service
go mod download
go run cmd/main.go
```

#### 3. Frontend Uygulamaları

```bash
cd frontend/web-portal
npm install
npm run dev
```

## 💻 Geliştirme

### Development Workflow

1. **Feature branch oluştur**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Değişiklikleri yap ve test et**
   ```bash
   # Kod değişiklikleri yap
   
   # Testleri çalıştır
   ./scripts/test/test-all.sh
   
   # Linting
   ./scripts/utils/lint.sh
   ```

3. **Commit ve push**
   ```bash
   git add .
   git commit -m "feat: add new feature"
   git push origin feature/your-feature-name
   ```

4. **Pull Request aç**
   - GitHub'da PR oluştur
   - CI/CD pipeline'ın geçmesini bekle
   - Code review sonrası merge et

### Coding Standards

- **Java**: Google Java Style Guide
- **Go**: Effective Go, Go Code Review Comments
- **TypeScript**: Airbnb TypeScript Style Guide
- **Commits**: Conventional Commits (feat, fix, docs, etc.)

### Testing

```bash
# Unit tests
./scripts/test/test-unit.sh

# Integration tests
./scripts/test/test-integration.sh

# E2E tests
./scripts/test/test-e2e.sh

# Load tests
cd tools/testing/load-tester
./load-test.sh --service=account-service
```

## 🚢 Deployment

### Development

```bash
./scripts/deploy/deploy-dev.sh
```

### Staging

```bash
./scripts/deploy/deploy-staging.sh
```

### Production

```bash
# Production deployment (approval gerektirir)
./scripts/deploy/deploy-prod.sh

# Rollback (gerekirse)
./scripts/deploy/rollback.sh --version=v1.2.3
```

### Kubernetes Deployment

```bash
# Namespace oluştur
kubectl create namespace triobank-prod

# Secrets oluştur
kubectl create secret generic db-credentials \
  --from-literal=username=admin \
  --from-literal=password=secret \
  -n triobank-prod

# Deploy et
kubectl apply -f infrastructure/kubernetes/overlays/production/ -n triobank-prod

# Rollout status
kubectl rollout status deployment/account-service -n triobank-prod
```

## 📊 Monitoring

### Grafana Dashboards

- **Service Metrics**: http://grafana.triobank.com/d/services
- **Business Metrics**: http://grafana.triobank.com/d/business
- **Infrastructure**: http://grafana.triobank.com/d/infra

### Logs

```bash
# Kubectl logs
kubectl logs -f deployment/account-service -n triobank-prod

# Kibana
http://kibana.triobank.com

# CLI ile
./tools/monitoring/log-analyzer/analyze-logs.sh --service=account-service
```

## 🤝 Katkıda Bulunma

Katkılarınızı bekliyoruz! Lütfen [CONTRIBUTING.md](docs/guides/contributing.md) dosyasını okuyun.

### Adımlar

1. Repository'yi fork edin
2. Feature branch oluşturun (`git checkout -b feature/amazing-feature`)
3. Değişikliklerinizi commit edin (`git commit -m 'feat: add amazing feature'`)
4. Branch'inizi push edin (`git push origin feature/amazing-feature`)
5. Pull Request açın

## 📝 Lisans

Bu proje [MIT License](LICENSE) altında lisanslanmıştır.

## 📧 İletişim

- **Email**: dev@triobank.com
- **Slack**: triobank.slack.com
- **Jira**: triobank.atlassian.net

## 🙏 Teşekkürler

Bu projeye katkıda bulunan herkese teşekkür ederiz!

---

**TrioBank Platform** - Modern Banking, Simplified.
