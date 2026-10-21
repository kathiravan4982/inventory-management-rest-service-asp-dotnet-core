# Inventory CRUD REST Service using ASP.NET CORE with JWT Token Authentication 

This repository contains the inventory management REST service, built with ASP.NET Core 3.1 and Entity Framework 3.1 to illustrate creating REST API to performing CRUD actions and, how to create JWT token and secure API.

## Key features

* **Full CRUD REST API** - Endpoints to create, read, update and delete product records exposed through the `ProductsController`.
* **JWT token authentication** - The `TokenController` validates the user credentials against the `UserInfo` table and returns a signed JWT access token (HMAC SHA-256) that expires after one day.
* **Token-secured endpoints** - The `ProductsController` is decorated with `[Authorize]`, so every inventory endpoint requires a valid bearer token.
* **Token validation pipeline** - `Startup.ConfigureServices` configures the `JwtBearer` authentication scheme to validate the issuer, audience and signing key supplied in the `Jwt` configuration section.
* **Server-side filtering and paging** - The products endpoint supports the optional `inStock`, `skip` and `take` query string parameters to filter available items and page through the results.
* **Entity Framework Core data access** - `InventoryContext` maps the `Products` and `UserInfo` entities to SQL Server, and the controllers use async data operations with concurrency handling.
* **Correct REST status codes** - The API responds with `CreatedAtAction`, `NoContent`, `BadRequest` and `NotFound` based on the outcome of each operation.

### API endpoints

| Method | Route | Description |
| ------ | ----- | ----------- |
| POST | `/api/token` | Requests a JWT access token using the email and password of a registered user. |
| GET | `/api/products` | Returns all products, optionally filtered by `inStock`, `skip` and `take`. |
| GET | `/api/products/{id}` | Returns the product that matches the supplied product id. |
| POST | `/api/products` | Adds a new product to the inventory. |
| PUT | `/api/products/{id}` | Updates the product that matches the supplied product id. |
| DELETE | `/api/products/{id}` | Removes the product that matches the supplied product id. |

## Prerequisites

* Visual Studio 2022
* SQL Server 2017
* Postman

## How to run the project

* Checkout this project to a location in your disk.
* Open the solution file using the Visual Studio 2022.
* Restore the NuGet packages by rebuilding the solution.
* Change the connection string in the appsettings.json file that points to your SQL Server.
* Run the project.