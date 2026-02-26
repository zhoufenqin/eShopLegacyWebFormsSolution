# eShopLegacyWebForms - Architecture Diagram

```mermaid
flowchart TD
    subgraph Client["Client Browser"]
        Browser["Web Browser\n(HTML / Bootstrap 4 / jQuery)"]
    end

    subgraph Presentation["Presentation Layer\nASP.NET WebForms (.NET 4.7.2)"]
        Pages["ASPX Pages\n(Default, Catalog: Create/Edit/Delete/Details)"]
        Masters["Master Pages\n(Site.Master, Site.Mobile.Master)"]
        ViewSwitcher["ViewSwitcher\n(Desktop / Mobile)"]
    end

    subgraph BusinessLogic["Business Logic Layer"]
        ICatalogService["ICatalogService\n(interface)"]
        CatalogService["CatalogService\n(EF-backed)"]
        CatalogServiceMock["CatalogServiceMock\n(in-memory, for dev/test)"]
        AppModule["ApplicationModule\n(Autofac DI)"]
    end

    subgraph DataAccess["Data Access Layer"]
        DBContext["CatalogDBContext\n(Entity Framework 6 Code First)"]
        HiLoGen["CatalogItemHiLoGenerator\n(HiLo ID strategy)"]
        DBInit["CatalogDBInitializer\n(seed data)"]
    end

    subgraph DataStorage["Data Storage"]
        SQLServer["SQL Server\n(LocalDB / MSSQLLocalDB)"]
        InMemory["In-Memory Mock Data\n(CSV seed files)"]
    end

    subgraph CrossCutting["Cross-Cutting Concerns"]
        AppInsights["Application Insights\n(monitoring / telemetry)"]
        Log4Net["log4net\n(logging)"]
        Session["InProc Session State\n(ASP.NET Session)"]
        Autofac["Autofac\n(IoC container)"]
    end

    Browser -->|"HTTP requests"| Pages
    Pages --> Masters
    Pages --> ViewSwitcher
    Pages -->|"calls"| ICatalogService
    AppModule -->|"registers"| ICatalogService
    ICatalogService --> CatalogService
    ICatalogService --> CatalogServiceMock
    CatalogService --> DBContext
    CatalogService --> HiLoGen
    DBContext --> DBInit
    DBContext -->|"reads/writes"| SQLServer
    CatalogServiceMock -->|"reads"| InMemory
    Pages -.->|"telemetry"| AppInsights
    Pages -.->|"logs"| Log4Net
    Pages -.->|"uses"| Session
    Autofac -.->|"injects"| Pages
```

## Architecture Summary

| Layer | Technology |
|---|---|
| **Framework** | ASP.NET WebForms on .NET Framework 4.7.2 |
| **UI** | ASPX pages, Master Pages, Bootstrap 4, jQuery 3.3 |
| **Dependency Injection** | Autofac 4.9 (Web integration) |
| **Business Logic** | CatalogService / CatalogServiceMock (ICatalogService) |
| **Data Access** | Entity Framework 6.2 (Code First, DbContext) |
| **Database** | SQL Server (LocalDB for development) |
| **Session State** | In-Process (InProc) ASP.NET session |
| **Monitoring** | Microsoft Application Insights 2.9 |
| **Logging** | log4net 2.0 |

## Assessment Summary (AppCAT)

| Metric | Value |
|---|---|
| Total Issues | 7 |
| Total Incidents | 8 |
| Total Effort (story points) | 24 |
| Mandatory Issues | 2 |
| Optional Issues | 4 |
| Potential Issues | 2 |

**Issue Categories**: Scale, Connection, Database, Identity, Security, Local
