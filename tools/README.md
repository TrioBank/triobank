# Tools

Bu dizin geliştirme araçlarını ve utilities'leri içerir.

## Dizin Yapısı

```
tools/
├── cli/                # Command-line tools
│   └── triobank-cli/  # TrioBank yönetim CLI
│
├── generators/         # Code generators
│   ├── service-generator/     # Yeni servis scaffold
│   ├── api-generator/         # API code generation
│   └── db-migration-generator/
│
├── monitoring/         # Monitoring araçları
│   ├── health-checker/
│   └── log-analyzer/
│
└── testing/           # Test utilities
    ├── mock-data-generator/
    ├── load-tester/
    └── api-tester/
```

## CLI Tool (triobank-cli)

TrioBank platformu için merkezi yönetim CLI aracı.

### Kurulum

```bash
cd tools/cli/triobank-cli
npm install -g .
# veya
go install
```

### Komutlar

```bash
# Service yönetimi
triobank service list                    # Tüm servisleri listele
triobank service create <name> --type=java  # Yeni servis oluştur
triobank service start <name>            # Servisi başlat
triobank service stop <name>             # Servisi durdur
triobank service logs <name>             # Servis loglarını göster

# Database yönetimi
triobank db migrate                      # Migration çalıştır
triobank db seed                         # Test data ekle
triobank db backup                       # Backup al
triobank db restore <file>               # Backup'tan restore et

# Deployment
triobank deploy dev                      # Dev'e deploy
triobank deploy staging                  # Staging'e deploy
triobank deploy prod                     # Production'a deploy
triobank rollback <version>              # Önceki versiona dön

# Testing
triobank test unit                       # Unit testleri çalıştır
triobank test integration                # Integration testleri çalıştır
triobank test e2e                        # E2E testleri çalıştır

# Monitoring
triobank health                          # Tüm servislerin health check
triobank metrics                         # Metrics göster
triobank logs --service=account --tail=100  # Log streaming

# Development
triobank dev start                       # Development ortamını başlat
triobank dev stop                        # Development ortamını durdur
triobank dev reset                       # Development ortamını sıfırla
```

## Code Generators

### Service Generator

Yeni mikroservis scaffold oluşturur:

```bash
cd tools/generators/service-generator

# Java Spring Boot servisi
./generate-service.sh \
  --name=loan-service \
  --type=java \
  --port=8085 \
  --database=postgresql

# Go servisi
./generate-service.sh \
  --name=fraud-detection-service \
  --type=go \
  --port=9091
```

Oluşturulacak yapı:
```
services/java/loan-service/
├── src/
│   └── main/
│       ├── java/com/triobank/loan/
│       │   ├── LoanServiceApplication.java
│       │   ├── controller/
│       │   ├── service/
│       │   ├── repository/
│       │   └── model/
│       └── resources/
│           ├── application.yml
│           └── application-dev.yml
├── pom.xml
├── Dockerfile
└── README.md
```

### API Generator

OpenAPI spec'ten kod generate eder:

```bash
cd tools/generators/api-generator

# Java client
./generate-client.sh \
  --spec=../../docs/api/openapi/account-service.yaml \
  --lang=java \
  --output=../../shared/libraries/java/account-client

# TypeScript client
./generate-client.sh \
  --spec=../../docs/api/openapi/account-service.yaml \
  --lang=typescript \
  --output=../../shared/libraries/typescript/account-client
```

## Testing Tools

### Mock Data Generator

Test için mock data oluşturur:

```bash
cd tools/testing/mock-data-generator

# JSON output
./generate-mock-data.sh --entity=customer --count=100 --format=json

# SQL insert statements
./generate-mock-data.sh --entity=account --count=50 --format=sql

# CSV format
./generate-mock-data.sh --entity=transaction --count=1000 --format=csv
```

### Load Tester

Servislere yük testi yapar:

```bash
cd tools/testing/load-tester

# Basit yük testi
./load-test.sh \
  --url=http://localhost:8080/api/accounts \
  --users=100 \
  --duration=60s

# JMeter script ile
./jmeter-test.sh \
  --script=./scripts/account-service.jmx \
  --threads=500 \
  --rampup=30
```

### API Tester

API'leri otomatik test eder:

```bash
cd tools/testing/api-tester

# Tüm servisleri test et
./test-apis.sh --environment=dev

# Belirli bir servisi test et
./test-apis.sh --service=account-service --environment=staging
```

## Monitoring Tools

### Health Checker

Tüm servislerin health durumunu kontrol eder:

```bash
cd tools/monitoring/health-checker

# Basit check
./health-check.sh

# Continuous monitoring
./health-check.sh --watch --interval=30s

# Alert gönder
./health-check.sh --alert-webhook=https://hooks.slack.com/...
```

### Log Analyzer

Logları analiz eder ve pattern bulur:

```bash
cd tools/monitoring/log-analyzer

# Error pattern analizi
./analyze-logs.sh \
  --service=transaction-service \
  --level=error \
  --since=1h

# Performance analizi
./analyze-logs.sh \
  --service=account-service \
  --type=performance \
  --threshold=1000ms
```

## Development Tools

### IDE Configurations

```
tools/ide/
├── vscode/
│   ├── settings.json
│   ├── extensions.json
│   └── launch.json
├── intellij/
│   ├── codeStyles/
│   └── runConfigurations/
└── eclipse/
    └── preferences.epf
```

### Git Hooks

```bash
tools/git-hooks/
├── pre-commit          # Linting, formatting
├── pre-push            # Tests
└── commit-msg          # Conventional commits check
```

Kurulum:
```bash
cp tools/git-hooks/* .git/hooks/
chmod +x .git/hooks/*
```

## Docker Compose Profiles

Local development için hazır compose profiller:

```bash
# Full stack (tüm servisler + dependencies)
docker-compose --profile full up

# Only infrastructure (DB, Redis, Kafka)
docker-compose --profile infra up

# Specific services
docker-compose --profile accounts up
```

## Utility Scripts

```bash
tools/utils/
├── port-checker.sh          # Port kullanımını kontrol et
├── version-bumper.sh        # Version bump (semantic versioning)
├── dependency-updater.sh    # Dependencies güncelle
└── security-scanner.sh      # Security scan (OWASP, Trivy)
```
