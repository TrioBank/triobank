# Services

Bu dizin tüm backend mikroservislerini içerir. Servisler teknolojilerine göre organize edilmiştir.

## Dizin Yapısı

```
services/
├── java/           # Java/Spring Boot mikroservisleri
│   ├── account-service/        # Hesap yönetimi servisi
│   ├── transaction-service/    # İşlem (transaction) servisi
│   ├── auth-service/           # Kimlik doğrulama ve yetkilendirme servisi
│   ├── customer-service/       # Müşteri yönetimi servisi
│   ├── payment-service/        # Ödeme işlemleri servisi
│   └── notification-service/   # Bildirim servisi (email, SMS)
│
└── go/             # Go mikroservisleri
    ├── analytics-service/      # Analitik ve raporlama servisi
    ├── reporting-service/      # Rapor oluşturma servisi
    └── monitoring-service/     # Sistem izleme servisi
```

## Teknoloji Seçimi

### Java Servisleri
- **Framework**: Spring Boot 3.x
- **Build Tool**: Maven veya Gradle
- **Database**: PostgreSQL (primary), Redis (cache)
- **Messaging**: Apache Kafka / RabbitMQ
- **API Documentation**: OpenAPI/Swagger

### Go Servisleri
- **Framework**: Gin / Echo / Chi
- **Database**: PostgreSQL, TimescaleDB (time-series data)
- **Monitoring**: Prometheus, Grafana

## Servis Standartları

Her servis aşağıdaki yapıya sahip olmalıdır:

```
service-name/
├── src/                    # Kaynak kod
├── tests/                  # Test dosyaları
├── Dockerfile             # Container imajı tanımı
├── README.md              # Servis dokümantasyonu
├── pom.xml / go.mod       # Bağımlılık yönetimi
└── .env.example           # Örnek environment değişkenleri
```

## Servisler Arası İletişim

- **Senkron**: REST API (HTTP/HTTPS)
- **Asenkron**: Message Queue (Kafka/RabbitMQ)
- **Service Discovery**: Kubernetes DNS veya Consul

## Güvenlik

- Her servis JWT token ile kimlik doğrulama kullanır
- Servisler arası iletişim mTLS ile şifrelenir
- API Gateway tüm dış istekleri yönetir
