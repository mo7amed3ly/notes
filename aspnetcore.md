# ASP.NET Core
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
- add database migration
```
dotnet ef migrations add Initial -o data\Migrations
```
- create database
```
dotnet ef database update
```
- view sqlite database 
  - install vs code ext sqlite
  - command palette sqlite view database
- create controller to access AppUser apis
```csharp
[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    private readonly DataContext _context;

    public UsersController(DataContext context)
    {
        _context = context;
    }

    [HttpGet]
    public async Task<ActionResult<IEnumerable<AppUser>>> Get() 
    { 
        return await _context.Users.ToListAsync();
    
    }

    [HttpGet("{id}")]
    public async Task<ActionResult<AppUser>> GetUser(int id)
    {
        return await _context.Users.FindAsync(id);
    }
}
```
