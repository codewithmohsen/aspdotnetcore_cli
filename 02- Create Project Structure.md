# Create Project Structure
## 1. Create The New Solution 
mkdir clean-architecture-proj 
```
dotnet new sln
```
result: clean-architecture-proj.sln created
## 2. Create Solution Layers as Projects
### 2.1. Domain
```
dotnet new classlib --output Domain
rm Domain/Class1.cs
dotnet sln clean-architecture-proj.sln add Domain
```
### 2.2. Application
```
dotnet new classlib --output Application
rm Application/Class1.cs
dotnet sln clean-architecture-proj.sln add Application
```
### 2.3. Infrastructure
```
dotnet new classlib --output Infrastructure
rm Infrastructure/Class1.cs
dotnet sln clean-architecture-proj.sln add Infrastructure
```
### 2.4. Presentation  
```
dotnet new mvc --output Presentation
dotnet sln clean-architecture-proj.sln add Presentation
```
## 3. Check Added projects in solution clean-architecture-proj
```
dotnet sln clean-architecture-proj.sln list

Project(s)
----------
Application/Application.csproj
Domain/Domain.csproj
Infrastructure/Infrastructure.csproj
Presentation/Presentation.csproj
```
## 4. Wire Up The References Between The Projects
### 4.1. Application Layer Project Needs Domain Layer Project
dotnet add Application/Application.csproj reference Domain/Domain.csproj
### 4.2. Infrastructure Layer Project Needs Application Layer Project
dotnet add Infrastructure/Infrastructure.csproj reference Application/Application.csproj
### 4.3. Presentation Layer Project Needs Infrastructure Layer Project
dotnet add Presentation/Presentation.csproj reference Infrastructure/Infrastructure.csproj

## 5. Make Projects' SubLayers
```
mkdir Domain/Entities Domain/DTOs Domain/Interfaces Domain/Data
mkdir Domain/Data/Context Domain/Data/Repositories
mkdir Application/Services Application/Extensions Application/Sender
mkdir Application/Services/Interfaces Application/Services/Implementations
mkdir Infrastructure/Dipendencies
```
## 6. Install Packages
```
cd Domain
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet add package Microsoft.EntityFrameworkCore.SqlServer 
```
```
dotnet list package

Project 'Domain' has the following package references
   [net8.0]: 
   Top-level Package                              Requested   Resolved
   > Microsoft.EntityFrameworkCore                8.0.4       8.0.4   
   > Microsoft.EntityFrameworkCore.Design         8.0.4       8.0.4   
   > Microsoft.EntityFrameworkCore.SqlServer      8.0.4       8.0.4   
   > Microsoft.EntityFrameworkCore.Tools          8.0.4       8.0.4   
```
```
cd Presentation
dotnet add package Microsoft.EntityFrameworkCore.Design
```
```
dotnet list package

Project 'Presentation' has the following package references
   [net8.0]: 
   Top-level Package                           Requested   Resolved
   > Microsoft.EntityFrameworkCore.Design      8.0.4       8.0.4  
```
## 7. Run Project
```
cd Presentation 
dotnet build
dotnet run
```
