# How to Build a JWT-Secured CRUD REST Service with ASP.NET Core

## Project Overview

This sample shows how an ASP.NET Core 3.1 Web API integrates JWT bearer authentication with Entity Framework Core to expose a token-protected inventory catalog over HTTP. The service issues a signed JSON Web Token from a dedicated login controller and uses the same token to authorize create, read, update, and delete operations against SQL Server through MVC controller actions.

The application focuses on practical server-side data handling such as JWT issuance, role- and claim-based authorization, paging, filtering, and full CRUD operations on a SQL Server database.

## Key Features

- **JWT token issuance**: `TokenController` issues a signed JSON Web Token through `POST api/token` using `JwtSecurityToken` and `SecurityAlgorithms.HmacSha256`, so clients can authenticate once and reuse the token.
- **Token-protected product API**: `ProductsController` is decorated with `[Authorize]` and exposed at `api/products`, blocking all calls that do not include a valid Bearer token.
- **Complete CRUD for products**: Supports creating, reading, updating, and deleting product records through standard MVC controller actions (`POST`, `GET`, `PUT`, `DELETE`).
- **Flexible query filtering**: `GET api/products` accepts `inStock`, `skip`, and `take` parameters and applies them with `IQueryable<T>`, `Where`, `Skip`, and `Take` so clients can paginate and filter inventory.
- **Entity Framework Core data access**: Uses `InventoryContext` (a `DbContext`) to read and write `Products` and `UserInfo` data against SQL Server with connection pooling.
- **Centralized configuration in `Startup`**: Registers JWT bearer authentication, EF Core, routing, authorization, and MVC controllers in `Startup.ConfigureServices` and `Startup.Configure` for a clean startup pipeline.

## Prerequisites

- Visual Studio 2022 or Visual Studio Code
- .NET SDK compatible with the project's target framework
- SQL Server 2017 or later / SQL Server Express / SQL Server LocalDB
- Postman or any HTTP client for testing the API

## Running the Application

### Visual Studio 2022

1. Clone or download this repository.
2. Open the solution file:

   `InventoryService.sln`

3. Update the SQL Server connection string under `ConnectionStrings:InventoryDatabase` in `InventoryService/appsettings.json` with the server name, database name, and authentication details for a database that you are authorized to access.
4. Update the JWT settings under `Jwt` in `InventoryService/appsettings.json` (`Key`, `Issuer`, `Audience`, `Subject`) with values suitable for your environment.
5. Restore the NuGet packages by rebuilding the solution.
6. Set the startup project to:

   `InventoryService`

7. Build the solution.
8. Run the application using `Ctrl+F5`.
9. Open the application URL displayed by Visual Studio after launch (for example `https://localhost:5001/swagger`, `https://localhost:5001/api/products`, or the IIS Express profile URL).
10. Verify that an unauthenticated request to `GET api/products` returns `401 Unauthorized` and that `POST api/token` with valid credentials returns a JWT that you can attach as a Bearer token to the `api/products` requests.

### Visual Studio Code

1. Clone or download this repository.
2. Open the repository folder in Visual Studio Code.
3. Open the integrated terminal.
4. Navigate to the project directory:

```bash
cd InventoryService
```

5. Update the SQL Server connection string under `ConnectionStrings:InventoryDatabase` in `InventoryService/appsettings.json` with the server name, database name, and authentication details for a database that you are authorized to access.
6. Update the JWT settings under `Jwt` in `InventoryService/appsettings.json` (`Key`, `Issuer`, `Audience`, `Subject`) with values suitable for your environment.
7. Restore the NuGet packages:

```bash
dotnet restore
```

8. Build the project:

```bash
dotnet build
```

9. Run the project:

```bash
dotnet run
```

10. Open the local URL displayed in the terminal after the application starts (for example `https://localhost:5001/api/products`).
11. Verify that an unauthenticated request to `GET api/products` returns `401 Unauthorized` and that `POST api/token` with valid credentials returns a JWT that you can attach as a Bearer token to the `api/products` requests.

## Project Structure

- `InventoryService/Controllers/TokenController.cs` — issues a signed JWT in response to a valid user login request.
- `InventoryService/Controllers/ProductsController.cs` — exposes the authorized CRUD endpoints for the inventory catalog.
- `InventoryService/Models/Products.cs` — defines the product model stored in SQL Server.
- `InventoryService/Models/UserInfo.cs` — defines the user model used by the token endpoint.
- `InventoryService/Models/InventoryContext.cs` — defines the Entity Framework Core database context used to access products and users.

## Support and Feedback

- To report an issue specific to this sample, open a GitHub issue in this repository.
- Official documentation: [ASP.NET Core documentation](https://learn.microsoft.com/aspnet/core/)
