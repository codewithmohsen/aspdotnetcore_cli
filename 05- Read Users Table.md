# Read Users Table

## 1. Create Interface
```
dotnet new interface --output Domain/Interfaces --name IUserRepository
```
## 2. Edit Interface
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
### 3. Create Class
```
dotnet new class --output Domain/Data/Repositories --name UserRepository
```
### 4. Update Class
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
