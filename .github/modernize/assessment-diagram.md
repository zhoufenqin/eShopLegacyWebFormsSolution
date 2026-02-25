# eShopLegacyWebForms - Architecture Diagram

Generated from AppCAT assessment results.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Browser"]
        Browser["Web Browser\n(HTML/CSS/JS)"]
    end

    subgraph Presentation["Presentation Layer\n(ASP.NET Web Forms - .NET 4.7.2)"]
        Pages["ASPX Pages\n(Default, Catalog, About, Contact)"]
        Masters["Master Pages\n(Site.Master, Site.Mobile.Master)"]
        Modules["HTTP Modules\n(Autofac, AppInsights, Session)"]
    end

    subgraph Business["Business Logic Layer"]
        CatalogSvc["CatalogService\n(ICatalogService)"]
        MockSvc["CatalogServiceMock\n(UseMockData mode)"]
        ViewModel["ViewModels\n(PaginatedItemsViewModel)"]
        DI["Dependency Injection\n(Autofac IoC Container)"]
    end

    subgraph DataAccess["Data Access Layer"]
        EF["Entity Framework 6\n(CatalogDBContext)"]
        HiLo["HiLo ID Generator\n(CatalogItemHiLoGenerator)"]
    end

    subgraph DataStore["Data Storage"]
        SQL[("SQL Server\n(LocalDB / MSSQLLocalDB)\nCatalogDb")]
        StaticFiles["Static Files\n(Pics, Images, CSS, JS)"]
        SessionStore["In-Process Session\n(InProc SessionState)"]
    end

    subgraph Monitoring["Observability"]
        AppInsights["Azure Application Insights\n(Telemetry and Monitoring)"]
        Log4Net["log4net\n(Logging)"]
    end

    Browser -->|HTTP Requests| Pages
    Pages --> Masters
    Pages --> Modules
    Pages --> CatalogSvc
    Pages --> MockSvc
    DI -->|Injects| CatalogSvc
    DI -->|Injects| MockSvc
    CatalogSvc --> ViewModel
    CatalogSvc --> EF
    EF --> HiLo
    EF -->|SQL Queries| SQL
    Pages -->|Serves| StaticFiles
    Modules -->|Manages| SessionStore
    Pages -.->|Telemetry| AppInsights
    Pages -.->|Log Events| Log4Net
```

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Framework | ASP.NET Web Forms (.NET 4.7.2) |
| ORM | Entity Framework 6.2 |
| IoC Container | Autofac 4.9.1 |
| Database | SQL Server (LocalDB) |
| Front-end | Bootstrap 4.3.1, jQuery 3.5.0 |
| Monitoring | Azure Application Insights 2.9.1 |
| Logging | log4net 2.0.10 |
| Session | ASP.NET In-Process Session |
| Bundling | ASP.NET Web Optimization 1.1.3 |

## Assessment Issues Summary

| Category | Count | Severity |
|----------|-------|----------|
| Scale | 2 | Mandatory / Optional |
| Security | 2 | Mandatory / Optional |
| Connection | 1 | Optional |
| Database | 1 | Optional |
| Identity | 1 | Optional |
| Local | 1 | Potential |

**Total**: 7 issues · 8 incidents · 24 story points

> **Note**: Assessed with [.NET AppCAT](https://aka.ms/appcat) for migration targets:
> Azure App Service · Azure Kubernetes Service · Azure Container Apps · Azure App Service Container · Azure App Service Managed Instance
