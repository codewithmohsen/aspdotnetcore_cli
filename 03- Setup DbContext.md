# Setup DbContext
## 1. Create /Domain/Data/Context/MyDbContext Class
```
dotnet new class --output Domain/Data/Context --name MyDbContext
```
## 2. Edit /Domain/Data/Context/MyDbContext Class
open Domain/Data/Context.MyDbContext.cs
replace
```
namespace Domain;
public class MyDbContext
{
}
```
with
```
namespace Domain;
using Microsoft.EntityFrameworkCore;
public class MyDbContext : DbContext
{
    public MyDbContext(DbContextOptions<MyDbContext> options)
            : base(options)
    {
    }
}
```
### 3. Edit /Presentation/appsettings.json
replace
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
with
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
    "AppConnectionString": "Server=localhost; Database=myDb; User Id=sa; Password=<your_password_here>; TrustServerCertificate=true"
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
add
```
#region Add MyDbContext
builder.Services.AddDbContext<MyDbContext>(options =>
{
    options.UseSqlServer(builder.Configuration.GetConnectionString("AppConnectionString"), b => b.MigrationsAssembly("Presentation"));
});
#endregion
```
