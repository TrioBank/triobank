# Shared

Bu dizin tüm servisler ve uygulamalar arasında paylaşılan kütüphaneler, şemalar ve contract'ları içerir.

## Dizin Yapısı

```
shared/
├── libraries/          # Paylaşılan kod kütüphaneleri
│   ├── java/          # Java shared libraries
│   │   ├── common-utils/
│   │   ├── security/
│   │   └── messaging/
│   ├── go/            # Go shared packages
│   │   ├── logger/
│   │   ├── metrics/
│   │   └── httputil/
│   └── typescript/    # TypeScript shared libraries
│       ├── types/
│       ├── utils/
│       └── api-client/
│
├── schemas/           # Data schemas
│   ├── avro/         # Avro schemas (Kafka messages)
│   ├── protobuf/     # Protocol Buffer definitions
│   └── json-schema/  # JSON schemas
│
└── contracts/        # API contracts
    ├── openapi/      # OpenAPI/Swagger specifications
    ├── grpc/         # gRPC proto files
    └── graphql/      # GraphQL schemas
```

## Paylaşılan Kütüphaneler

### Java Shared Libraries

**common-utils**
- Date/Time utilities
- String manipulations
- Validation helpers
- Constants

**security**
- JWT token handling
- Encryption/Decryption
- Password hashing
- OAuth2 utilities

**messaging**
- Kafka producer/consumer abstractions
- Message serialization/deserialization
- Event models

Örnek kullanım:
```xml
<dependency>
    <groupId>com.triobank</groupId>
    <artifactId>common-utils</artifactId>
    <version>1.0.0</version>
</dependency>
```

### Go Shared Packages

**logger**
```go
import "github.com/triobank/shared/go/logger"

log := logger.New(logger.Config{
    Level: "info",
    Format: "json",
})
```

**metrics**
```go
import "github.com/triobank/shared/go/metrics"

metrics.Counter("requests_total").Inc()
```

### TypeScript Shared Libraries

**@triobank/types**
```typescript
import { User, Account, Transaction } from '@triobank/types';
```

**@triobank/api-client**
```typescript
import { TrioBankClient } from '@triobank/api-client';

const client = new TrioBankClient({
  baseURL: 'https://api.triobank.com'
});
```

## Schemas

### Avro Schemas (Kafka Messages)

```json
{
  "type": "record",
  "name": "TransactionEvent",
  "namespace": "com.triobank.events",
  "fields": [
    {"name": "transactionId", "type": "string"},
    {"name": "accountId", "type": "string"},
    {"name": "amount", "type": "double"},
    {"name": "timestamp", "type": "long"}
  ]
}
```

### Protocol Buffers

```protobuf
syntax = "proto3";

package triobank.v1;

message Account {
  string id = 1;
  string customer_id = 2;
  double balance = 3;
  string currency = 4;
}
```

## API Contracts

### OpenAPI Specifications

Her servis için OpenAPI 3.0 specification:

```yaml
openapi: 3.0.0
info:
  title: Account Service API
  version: 1.0.0
paths:
  /accounts:
    get:
      summary: List accounts
      responses:
        '200':
          description: Success
```

### Contract Testing

- Pact kullanarak consumer-driven contract testing
- Schema validation ile API contract enforcement

## Versiyonlama

- Semantic versioning (MAJOR.MINOR.PATCH)
- Breaking changes için major version bump
- Backward compatibility için deprecation period

## Yayınlama

### Java Libraries
```bash
mvn clean deploy
```

### Go Packages
```bash
# Go modules ile version tagging
git tag shared/go/v1.0.0
git push origin shared/go/v1.0.0
```

### NPM Packages
```bash
npm publish --access public
```
