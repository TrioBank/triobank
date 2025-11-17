# TrioBank Platform - Başlangıç Kılavuzu

Bu kılavuz, TrioBank Platform'da geliştirmeye başlamak için gereken tüm adımları içerir.

## 📋 İçindekiler

1. [Gereksinimler](#gereksinimler)
2. [Kurulum](#kurulum)
3. [Development Environment](#development-environment)
4. [İlk Servisinizi Çalıştırın](#ilk-servisinizi-çalıştırın)
5. [Veritabanı Kurulumu](#veritabanı-kurulumu)
6. [Test Çalıştırma](#test-çalıştırma)
7. [Debugging](#debugging)
8. [Sık Karşılaşılan Sorunlar](#sık-karşılaşılan-sorunlar)

## 🔧 Gereksinimler

### Zorunlu

| Tool | Version | Kontrol Komutu |
|------|---------|---------------|
| Java JDK | 17+ | `java -version` |
| Go | 1.21+ | `go version` |
| Node.js | 20 LTS | `node --version` |
| npm | 9+ | `npm --version` |
| Docker | 24+ | `docker --version` |
| Docker Compose | 2.20+ | `docker-compose --version` |
| Git | 2.40+ | `git --version` |

### Önerilen

| Tool | Purpose | Kurulum |
|------|---------|---------|
| Maven | Java build | `brew install maven` |
| kubectl | Kubernetes CLI | `brew install kubectl` |
| k9s | Kubernetes TUI | `brew install k9s` |
| Postman | API testing | [Download](https://postman.com) |
| DBeaver | Database client | [Download](https://dbeaver.io) |

### IDE Önerileri

**Java Development:**
- IntelliJ IDEA (önerilen)
- Eclipse
- VS Code + Java Extension Pack

**Go Development:**
- VS Code + Go extension (önerilen)
- GoLand
- Vim/Neovim + gopls

**Frontend Development:**
- VS Code (önerilen)
- WebStorm

## 📥 Kurulum

### 1. Repository'yi Clone Edin

```bash
# HTTPS
git clone https://github.com/TrioBank/triobank-platform.git

# veya SSH
git clone git@github.com:TrioBank/triobank-platform.git

cd triobank-platform
```

### 2. Gerekli Tool'ları Kontrol Edin

```bash
# Java
java -version
# Beklenen: openjdk 17.x.x veya üzeri

# Go
go version
# Beklenen: go1.21.x veya üzeri

# Node.js
node --version
# Beklenen: v20.x.x

# Docker
docker --version
docker-compose --version
```

### 3. Dependencies'leri Yükleyin

#### Otomatik Kurulum (Önerilen)

```bash
# Tüm gerekli bağımlılıkları yükle
./scripts/setup/setup-dev.sh
```

#### Manuel Kurulum

**Java Dependencies:**
```bash
# Her Java servisi için
cd services/java/account-service
mvn clean install -DskipTests
cd ../../..
```

**Go Dependencies:**
```bash
# Her Go servisi için
cd services/go/analytics-service
go mod download
cd ../../..
```

**Frontend Dependencies:**
```bash
# Her frontend app için
cd frontend/web-portal
npm install
cd ../..
```

## 🐳 Development Environment

### Docker Compose ile (Hızlı Başlangıç)

En kolay yol, tüm servisleri Docker Compose ile çalıştırmaktır:

```bash
# Infrastructure servisleri başlat (PostgreSQL, Redis, Kafka)
docker-compose -f config/docker/docker-compose.yml up -d

# Logları takip et
docker-compose -f config/docker/docker-compose.yml logs -f
```

### Manuel Kurulum (Development için önerilen)

Infrastructure servislerini Docker'da, application servislerini lokal çalıştırın:

```bash
# Sadece infrastructure
docker-compose -f config/docker/docker-compose.yml up postgres redis kafka -d
```

## 🚀 İlk Servisinizi Çalıştırın

### Account Service (Java)

#### 1. Environment Değişkenlerini Ayarlayın

```bash
cd services/java/account-service

# .env dosyası oluştur
cat > .env << EOF
SPRING_PROFILES_ACTIVE=dev
DB_HOST=localhost
DB_PORT=5432
DB_NAME=triobank
DB_USER=triobank
DB_PASSWORD=password
REDIS_HOST=localhost
REDIS_PORT=6379
JWT_SECRET=dev-secret-key
EOF
```

#### 2. Build ve Run

```bash
# Maven ile build
mvn clean package -DskipTests

# Spring Boot çalıştır
mvn spring-boot:run

# veya jar dosyasından
java -jar target/account-service-1.0.0.jar
```

#### 3. Servisi Test Edin

```bash
# Health check
curl http://localhost:8080/actuator/health

# Beklenen response:
# {"status":"UP"}
```

### Analytics Service (Go)

#### 1. Environment Değişkenlerini Ayarlayın

```bash
cd services/go/analytics-service

# .env dosyası oluştur
cat > .env << EOF
PORT=9090
DB_HOST=localhost
DB_PORT=5432
DB_NAME=triobank
DB_USER=triobank
DB_PASSWORD=password
REDIS_URL=redis://localhost:6379
LOG_LEVEL=debug
EOF
```

#### 2. Build ve Run

```bash
# Dependencies download
go mod download

# Run
go run cmd/main.go

# veya build et ve çalıştır
go build -o bin/analytics-service cmd/main.go
./bin/analytics-service
```

#### 3. Servisi Test Edin

```bash
# Health check
curl http://localhost:9090/health

# Beklenen response:
# {"status":"healthy"}
```

### Web Portal (React)

#### 1. Environment Değişkenlerini Ayarlayın

```bash
cd frontend/web-portal

# .env dosyası oluştur
cat > .env << EOF
REACT_APP_API_URL=http://localhost:8000
REACT_APP_AUTH_URL=http://localhost:8082
REACT_APP_ENVIRONMENT=development
EOF
```

#### 2. Run

```bash
# Dependencies yükle
npm install

# Development server başlat
npm start

# Browser otomatik açılacak: http://localhost:3000
```

## 🗄️ Veritabanı Kurulumu

### PostgreSQL

#### 1. Docker ile PostgreSQL Başlat

```bash
docker-compose -f config/docker/docker-compose.yml up postgres -d
```

#### 2. Database Oluştur

```bash
# PostgreSQL'e bağlan
docker exec -it triobank-postgres psql -U triobank

# Database kontrol et
\l

# Tables listele
\dt

# Çıkış
\q
```

#### 3. Migration Çalıştır

```bash
# Henüz migration tool'u kurulmadı
# TODO: Flyway veya Liquibase ile migration setup
```

#### 4. Test Data Ekle (Seed)

```bash
# Seed script çalıştır (oluşturulacak)
./scripts/db/seed.sh
```

### Redis

```bash
# Redis başlat
docker-compose -f config/docker/docker-compose.yml up redis -d

# Redis'e bağlan
docker exec -it triobank-redis redis-cli

# Test et
127.0.0.1:6379> PING
PONG

127.0.0.1:6379> SET test "Hello TrioBank"
OK

127.0.0.1:6379> GET test
"Hello TrioBank"
```

## 🧪 Test Çalıştırma

### Unit Tests

```bash
# Tüm testler
./scripts/test/test-all.sh

# Sadece Java testleri
./scripts/test/test-unit.sh java

# Sadece Go testleri
./scripts/test/test-unit.sh go

# Sadece Frontend testleri
./scripts/test/test-unit.sh frontend
```

### Spesifik Servis Testi

**Java:**
```bash
cd services/java/account-service
mvn test

# Spesifik test class
mvn test -Dtest=AccountServiceTest

# Test coverage
mvn test jacoco:report
# Report: target/site/jacoco/index.html
```

**Go:**
```bash
cd services/go/analytics-service
go test ./...

# Verbose
go test -v ./...

# Coverage
go test -cover ./...
go test -coverprofile=coverage.out ./...
go tool cover -html=coverage.out
```

**Frontend:**
```bash
cd frontend/web-portal
npm test

# Coverage
npm test -- --coverage

# Watch mode
npm test -- --watch
```

### Integration Tests

```bash
# Integration testleri çalıştır
./scripts/test/test-integration.sh

# Bu çalıştırmadan önce tüm servislerin ayakta olması gerekir
```

## 🐛 Debugging

### Java Service Debug

**IntelliJ IDEA:**
1. Run > Edit Configurations
2. Add New Configuration > Spring Boot
3. Main class: `com.triobank.account.AccountServiceApplication`
4. VM options: `-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5005`
5. Run in Debug mode

**VS Code:**
```json
// .vscode/launch.json
{
  "type": "java",
  "name": "Debug Account Service",
  "request": "launch",
  "mainClass": "com.triobank.account.AccountServiceApplication",
  "projectName": "account-service"
}
```

**Command Line:**
```bash
# Debug mode ile başlat
mvn spring-boot:run -Dspring-boot.run.jvmArguments="-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5005"

# IntelliJ/VS Code'dan localhost:5005'e attach et
```

### Go Service Debug

**VS Code:**
```json
// .vscode/launch.json
{
  "type": "go",
  "request": "launch",
  "name": "Debug Analytics Service",
  "program": "${workspaceFolder}/services/go/analytics-service/cmd/main.go"
}
```

**Delve (Go debugger):**
```bash
# Delve yükle
go install github.com/go-delve/delve/cmd/dlv@latest

# Debug mode'da başlat
cd services/go/analytics-service
dlv debug cmd/main.go

# Komutlar:
# b main.main    - Breakpoint
# c              - Continue
# n              - Next
# s              - Step into
# p variable     - Print variable
```

### Frontend Debug

**VS Code:**
```json
// .vscode/launch.json
{
  "type": "chrome",
  "request": "launch",
  "name": "Debug Web Portal",
  "url": "http://localhost:3000",
  "webRoot": "${workspaceFolder}/frontend/web-portal/src"
}
```

**Chrome DevTools:**
1. `npm start` ile uygulamayı başlat
2. Chrome'da F12 ile DevTools'u aç
3. Sources tab'ında breakpoint koy
4. Network tab'ında API isteklerini izle

## 🔍 Servis Health Check

Tüm servislerin sağlık durumunu kontrol edin:

```bash
# Otomatik health check script
./scripts/utils/check-health.sh

# Manuel kontrol
curl http://localhost:8080/actuator/health  # Account Service
curl http://localhost:8081/actuator/health  # Transaction Service
curl http://localhost:8082/actuator/health  # Auth Service
curl http://localhost:9090/health           # Analytics Service
```

## 📊 Monitoring (Local Development)

### Prometheus & Grafana

```bash
# Monitoring stack başlat
docker-compose -f config/docker/docker-compose.monitoring.yml up -d

# Prometheus: http://localhost:9090
# Grafana: http://localhost:3001
# Default credentials: admin/admin
```

### Logs

```bash
# Tüm container logları
docker-compose logs -f

# Spesifik servis
docker-compose logs -f postgres

# Son 100 satır
docker-compose logs --tail=100 redis

# Realtime Java application logs
tail -f services/java/account-service/logs/application.log
```

## ❓ Sık Karşılaşılan Sorunlar

### Port Already in Use

```bash
# Portu kullanan process'i bul
lsof -i :8080

# veya
netstat -tulpn | grep 8080

# Process'i sonlandır
kill -9 <PID>
```

### Docker Container Çalışmıyor

```bash
# Container durumunu kontrol et
docker ps -a

# Logs kontrol et
docker logs triobank-postgres

# Container'ı yeniden başlat
docker restart triobank-postgres

# Tüm container'ları temizle ve yeniden başlat
docker-compose down
docker-compose up -d
```

### Maven Build Hatası

```bash
# Maven cache temizle
mvn clean

# Dependencies yeniden indir
mvn dependency:purge-local-repository

# Offline mode'u devre dışı bırak
mvn clean install -U
```

### Go Module Sorunları

```bash
# Module cache temizle
go clean -modcache

# Dependencies yeniden yükle
go mod download

# Go version güncellemesi gerekiyorsa
go mod tidy
```

### Database Connection Hatası

```bash
# PostgreSQL çalışıyor mu?
docker ps | grep postgres

# Port açık mı?
nc -zv localhost 5432

# PostgreSQL'e manuel bağlan
psql -h localhost -U triobank -d triobank

# Connection string kontrol et
# postgresql://triobank:password@localhost:5432/triobank
```

## 📚 Daha Fazla Bilgi

- [Architecture Overview](../architecture/overview.md)
- [API Documentation](../api/README.md)
- [Contributing Guide](CONTRIBUTING.md)
- [Deployment Guide](deployment.md)
- [Troubleshooting Guide](troubleshooting.md)

## 💬 Yardım

Sorunlarınız için:
- **Slack**: #dev-support kanalı
- **Email**: dev@triobank.com
- **GitHub Issues**: Bug report veya feature request

## 🎉 Hazırsınız!

Artık TrioBank Platform'da geliştirmeye başlayabilirsiniz. İyi kodlamalar! 🚀
