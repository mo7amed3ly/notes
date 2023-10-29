#ASP.NET Core
- install dotnet sdk
- create application
  - dotnet new webapi -n API
  - dotnet new sln
  - dotnet sln add API\API.csproj
- add nuget packages for entityframework and sqllite:
  - Microsoft.EntityFrameworkCore.Sqlite
  - Microsoft.EntityFrameworkCore.Design
- install dotnet ef tool globally
  - dotnet tool install --global dotnet-ef --version 7.0.13
- create entity class
  - `class AppUser{prop Id; prop Username}`
- create connection string in appSettings.json
  - `"ConnectionStrings":{"DefaultConnection":"Data source=database.db"}`
- create a db context class
```
    class DataContext:DbContext
    {
    public DataContext(options):DbContext(options){}
    prop List<AppUser> Users;
    }
    ```
- inject db context
```
builder.Services.AddDbContext<DataContext>(
    opt =>
    {
        opt.UseSqlite(builder.Configuration.GetConnectionString("DefaultConnection"));
    }
    );
```
