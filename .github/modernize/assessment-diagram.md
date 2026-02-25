# eShopLegacyWebForms Architecture Diagram

```mermaid
flowchart TD
    subgraph Client["Client Browser"]
        Browser["Web Browser\nBootstrap 4, jQuery, Modernizr"]
    end

    subgraph Presentation["Presentation Layer - ASP.NET Web Forms (.NET 4.7.2)"]
        Pages["Web Forms Pages\nDefault.aspx, Catalog, About, Contact"]
        Master["Master Pages\nSite.Master, Site.Mobile.Master"]
        Modules["HTTP Modules"]
    end

    subgraph Business["Business Logic Layer"]
        ICatalog["ICatalogService"]
        CatalogSvc["CatalogService"]
        CatalogMock["CatalogServiceMock"]
        DI["Autofac IoC Container"]
    end

    subgraph DataAccess["Data Access Layer"]
        EF["Entity Framework 6"]
        DBContext["CatalogDBContext"]
        Models["Domain Models\nCatalogItem, CatalogBrand, CatalogType"]
    end

    subgraph Storage["Data Storage"]
        SQL[("SQL Server\nLocalDB")]
    end

    subgraph CrossCutting["Cross-Cutting Concerns"]
        Log4Net["log4net\nLogging"]
        AppInsights["Application Insights\nMonitoring and Telemetry"]
    end

    Browser -- "HTTP Requests" --> Presentation
    Presentation --> Business
    DI -- "Injects" --> ICatalog
    ICatalog --> CatalogSvc
    ICatalog --> CatalogMock
    Business --> DataAccess
    EF --> DBContext
    DBContext --> Models
    DBContext -- "SQL Queries" --> Storage
    Business --> CrossCutting
    Presentation --> CrossCutting
```

## Technology Stack

| Layer | Technology |
|-------|------------|
| Framework | ASP.NET Web Forms on .NET Framework 4.7.2 |
| UI | Bootstrap 4, jQuery 3.5, Modernizr |
| IoC | Autofac 4.9 |
| ORM | Entity Framework 6.2 |
| Database | SQL Server (LocalDB) |
| Logging | log4net 2.0 |
| Monitoring | Application Insights 2.9 |
| Serialization | Newtonsoft.Json 12.0 |
