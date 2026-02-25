# eShopLegacyWebForms Architecture Diagram

```mermaid
flowchart TD
    subgraph Client["Client Browser"]
        Browser["Web Browser\n(HTML/CSS/JS)"]
    end

    subgraph Presentation["Presentation Layer\n(ASP.NET WebForms)"]
        Pages["ASPX Pages\n(Default, Catalog, About, Contact)"]
        Masters["Master Pages\n(Site.Master, Site.Mobile.Master)"]
        Scripts["Client Scripts\njQuery 3.3 / Bootstrap 4 / Modernizr"]
    end

    subgraph Business["Business Logic Layer"]
        CatalogService["CatalogService\n(ICatalogService)"]
        MockService["CatalogServiceMock\n(Mock/Test Data)"]
        Autofac["Autofac IoC Container\n(Dependency Injection)"]
    end

    subgraph DataAccess["Data Access Layer"]
        EF["Entity Framework 6\n(CatalogDBContext)"]
        Models["Domain Models\n(CatalogItem, CatalogBrand, CatalogType)"]
    end

    subgraph Storage["Data Storage"]
        SQLDB["SQL Server\n(CatalogDB)"]
        LocalDB["LocalDB\n(Development)"]
    end

    subgraph CrossCutting["Cross-Cutting Concerns"]
        AppInsights["Application Insights\n(Telemetry/Monitoring)"]
        Log4Net["log4net\n(File-based Logging)"]
        Session["In-Process Session State\n(ASP.NET Session)"]
        Bundle["Bundle and Minification\n(Web Optimization)"]
    end

    Browser -->|HTTP requests| Pages
    Pages --> Masters
    Pages --> Scripts
    Pages --> CatalogService
    Pages --> Session
    Autofac -->|injects| CatalogService
    Autofac -->|injects| MockService
    CatalogService --> EF
    MockService -.->|mock mode| Pages
    EF --> Models
    EF --> SQLDB
    EF -.->|dev environment| LocalDB
    Pages --> AppInsights
    Pages --> Log4Net
    Pages --> Bundle
```

## Architecture Summary

**Application**: eShopLegacyWebForms  
**Type**: ASP.NET WebForms  
**Framework**: .NET Framework 4.7.2  

### Key Components

| Layer | Technology |
|-------|-----------|
| Presentation | ASP.NET WebForms (ASPX pages, Master pages) |
| UI Framework | Bootstrap 4.3, jQuery 3.3, Modernizr |
| Dependency Injection | Autofac 4.9 |
| Data Access | Entity Framework 6.2 |
| Database | SQL Server / LocalDB (CatalogDB) |
| Logging | log4net 2.0 (file appender) |
| Monitoring | Microsoft Application Insights 2.9 |
| Session | ASP.NET In-Process Session State |

### Assessment Issues Summary

| Category | Count | Severity |
|----------|-------|----------|
| Scale | 2 | Optional / Potential |
| Connection | 1 | Potential |
| Database | 1 | Optional |
| Identity | 1 | **Mandatory** |
| Security | 2 | Optional |
| Local | 1 | **Mandatory** |

**Total**: 7 issues, 8 incidents, 24 story points
