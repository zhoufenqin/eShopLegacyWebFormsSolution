# eShopLegacyWebForms Architecture Diagram

```mermaid
flowchart TD
    subgraph Client["Client Browser"]
        Browser["Web Browser"]
    end

    subgraph Presentation["Presentation Layer - ASP.NET Web Forms (.NET 4.7.2)"]
        Pages["ASPX Pages\n(Default, About, Contact,\nCatalog CRUD)"]
        Masters["Master Pages\n(Site.Master, Site.Mobile.Master)"]
        Controls["User Controls\n(ViewSwitcher)"]
        Bundles["Script and Style Bundles\n(Bootstrap 4, jQuery 3)"]
    end

    subgraph BusinessLogic["Business Logic Layer"]
        CatalogSvc["CatalogService\n(ICatalogService)"]
        MockSvc["CatalogServiceMock\n(UseMockData mode)"]
        ViewModel["PaginatedItemsViewModel"]
        DI["Dependency Injection\n(Autofac)"]
    end

    subgraph DataAccess["Data Access Layer"]
        DBContext["CatalogDBContext\n(Entity Framework 6)"]
        Models["Domain Models\n(CatalogItem, CatalogBrand,\nCatalogType)"]
        DBInit["CatalogDBInitializer\n(Seed data)"]
    end

    subgraph Storage["Data Storage"]
        SQL["SQL Server\n(LocalDB / MSSQLLocalDB)\nCatalogDb"]
        Images["File System\n(Product Images)"]
    end

    subgraph Monitoring["Monitoring"]
        AppInsights["Application Insights\n(Telemetry)"]
        Log4Net["log4net\n(Logging)"]
    end

    Browser -->|"HTTP/HTTPS requests"| Pages
    Pages --> Masters
    Pages --> Controls
    Pages --> Bundles
    Pages -->|"calls"| CatalogSvc
    Pages -->|"calls"| MockSvc
    DI -->|"injects"| CatalogSvc
    DI -->|"injects"| MockSvc
    CatalogSvc -->|"queries"| DBContext
    CatalogSvc -->|"uses"| ViewModel
    DBContext -->|"maps"| Models
    DBContext -->|"connects"| SQL
    DBInit -->|"seeds"| SQL
    CatalogSvc -->|"reads"| Images
    Pages -->|"telemetry"| AppInsights
    CatalogSvc -->|"logs"| Log4Net
```

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Framework | ASP.NET Web Forms on .NET 4.7.2 |
| Presentation | ASPX Pages, Bootstrap 4.3, jQuery 3.3 |
| Dependency Injection | Autofac 4.9 |
| Data Access | Entity Framework 6.2 (Code-First) |
| Database | SQL Server (LocalDB) |
| Logging | log4net 2.0 |
| Monitoring | Microsoft Application Insights |

## Assessment Summary

- **Issues Found**: 7
- **Incidents**: 8
- **Story Points**: 24
- **Mandatory Issues**: 2
- **Optional Issues**: 4
- **Potential Issues**: 2
