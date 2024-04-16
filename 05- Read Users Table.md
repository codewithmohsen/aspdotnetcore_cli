# Read Users Table

## 1. Create IUserRepository Interface
```
dotnet new interface --output Domain/Interfaces --name IUserRepository
```
## 2. Edit IUserRepository Interface
update
```
namespace Domain;
public interface IUserRepository
{
}
```
to
```
namespace Domain;
public interface IUserRepository
{
    #region User
    Task<IEnumerable<User>> GetAllUsersAsync();
    Task<User?> GetUserByIdAsync(int id);
    #endregion
}
```
### 3. Create UserRepository Class
```
dotnet new class --output Domain/Data/Repositories --name UserRepository
```
### 4. Update UserRepository Class
update
```
namespace Domain;
public class UserRepository
{
}
```
to
```
using System;
using Microsoft.EntityFrameworkCore;
namespace Domain;
public class UserRepository : IUserRepository
{
    #region Counstructor
    private readonly MyDbContext _context;
    public UserRepository(MyDbContext context)
    {
        _context = context;
    }
    #endregion
    #region User
    public async Task<IEnumerable<User>> GetAllUsersAsync()
    => await _context.Users.ToListAsync();
    public async Task<User?> GetUserByIdAsync(int id)
    => await _context.Users.FirstOrDefaultAsync(u => u.UserId == id);
    #endregion

}
```
## 5. Create IUserService Interface
```
dotnet new interface --output Application/Services/Interfaces --name IUserService
```
## 6. Edit IUserService Interface
update
```
namespace Application;
public interface IUserService
{
}
```
to
```
using Domain;
namespace Application;
public interface IUserService
{
    Task<IEnumerable<User>> GetAllUsersAsync();
    Task<User?> GetUserByIdAsync(int id);
}
```
## 7. Create UserRepository Class
```
dotnet new class --output Application/Services/Implementations --name UserService
```
## 8. Update UserRepository Class
update
```
namespace Application;
public class UserService
{
}
```
to
```
using Domain;
namespace Application;
public class UserService : IUserService
{
    #region Constructor
    private readonly IUserRepository _userRepository;
    public UserService(IUserRepository userRepository)
    {
        _userRepository = userRepository;
    }
    #endregion
    #region User
    #endregion
    public async Task<IEnumerable<User>> GetAllUsersAsync()
    => await _userRepository.GetAllUsersAsync();

    public async Task<User?> GetUserByIdAsync(int id)
    => await _userRepository.GetUserByIdAsync(id);
}
```
## 9. Create DependencyContainer Class
```
dotnet new class --output Infrastructure/Dipendencies --name DependencyContainer
```
## 10. Update  Class
update
```
namespace Infrastructure;
public class DependencyContainer
{
}
```
to
```
using Application;
using Domain;
using Microsoft.Extensions.DependencyInjection;
namespace Infrastructure;
public static class DependencyContainer
{
    public static void RegisterServices(this IServiceCollection services)
    {
        #region Service
        services.AddScoped<IUserService, UserService>();
        #endregion
        #region Repository
        services.AddScoped<IUserRepository, UserRepository>();
        #endregion
    }
}
```
## 11. Update Application.program.cs
add at top
```
using Infrastructure;
```
now add
```
#region Add Services
builder.Services.RegisterServices();
#endregion
```
between
```
var builder = WebApplication.CreateBuilder(args);
// Add services to the container.
builder.Services.AddControllersWithViews();
```
and
```
#region Add MyDbContext
builder.Services.AddDbContext<MyDbContext>(options =>
{
    options.UseSqlServer(builder.Configuration.GetConnectionString("AppConnectionString"), b => b.MigrationsAssembly("Presentation"));
});
#endregion
```
