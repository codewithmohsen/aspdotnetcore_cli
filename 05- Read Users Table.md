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
