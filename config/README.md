# Config

Bu dizin global konfigürasyon dosyalarını içerir.

## Dizin Yapısı

```
config/
├── environments/       # Environment-specific configs
│   ├── dev.env
│   ├── staging.env
│   └── production.env
│
├── docker/            # Docker configurations
│   ├── docker-compose.yml
│   ├── docker-compose.dev.yml
│   └── docker-compose.prod.yml
│
├── ci-cd/             # CI/CD pipeline configs
│   ├── github-actions/
│   │   ├── build.yml
│   │   ├── test.yml
│   │   └── deploy.yml
│   ├── jenkins/
│   │   └── Jenkinsfile
│   └── gitlab-ci/
│       └── .gitlab-ci.yml
│
├── nginx/             # Nginx configurations
│   ├── nginx.conf
│   ├── sites-available/
│   └── ssl/
│
├── monitoring/        # Monitoring configs
│   ├── prometheus/
│   │   └── prometheus.yml
│   ├── grafana/
│   │   └── dashboards/
│   └── alertmanager/
│       └── alertmanager.yml
│
└── logging/           # Logging configs
    ├── logback.xml          # Java logging
    ├── log4j2.xml
    └── fluentd/
        └── fluent.conf
```

## Environment Configurations

### dev.env
```bash
# Application
ENVIRONMENT=development
LOG_LEVEL=debug

# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=triobank_dev
DB_USER=triobank
DB_PASSWORD=devpassword

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# Kafka
KAFKA_BROKERS=localhost:9092

# API Gateway
API_GATEWAY_URL=http://localhost:8000

# JWT
JWT_SECRET=dev-secret-key-change-in-production
JWT_EXPIRATION=3600

# External Services
PAYMENT_GATEWAY_URL=https://sandbox.payment.com
EMAIL_SERVICE_URL=https://dev-email.triobank.com
```

### production.env (template)
```bash
# Application
ENVIRONMENT=production
LOG_LEVEL=info

# Database (use secrets manager)
DB_HOST=${DB_HOST}
DB_PORT=5432
DB_NAME=triobank_prod
DB_USER=${DB_USER}
DB_PASSWORD=${DB_PASSWORD}

# Redis Cluster
REDIS_CLUSTER_NODES=${REDIS_CLUSTER_NODES}

# Kafka Cluster
KAFKA_BROKERS=${KAFKA_BROKERS}

# API Gateway
API_GATEWAY_URL=https://api.triobank.com

# JWT (use secrets manager)
JWT_SECRET=${JWT_SECRET}
JWT_EXPIRATION=1800

# External Services
PAYMENT_GATEWAY_URL=https://api.payment.com
EMAIL_SERVICE_URL=https://email.triobank.com

# Monitoring
METRICS_ENABLED=true
TRACING_ENABLED=true
```

## Docker Compose

### docker-compose.yml (Base)
```yaml
version: '3.8'

services:
  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: triobank
      POSTGRES_USER: triobank
      POSTGRES_PASSWORD: password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

  kafka:
    image: confluentinc/cp-kafka:7.5.0
    environment:
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
    ports:
      - "9092:9092"
    depends_on:
      - zookeeper

  zookeeper:
    image: confluentinc/cp-zookeeper:7.5.0
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
    ports:
      - "2181:2181"

volumes:
  postgres_data:
  redis_data:
```

### docker-compose.dev.yml (Development Override)
```yaml
version: '3.8'

services:
  account-service:
    build: ./services/java/account-service
    ports:
      - "8080:8080"
    environment:
      SPRING_PROFILES_ACTIVE: dev
      DB_HOST: postgres
    depends_on:
      - postgres
      - redis

  transaction-service:
    build: ./services/java/transaction-service
    ports:
      - "8081:8081"
    environment:
      SPRING_PROFILES_ACTIVE: dev
      DB_HOST: postgres
    depends_on:
      - postgres
      - kafka

  web-portal:
    build: ./frontend/web-portal
    ports:
      - "3000:3000"
    environment:
      REACT_APP_API_URL: http://localhost:8000
```

## CI/CD Configurations

### GitHub Actions (.github/workflows/ci.yml)
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'
          
      - name: Build Java Services
        run: ./scripts/build/build-java.sh
        
      - name: Run Tests
        run: ./scripts/test/test-all.sh
        
      - name: Security Scan
        run: |
          docker run --rm -v $(pwd):/scan aquasec/trivy fs /scan
```

## Nginx Configuration

### nginx.conf
```nginx
upstream api_gateway {
    server api-gateway:8000;
}

upstream web_portal {
    server web-portal:3000;
}

server {
    listen 80;
    server_name triobank.com;

    location /api/ {
        proxy_pass http://api_gateway/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location / {
        proxy_pass http://web_portal/;
        proxy_set_header Host $host;
    }
}

server {
    listen 443 ssl http2;
    server_name triobank.com;

    ssl_certificate /etc/nginx/ssl/triobank.crt;
    ssl_certificate_key /etc/nginx/ssl/triobank.key;

    location /api/ {
        proxy_pass http://api_gateway/;
    }

    location / {
        proxy_pass http://web_portal/;
    }
}
```

## Monitoring Configurations

### Prometheus (prometheus.yml)
```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'account-service'
    static_configs:
      - targets: ['account-service:8080']
    metrics_path: '/actuator/prometheus'

  - job_name: 'transaction-service'
    static_configs:
      - targets: ['transaction-service:8081']
    metrics_path: '/actuator/prometheus'

  - job_name: 'postgres-exporter'
    static_configs:
      - targets: ['postgres-exporter:9187']
```

### AlertManager (alertmanager.yml)
```yaml
global:
  resolve_timeout: 5m
  slack_api_url: 'https://hooks.slack.com/services/...'

route:
  group_by: ['alertname', 'service']
  group_wait: 10s
  group_interval: 10s
  repeat_interval: 1h
  receiver: 'slack-notifications'

receivers:
  - name: 'slack-notifications'
    slack_configs:
      - channel: '#alerts'
        title: 'TrioBank Alert'
        text: '{{ range .Alerts }}{{ .Annotations.description }}{{ end }}'
```

## Logging Configuration

### Logback (logback.xml)
```xml
<configuration>
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>logs/application.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>logs/application-%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="FILE" />
    </root>
</configuration>
```

## Kullanım

```bash
# Environment değişkenlerini yükle
source config/environments/dev.env

# Docker Compose ile başlat
docker-compose -f config/docker/docker-compose.yml up

# Development ortamı
docker-compose -f config/docker/docker-compose.yml \
               -f config/docker/docker-compose.dev.yml up
```
