# Scripts

Bu dizin otomasyonlar, build scriptleri ve utility araçlarını içerir.

## Dizin Yapısı

```
scripts/
├── setup/              # Kurulum scriptleri
│   ├── setup-dev.sh           # Development environment setup
│   ├── setup-db.sh            # Database initialization
│   └── install-deps.sh        # Tüm dependencies install
│
├── build/              # Build scriptleri
│   ├── build-all.sh           # Tüm servisleri build et
│   ├── build-java.sh          # Java servisleri build et
│   ├── build-go.sh            # Go servisleri build et
│   └── build-frontend.sh      # Frontend uygulamaları build et
│
├── test/               # Test scriptleri
│   ├── test-all.sh            # Tüm testleri çalıştır
│   ├── test-unit.sh           # Unit testler
│   ├── test-integration.sh    # Integration testler
│   └── test-e2e.sh            # E2E testler
│
├── deploy/             # Deployment scriptleri
│   ├── deploy-dev.sh          # Dev environment'a deploy
│   ├── deploy-staging.sh      # Staging'e deploy
│   ├── deploy-prod.sh         # Production'a deploy
│   └── rollback.sh            # Rollback script
│
├── db/                 # Database scriptleri
│   ├── migrate.sh             # Database migration
│   ├── seed.sh                # Test data seeding
│   ├── backup.sh              # Database backup
│   └── restore.sh             # Database restore
│
└── utils/              # Utility scriptleri
    ├── generate-swagger.sh    # API docs generate
    ├── check-health.sh        # Health check tüm servisler
    ├── logs.sh                # Log collection
    └── cleanup.sh             # Cleanup temporary files
```

## Script Örnekleri

### setup-dev.sh

```bash
#!/bin/bash
set -e

echo "🚀 Setting up TrioBank development environment..."

# Check prerequisites
command -v java >/dev/null 2>&1 || { echo "Java is required"; exit 1; }
command -v go >/dev/null 2>&1 || { echo "Go is required"; exit 1; }
command -v node >/dev/null 2>&1 || { echo "Node.js is required"; exit 1; }
command -v docker >/dev/null 2>&1 || { echo "Docker is required"; exit 1; }

# Install dependencies
echo "📦 Installing dependencies..."
./scripts/setup/install-deps.sh

# Setup database
echo "🗄️  Setting up databases..."
./scripts/setup/setup-db.sh

# Build all services
echo "🔨 Building all services..."
./scripts/build/build-all.sh

echo "✅ Development environment ready!"
```

### build-all.sh

```bash
#!/bin/bash
set -e

echo "🔨 Building all services..."

# Build Java services
echo "Building Java services..."
./scripts/build/build-java.sh

# Build Go services
echo "Building Go services..."
./scripts/build/build-go.sh

# Build frontend
echo "Building frontend applications..."
./scripts/build/build-frontend.sh

echo "✅ All services built successfully!"
```

### test-all.sh

```bash
#!/bin/bash
set -e

EXIT_CODE=0

echo "🧪 Running all tests..."

# Java tests
echo "Running Java tests..."
cd services/java
for service in */; do
    echo "Testing $service..."
    cd "$service"
    mvn test || EXIT_CODE=1
    cd ..
done
cd ../..

# Go tests
echo "Running Go tests..."
cd services/go
for service in */; do
    echo "Testing $service..."
    cd "$service"
    go test ./... || EXIT_CODE=1
    cd ..
done
cd ../..

# Frontend tests
echo "Running frontend tests..."
cd frontend
for app in */; do
    echo "Testing $app..."
    cd "$app"
    npm test || EXIT_CODE=1
    cd ..
done
cd ..

if [ $EXIT_CODE -eq 0 ]; then
    echo "✅ All tests passed!"
else
    echo "❌ Some tests failed!"
fi

exit $EXIT_CODE
```

### check-health.sh

```bash
#!/bin/bash

services=(
    "http://localhost:8080/actuator/health:Account Service"
    "http://localhost:8081/actuator/health:Transaction Service"
    "http://localhost:8082/actuator/health:Auth Service"
    "http://localhost:9090/health:Analytics Service"
)

echo "🏥 Checking health of all services..."

for service in "${services[@]}"; do
    IFS=: read -r url name <<< "$service"
    if curl -sf "$url" > /dev/null; then
        echo "✅ $name - UP"
    else
        echo "❌ $name - DOWN"
    fi
done
```

### deploy-dev.sh

```bash
#!/bin/bash
set -e

ENVIRONMENT="dev"
NAMESPACE="triobank-dev"

echo "🚀 Deploying to $ENVIRONMENT..."

# Build Docker images
echo "📦 Building Docker images..."
./scripts/build/build-all.sh

# Tag images
echo "🏷️  Tagging images..."
docker tag triobank/account-service:latest triobank/account-service:dev-$(date +%Y%m%d-%H%M%S)

# Push to registry
echo "⬆️  Pushing to registry..."
docker push triobank/account-service:dev-$(date +%Y%m%d-%H%M%S)

# Deploy to Kubernetes
echo "☸️  Deploying to Kubernetes..."
kubectl apply -f infrastructure/kubernetes/overlays/$ENVIRONMENT/ -n $NAMESPACE

# Wait for rollout
echo "⏳ Waiting for rollout..."
kubectl rollout status deployment/account-service -n $NAMESPACE

echo "✅ Deployment complete!"
```

## Script Standartları

### Genel Kurallar

1. **Shebang**: Her script `#!/bin/bash` ile başlamalı
2. **Error handling**: `set -e` kullan
3. **Logging**: Emoji ile user-friendly output
4. **Exit codes**: Success için 0, failure için 1
5. **Comments**: Önemli bölümleri açıkla

### Renkli Output

```bash
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

echo -e "${GREEN}✅ Success${NC}"
echo -e "${RED}❌ Error${NC}"
echo -e "${YELLOW}⚠️  Warning${NC}"
```

### Error Handling

```bash
#!/bin/bash

set -e  # Exit on error
set -u  # Exit on undefined variable
set -o pipefail  # Exit on pipe failure

trap 'echo "❌ Script failed at line $LINENO"' ERR
```

## Kullanım

```bash
# Executable yap
chmod +x scripts/**/*.sh

# Development setup
./scripts/setup/setup-dev.sh

# Build all
./scripts/build/build-all.sh

# Run tests
./scripts/test/test-all.sh

# Deploy to dev
./scripts/deploy/deploy-dev.sh
```
