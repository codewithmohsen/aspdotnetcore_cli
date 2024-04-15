# Create Project
## 1. Create The New Solution 
mkdir clean-architecture-proj 
```
dotnet new sln
```
result: clean-architecture-proj.sln created
## 2. Create Layers
### 2.1. Domain
```
dotnet new classlib -o Domain
rm Domain/Class1.cs
dotnet sln clean-architecture-proj.sln add Domain
```
### 2.2. Application
```
dotnet new classlib -o Application
rm Application/Class1.cs
dotnet sln clean-architecture-proj.sln add Application
```
### 2.3. Infrastructure
```
dotnet new classlib -o Infrastructure
rm Infrastructure/Class1.cs
dotnet sln clean-architecture-proj.sln add Infrastructure
```
### 2.4. Presentation  
```
dotnet new webapi -o Presentation
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

