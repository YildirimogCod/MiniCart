# CLAUDE.md — MiniCart

## Kullanıcı hakkında
- Bilgisayar mühendisliği 4. sınıf öğrencisi. 12 Ekim 2026'da LC Waikiki'de (sepet/ödeme birimi, .NET) trainee olarak başlıyor.
- Java/Spring Boot temeli güçlü; C#/.NET temellerini çalıştı, ASP.NET Core Web API öğreniyor.
- Hedef: Ocak 2027'de Junior Backend Developer olarak devam etmek.
- Dil: Türkçe konuş. Kod, commit mesajları ve isimlendirme İngilizce.

## Proje
- MiniCart: Cart + Order e-ticaret API'si. Plan ve günlük program için `ROADMAP.md` dosyasını oku ve takip et.
- .NET 10, ASP.NET Core (controller tabanlı), EF Core + SQL Server, Redis, RabbitMQ, Kafka, Docker Compose.
- Mimari: Clean Architecture (Domain ← Application ← Infrastructure, Api). Domain hiçbir projeye bağımlı değil.
- Test: xUnit + NSubstitute (unit), WebApplicationFactory + Testcontainers (integration), Reqnroll (BDD).
- AutoMapper, MediatR, FluentAssertions, MassTransit kullanma (lisans + öğrenme amacı). Elle mapping, xUnit Assert/Shouldly.

## Geliştirme ortamı
- Editör: VS Code + C# Dev Kit. IDE'ye özgü (Visual Studio/Rider) adımlar önerme; her işi **dotnet CLI** ile yap veya göster.
- HTTP istekleri: REST Client eklentisi, `requests.http` dosyası (Postman yok).
- SQL: VS Code SQL Server (mssql) eklentisi; T-SQL çalışmaları `sql/` klasöründe `.sql` dosyaları.
- BDD: `.feature` dosyaları için Cucumber eklentisi; testler Test Explorer veya `dotnet test`.
- Yeni proje/paket eklerken `dotnet new` / `dotnet add package` / `dotnet sln add` komutlarını kullan ve komutu kullanıcıya açıkla.

## Komutlar
- Build: `dotnet build`
- Test: `dotnet test` (filtre: `dotnet test --filter "FullyQualifiedName~Cart"`)
- Çalıştır: `dotnet run --project src/MiniCart.Api` (geliştirirken `dotnet watch --project src/MiniCart.Api`)
- Migration: `dotnet ef migrations add <Ad> -p src/MiniCart.Infrastructure -s src/MiniCart.Api`
- DB güncelle: `dotnet ef database update -p src/MiniCart.Infrastructure -s src/MiniCart.Api`
- Altyapı: `docker compose up -d` / `docker compose down`

## Çalışma kuralları (önemli)
1. Her konuda önce kavramı açıkla: nedir, neden gerekli, LC Waikiki gibi bir e-ticaret sisteminde nerede karşıma çıkar, Java/Spring karşılığı nedir. Sonra kodla.
2. Kullanıcıyı sıfırdan başlayan biri gibi ele alma; bildiği backend kavramlarını tekrar anlatma, .NET karşılığını göster.
3. Git işlemlerini (branch, commit, push, PR) kullanıcı kendisi yapar. Sen yapma; gerekirse komutu öner.
4. Çekirdek mantığı (domain kuralları, servis mantığı, SQL sorguları) kullanıcı yazar. Sen yaklaşımı tartış, ipucu ver, review yap.
5. Boilerplate, DTO, test iskeleti, Docker/compose dosyaları, Gherkin taslakları için doğrudan yardım edebilirsin.
6. Review isterken: hataları ve iyileştirmeleri listele, kullanıcı açıkça istemedikçe düzeltmeyi kendin yapma.
7. Yaptığın her önemli değişikliği "neden böyle" sorusuyla birlikte açıkla.
8. Gün sonunda ROADMAP.md'deki "Kendini test et" sorularını sor.
