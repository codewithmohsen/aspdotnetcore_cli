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
