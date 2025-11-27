# OnlyTools2

OnlyTools2 is a Razor Pages / ASP.NET Core (.NET 6) platform for:
- Renting and lending tools between users
- Posting and browsing job listings (offerings / requests)
- Publishing and reading home-improvement articles (tips & tricks)

This repository contains a server-rendered web app (Razor/Controllers + Views) with Identity-based authentication and EF Core data access.

## High-level architecture

- Web (MVC + Razor views) project: controllers, views, static assets (wwwroot).
- Core project: service interfaces, DTO/view models.
- Infrastructure project: EF Core DbContext, EF models, migrations, seeders.
- Identity: custom `ApplicationUser : IdentityUser<Guid>` used for profiles and relationships.

Key controllers and services
- `Controllers/ToolController.cs` — tool browsing, add/edit, rent/return, details, MyTools/MyRentedTools
- `Controllers/JobListingController.cs` — job listing CRUD and MyJobs
- `Controllers/TipController.cs` — tips CRUD and MyTips
- `Controllers/UserController.cs` — registration, login, profile, upload picture
- `Controllers/LikeController.cs` — like/unlike article actions
- Services / contracts under `OnlyTools.Core.Contracts`:
  - `IToolServices`, `IJobListingServices`, `ITipServices`, `IUserServices`, `ICategoriesServices`
- EF models (Infrastructure):
  - `OnlyTools.Infrastructure.Data.Models.Tool`, `JobListing`, `Tip`, `ToolCategory`, `TipCategory`, `Like`
- Core view models:
  - `OnlyTools.Core.Models.Tool.*`, `OnlyTools.Core.Models.JobListing.*`, `OnlyTools.Core.Models.Tips.*`, `OnlyTools.Core.Models.User.UserModel`

DbContext & Migrations
- `OnlyTools.Infrastructure.Data.OnlyToolsDbContext` contains DbSets and configuration.
- Migrations live in `OnlyTools.Infrastructure/Migrations`.
- Seeders: `ToolsSeeder`, `TipsSeeder`, `ToolCategorySeeder`, `ApplicationUserSeeder`, etc. Review seeders — some seed data expects real GUIDs for seeded users; update before running if needed.

## Prerequisites

- .NET 6 SDK
- Visual Studio 2022 with the __ASP.NET and web development__ workload (recommended)
- SQL Server (or adjust connection string to your provider)

## Local setup (CLI)

1. Clone:
   git clone https://github.com/MaryPeteva/OnlyTools2.git
   cd OnlyTools2

2. Restore & build:
   dotnet restore
   dotnet build

3. Configure database connection:
   - Edit the connection string in `appsettings.Development.json` or use user-secrets / environment variables for sensitive data.

4. Apply EF migrations and seed:
   - Using CLI:
     dotnet ef database update --project OnlyTools.Infrastructure --startup-project OnlyTools
   - Or use Visual Studio __Package Manager Console__:
     Update-Database -Project OnlyTools.Infrastructure -StartupProject OnlyTools

   Note: If seeders require seeded user GUIDs, inspect and set appropriate values before running.

5. Run:
   dotnet run --project OnlyTools
   or open the solution in Visual Studio and set the web project as startup; start with __Debug > Start Debugging__ (F5).

## Where to look in the codebase

- Controllers: `OnlyTools/Controllers`
- Views (Razor): `OnlyTools/Views`
- Core models & services: `OnlyTools.Core/*`
- EF models, DbContext, migrations, seeders: `OnlyTools.Infrastructure/*`
- Static assets: `OnlyTools/wwwroot` (css, images, js)