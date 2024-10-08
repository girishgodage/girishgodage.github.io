---
title: Production Readiness and Deployment
date: 2019-10-24 05:54:00 Z
permalink: "/blog/ProductionReadinessandDeployment"
categories:
- AspDotnetCore
tags:
- learning
summary: Production Readiness and Deployment
image: "/img/aspnetcore.png"
author: Girish Godage
layout: posts
prevurl: "/blog/Addpersonalagenda"
nexturl: "/blog/Challenges"
discussion_id: 2019-10-24-confplannertut7
---

# Production Readiness and Deployment

## Adding EF Healthchecks to the BackEnd
1. Add a reference to the NuGet package `Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore` version `3.0.0`.
    > This can be done from the command line using `dotnet add package Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore --version 3.0.0`
1. Add a DbContext health check in `Startup.cs` by adding the following code to `ConfigureServices`:
   ```csharp
   services.AddHealthChecks()
           .AddDbContextCheck<ApplicationDbContext>();
   ```
1. Wire up health checks by modifying the `UseEndpoints` to add a call to `endpoints.MapHealthChecks("/health");`. This will configure the health checks on the `/health` end point.
1. Test the end point by navigating to `/health`, it should return the text `Healthy`.

## Adding EF Healthchecks to the FrontEnd
1. Add a reference to the NuGet package `Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore` version `3.0.0`.
    > This can be done from the command line using `dotnet add package Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore --version 3.0.0`

    > Note: The `FrontEnd` and `BackEnd` projects have different versions of this package because they reference different versions of EntityFrameworkCore.
1. Add a DbContext health check in `Startup.cs` by adding the following code to `ConfigureServices`:
   ```csharp
   services.AddHealthChecks()
           .AddDbContextCheck<IdentityDbContext>();
   ```
1. Wire up health checks by modifying the `UseEndpoints` to add a call to `endpoints.MapHealthChecks("/health");`. This will configure the health checks on the `/health` end point.
1. Test the end point by navigating to `/health`, it should return the text `Healthy`.

## Adding a custom health check to test for BackEnd availability
1. Add a `CheckHealthAsync` method to `IApiClient`.
   ```c#
   public interface IApiClient
   {
       ...
       Task<bool> CheckHealthAsync();
   }
   ```
1. Implement the `CheckHealthAsync` method in `ApiClient` by adding the following code:
    ```csharp
    public async Task<bool> CheckHealthAsync()
    {
        try
        {
            var response = await _httpClient.GetStringAsync("/health");

            return string.Equals(response, "Healthy", StringComparison.OrdinalIgnoreCase);
        }
        catch
        {
            return false;
        }
    }
    ```
1. Create a `HealthChecks` folder under the root folder in the FrontEnd project.
1. Create a file called `BackendHealthCheck.cs` with the following implementation:
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading;
    using System.Threading.Tasks;
    using FrontEnd.Services;
    using Microsoft.Extensions.Diagnostics.HealthChecks;

    namespace FrontEnd.HealthChecks
    {
        public class BackendHealthCheck : IHealthCheck
        {
            private readonly IApiClient _client;

            public BackendHealthCheck(IApiClient client)
            {
                _client = client;
            }

            public async Task<HealthCheckResult> CheckHealthAsync(HealthCheckContext context, CancellationToken cancellationToken = default(CancellationToken))
            {
                if (await _client.CheckHealthAsync())
                {
                    return HealthCheckResult.Healthy();
                }

                return HealthCheckResult.Unhealthy();
            }
        }
    }
    ```
1. Register the `BackendHealthCheck` in `ConfigureServices`:
    ```csharp
    services.AddHealthChecks()
            .AddCheck<BackendHealthCheck>("backend")
            .AddDbContextCheck<IdentityDbContext>();
    ```
1. Test the end point by navigating to `/health`, it should return the text `Healthy`.

## Publishing locally

https://docs.microsoft.com/en-us/aspnet/core/publishing/#publish-to-a-folder

## Publishing to Production (Azure App Service)

- [Visual Studio](https://docs.microsoft.com/en-us/aspnet/core/tutorials/publish-to-azure-webapp-using-vs)

1. Right Click Backend | Publish
2. Select App Service, make sure the publish drop down is set to "Create Profile", click "Create Profile"
3. Fill in Name, Resource Group, Hosting Plan
4. Click "Create SQL Database", fill in name, press "New..." to create Database server, fill in fields click "OK"
5. Click "Create"
6. On the publish page click "Edit". Click Settings. Open Databases and check the box for the default database. Click Migrations and check the box to apply migrations on publish. Click Save.
7. Click the "Publish" button. When the app launches, find the Sessions section, scroll to /api/sessions/upload. Click the post button to upload your session data

8. In the FrontEnd application, open the AppSettings.json and replace the ServiceUrl with the URL from your backend in App Service.
9. Right Click Frontend | Publish
10. Select App Service, make sure the publish drop down is set to "Create Profile", click "Create Profile"
11. Fill in name, same resource group as above and same hosting plan as above.
12. Click "Create Database", fill in name, select the same database server from above, fill in the same password as above and change the Connection string name to "IdentityDbContextConnection". Press OK.
13. Click "Create".
15. On the publish page click "Edit". Click Settings. Open Databases and check the box for the default database. Click Migrations and check the box to apply migrations on publish. Click Save.
16. Click the "Publish" button.

- [Git](https://docs.microsoft.com/en-us/aspnet/core/publishing/azure-continuous-deployment)

## Publishing using Docker

**Prerequisites**

- [Docker for Windows](https://www.docker.com/docker-windows) or [Docker for Mac](https://www.docker.com/docker-mac)
- [Visual Studio Tools for Docker with ASP.NET Core](https://docs.microsoft.com/aspnet/core/host-and-deploy/docker/visual-studio-tools-for-docker)

**Enable Database Errors and switch to SQL since that is what runs in the container**

For both the front end and backend make the following code chanages:
1. Add the package "Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore"
2. Add the database error page in Startup.cs, after app.UseDeveloperExceptionPage() add app.UseDatabaseErrorPage():

    ```csharp
    app.UseDeveloperExceptionPage();
    app.UseDatabaseErrorPage();
    ```
3. In the backend project, in Startup.cs, comment out the code to support SQLite:

    ```csharp
   //if (RuntimeInformation.IsOSPlatform(OSPlatform.Windows))
   //{
       options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection"));
   //}
   //else
   //{
   //    options.UseSqlite("Data Source=conferences.db");
   //}
    ```
### Using Visual Studio

Add Docker support to the **BackEnd** project by right clicking the project file and selecting Add > Docker Support

A **Dockerfile** is added to the **BackEnd** project.

```Dockerfile
FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /src
COPY ["BackEnd/BackEnd.csproj", "BackEnd/"]
COPY ["ConferenceDTO/ConferenceDTO.csproj", "ConferenceDTO/"]
RUN dotnet restore "BackEnd/BackEnd.csproj"
COPY . .
WORKDIR "/src/BackEnd"
RUN dotnet build "BackEnd.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BackEnd.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BackEnd.dll"]
```
Add Container Orchestration support to the project, right click the project file and selecting Add > Container Orchestration Support Select Docker Comppose and click OK. This creates a **docker-compose** project with a docker.compose.yml file.

Repeat the same step for the **FrontEnd** project. The Dockerfile is added to the project for it.

Add Docker support to the **FrontEnd** project by right clicking the project file and selecting Add > Docker Support

```Dockerfile
FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /src
COPY ["FrontEnd/FrontEnd.csproj", "FrontEnd/"]
COPY ["ConferenceDTO/ConferenceDTO.csproj", "ConferenceDTO/"]
RUN dotnet restore "FrontEnd/FrontEnd.csproj"
COPY . .
WORKDIR "/src/FrontEnd"
RUN dotnet build "FrontEnd.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "FrontEnd.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FrontEnd.dll"]
```
Add Container Orchestration support to the project, right click the project file and selecting Add > Container Orchestration Support Select Docker Comppose and click OK.

The **docker-compose.yml** file is updated to reflect that there are two projects to build images.

```yaml
version: '3.4'

services:

  frontend:
    image: ${DOCKER_REGISTRY-}frontend
    build:
      context: .
      dockerfile: FrontEnd/Dockerfile

  backend:
    image: ${DOCKER_REGISTRY-}backend
    build:
      context: .
      dockerfile: BackEnd/Dockerfile
```

#### Linking / Networking

In the previous architecture, there were some setting and manual orchestration to start up and wire the applications together.

- **BackEnd** tested for OS type and set the application to use SQL Server or SQLite
- **FrontEnd** application had the API url set in the *appsetting.json* file.

Using Docker, adding a container for SQL Server and linking the containers in the compose file simplifies this.

#### Adding SQL Server

Open the docker-compose.yml file and add the following entry. *Note the $ is doubled for escaping*

```yaml
  db:
    image: "microsoft/mssql-server-linux"
    environment:
      SA_PASSWORD: "ConferencePlanner1234$$"
      ACCEPT_EULA: "Y
```

Since the **BackEnd** application must have connectivity and cannot start until the database container is ready. Add the **depends_on** entry to the **backend** definition in the compose file.

```yaml
  backend:
    image: backend
    build:
      context: .
      dockerfile: BackEnd/Dockerfile
    depends_on:
      - db
```

Finally, change the connection string for the database in the BackEnd\appsettings.json file.

```javascript
  "ConnectionStrings": {
    "DefaultConnection": "Server=db;Initial Catalog=ConferencePlanner;User=sa;Password=ConferencePlanner1234$;MultipleActiveResultSets=true"
  }
```

##### Linking / Networking FrontEnd & BackEnd

In the **docker-compose.yml** file, add the **links** section to the **frontend** definition. This sets up the host name in the Docker networking allowing for the web application to call the API by name. `http://backend`

```yaml
  frontend:
    image: frontend
    build:
      context: .
      dockerfile: FrontEnd/Dockerfile
    links:
      - backend
```

Change the value for the **ServiceUrl** in FrontEnd/appsetting.json

```javascript
{
  "ServiceUrl": "http://backend/",
```

Remove or comment out the `app.UseHttpsRedirection();` in BackEnd\Startuo.cs.

#### Starting and Debugging

Once the changes are complete, F5 to build start the application in Docker. Debugging is still available in all projects, but now the application is running in containers.

Changes can be made to Razor pages and seen immediately without rebuilds, however and *.cs file changes require rebuilds.

### Using VS Code or other editors

Create and add the following Dockerfile for the BackEnd application.

```Dockerfile
FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /src
COPY ["BackEnd/BackEnd.csproj", "BackEnd/"]
COPY ["ConferenceDTO/ConferenceDTO.csproj", "ConferenceDTO/"]
RUN dotnet restore "BackEnd/BackEnd.csproj"
COPY . .
WORKDIR "/src/BackEnd"
RUN dotnet build "BackEnd.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BackEnd.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
```

Create and add the following Dockerfile for the FrontEnd application.

```Dockerfile
FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /src
COPY ["BackEnd/BackEnd.csproj", "BackEnd/"]
COPY ["ConferenceDTO/ConferenceDTO.csproj", "ConferenceDTO/"]
RUN dotnet restore "BackEnd/BackEnd.csproj"
COPY . .
WORKDIR "/src/BackEnd"
RUN dotnet build "BackEnd.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BackEnd.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
```

At the root of the ConferencePlanner solution, add the following **docker-compose.yml** file.

```yaml
version: '3.4'

services:

  frontend:
    image: ${DOCKER_REGISTRY-}frontend
    build:
      context: .
      dockerfile: FrontEnd/Dockerfile

  backend:
    image: ${DOCKER_REGISTRY-}backend
    build:
      context: .
      dockerfile: BackEnd/Dockerfile
    depends_on:
      - db 

  db:
    image: "microsoft/mssql-server-linux"
    environment:
      SA_PASSWORD: "ConferencePlanner1234$$"
      ACCEPT_EULA: "Y"
```
At the root of the ConferencePlanner solution, add the following **docker-compose.override.yml** file.

```yaml
version: '3.4'

services:
  backend:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_HTTPS_PORT=44370
    ports:
      - "63145:80"
      - "44370:443"
    volumes:
      - ${APPDATA}/Microsoft/UserSecrets:/root/.microsoft/usersecrets:ro
      - ${APPDATA}/ASP.NET/Https:/root/.aspnet/https:ro
  frontend:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_HTTPS_PORT=44384
    ports:
      - "56004:80"
      - "44384:443"
    volumes:
      - ${APPDATA}/Microsoft/UserSecrets:/root/.microsoft/usersecrets:ro
      - ${APPDATA}/ASP.NET/Https:/root/.aspnet/https:ro
```

#### SQL Server Configuration

Change the connection string for the database in the BackEnd\appsettings.json file.

```javascript
  "ConnectionStrings": {
    "DefaultConnection": "Server=db;Initial Catalog=ConferencePlanner;User=sa;Password=ConferencePlanner1234$;MultipleActiveResultSets=true"
  }
```

#### Linking / Networking FrontEnd & Backend

Change the value for the **ServiceUrl** in FrontEnd/appsetting.json

```javascript
{
  "ServiceUrl": "http://backend/",
```

Remove or comment out the `app.UseHttpsRedirection();` in BackEnd\Startuo.cs.

#### Building and Running

- Build the Docker images `docker-compose build`
- Run `docker-compose up -d` to start the application
- Run `docker-compose down` to stop the application
- Open the application at <http://localhost:5002>

---
{% include conf_sessions.md %}

{% include blogslide.html %}

