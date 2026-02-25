# eShopLegacyWebForms - Architecture Diagram

```mermaid
flowchart TD
    subgraph Client["Client Browser"]
        UI["Bootstrap + jQuery\nResponsive UI"]
    end

    subgraph Presentation["Presentation Layer\nASP.NET Web Forms (.NET 4.7.2)"]
        MasterPage["Site.Master / Site.Mobile.Master\nMaster Pages"]
        Pages["Web Pages\nDefault.aspx, About.aspx, Contact.aspx"]
        CatalogPages["Catalog Pages\nCreate / Edit / Delete / Details"]
        ViewModel["ViewModels\nPaginatedItemsViewModel"]
    end

    subgraph Business["Business Logic Layer"]
        ICatalog["ICatalogService\nInterface"]
        CatalogSvc["CatalogService\nReal Implementation"]
        CatalogMock["CatalogServiceMock\nMock for Development"]
    end

    subgraph DI["Dependency Injection"]
        Autofac["Autofac IoC Container\nAutofac.Web Integration"]
    end

    subgraph DataAccess["Data Access Layer"]
        EF["Entity Framework 6\nCode First"]
        DBContext["CatalogDBContext\nDbContext"]
        Models["Domain Models\nCatalogItem, CatalogBrand, CatalogType"]
        DBInit["CatalogDBInitializer\nDB Seed Data"]
    end

    subgraph Storage["Data Storage"]
        SQL["SQL Server\nLocalDB / MSSQLLocalDB"]
    end

    subgraph Observability["Observability"]
        AppInsights["Application Insights\nTelemetry and Monitoring"]
        Log4Net["log4net\nFile Logging"]
    end

    subgraph Session["Session Management"]
        InProc["In-Process Session State\nIIS Session Module"]
    end

    UI -->|HTTP Requests| MasterPage
    MasterPage --> Pages
    MasterPage --> CatalogPages
    Pages --> ViewModel
    CatalogPages --> ViewModel

    Pages -->|Calls| ICatalog
    CatalogPages -->|Calls| ICatalog

    Autofac -->|Injects| ICatalog
    ICatalog --> CatalogSvc
    ICatalog --> CatalogMock

    CatalogSvc -->|Queries| DBContext
    DBContext --> Models
    DBContext --> DBInit
    DBContext -->|EF6 ORM| SQL

    Pages -->|Session| InProc
    Pages -->|Logs| Log4Net
    Pages -->|Telemetry| AppInsights
    CatalogSvc -->|Telemetry| AppInsights
```

## Summary

**Application**: eShopLegacyWebForms  
**Type**: ASP.NET Web Forms (legacy)  
**Framework**: .NET Framework 4.7.2  

### Architecture Layers

| Layer | Technology |
|---|---|
| Presentation | ASP.NET Web Forms, Bootstrap 4, jQuery 3 |
| Business Logic | Service pattern with interface abstraction |
| Dependency Injection | Autofac 4.9 |
| Data Access | Entity Framework 6 (Code First) |
| Data Storage | SQL Server (LocalDB in dev) |
| Logging | log4net 2.0 |
| Monitoring | Application Insights 2.9 |
| Session | In-Process Session State |

### Key Assessment Findings

| Category | Issues |
|---|---|
| Scale | 2 issues - static files and in-process session not suitable for scale-out |
| Security | 2 issues - configuration contains sensitive data |
| Database | 1 issue - SQL Server connection string references LocalDB |
| Connection | 1 issue - connection string in config |
| Identity | 1 issue - Windows Integrated Security |
| Local | 1 issue - log4net file-based logging |
