# eShopLegacyWebForms - Architecture Diagram

```mermaid
flowchart TD
    subgraph Client["Client Browser"]
        Browser["Web Browser\n(Bootstrap, jQuery, MS Ajax)"]
    end

    subgraph Presentation["Presentation Layer - ASP.NET Web Forms"]
        Default["Default.aspx\n(Catalog Listing)"]
        Create["Catalog/Create.aspx"]
        Edit["Catalog/Edit.aspx"]
        Delete["Catalog/Delete.aspx"]
        Details["Catalog/Details.aspx"]
        About["About.aspx"]
        Contact["Contact.aspx"]
    end

    subgraph AppInfra["Application Infrastructure"]
        Global["Global.asax\n(App Startup)"]
        Autofac["Autofac DI Container"]
        BundleRoute["Bundle and Route Config"]
    end

    subgraph BusinessLogic["Business Logic Layer - Services"]
        ICatalog["ICatalogService"]
        CatalogSvc["CatalogService\n(EF-backed)"]
        CatalogMock["CatalogServiceMock\n(In-memory)"]
    end

    subgraph DataAccess["Data Access Layer - Entity Framework 6"]
        DBContext["CatalogDBContext\n(DbContext)"]
        HiLo["CatalogItemHiLoGenerator"]
        DBInit["CatalogDBInitializer"]
    end

    subgraph Models["Domain Models"]
        CatalogItem["CatalogItem"]
        CatalogBrand["CatalogBrand"]
        CatalogType["CatalogType"]
    end

    subgraph DataStorage["Data Storage"]
        SQLServer["SQL Server\n(LocalDB / MSSQLLocalDB)"]
        MockData["In-Memory Mock Data\n(PreconfiguredData)"]
        Pics["Local File System\n(Product Images)"]
    end

    subgraph Observability["Observability"]
        AppInsights["Application Insights"]
        Log4Net["log4net"]
    end

    Browser -->|HTTP requests| Presentation
    Presentation --> Global
    Global --> Autofac
    Global --> BundleRoute
    Autofac -->|injects| ICatalog
    ICatalog --> CatalogSvc
    ICatalog --> CatalogMock
    CatalogSvc --> DBContext
    DBContext --> Models
    DBContext --> HiLo
    DBContext --> DBInit
    DBContext -->|EF6 Code-First| SQLServer
    CatalogMock --> MockData
    Presentation -->|reads| Pics
    Presentation --> AppInsights
    Global --> Log4Net
```

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Framework | ASP.NET Web Forms (.NET Framework 4.x) |
| UI | ASPX pages, Bootstrap 4, jQuery 3, MS Ajax |
| Dependency Injection | Autofac 4.9 |
| ORM / Data Access | Entity Framework 6 |
| Database | SQL Server (LocalDB) |
| Logging | log4net 2.0 |
| Monitoring | Application Insights 2.9 |
| Image Storage | Local file system (Pics/) |

## Key Components

- **Catalog Management**: Full CRUD (Create, Read, Update, Delete) for catalog items
- **Mock Data Support**: Switchable between live SQL Server and in-memory mock data via `UseMockData` app setting
- **HiLo ID Generation**: Custom ID generator for catalog items to avoid DB round-trips
- **Session Tracking**: Machine name and session start time stored in session state
