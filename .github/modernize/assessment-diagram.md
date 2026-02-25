# eShopLegacyWebForms Architecture Diagram

```mermaid
flowchart TD
    subgraph Client["Client Browser"]
        Browser["Web Browser"]
    end

    subgraph Presentation["Presentation Layer (ASP.NET Web Forms / .NET 4.7.2)"]
        MasterPage["Site.Master / Site.Mobile.Master\n(Master Pages)"]
        Pages["Web Pages\n(Default, About, Contact, Catalog/*)"]
        UserControls["User Controls\n(ViewSwitcher.ascx)"]
        StaticAssets["Static Assets\n(Bootstrap, jQuery, Modernizr, Images)"]
    end

    subgraph AppInfra["Application Infrastructure"]
        Autofac["Autofac IoC Container\n(Dependency Injection)"]
        AppInsights["Application Insights\n(Telemetry and Monitoring)"]
        Log4Net["log4net\n(File-based Logging)"]
        BundleOptim["ASP.NET Bundling and Minification"]
    end

    subgraph BusinessLogic["Business / Service Layer"]
        ICatalogService["ICatalogService\n(Interface)"]
        CatalogService["CatalogService\n(EF-backed implementation)"]
        CatalogServiceMock["CatalogServiceMock\n(Mock implementation)"]
    end

    subgraph DataAccess["Data Access Layer"]
        EF6["Entity Framework 6\n(ORM)"]
        DBContext["CatalogDBContext\n(DbContext)"]
        Models["Domain Models\n(CatalogItem, CatalogBrand, CatalogType)"]
        HiLoGen["CatalogItemHiLoGenerator\n(ID Generation)"]
    end

    subgraph DataStorage["Data Storage"]
        SQLServer["SQL Server\n(CatalogDBContext)"]
    end

    Browser -->|HTTP requests| Presentation
    Pages --> Autofac
    Autofac -->|resolves| ICatalogService
    ICatalogService --> CatalogService
    ICatalogService --> CatalogServiceMock
    CatalogService --> EF6
    EF6 --> DBContext
    DBContext --> Models
    DBContext --> HiLoGen
    DBContext -->|System.Data.SqlClient| SQLServer
    Pages --> AppInsights
    Pages --> Log4Net
    MasterPage --> BundleOptim
    BundleOptim --> StaticAssets
```

## Architecture Summary

**Application**: eShopLegacyWebForms  
**Framework**: ASP.NET Web Forms on .NET Framework 4.7.2  
**Language**: C#

### Layers

| Layer | Technologies |
|-------|-------------|
| Presentation | ASP.NET Web Forms, Bootstrap 4, jQuery, Modernizr |
| DI / Infrastructure | Autofac 4.9, Application Insights, log4net 2.0 |
| Business / Service | ICatalogService interface with EF-backed and mock implementations |
| Data Access | Entity Framework 6, System.Data.SqlClient |
| Data Storage | SQL Server (LocalDB in development) |

### Key Assessment Findings

| Issue | Severity | Description |
|-------|----------|-------------|
| Static content | Optional | 83 static files served directly; consider Azure Blob Storage and Azure CDN |
| File-based logging | Mandatory | log4net writes to local disk; not compatible with Azure App Service or Containers |
| Connection strings | Potential | Hard-coded SQL Server connection strings; migrate database and use Azure Key Vault |
| System.Data.SqlClient | Optional | Upgrade to Microsoft.Data.SqlClient for better Azure integration |
| Windows Authentication | Mandatory | Incompatible with Azure App Service and AKS; use Azure AD authentication |
| In-proc session state | Potential | Prevents horizontal scaling; use Azure Cache for Redis or Cosmos DB |
| Secrets in config | Optional | Connection strings and app settings in Web.config; use Azure App Configuration |
