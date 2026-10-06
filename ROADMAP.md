# MiniCart — LC Waikiki Trainee Hazırlık Yol Haritası

> **Süre:** 5 Ekim (Pazartesi) → 11 Ekim (Pazar) akşamı · **7 gün**
> **Başlangıç:** 12 Ekim 2026, LC Waikiki — Backend / .NET / Sepet & Ödeme birimi
> **Araçlar:** .NET 10 · VS Code (C# Dev Kit) · Claude Code · Git/GitHub · Docker · SQL Server · Redis · RabbitMQ · Kafka

---

## 0. Bu dosya nasıl kullanılır?

Her gün aynı döngüyle ilerlenir. **Kod, kavram anlaşılmadan yazılmaz.**

```text
1. KAVRAM       → Nedir? Hangi problemi çözer?
2. NEDEN        → Olmasa ne olurdu?
3. ŞİRKETTE     → LC Waikiki gibi bir e-ticaret sisteminde nerede karşıma çıkar?
4. SPRING       → Java/Spring'deki karşılığı ne?
5. KODLA        → MiniCart'a uygula (Issue → branch → PR → merge)
6. KENDİNİ TEST ET → Günün sorularını sesli cevaplayabiliyor muyum?
```

**Gerçekçi beklenti:** Gün 1–5 "sağlam bilmen gerekenler", Gün 6–7 "çalışan demo + kavramı anlamak" seviyesindedir. 7 günde mikroservis ustası olunmaz; amaç 12 Ekim'de bu kavramlar konuşulurken masada kaybolmamak.

---

## 1. Trainee olarak en çok neyle karşılaşacağım?

Bu liste programın önceliklerini belirler. Üstteki maddeler daha sık ve daha erken karşına çıkar.

| # | Ne? | Ne sıklıkta? | Programda nerede? |
|---|-----|-------------|-------------------|
| 1 | Ortam kurulumu, projeyi yerelde ayağa kaldırma | İlk hafta | Gün 1 |
| 2 | **Başkasının kodunu okuma**, bir request'i uçtan uca takip etme | Her gün | Her gün |
| 3 | Git: branch, commit, PR, review yorumu düzeltme, conflict | Her gün | Her gün (Gün 1 temel) |
| 4 | Bug fix: log oku → debug et → düzelt → test ekle | Çok sık | Gün 2, 4 |
| 5 | **SQL / T-SQL**: sorgu yazma, düzeltme, stored procedure, performans | Çok sık | Gün 3 |
| 6 | Küçük feature: endpoint'e alan ekleme, yeni endpoint, validation kuralı | Sık | Gün 2 |
| 7 | Unit test yazma, mevcut testi kırmamak | Sık | Gün 4 |
| 8 | Agile/Scrum: daily, sprint planning, Jira/Azure Boards task'ı | Her gün | Bölüm 7 |
| 9 | Config / environment (`appsettings.Development.json` vs Production) | Sık | Gün 2 |
| 10 | Cache (Redis), auth (JWT/token), idempotency | Orta | Gün 5 |
| 11 | Mesajlaşma (RabbitMQ/Kafka) consumer kodu okumak | Orta | Gün 6–7 |
| 12 | CI/CD pipeline, Docker, Kubernetes | Görürsün, nadiren dokunursun | Gün 5–7 (okuma seviyesi) |

### "Gördüğümde tanımam gerekenler" sözlüğü

Bunları yazman beklenmez; **karşına çıkınca ne olduğunu bilmen** yeterli. Her birine 5–10 dakika okuma ayır.

- **Kod/mimari:** Repository, Unit of Work, CQRS, MediatR, Mediator pattern, FluentValidation, Result pattern, Specification pattern
- **Veri:** Dapper, stored procedure, execution plan, index seek/scan, deadlock, isolation level, `RowVersion`
- **Eski stack:** .NET Framework 4.x, `Startup.cs`, `Global.asax`, `web.config`, ADO.NET, WCF
- **Dayanıklılık:** Polly, retry, circuit breaker, timeout, `Microsoft.Extensions.Http.Resilience`
- **Mesajlaşma:** exchange, queue, binding, topic, partition, offset, consumer group, dead-letter queue, outbox, saga
- **Altyapı:** `Dockerfile`, `docker-compose.yml`, Kubernetes pod/deployment, Azure DevOps / GitLab pipeline, feature flag
- **İzleme:** structured logging, correlation id, OpenTelemetry, Kibana/Elastic, Grafana, Application Insights

---

## 2. Kurulum (Gün 1 sabahı)

### 2.1 Temel araçlar

- [ ] .NET 10 SDK → `dotnet --version`
- [ ] **VS Code** (1.98.0 veya üstü — Claude Code eklentisi bunu istiyor) → `code --version`
- [ ] Git → `git --version`
- [ ] Docker Desktop → `docker --version`, `docker compose version`
- [ ] (Opsiyonel) GitHub CLI → `gh --version` — PR'ları terminalden açmak için
- [ ] `code` komutu PATH'te değilse: VS Code'da `Ctrl+Shift+P` → **"Shell Command: Install 'code' command in PATH"**

```bash
git config --global user.name "Ad Soyad"
git config --global user.email "mail@ornek.com"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"     # commit/rebase mesajları VS Code'da açılsın

ssh-keygen -t ed25519 -C "mail@ornek.com"         # sonra public key'i GitHub → Settings → SSH keys'e ekle
ssh -T git@github.com                             # bağlantıyı test et

dotnet tool install --global dotnet-ef            # EF Core migration komutları için
dotnet ef --version
```

### 2.2 VS Code eklentileri

Hepsini terminalden tek seferde kurabilirsin:

```bash
code --install-extension anthropic.claude-code
code --install-extension ms-dotnettools.csdevkit
code --install-extension humao.rest-client
code --install-extension ms-mssql.mssql
code --install-extension ms-azuretools.vscode-containers
code --install-extension eamodio.gitlens
code --install-extension github.vscode-pull-request-github
code --install-extension CucumberOpen.cucumber-official
code --install-extension redhat.vscode-yaml
code --install-extension editorconfig.editorconfig
code --install-extension usernamehw.errorlens
```

| Eklenti | Ne işe yarar? | Not |
|---------|---------------|-----|
| **Claude Code** (Anthropic) | Claude Code'un grafik paneli; diff'leri editörde gösterir, seçtiğin kodu görür | Zorunlu |
| **C# Dev Kit** (Microsoft) | C# dil desteği, Solution Explorer, debug, **Test Explorer**; C# ve .NET Install Tool eklentilerini de kurar | Zorunlu. Lisansı bireysel/eğitim kullanımında ücretsiz; kurumsal kullanım koşullarını kontrol et |
| **REST Client** | `.http` dosyalarından istek atma (Postman'e gerek kalmaz, istekler repoda versiyonlanır) | Zorunlu |
| **SQL Server (mssql)** | SQL Server'a bağlanma, T-SQL yazma, **execution plan** görme | Zorunlu. Azure Data Studio emekliye ayrıldığı için Microsoft bu eklentiyi öneriyor |
| **Container Tools** (Microsoft) | Container/image görüntüleme, compose dosyası desteği | Marketplace'te "Docker" adıyla da görünebilir |
| **GitLens** | Satır bazında "bunu kim, ne zaman, neden yazdı" (blame), commit geçmişi | Production kodu okurken çok işe yarar |
| **GitHub Pull Requests** | PR'ları VS Code içinden açma, review etme, yorum yazma | |
| **Cucumber (Gherkin)** | `.feature` dosyalarında syntax highlighting ve otomatik tamamlama | BDD için |
| **YAML** (Red Hat) | `docker-compose.yml` doğrulama | |
| EditorConfig, Error Lens | Kod stili tutarlılığı; hataları satırın yanında gösterme | Opsiyonel |

### 2.3 Claude Code: eklenti mi CLI mı?

- **Eklenti (önerilen):** Sağ üstteki Claude ikonuyla paneli açarsın. İlk açılışta Claude hesabınla giriş yaparsın. Önerilen değişiklikleri diff olarak editörde görür, onaylar ya da reddedersin.
- **CLI:** Eklenti, entegre terminalde `claude` komutunu çalıştırmak için yeterli olmayabilir; resmi dokümana göre bunun için CLI'ı ayrıca kurman gerekebilir. Güncel kurulum komutu: `code.claude.com/docs` → Setup. Kurduktan sonra `claude --version` ile kontrol et.
- Eklenti görünmüyorsa: VS Code'u yeniden başlat veya `Ctrl+Shift+P` → **"Developer: Reload Window"**.
- Pratik kullanım: günlük işte **eklenti paneli**, slash komutları ve detaylı ayarlar için **CLI**. İkisi aynı `CLAUDE.md`'yi okur.

### 2.4 Günlük CLI kopya kâğıdı

```bash
# .NET
dotnet build                                   # derle
dotnet run --project src/MiniCart.Api          # çalıştır
dotnet watch --project src/MiniCart.Api        # kod değişince otomatik yeniden başlat
dotnet test                                    # tüm testler
dotnet test --filter "FullyQualifiedName~Cart" # sadece adı Cart içeren testler
dotnet add src/MiniCart.Infrastructure package Microsoft.EntityFrameworkCore.SqlServer
dotnet list package --outdated                 # güncellenebilir paketler

# EF Core (migration Infrastructure'da, başlangıç projesi Api)
dotnet ef migrations add InitialCreate -p src/MiniCart.Infrastructure -s src/MiniCart.Api
dotnet ef database update               -p src/MiniCart.Infrastructure -s src/MiniCart.Api
dotnet ef migrations remove             -p src/MiniCart.Infrastructure -s src/MiniCart.Api
dotnet ef migrations script             -p src/MiniCart.Infrastructure -s src/MiniCart.Api  # üretilen SQL'i gör

# Docker
docker compose up -d        # altyapıyı arka planda başlat
docker compose ps           # neler çalışıyor
docker compose logs -f api  # bir servisin log'unu izle
docker compose down         # durdur (-v eklersen volume'ları da siler, veri gider)

# Git + GitHub CLI
git switch -c feature/7-global-exception-handling
git status && git diff
git add -p                  # değişiklikleri parça parça seçerek stage'e al (çok faydalı)
git commit -m "feat: add global exception handler"
git push -u origin HEAD
gh pr create --fill         # PR aç (veya GitHub web arayüzünden)
gh pr view --web
git log --oneline --graph --all
```

### 2.5 VS Code kısayolları

| Kısayol | İş |
|---------|----|
| `Ctrl+Shift+P` | Komut paleti (her şey buradan) |
| `Ctrl+P` | Dosya aç |
| `F12` / `Shift+F12` | Tanıma git / kullanıldığı yerleri bul — **kod okumanın temel aracı** |
| `Ctrl+.` | Hızlı düzeltme (using ekle, interface implement et) |
| `F2` | Yeniden adlandır (refactor) |
| `F5` / `F9` / `F10` / `F11` | Debug başlat / breakpoint / adım atla / içine gir |
| ``Ctrl+` `` | Entegre terminal |
| `Ctrl+Shift+G` | Source Control paneli |

### Claude Code ile çalışma kuralları

1. **Git'i ben yaparım.** Branch, commit, push, PR terminalden benim elimle.
2. **Önce Plan Mode** (`Shift+Tab`): kavram ve yaklaşımı tartış, sonra kodla.
3. **Çekirdek mantığı ben yazarım.** Domain kuralları, servis mantığı, sorgular benim.
4. **Claude'a delege:** boilerplate, DTO, test iskeleti, Docker Compose, Gherkin taslakları.
5. **Claude = reviewer.** "Hataları söyle, düzeltme, ben düzelteyim."
6. **Anlamadığım satır repoya girmez.** Her diff'i okur, "neden böyle?" diye sorarım.
7. Konu değişince `/clear`, oturum uzayınca `/compact`.

---

## 3. Mimari yaklaşım

### 3.1 Neden Clean Architecture (katmanlı)?

Kurumsal .NET projelerinin büyük kısmı **N-Layer** veya **Clean/Onion Architecture** kullanır. Bunu bilirsen LCW'deki solution yapısını açtığında hangi projenin ne iş yaptığını hemen anlarsın.

**Temel kural — bağımlılıklar içe doğru akar:**

```text
            ┌──────────────────────────┐
            │         Api              │  Controller, Middleware, DI kayıtları
            └───────────┬──────────────┘
                        │ referans
     ┌──────────────────┼────────────────────┐
     ▼                                       ▼
┌──────────────┐                    ┌──────────────────┐
│ Application  │ ◄───── referans ── │ Infrastructure   │
│ Use case'ler │                    │ EF Core, SQL,    │
│ Interface'ler│                    │ Redis, RabbitMQ  │
└──────┬───────┘                    └──────────────────┘
       │ referans
       ▼
┌──────────────┐
│   Domain     │  Entity, iş kuralları, hiçbir şeye bağımlı değil
└──────────────┘
```

| Katman | İçerik | Bağımlı olduğu | Spring karşılığı |
|--------|--------|---------------|------------------|
| **Domain** | `Cart`, `Order`, `Product`, iş kuralları, domain exception'ları | Hiçbir şey | `@Entity` ama framework'süz saf model |
| **Application** | Servisler/use case'ler, DTO'lar, `ICartRepository` gibi **interface'ler** | Domain | `@Service` + port interface'leri |
| **Infrastructure** | `DbContext`, repository implementasyonları, Redis, mesaj broker | Application | `@Repository`, adapter'lar |
| **Api** | Controller, middleware, `Program.cs`, config | Application + Infrastructure | `@RestController`, `@ControllerAdvice` |

**Neden önemli?** Ödeme sağlayıcısı değişirse veya SQL Server yerine başka bir şey gelirse sadece Infrastructure değişir; iş kuralları dokunulmadan kalır. Ayrıca Domain ve Application, veritabanı olmadan **unit test** edilebilir. Spring'deki Hexagonal (Ports & Adapters) ile aynı fikir.

**Aşırıya kaçma:** Bu projede CQRS/MediatR kullanmıyoruz. Önce sade servis katmanını oturt, sonra gerektiğinde ekle. Şirkette MediatR görürsen: "Controller → Mediator → Handler" zinciri, servis çağrısının dolaylı hali.

### 3.2 Domain: neyi modelliyoruz?

```text
Product   (Id, Name, Price, StockQuantity, RowVersion)
Cart      (Id, CustomerId, Items[])            → CartItem (ProductId, Quantity, UnitPrice)
Order     (Id, CustomerId, Items[], Total, Status, IdempotencyKey, CreatedAt)
```

**İş kuralları** (testlerin ve BDD senaryolarının kaynağı):

1. Sepete stoktan fazla ürün eklenemez.
2. Bir üründen sepete en fazla 10 adet eklenebilir.
3. Aynı ürün tekrar eklenirse yeni satır açılmaz, adet artar.
4. Boş sepetten sipariş oluşturulamaz.
5. Sipariş oluşturulunca stok düşer (atomik — transaction).
6. Aynı `Idempotency-Key` ile gelen ikinci sipariş isteği **yeni sipariş oluşturmaz**, ilkini döner.
7. Sipariş durumu: `Pending → Paid → Shipped` veya `Pending → Cancelled`. `Shipped` sipariş iptal edilemez.

### 3.3 Solution yapısı

```text
MiniCart/
├── src/
│   ├── MiniCart.Api/
│   ├── MiniCart.Application/
│   ├── MiniCart.Domain/
│   └── MiniCart.Infrastructure/
├── tests/
│   ├── MiniCart.UnitTests/          # Domain + Application, DB yok
│   ├── MiniCart.IntegrationTests/   # WebApplicationFactory + gerçek SQL Server (Testcontainers)
│   └── MiniCart.AcceptanceTests/    # BDD — Reqnroll + Gherkin
├── services/                         # Gün 6–7: mikroservisler
├── docker-compose.yml
├── CLAUDE.md
├── ROADMAP.md
└── README.md
```

### 3.4 Evrim planı: monolith → mikroservis

```text
Gün 1–5: Tek API (modüler monolith)       Gün 6–7: Servislere böl
┌────────────────────────┐                ┌──────────┐  ┌────────────┐  ┌──────────┐
│ MiniCart.Api           │                │ Ordering │  │ Inventory  │  │ Payment  │
│  ├ Catalog             │      ──►       │  + DB    │  │  + DB      │  │ (worker) │
│  ├ Cart                │                └────┬─────┘  └─────┬──────┘  └────┬─────┘
│  └ Ordering            │                     └──── RabbitMQ / Kafka ───────┘
└────────────────────────┘
```

**Önce monolith, sonra bölme** — bu bilinçli bir seçim. Gerçek şirketlerde de çoğu sistem monolith olarak başlar; servis sınırları domain'i tanıdıktan sonra çizilir. Gün 6'da "nereden böleceğiz?" sorusunu, 5 gün boyunca tanıdığın bir domain üzerinde cevaplayacaksın.

---

## 4. Test stratejisi ve BDD

### 4.1 Test piramidi

```text
          /\        Acceptance / BDD  (az, yavaş, iş diliyle)
         /  \       "Müşteri sepete stoktan fazla ürün ekleyemez"
        /────\
       /      \     Integration  (orta) — API + gerçek DB
      /────────\
     /          \   Unit  (çok, hızlı) — Domain + Application, DB yok
    /────────────\
```

| Tür | Neyi test eder? | Araç | Spring karşılığı |
|-----|----------------|------|------------------|
| Unit | Tek sınıf/metot, bağımlılıklar mock | xUnit + NSubstitute (veya Moq) | JUnit 5 + Mockito |
| Integration | HTTP → Controller → DB uçtan uca | `WebApplicationFactory` + Testcontainers | `@SpringBootTest` + Testcontainers |
| Acceptance (BDD) | İş kuralı, iş diliyle | **Reqnroll** + xUnit | Cucumber-JVM |

> **Lisans notu:** 2025'te bazı popüler .NET kütüphaneleri ticari lisansa geçti (AutoMapper, MediatR, FluentAssertions'ın yeni sürümleri, MassTransit v9 gibi). Kullanmadan önce güncel lisans durumuna bak. Bu projede assertion için **xUnit `Assert`** veya **Shouldly**, mapping için **elle mapping** kullanıyoruz — öğrenme açısından da daha iyi. Şirkette AutoMapper görürsen ne yaptığını Mert Özen'in 9. videosundan biliyor olacaksın.

### 4.2 TDD vs BDD — kavramlar

**TDD (Test-Driven Development):** Önce kırmızı test → testi geçiren minimum kod → refactor. Geliştiricinin dilidir, "bu metot doğru çalışıyor mu?" sorusunu cevaplar.

**BDD (Behavior-Driven Development):** TDD'nin üstüne kurulur ama soruyu değiştirir: **"Sistem iş açısından doğru davranıyor mu?"** Senaryolar iş birimi, QA ve geliştiricinin birlikte okuyabileceği dilde yazılır.

- **Gherkin:** `Feature`, `Scenario`, `Given` (ön koşul), `When` (eylem), `Then` (beklenen sonuç), `And`/`But`.
- **Three Amigos:** Ürün sahibi + geliştirici + test uzmanı, kodlamadan önce senaryoları birlikte yazar. Belirsizlikler kod yazılmadan çıkar.
- **Living documentation:** Feature dosyaları hem test hem güncel dokümantasyondur.
- **Step definitions:** Her Gherkin satırını C# koduna bağlayan metotlar.
- **Scenario Outline + Examples:** Aynı senaryoyu farklı verilerle çalıştırma (JUnit `@ParameterizedTest` gibi).

**Neden önemli?** Sepet ve ödeme gibi alanlarda bir kuralın yanlış anlaşılması doğrudan para kaybıdır. "Kupon indirimden önce mi uygulanır, sonra mı?" sorusu kodda değil, senaryoda netleşir.

**Şirkette nerede karşına çıkar?** Jira'daki task'ların "Acceptance Criteria" kısmı çoğu zaman Given/When/Then formatındadır. Sen kod yazmasan bile bu formatı okuyup testine çevirmen beklenebilir. Bazı ekiplerde QA ekibi Reqnroll/SpecFlow feature'ları yazar. (Not: SpecFlow geliştirmesi sonlandı; topluluk devamı **Reqnroll**'dur. Eski projelerde SpecFlow görebilirsin, sözdizimi neredeyse aynı.)

### 4.3 Örnek feature dosyası

```gherkin
Feature: Sepete ürün ekleme
  Müşteri olarak
  Satın almak istediğim ürünleri sepete eklemek istiyorum
  Böylece tek seferde sipariş verebilirim

  Background:
    Given stokta 5 adet "Basic Tişört" var ve fiyatı 199.99 TL

  Scenario: Stokta yeterli ürün varken sepete ekleme
    When müşteri sepete 2 adet "Basic Tişört" ekler
    Then sepette 2 adet "Basic Tişört" bulunur
    And sepet toplamı 399.98 TL olur

  Scenario: Stoktan fazla ürün eklenemez
    When müşteri sepete 6 adet "Basic Tişört" eklemeye çalışır
    Then "Yetersiz stok" hatası alır
    And sepet boş kalır

  Scenario Outline: Aynı ürün tekrar eklendiğinde adet artar
    Given müşterinin sepetinde <ilk> adet "Basic Tişört" var
    When müşteri sepete <ek> adet daha ekler
    Then sepette <toplam> adet "Basic Tişört" bulunur

    Examples:
      | ilk | ek | toplam |
      | 1   | 1  | 2      |
      | 2   | 3  | 5      |
```

---

## 5. Git iş akışı (her gün)

```text
GitHub Issue aç  →  git switch main && git pull  →  git switch -c feature/<no>-<kisa-ad>
→ küçük commit'ler  →  git push -u origin <branch>  →  PR aç (şablonla)
→ kendi PR'ını review et (en az 2 yorum)  →  düzelt  →  squash merge  →  branch sil
```

**Branch adı:** `feature/7-global-exception-handling`, `fix/12-cart-total-rounding`
**Commit mesajı (Conventional Commits):** `feat: add cart item quantity limit`, `fix: ...`, `test: ...`, `refactor: ...`, `chore: ...`
**PR şablonu** (`.github/pull_request_template.md`):

```markdown
## Ne değişti?
## Neden?
## Nasıl test edilir?
## Kontrol listesi
- [ ] Testler geçiyor (`dotnet test`)
- [ ] Yeni davranış için test eklendi
- [ ] Secret / bin / obj commit'lenmedi
```

**Altın kurallar:** `main`'e direkt push yok · PR küçük ve tek amaçlı · push'tan önce `git status` + `git diff` · paylaşılan branch'te force push yok (kendi branch'inde gerekiyorsa `--force-with-lease`) · push edilmiş commit'i geri almak için `revert`.

---

## 6. Günlük program

### Gün 1 — 5 Ekim: Kurulum, Git, mimari iskelet

**Kavram:** Git'in 4 alanı (working dir, staging, local repo, remote), branch = commit'i gösteren etiket, PR = review isteği. Clean Architecture bağımlılık kuralı.
**Kaynak:** Git kursu (seçici izleme listesi: iş akışı, branch/merge, stash, merge conflict, rebase conflict, GitHub + SSH, fork/PR — SourceTree, P4Merge, Atom, Cmder, IntelliJ, GitHub Desktop derslerini atla).

**Kodla:**

```bash
mkdir MiniCart && cd MiniCart
git init
dotnet new gitignore
dotnet new sln -n MiniCart      # .NET 10'da .slnx üretebilir, sorun değil

dotnet new webapi    -n MiniCart.Api            -o src/MiniCart.Api --use-controllers
dotnet new classlib  -n MiniCart.Application    -o src/MiniCart.Application
dotnet new classlib  -n MiniCart.Domain         -o src/MiniCart.Domain
dotnet new classlib  -n MiniCart.Infrastructure -o src/MiniCart.Infrastructure
dotnet new xunit     -n MiniCart.UnitTests        -o tests/MiniCart.UnitTests
dotnet new xunit     -n MiniCart.IntegrationTests -o tests/MiniCart.IntegrationTests
dotnet new xunit     -n MiniCart.AcceptanceTests  -o tests/MiniCart.AcceptanceTests

dotnet sln add src/*/*.csproj tests/*/*.csproj

dotnet add src/MiniCart.Application    reference src/MiniCart.Domain
dotnet add src/MiniCart.Infrastructure reference src/MiniCart.Application
dotnet add src/MiniCart.Api            reference src/MiniCart.Application src/MiniCart.Infrastructure
dotnet add tests/MiniCart.UnitTests        reference src/MiniCart.Application src/MiniCart.Domain
dotnet add tests/MiniCart.IntegrationTests reference src/MiniCart.Api
dotnet add tests/MiniCart.AcceptanceTests  reference src/MiniCart.Application src/MiniCart.Domain

dotnet build && dotnet test
```

- [ ] GitHub repo, README, PR şablonu, bu `ROADMAP.md` ve `CLAUDE.md` repoda
- [ ] Bilerek bir merge conflict üretip çözdün
- [ ] `restore`, `restore --staged`, `commit --amend`, `revert`, `stash` denendi

**Kendini test et:** `git fetch` ile `git pull` farkı? Rebase neden commit hash'lerini değiştirir? Domain projesi neden Infrastructure'a referans vermez?

---

### Gün 2 — 6 Ekim: ASP.NET Core çekirdeği

**Kaynak (Mert Özen):** 2, 3, 4, 5, 6, 9, 11, 12, 13

| Kavram | Neden önemli / şirkette | Spring karşılığı |
|--------|------------------------|------------------|
| `Program.cs`, host, builder | Projeyi açınca ilk bakacağın yer: neler kayıtlı, pipeline nasıl | `@SpringBootApplication` + auto-config |
| Routing, Controller, `[ApiController]` | Endpoint ekleme/değiştirme en sık task | `@RestController`, `@RequestMapping` |
| Model binding, validation | Hatalı istek DB'ye kadar gitmesin | `@RequestBody`, `@Valid` |
| **Middleware pipeline** | Sıra hatası = auth çalışmıyor, CORS bozuk vb. | Servlet Filter zinciri |
| **DI lifetime'ları** | Captive dependency, `DbContext` thread-safety hataları | Bean scope'ları |
| Global exception handling, ProblemDetails | Tek tip hata formatı, log'larda izlenebilirlik | `@ControllerAdvice` |
| Configuration, Options pattern | Ortam bazlı ayar, secret yönetimi | `application.yml`, `@ConfigurationProperties` |
| Logging (structured) | Bug fix'in yarısı log okumak | SLF4J + Logback |

**DI lifetime'ları — mutlaka bil:**

| Lifetime | Ne zaman oluşur? | Örnek | Spring |
|----------|------------------|-------|--------|
| Singleton | Uygulama boyunca 1 kez | Config, `HttpClient` factory, cache istemcisi | singleton |
| Scoped | Her HTTP isteğinde 1 kez | `DbContext`, repository, servis | request |
| Transient | Her istendiğinde yeni | Hafif, durumsuz yardımcılar | prototype |

⚠️ **Captive dependency:** Singleton içine Scoped inject edilirse Scoped nesne uygulama boyunca yaşar → `DbContext` paylaşılır → concurrency hataları. .NET Development ortamında bu hatayı başlangıçta yakalar.

**Kodla:**
- [ ] Domain: `Product`, `Cart`, `CartItem` — kurallar **entity'nin içinde** (`cart.AddItem(...)` stok ve 10 adet kuralını kontrol eder)
- [ ] Application: `ICartService`, `CartService`, `IProductRepository` interface'i, request/response DTO'ları (elle mapping)
- [ ] Infrastructure: şimdilik in-memory repository (yarın EF Core ile değişecek — mimarinin faydasını göreceksin)
- [ ] Api: `ProductsController`, `CartsController`
- [ ] `IExceptionHandler` ile global handler: domain exception → 400/409, beklenmeyen → 500 + ProblemDetails
- [ ] Her isteğe `X-Correlation-Id` ekleyen ve süreyi loglayan **kendi middleware'in**
- [ ] OpenAPI + bir UI (Scalar veya Swashbuckle; .NET 9+ şablonları yerleşik OpenAPI ile gelir)
- [ ] `requests.http` dosyası yaz, endpoint'leri REST Client ile VS Code'dan çağır (dosya repoda kalsın)
- [ ] F5 ile debug: `CartService.AddItem` içine breakpoint koy, isteği at, call stack'te Controller → Service yolunu izle

**Kendini test et:** `UseAuthentication` neden `UseAuthorization`'dan önce? Controller'a `DbContext` neden Singleton olarak verilmez? Bir isteğin `Program.cs`'ten DB'ye yolunu sesli anlat.

---

### Gün 3 — 7 Ekim: EF Core + SQL Server + T-SQL

**Kaynak:** Mert Özen 7, 8 + T-SQL pratiği (kendi verinle)

| Kavram | Neden önemli / şirkette | Spring/JPA karşılığı |
|--------|------------------------|---------------------|
| `DbContext` | Unit of Work + change tracking | `EntityManager`, Persistence Context |
| Change Tracker, `SaveChanges` | Ne zaman SQL'e gider? | Dirty checking + flush |
| Migrations | Şema değişikliği PR'larının parçası | Flyway / Liquibase |
| LINQ → SQL, `IQueryable` vs `IEnumerable` | Yanlış yerde `ToList()` = tüm tabloyu çekmek | JPQL / Criteria |
| **N+1 problemi**, `Include` | En yaygın performans bug'ı | Lazy loading, `JOIN FETCH` |
| `AsNoTracking` | Okuma sorgularında performans | read-only transaction |
| Transaction | Sipariş + stok düşümü atomik | `@Transactional` |
| Optimistic concurrency (`RowVersion`) | Son ürünü iki kişi aynı anda alırsa | `@Version` |
| Dapper / raw SQL | Büyük sistemlerde EF yanında sık kullanılır | `JdbcTemplate` |

**Kodla:**

```bash
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Your_strong_Pass123" \
  -p 1433:1433 --name minicart-sql -d mcr.microsoft.com/mssql/server:2022-latest
```

- [ ] `MiniCartDbContext`, entity configuration'ları (`IEntityTypeConfiguration<T>`), seed data
- [ ] Migration'ı CLI ile oluştur ve uygula (Bölüm 2.4): `dotnet ef migrations add InitialCreate ...`, `dotnet ef database update ...`; `migrations script` ile üretilen SQL'i oku
- [ ] VS Code **SQL Server (mssql)** eklentisiyle `localhost,1433` adresine bağlan, tabloları gör
- [ ] In-memory repository → EF repository (Application katmanı **hiç değişmemeli**)
- [ ] `Order` entity + `OrderService.CreateOrder`: tek transaction'da sipariş + stok düşümü
- [ ] `Product.RowVersion` ile concurrency; çakışmada 409 dön
- [ ] Sipariş listeleme: paging + filtering + sorting
- [ ] EF'in ürettiği SQL'i logla, N+1'i bilerek üret, `Include` ile düzelt

**T-SQL pratiği (VS Code mssql eklentisinde, `sql/` klasöründe `.sql` dosyaları olarak — repoda kalsın):**
- [ ] JOIN: siparişler + kalemler + ürünler
- [ ] GROUP BY / HAVING: ürün bazlı satış adedi, 3'ten fazla sipariş veren müşteriler
- [ ] CTE + `ROW_NUMBER()`: her müşterinin son siparişi
- [ ] `CASE WHEN`: sipariş tutarına göre segment
- [ ] Index: `Orders(CustomerId, CreatedAt)` ekle, mssql eklentisinin **execution plan** görünümünde önce/sonra farkına bak (scan vs seek)
- [ ] Stored procedure: `usp_GetCustomerOrderSummary @CustomerId` yaz, .NET'ten (EF `FromSql` veya Dapper) çağır
- [ ] `BEGIN TRAN / COMMIT / ROLLBACK` + `TRY/CATCH`

**Kendini test et:** `IQueryable` üzerinde `Where` ile `ToList()` sonrası `Where` arasındaki SQL farkı? Index neden yazma işlemlerini yavaşlatır? Optimistic ve pessimistic locking farkı?

---

### Gün 4 — 8 Ekim: Test günü (Unit + Integration + BDD)

**Kavram:** Bölüm 4'ü baştan oku. Test piramidi, AAA (Arrange-Act-Assert), mock vs stub vs fake, TDD döngüsü, BDD, Gherkin, Three Amigos.

**Unit test:**
- [ ] `dotnet add tests/MiniCart.UnitTests package NSubstitute` (veya Moq)
- [ ] Domain testleri: 7 iş kuralının her biri için en az bir test (`Cart.AddItem_WhenQuantityExceedsStock_Throws`)
- [ ] Servis testi: repository mock'lanarak `CartService`
- [ ] **Bir kuralı TDD ile yaz:** "Shipped sipariş iptal edilemez" — önce kırmızı test, sonra kod

**Integration test:**
- [ ] `Microsoft.AspNetCore.Mvc.Testing` + `Testcontainers.MsSql`
- [ ] `WebApplicationFactory<Program>` ile gerçek HTTP → gerçek SQL Server
- [ ] Senaryo: ürün ekle → sepete ekle → sipariş ver → stok düştü mü?
- [ ] Idempotency: aynı key ile iki POST → tek sipariş (`Idempotency-Key` header'ını şimdi uygula)

**BDD (Reqnroll):**
- [ ] `dotnet add tests/MiniCart.AcceptanceTests package Reqnroll.xUnit`
- [ ] Cucumber (Gherkin) eklentisi ile `.feature` dosyası yaz; testleri C# Dev Kit Test Explorer'dan veya `dotnet test` ile çalıştır
- [ ] Bölüm 4.3'teki feature dosyasını ekle, step definition'ları yaz
- [ ] Kendi senaryonu yaz: "Boş sepetten sipariş oluşturulamaz", "Aynı idempotency key ile ikinci istek"
- [ ] Önce Gherkin'i yaz (Three Amigos'u kendin oyna: iş birimi gözüyle kural, QA gözüyle uç durumlar)

**Kendini test et:** Hangi kural unit, hangisi integration, hangisi BDD ile test edilmeli? Mock'u neden her yerde kullanmamalıyız? Jira'daki bir Acceptance Criteria'yı Gherkin'e çevir.

---

### Gün 5 — 9 Ekim: Auth, Redis, Docker Compose

**Kaynak:** Mert Özen 15, 16, 19

| Kavram | Neden önemli / şirkette | Spring karşılığı |
|--------|------------------------|------------------|
| Authentication vs Authorization | "Kimsin?" vs "Yetkin var mı?" | Spring Security |
| JWT, claims, policy | Mikroservislerde token taşınır | `JwtAuthenticationFilter`, `@PreAuthorize` |
| Cache-aside, TTL, invalidation | Ürün/sepet okumaları yoğun | `@Cacheable` + Redis |
| `IDistributedCache` / StackExchange.Redis | Birden fazla instance aynı cache'i görsün | Spring Data Redis |
| Docker Compose | Yerel ortamı tek komutla kurmak | — |

**Kodla:**
- [ ] JWT üreten basit `/auth/token` (gerçek kimlik doğrulama yok, eğitim amaçlı) + `[Authorize]` sepet/sipariş endpoint'leri
- [ ] Policy: sadece `Admin` rolü ürün ekleyebilir
- [ ] Redis: ürün detayı cache-aside, ürün güncellenince cache invalidation
- [ ] **Sepeti Redis'te tutma** seçeneğini tartış (e-ticarette yaygın: sepet geçici, yüksek okuma/yazma)
- [ ] `Dockerfile` (multi-stage) + `docker-compose.yml`: api + sqlserver + redis
- [ ] Health check endpoint'i (`/health`) — SQL ve Redis kontrolü

```yaml
services:
  sqlserver:
    image: mcr.microsoft.com/mssql/server:2022-latest
    environment: { ACCEPT_EULA: "Y", MSSQL_SA_PASSWORD: "Your_strong_Pass123" }
    ports: ["1433:1433"]
  redis:
    image: redis:7
    ports: ["6379:6379"]
  api:
    build: ./src/MiniCart.Api   # Dockerfile yoluna göre düzenle
    depends_on: [sqlserver, redis]
    ports: ["8080:8080"]
```

**Kendini test et:** JWT neden server'da session tutmaz, dezavantajı ne? Cache invalidation neden zordur? Aynı ödeme isteği iki kez gelirse sistemin hangi katmanı korur?

---

### Gün 6 — 10 Ekim: Mikroservislere bölme + RabbitMQ + Outbox

**Kavramlar (önce sabah 2–3 saat okuma + çizim):**

| Kavram | Açıklama | Şirkette |
|--------|---------|----------|
| Monolith vs Microservices | Bağımsız deploy/ölçekleme vs dağıtık sistem karmaşıklığı | Büyük e-ticaret sistemleri genelde ikisinin karışımı |
| Service boundary / Bounded Context | Servis sınırı domain'e göre çizilir (DDD) | "Sepet ekibi", "Ödeme ekibi" |
| Database per service | Her servis kendi verisinin sahibi | Servisler arası JOIN yok |
| Senkron (HTTP/gRPC) vs asenkron (mesaj) | HTTP: anlık cevap ama bağımlılık; mesaj: gevşek bağlılık | Ödeme sonucu genelde asenkron |
| Event vs Command | Event: "oldu" (`OrderCreated`), Command: "yap" (`ReserveStock`) | — |
| Eventual consistency | Veri bir süre tutarsız olabilir, sonunda tutarlı olur | "Sipariş alındı, onay bekleniyor" |
| **Saga** (choreography / orchestration) | Dağıtık transaction yerine adım adım + telafi | Stok ayrıldı ama ödeme başarısız → stok iade |
| **Outbox pattern** | DB'ye yaz + mesaj gönder atomik olsun | En çok sorulan mülakat sorularından |
| Idempotent consumer | Mesaj iki kez gelirse iki kez işleme | At-least-once teslimatın sonucu |
| API Gateway | Tek giriş noktası, routing, auth | YARP, Ocelot, cloud gateway'ler |
| Resilience | Retry, timeout, circuit breaker | Polly / `Microsoft.Extensions.Http.Resilience` |

**RabbitMQ kavramları:** producer → **exchange** (direct / topic / fanout) → **binding** → **queue** → consumer, ack/nack, prefetch, **dead-letter queue**.
Spring karşılığı: Spring AMQP. .NET'te şirketlerde genelde **MassTransit** gibi bir soyutlama görülür (lisansını kontrol et); bu projede kavramı görmek için doğrudan **`RabbitMQ.Client`** kullanıyoruz.

**Hedef akış (choreography saga):**

```text
Ordering.Api ──OrderCreated──► Inventory.Api ──StockReserved──► Payment.Worker
     ▲                              │                                │
     │                    StockReservationFailed            PaymentSucceeded / PaymentFailed
     └──────────────────────────────┴────────────────────────────────┘
                     Ordering sipariş durumunu günceller
          PaymentFailed → Inventory stoğu iade eder (telafi adımı)
```

**Kodla:**
- [ ] `services/` altında: `Ordering.Api` (kendi DB'si), `Inventory.Api` (kendi DB'si), `Payment.Worker` (BackgroundService, ödemeyi rastgele başarılı/başarısız simüle eder)
- [ ] Her servis aynı katman fikrini taşır ama sade tut (tek proje içinde klasörler yeterli)
- [ ] Ortak mesaj sözleşmeleri: `Contracts` projesi (`OrderCreated`, `StockReserved` record'ları)
- [ ] RabbitMQ'yu compose'a ekle (`rabbitmq:management`), yönetim panelinden queue'ları izle
- [ ] **Outbox:** `Ordering` siparişi ve `OutboxMessages` satırını aynı transaction'da yazar; bir `BackgroundService` outbox'ı okuyup RabbitMQ'ya gönderir
- [ ] Idempotent consumer: işlenen mesaj id'lerini tut, tekrarını atla
- [ ] Dead-letter queue: 3 başarısız denemeden sonra mesaj DLQ'ya
- [ ] (Opsiyonel) YARP ile basit API Gateway

**Kendini test et:** Outbox olmasaydı hangi iki hata senaryosu oluşurdu? Ordering neden Inventory'nin DB'sine doğrudan bakmamalı? Choreography ile orchestration farkı ve ne zaman hangisi?

---

### Gün 7 — 11 Ekim: Kafka + kapanış + 12 Ekim hazırlığı

**Sabah — Kafka kavramları:**

```text
Producer ──► Topic "order-events"
               ├─ Partition 0: [e1][e4][e7] ...  ← offset 0,1,2
               ├─ Partition 1: [e2][e5] ...
               └─ Partition 2: [e3][e6] ...
                        │
        ┌───────────────┴────────────────┐
  Consumer Group "notification"    Consumer Group "analytics"
  (her partition grupta tek         (aynı event'leri bağımsız
   consumer'a atanır)                okur, kendi offset'i var)
```

| Kavram | Açıklama |
|--------|---------|
| Broker / cluster | Kafka sunucuları |
| Topic | Event'lerin mantıksal kategorisi |
| **Partition** | Topic'in sıralı parçası; paralellik birimi. **Sıra yalnızca partition içinde garantilidir** |
| **Key** | Aynı key (ör. `orderId`) her zaman aynı partition'a → bir siparişin event'leri sıralı |
| **Offset** | Consumer'ın partition'da nerede kaldığı |
| **Consumer group** | Gruptaki consumer'lar partition'ları paylaşır; farklı gruplar aynı veriyi bağımsız okur |
| Retention | Mesajlar okunduktan sonra silinmez, süre/boyuta göre tutulur → **replay** mümkün |
| Delivery semantics | at-most-once / at-least-once / exactly-once (pratikte at-least-once + idempotent consumer) |

**RabbitMQ vs Kafka:**

| | RabbitMQ | Kafka |
|---|---|---|
| Model | Mesaj kuyruğu (broker mesajı consumer'a iter, ack sonrası siler) | Dağıtık log (consumer çeker, mesaj kalır) |
| Güçlü olduğu yer | Görev dağıtımı, routing, command'lar | Yüksek hacimli event stream, birden fazla bağımsız okuyucu, replay |
| E-ticaret örneği | "Bu siparişin faturasını kes" | Tüm sipariş/tıklama event'lerinin analitik, öneri, bildirim sistemlerine akışı |

**Kodla (`Confluent.Kafka`):**
- [ ] Kafka'yı compose'a ekle (tek node, KRaft modunda — ör. `apache/kafka` image'ı)
- [ ] `Ordering`, sipariş durum değişikliklerini `order-events` topic'ine `orderId` key'i ile yayınlar
- [ ] İki farklı consumer group: `Notification.Worker` (log'a "e-posta gönderildi" yazar) ve `Analytics.Worker` (günlük sipariş sayısını tutar)
- [ ] Bir consumer'ı durdur, event üret, tekrar başlat → offset'ten devam ettiğini gör
- [ ] Aynı gruba ikinci consumer ekle → partition'ların paylaşıldığını gör

**Öğleden sonra — kapanış ve hazırlık:**
- [ ] README: mimari diyagram, nasıl çalıştırılır, hangi kavramlar uygulandı (CV/mülakat için)
- [ ] **Kod okuma pratiği:** olgun bir açık kaynak .NET projesinde (ör. Microsoft'un eShop referans uygulaması) bir endpoint'in yolunu Controller → DB/mesaj kadar takip et, not al
- [ ] Bölüm 1'deki sözlüğü gözden geçir
- [ ] Bölüm 7'deki ilk hafta soru listesini hazırla
- [ ] Erken yat 🙂

**Kendini test et:** Partition sayısı neden consumer sayısının üst sınırıdır? Sipariş event'lerinin sırası neden önemli ve nasıl garanti edilir? Bu sistemde RabbitMQ'yu ne için, Kafka'yı ne için seçerdin?

---

## 7. Staj ilk hafta rehberi

**İlk günlerde sor (not alarak):**
- Projeyi yerelde nasıl ayağa kaldırırım? Hangi DB/servislere erişimim var?
- Branch adlandırma ve commit mesajı standardı nedir? PR'ları kim review eder, kaç onay gerekir?
- Log'lara ve hata takibine nereden bakarım?
- Task'lar nerede (Jira / Azure Boards)? "Done" tanımı nedir?
- Test yazma beklentisi nedir? CI'da hangi kontroller çalışır?
- Bana ilk aşinalık için okumam gereken bir servis/akış var mı?

**Çalışma alışkanlıkları:**
- **Soru sormadan önce 15–30 dakika kendin dene**, sonra şu formatla sor: "X'i yapmaya çalışıyorum, şunu denedim, şu sonucu aldım, şurada takıldım."
- Her gün kısa not: ne öğrendim, neyi anlamadım, yarın ne yapacağım. Daily'de bu notlar işine yarar.
- İlk PR'ını küçük tut. Review yorumlarını kişisel algılama; en hızlı öğrenme kanalı bu.
- Production'a dokunan her şeyde (özellikle ödeme) **emin değilsen sor**.
- Hedefi erken konuş: "Burada kalıcı olarak devam etmek istiyorum, bunun için neyi göstermem gerekir?"

**Agile/Scrum sözlüğü:** sprint, backlog, user story, acceptance criteria, story point, daily stand-up, sprint planning, review, retrospective, definition of done.

---

## 8. GitHub Issue listesi

| # | Gün | Issue |
|---|-----|-------|
| 1 | 1 | chore: solution iskeleti, gitignore, README, PR şablonu |
| 2 | 2 | feat: Product ve Cart domain modeli + iş kuralları |
| 3 | 2 | feat: Cart API (in-memory repository) |
| 4 | 2 | feat: global exception handling + ProblemDetails |
| 5 | 2 | feat: correlation id + request logging middleware |
| 6 | 3 | feat: EF Core + SQL Server + migration + seed |
| 7 | 3 | feat: sipariş oluşturma (transaction + stok düşümü + RowVersion) |
| 8 | 3 | feat: sipariş listeleme (paging/filtering/sorting) + T-SQL stored procedure |
| 9 | 4 | test: domain ve servis unit testleri |
| 10 | 4 | test: integration testleri (WebApplicationFactory + Testcontainers) + idempotency |
| 11 | 4 | test: BDD acceptance testleri (Reqnroll) |
| 12 | 5 | feat: JWT authentication + authorization policy |
| 13 | 5 | feat: Redis cache + Docker Compose + health checks |
| 14 | 6 | feat: Ordering / Inventory / Payment servislerine bölme |
| 15 | 6 | feat: RabbitMQ saga + outbox + idempotent consumer + DLQ |
| 16 | 7 | feat: Kafka order-events + iki consumer group |
| 17 | 7 | docs: README, mimari diyagram, öğrenilenler |

---

## 9. Staj sırasında devam (Faz 2, akşamları)

7 günde temas edilen ama derinleşilmeyen konular. Stajda gördüklerine göre sırala:

- EF Core ileri seviye (compiled query, split query, bulk işlemler), T-SQL performans tuning
- Resilience (Polly), API Gateway, servisler arası auth
- OpenTelemetry ile distributed tracing, metrikler
- MassTransit gibi mesajlaşma soyutlamaları, orchestration saga
- Kubernetes temelleri, CI/CD pipeline yazma
- Spring Boot tarafında aynı MiniCart'ı yazmak (iki ekosistemi karşılaştırmak için güçlü bir portfolyo)
- Uzun vade: Python + LLM/RAG'ı backend servislerine entegre etmek
