# Read Users Table

## 1. Create IUserRepository Interface
```
dotnet new interface --output Domain/Interfaces --name IUserRepository
```
## 2. Edit IUserRepository Interface
replace
```
namespace Domain;
public interface IUserRepository
{
}
```
with
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
replace
```
namespace Domain;
public class UserRepository
{
}
```
with
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
replace
```
namespace Application;
public interface IUserService
{
}
```
with
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
replace
```
namespace Application;
public class UserService
{
}
```
with
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
replace
```
namespace Infrastructure;
public class DependencyContainer
{
}
```
with
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
### 11.1
add using
```
using Infrastructure;
```
### 11.2
add
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
## 12. Create UserController API Controller
```
dotnet new apicontroller --output Presentation/Controllers --actions true --name UserController
```
now you have
```
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
namespace MyApp.Namespace
{
    [Route("api/[controller]")]
    [ApiController]
    public class UserController : ControllerBase
    {
        // GET: api/<UserController>
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }
        // GET api/<UserController>/5
        [HttpGet("{id}")]
        public string Get(int id)
        {
            return "value";
        }
        // POST api/<UserController>
        [HttpPost]
        public void Post([FromBody] string value)
        {
        }
        // PUT api/<UserController>/5
        [HttpPut("{id}")]
        public void Put(int id, [FromBody] string value)
        {
        }
        // DELETE api/<UserController>/5
        [HttpDelete("{id}")]
        public void Delete(int id)
        {
        }
    }
}
```
replace
```
public class UserController : ControllerBase
```
with
```
public class UserController : BaseController
```
remove
```
    [Route("api/[controller]")]
    [ApiController]
```
## 13. Edit UserController API Controller
### 13.1
add using
```
using Application;
using Domain;
```
### 13.2
add IUserService object and Constructor
```
        private readonly IUserService _userService;
        public UserController(IUserService userService)
        {
            _userService = userService;
        }
```
### 13.3 
replace
```
        // GET: api/<UserController>
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }
```
with
```
        // GET: api/<UserController>
        [HttpGet]
        public async Task<IEnumerable<User>> Get()
        {
            return await _userService.GetAllUsersAsync();
        }
```
### 13.4
replace
```
        // GET api/<UserController>/5
        [HttpGet("{id}")]
        public string Get(int id)
        {
            return "value";
        }
```
with
```
        // GET api/<UserController>/5
        [HttpGet("{id}")]
        public async Task<User?> Get(int id)
        {
            return await _userService.GetUserByIdAsync(id);
        }
```
## 14. Run Project
```
cd presentation
dotnet run
```
http://localhost:5168/api/user
http://localhost:5168/api/user/1
