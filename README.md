# ASP.NET-APP

ASP.NET-APP is a .NET 8 solution for managing customers, products, orders, documents, notes, meetings, and tasks. The repository contains an ASP.NET Core MVC front end, an ASP.NET Core Web API backend, and a shared data project with Entity Framework Core models and DTOs.

## Solution structure

- `InternetApp.sln` - solution entry point
- `OZEsome/` - ASP.NET Core MVC application with Razor views
- `OzeSomeAPI/` - ASP.NET Core Web API used by the MVC app
- `OzeSome.Data/` - shared models, DTOs, and `DatabaseContext`
- `OpenAPIs/` and `Generated/` - generated API client artifacts

## Features

- Customer management with address support
- Product and category management
- Order management with order items and statuses
- Meeting scheduling and tracking
- Task tracking with statuses
- Notes and document management
- Swagger/OpenAPI support for the API

## Tech stack

- .NET 8
- ASP.NET Core MVC
- ASP.NET Core Web API
- Entity Framework Core
- SQL Server
- AutoMapper
- Swashbuckle / Swagger
- NSwag-generated API client for the MVC project

## How the projects work together

The MVC application in `OZEsome` communicates with `OzeSomeAPI` through a generated client configured with the API base URL `https://localhost:7152/`. Both applications use the shared `OzeSome.Data` project for the Entity Framework Core data model and DTO definitions.

## Prerequisites

- .NET 8 SDK
- SQL Server instance
- A local connection string named `DatabaseContext`

## Configuration

The repository only includes `appsettings.Development.json` files with logging configuration. Before running the applications, provide a `DatabaseContext` connection string by using one of the following approaches:

- local `appsettings.json`
- `appsettings.Development.json`
- environment variables
- .NET user secrets

Example:

```json
{
  "ConnectionStrings": {
    "DatabaseContext": "Server=localhost;Database=OzeSomeDb;Trusted_Connection=True;TrustServerCertificate=True"
  }
}
```

## Running the application

### 1. Start the API

```bash
dotnet run --project OzeSomeAPI/OzeSomeAPI.csproj
```

When it starts in development mode, Swagger UI is available at:

- `https://localhost:7152/swagger`

### 2. Start the MVC application

```bash
dotnet run --project OZEsome/OZEsome.csproj
```

The MVC application is configured to call the API at `https://localhost:7152/`, so the API should be running first.

## API areas

The API exposes controllers for:

- `Customers`
- `Addresses`
- `Products`
- `Categories`
- `Orders`
- `OrderItems`
- `Meetings`
- `Tasks`
- `Notes`
- `Documents`

Routes follow the standard pattern:

```text
/api/{controller}
```

## Build and test

Standard .NET commands for the repository are:

```bash
dotnet build InternetApp.sln
dotnet test InternetApp.sln
```

At the time of writing, the solution has no test projects and the API project contains a stale project reference to `../../MobileApp/OzeSome.Data/OzeSome.Data.csproj`, which prevents a clean solution build in the current repository snapshot. To restore solution builds locally, update that reference in `OzeSomeAPI/OzeSomeAPI.csproj` so it points to the checked-in data project at `../OzeSome.Data/OzeSome.Data.csproj`.

## Notes for development

- The shared database model is defined in `OzeSome.Data/Models/Contexts/DatabaseContext.cs`.
- The API registers services in `OzeSomeAPI/Program.cs`.
- The MVC application registers the generated API client in `OZEsome/Program.cs`.
- Swagger output is also checked into the repository under `OpenAPIs/` and `OZEsome/OpenAPIs/`.
