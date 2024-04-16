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
    Task<IEnumerable<User>> GetAllUsersAsync();
    Task<User> GetUserByIdAsync(int id);
}
```
### 3. Create Class
```
dotnet new class --output Domain/Data/Repositories --name UserRepository
```
### 4. Update Class
update
```
```
to
```
```
