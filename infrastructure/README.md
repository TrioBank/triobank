# Infrastructure

Bu dizin altyapı yapılandırma dosyalarını ve deployment scriptlerini içerir.

## Dizin Yapısı

```
infrastructure/
├── kubernetes/         # Kubernetes manifests
│   ├── base/          # Base configurations
│   ├── overlays/      # Environment-specific overlays
│   │   ├── dev/
│   │   ├── staging/
│   │   └── production/
│   ├── helm-charts/   # Helm charts
│   └── operators/     # Custom operators
│
├── terraform/         # Infrastructure as Code
│   ├── modules/       # Reusable modules
│   ├── environments/  # Environment configs
│   │   ├── dev/
│   │   ├── staging/
│   │   └── production/
│   └── README.md
│
└── docker/           # Docker configurations
    ├── base/         # Base images
    ├── compose/      # Docker Compose files
    └── README.md
```

## Kubernetes

### Namespace Organizasyonu

```yaml
namespaces:
  - triobank-services      # Mikroservisler
  - triobank-frontend      # Frontend uygulamaları
  - triobank-data         # Databases, caches
  - triobank-monitoring   # Monitoring stack
  - triobank-ingress      # Ingress controllers
```

### Kaynaklar

Her servis için:
- Deployment
- Service
- ConfigMap
- Secret (sealed-secrets kullanarak)
- HorizontalPodAutoscaler
- PodDisruptionBudget
- NetworkPolicy

### Helm Charts

Standart Helm chart yapısı:
```
chart-name/
├── Chart.yaml
├── values.yaml
├── values-dev.yaml
├── values-prod.yaml
└── templates/
    ├── deployment.yaml
    ├── service.yaml
    ├── ingress.yaml
    └── ...
```

## Terraform

### Cloud Providers

- **AWS**: EKS, RDS, ElastiCache, S3, CloudFront
- **Azure**: AKS, Azure Database, Redis Cache (alternatif)
- **GCP**: GKE, Cloud SQL (alternatif)

### Module Örneği

```hcl
module "eks_cluster" {
  source = "./modules/eks"
  
  cluster_name    = "triobank-${var.environment}"
  cluster_version = "1.28"
  vpc_id          = module.vpc.vpc_id
  subnet_ids      = module.vpc.private_subnets
}
```

### State Management

- Terraform state S3'te saklanır
- DynamoDB ile state locking
- Backend configuration her environment için ayrı

## Docker

### Base Images

```dockerfile
# Java base image
FROM eclipse-temurin:17-jre-alpine

# Go base image
FROM golang:1.21-alpine AS builder

# Node.js base image
FROM node:20-alpine
```

### Docker Compose (Local Development)

```yaml
version: '3.8'
services:
  postgres:
    image: postgres:15
  redis:
    image: redis:7-alpine
  kafka:
    image: confluentinc/cp-kafka:latest
```

## CI/CD Pipeline

1. **Build**: Docker images oluşturulur
2. **Test**: Unit, integration testler çalıştırılır
3. **Security Scan**: Trivy, Snyk ile güvenlik taraması
4. **Push**: Container registry'ye push
5. **Deploy**: Kubernetes'e deploy (ArgoCD veya Flux)

## Monitoring Stack

- **Metrics**: Prometheus + Grafana
- **Logging**: ELK Stack (Elasticsearch, Logstash, Kibana) veya Loki
- **Tracing**: Jaeger / Tempo
- **Alerting**: AlertManager + PagerDuty/Slack

## Güvenlik

- Secrets: Sealed Secrets / External Secrets Operator
- Network: NetworkPolicies, Service Mesh (Istio/Linkerd)
- Image Scanning: Trivy, Clair
- Policy Engine: OPA (Open Policy Agent)
