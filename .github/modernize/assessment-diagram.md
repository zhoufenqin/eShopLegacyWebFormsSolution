# eShopLegacyWebForms - Architecture Diagram

## Application Overview

**eShopLegacyWebForms** is a legacy ASP.NET WebForms e-commerce catalog application targeting .NET Framework 4.7.2. It provides CRUD operations for a product catalog backed by SQL Server via Entity Framework 6.

## Architecture Diagram

```mermaid
flowchart TD
    subgraph Client["Client Browser"]
        B[Web Browser]
    end

    subgraph Presentation["Presentation Layer - ASP.NET WebForms (.NET 4.7.2)"]
        P1[Default.aspx\nProduct Listing]
        P2[Catalog/Create.aspx\nCreate Product]
        P3[Catalog/Edit.aspx\nEdit Product]
        P4[Catalog/Details.aspx\nProduct Details]
        P5[Catalog/Delete.aspx\nDelete Product]
        SM[Site.Master\nLayout Template]
    end

    subgraph Frontend["Frontend Assets"]
        F1[Bootstrap 4.3.1]
        F2[jQuery 3.5.0]
        F3[Modernizr 2.8.3]
    end

    subgraph BusinessLogic["Business Logic Layer"]
        SVC[ICatalogService]
        SVC_REAL[CatalogService\nReal Implementation]
        SVC_MOCK[CatalogServiceMock\nMock Implementation]
    end

    subgraph DI["Dependency Injection"]
        AUTOFAC[Autofac 4.9.1\nIoC Container]
    end

    subgraph DataAccess["Data Access Layer"]
        CTX[CatalogDBContext\nEntity Framework 6.2.0]
        INIT[CatalogDBInitializer\nDB Seeding]
    end

    subgraph DataStorage["Data Storage"]
        DB[(SQL Server\nLocalDB / MSSQLLocalDB\nCatalogDb)]
        PIC[Image Files\nPics Folder]
    end

    subgraph CrossCutting["Cross-Cutting Concerns"]
        LOG[log4net 2.0.10\nLogging]
        AI[Application Insights 2.9.1\nMonitoring and Telemetry]
        BUNDLE[ASP.NET Bundling\nand Minification]
    end

    B -->|HTTP Requests| Presentation
    SM --> Presentation
    Frontend --> Presentation
    Presentation -->|Service Calls| SVC
    AUTOFAC -->|Injects| SVC
    SVC --> SVC_REAL
    SVC --> SVC_MOCK
    SVC_REAL --> CTX
    CTX --> INIT
    CTX -->|SQL Queries| DB
    SVC_REAL -->|Reads| PIC
    LOG --> Presentation
    LOG --> BusinessLogic
    AI --> Presentation
    BUNDLE --> Presentation
```

## Technology Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Framework | ASP.NET WebForms | .NET Framework 4.7.2 |
| ORM | Entity Framework | 6.2.0 |
| Database | SQL Server (LocalDB) | - |
| DI Container | Autofac | 4.9.1 |
| Logging | log4net | 2.0.10 |
| Monitoring | Application Insights | 2.9.1 |
| UI Framework | Bootstrap | 4.3.1 |
| JavaScript | jQuery | 3.5.0 |
| Serialization | Newtonsoft.Json | 12.0.1 |

## Key Components

- **Presentation**: ASP.NET WebForms pages for product catalog CRUD operations
- **Services**: `ICatalogService` abstraction with real (EF6+SQL) and mock implementations
- **Data Models**: `CatalogItem`, `CatalogBrand`, `CatalogType` entities
- **Database Context**: `CatalogDBContext` with Code First EF6 configuration
- **DI**: Autofac injected via HTTP modules into WebForms pages
- **Monitoring**: Application Insights for telemetry and request tracking

## Assessment Summary

The AppCAT assessment identified **7 issues** and **8 incidents** totaling **24 story points**:

- **2 Mandatory** issues requiring changes before migration
- **4 Optional** improvements recommended
- **2 Potential** issues to investigate

These findings indicate migration effort needed to move from ASP.NET WebForms on .NET Framework to a modern .NET platform (e.g., ASP.NET Core on Azure App Service).
