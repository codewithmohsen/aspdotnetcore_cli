# Create Project
## 1. Create The New Solution 
mkdir clean-architecture-proj 
```
dotnet new sln
```
result: clean-architecture-proj.sln created
## 2. Create Layers
### 2.1. Core/Domain
```
dotnet new classlib -o Core/Domain
rm Core/Domain/Class1.cs
dotnet sln clean-architecture-proj.sln add Core/Domain
```
### 2.2. Core/Data
```
dotnet new classlib -o Core/Data
rm Core/Data/Class1.cs
dotnet sln clean-architecture-proj.sln add Core/Data
```
### 2.3. Core/Application
```
dotnet new classlib -o Core/Application
rm Core/Application/Class1.cs
dotnet sln clean-architecture-proj.sln add Core/Application
```
### 2.4. Infrastructure/IOC
```
dotnet new classlib -o Infrastructure/IOC
rm Infrastructure/IOC/Class1.cs
dotnet sln clean-architecture-proj.sln add Infrastructure/IOC
```
### 2.5. Infrastructure/Presentation
