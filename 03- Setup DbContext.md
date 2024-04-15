# Setup DbContext
## 1. Create /Domain/Data/Context/myDbContext Class
```
cd Domain/Data/Context
dotnet new class --name myDbContext
```
## 2. Edit /Domain/Data/Context/myDbContext Class
open Domain/Data/Context.myDbContext.cs
update
```
namespace Domain;
public class myDbContext
{
}
```
to
```
namespace Domain;
using Microsoft.EntityFrameworkCore;
public class myDbContext : DbContext
{
    public myDbContext(DbContextOptions<myDbContext> options)
            : base(options)
    {
    }
}
```
### 3. Edit /Presentation/appsettings.json
update
```
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```
to
```
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "AppConnectionString": "Data source=.;Initial Catalog=App_Db;Integrated Security=true;"
  }
}
```
### 4. Edit .Presentation/Program.cs
add at top
```
using Domain;
using Microsoft.EntityFrameworkCore;
```
now between
```
using DataLayer;
using Microsoft.EntityFrameworkCore;
var builder = WebApplication.CreateBuilder(args);
```
and
```
var app = builder.Build();
```
add bottom code
```
#region Add DbContext
builder.Services.AddDbContext<myDbContext>(options =>
{
    options.UseSqlServer(builder.Configuration.GetConnectionString("AppConnectionString"));
});
#endregion
```
