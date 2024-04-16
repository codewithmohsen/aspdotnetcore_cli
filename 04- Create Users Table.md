# Create Users Table
## 1. Create /Domain/Entities/User Class
```
cd Domain/Entities/
dotnet new class --name User
```
## 2. Edit /Domain/Entities/User Class
update
```
namespace Domain;
public class User
{
}
```
to
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
## 3. Edit /Domain/Data/Context/myDbContext Class
update
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
    #region Users
    public DbSet<User> Users { get; set; }
    #endregion
}
```
## 4. Create Migration
```
cd Presentation
dotnet ef migrations add addUserTable   
```
## 5. Update Database
```
dotnet ef database update
```
