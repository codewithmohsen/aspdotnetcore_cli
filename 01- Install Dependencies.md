# Install Dependencies
## 1. VScode
https://code.visualstudio.com/download
## 2. DotnetCore
https://dotnet.microsoft.com/en-us/download/dotnet
#### Note
close vs code when you are installing DotNet Core
## 3. Postman
https://www.postman.com/downloads
## 4. Docker
https://www.docker.com/products/docker-desktop/
## 5. Azure Data Studio 
https://azure.microsoft.com/en-us/products/data-studio
## 6. SQL Server
### 6.1. Config Docker
Make sure your the memory in used is at least 4 GB in setting.
#### Note
If error "Another application changed your Desktop configurations" happened you have 2 works to do:
##### solution 1
> Docker > Setting > Advance > [ ] automaticly check configuration.
(ref: https://github.com/docker/for-mac/issues/6898)
##### solution 2
```
sudo ln -sf /Applications/Docker.app/Contents/Resources/bin/docker-credential-ecr-login /usr/local/bin/docker-credential-ecr-login
```
### 6.2. Download SQL Server Image on Docker
```
sudo docker pull mcr.microsoft.com/mssql/server:2022-latest
```
(ref: https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker)
### 6.3. Install SQL Server Image on Docker
```
sudo docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<your_password_here>" \
   -p 1433:1433 --name sql1 --hostname sql1 \
   -d \
   mcr.microsoft.com/mssql/server:2022-latest
```
(ref: https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker)
### 6.4. Connect to SQL Server
Azure Data Studio > Create a Connection >
```
  server: localhost
  authentication type: SQL login
  user name: SA
  password: <your_password_here>
```

### 7. dotnet ef tool
```
dotnet tool install --global dotnet-ef
```
```
dotnet ef
                     _/\__       
               ---==/    \\      
         ___  ___   |.    \|\    
        | __|| __|  |  )   \\\   
        | _| | _|   \_/ |  //|\\ 
        |___||_|       /   \\\/\\

Entity Framework Core .NET Command-line Tools 8.0.4
```
(ref: https://learn.microsoft.com/en-us/ef/core/cli/dotnet)
