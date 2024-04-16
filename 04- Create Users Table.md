# Create Users Table
## 1. Create /Domain/Entities/User Class
```
dotnet new class --output Domain/Entities --name User
```
## 2. Edit /Domain/Entities/User Class
replace
```
namespace Domain;
public class User
{
}
```
with
```
namespace Domain;
public class User
{
    [Key]
    public int UserId { get; set; }
    [Display(Name = "ایمیل")]
    [Required(ErrorMessage = "لطفا {0} را وارد کنید")]
    [MaxLength(350, ErrorMessage = "حداکثر کاراکتر مجاز {1} میباشد")]
    public string Email { get; set; }
    [Display(Name = "کلمه عبور")]
    [Required(ErrorMessage = "لطفا {0} را وارد کنید")]
    [MaxLength(350, ErrorMessage = "حداکثر کاراکتر مجاز {1} میباشد")]
    public string Password { get; set; }
    [Display(Name = "نام کاربری")]
    [Required(ErrorMessage = "لطفا {0} را وارد کنید")]
    [MaxLength(350, ErrorMessage = "حداکثر کاراکتر مجاز {1} میباشد")]
    public string UserName { get; set; }
}
```
## 3. Edit /Domain/Data/Context/MyDbContext Class
replace
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
    #region Users
    public DbSet<User> Users { get; set; }
    #endregion
}
```
### 4. Check DbContext Info
```
dotnet ef dbcontext info                                                                       took 4s
Build started...
Build succeeded.
Type: Domain.MyDbContext
Provider name: Microsoft.EntityFrameworkCore.SqlServer
Database name: myDb
Data source: localhost
Options: MigrationsAssembly=Presentation
```
## 5. Create Migration
```
cd Presentation
dotnet ef migrations add addUserTable   
```
## 6. Update Database
```
dotnet ef database update
```
