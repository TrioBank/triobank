# Katkıda Bulunma Rehberi

TrioBank Platform'a katkıda bulunmak istediğiniz için teşekkür ederiz! Bu rehber, projeye nasıl katkıda bulunabileceğinizi açıklar.

## 📋 İçindekiler

- [Davranış Kuralları](#davranış-kuralları)
- [Nasıl Katkıda Bulunabilirim?](#nasıl-katkıda-bulunabilirim)
- [Geliştirme Süreci](#geliştirme-süreci)
- [Coding Standards](#coding-standards)
- [Commit Mesajları](#commit-mesajları)
- [Pull Request Süreci](#pull-request-süreci)
- [Test Yazma](#test-yazma)

## 📜 Davranış Kuralları

Bu proje [Contributor Covenant](https://www.contributor-covenant.org/) davranış kurallarını benimser. Projeye katılarak, bu kurallara uyacağınızı kabul etmiş olursunuz.

### Temel İlkeler

- Saygılı ve kapsayıcı olun
- Farklı bakış açılarına açık olun
- Yapıcı eleştiri yapın ve kabul edin
- Topluluk çıkarlarını öncelikli tutun
- Empati gösterin

## 🤝 Nasıl Katkıda Bulunabilirim?

### Bug Raporlama

Bug bulduğunuzda, lütfen aşağıdaki bilgileri içeren bir issue açın:

- Bug'ın açık ve net bir tanımı
- Bug'ı yeniden oluşturma adımları
- Beklenen davranış
- Gerçekleşen davranış
- Ekran görüntüleri (varsa)
- Ortam bilgileri (OS, browser, servis versiyonu)

**Örnek:**

```markdown
### Bug Tanımı
Account Service'te bakiye sorgulama 500 hatası veriyor.

### Yeniden Oluşturma Adımları
1. POST /api/accounts/create ile yeni hesap oluştur
2. GET /api/accounts/{id}/balance endpoint'ine istek at
3. 500 Internal Server Error alınıyor

### Beklenen Davranış
Hesap bakiyesi dönmeli

### Gerçekleşen Davranış
500 hatası ile "NullPointerException" hatası

### Ortam
- OS: Ubuntu 22.04
- Java: 17
- Service Version: 1.2.3
```

### Özellik Önerisi

Yeni özellik önerilerinde:

- Özelliğin ne olduğunu açıklayın
- Neden gerekli olduğunu belirtin
- Mümkünse, nasıl implement edileceğine dair fikirler paylaşın
- Varsa, örnek kullanım senaryoları ekleyin

### Dokümantasyon

Dokümantasyon iyileştirmeleri de değerlidir:

- Typo düzeltmeleri
- Açıklama iyileştirmeleri
- Yeni örnekler ekleme
- Eksik dokümantasyon tamamlama

## 🔧 Geliştirme Süreci

### 1. Repository'yi Fork Edin

```bash
# GitHub üzerinden fork edin, sonra clone edin
git clone https://github.com/YOUR_USERNAME/triobank-platform.git
cd triobank-platform

# Upstream ekleyin
git remote add upstream https://github.com/TrioBank/triobank-platform.git
```

### 2. Development Environment Kurun

```bash
# Geliştirme ortamını kurun
./scripts/setup/setup-dev.sh

# Dependencies install edin
./scripts/setup/install-deps.sh
```

### 3. Feature Branch Oluşturun

```bash
# Main branch'ten güncel kodu çekin
git checkout main
git pull upstream main

# Feature branch oluşturun
git checkout -b feature/your-feature-name
# veya
git checkout -b fix/bug-description
```

Branch isimlendirme:
- `feature/` - Yeni özellikler
- `fix/` - Bug düzeltmeleri
- `docs/` - Dokümantasyon değişiklikleri
- `refactor/` - Code refactoring
- `test/` - Test eklemeleri
- `chore/` - Bakım işleri

### 4. Değişikliklerinizi Yapın

- Kod yazın
- Test yazın
- Dokümantasyon güncelleyin (gerekirse)

### 5. Değişikliklerinizi Test Edin

```bash
# Unit testler
./scripts/test/test-unit.sh

# Integration testler
./scripts/test/test-integration.sh

# Linting
./scripts/utils/lint.sh

# Spesifik servis testi
cd services/java/account-service
mvn clean test
```

### 6. Commit Yapın

```bash
# Değişiklikleri stage'e ekleyin
git add .

# Commit yapın (Conventional Commits formatında)
git commit -m "feat: add balance inquiry API"
```

### 7. Push Edin

```bash
git push origin feature/your-feature-name
```

### 8. Pull Request Oluşturun

GitHub'da repository'nize gidin ve "New Pull Request" butonuna tıklayın.

## 📝 Coding Standards

### Java

[Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) kullanıyoruz.

**Önemli noktalar:**

```java
// Class isimleri PascalCase
public class AccountService {
    
    // Method isimleri camelCase
    public Account createAccount(AccountRequest request) {
        // ...
    }
    
    // Constants UPPER_SNAKE_CASE
    private static final int MAX_RETRY_ATTEMPTS = 3;
    
    // Private fields camelCase
    private final AccountRepository accountRepository;
}
```

**Checkstyle:**

```bash
mvn checkstyle:check
```

### Go

[Effective Go](https://golang.org/doc/effective_go.html) ve [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments) kullanıyoruz.

**Önemli noktalar:**

```go
// Package comment
// Package analytics provides analytics functionality
package analytics

// Exported function PascalCase
func CalculateMetrics(data []float64) Metrics {
    // ...
}

// Unexported function camelCase
func calculateAverage(data []float64) float64 {
    // ...
}

// Constants PascalCase
const MaxDataPoints = 1000
```

**Linting:**

```bash
golangci-lint run ./...
```

### TypeScript/JavaScript

[Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) kullanıyoruz.

**ESLint:**

```bash
npm run lint
npm run lint:fix  # Auto-fix
```

### SQL

- Table isimleri: `snake_case` (örn: `customer_accounts`)
- Column isimleri: `snake_case` (örn: `account_number`)
- Primary keys: `id`
- Foreign keys: `{table}_id` (örn: `customer_id`)

## 📝 Commit Mesajları

[Conventional Commits](https://www.conventionalcommits.org/) formatını kullanıyoruz.

### Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

- **feat**: Yeni özellik
- **fix**: Bug düzeltme
- **docs**: Dokümantasyon değişikliği
- **style**: Code formatting (logic değişikliği yok)
- **refactor**: Refactoring (bug fix veya feature değil)
- **test**: Test ekleme veya düzeltme
- **chore**: Build process veya tool değişiklikleri
- **perf**: Performance iyileştirmeleri
- **ci**: CI/CD değişiklikleri

### Örnekler

```bash
# Basit commit
feat: add balance inquiry API

# Scope ile
feat(account-service): add balance inquiry API

# Breaking change
feat(auth-service)!: change JWT token format

BREAKING CHANGE: JWT token structure has changed.
Old tokens will not work with this version.

# Body ile
fix(transaction-service): fix duplicate transaction issue

Added transaction deduplication logic using Redis cache.
Transactions are now checked against a 5-minute cache window.

Fixes #123
```

### Commit Kuralları

- Subject line 50 karakter veya daha az
- Subject imperative mood'da ("add" not "added")
- Body 72 karakterde wrap et
- Footer'da issue reference ekle

## 🔍 Pull Request Süreci

### PR Oluşturma

1. **Başlık**: Net ve açıklayıcı
   ```
   feat(account-service): add balance inquiry endpoint
   ```

2. **Açıklama**: PR template'ini doldurun
   ```markdown
   ## Değişiklik Türü
   - [x] Bug fix
   - [ ] New feature
   - [ ] Breaking change
   
   ## Açıklama
   Balance inquiry endpoint'i eklendi.
   
   ## Test Edilen Senaryolar
   - [x] Geçerli account ID ile test
   - [x] Geçersiz account ID ile test
   - [x] Authorization test
   
   ## Checklist
   - [x] Tests added/updated
   - [x] Documentation updated
   - [x] Code follows style guidelines
   - [x] Self-review completed
   ```

3. **Reviewers**: İlgili team member'ları ekleyin

4. **Labels**: Uygun label'ları ekleyin
   - `bug`, `enhancement`, `documentation`, etc.

### PR Review Süreci

1. **Automated Checks**: CI/CD pipeline pass olmalı
   - Build success
   - Tests pass
   - Linting pass
   - Code coverage threshold

2. **Code Review**: En az 1 approval gerekli
   - Code quality
   - Test coverage
   - Documentation
   - Security considerations

3. **Address Feedback**: Review comment'leri yanıtlayın
   ```bash
   # Değişiklik yapın
   git add .
   git commit -m "fix: address review comments"
   git push origin feature/your-feature-name
   ```

4. **Merge**: Approval sonrası merge edilir
   - Squash and merge (preferred)
   - Clean commit history

## 🧪 Test Yazma

### Test Coverage

- Minimum %80 code coverage
- Her yeni özellik için test yazılmalı
- Bug fix'ler için regression test

### Unit Tests

**Java (JUnit 5):**

```java
@Test
@DisplayName("Should create account successfully")
void shouldCreateAccountSuccessfully() {
    // Given
    AccountRequest request = new AccountRequest("John", "Doe");
    
    // When
    Account account = accountService.createAccount(request);
    
    // Then
    assertNotNull(account.getId());
    assertEquals("John", account.getFirstName());
}
```

**Go:**

```go
func TestCalculateMetrics(t *testing.T) {
    data := []float64{1.0, 2.0, 3.0}
    
    result := CalculateMetrics(data)
    
    assert.Equal(t, 2.0, result.Average)
}
```

**TypeScript (Jest):**

```typescript
describe('AccountService', () => {
  it('should create account', async () => {
    const request = { firstName: 'John', lastName: 'Doe' };
    
    const account = await accountService.create(request);
    
    expect(account.id).toBeDefined();
    expect(account.firstName).toBe('John');
  });
});
```

### Integration Tests

```java
@SpringBootTest
@AutoConfigureMockMvc
class AccountControllerIntegrationTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Test
    void shouldCreateAccountViaAPI() throws Exception {
        mockMvc.perform(post("/api/accounts")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"firstName\":\"John\",\"lastName\":\"Doe\"}"))
                .andExpect(status().isCreated())
                .andExpect(jsonPath("$.id").exists());
    }
}
```

## ❓ Sorular?

- **Slack**: triobank.slack.com #dev-support
- **Email**: dev@triobank.com
- **GitHub Discussions**: Genel sorular için

## 🙏 Teşekkürler!

Zaman ayırıp katkıda bulunduğunuz için teşekkür ederiz! Her katkı, TrioBank Platform'u daha iyi hale getiriyor.
