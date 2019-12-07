# Web API with Dapper and Bcrypt .NET core 3.0

We could quickly create a web api that connects to a SQL database in a few steps on the command line.


Create a folder, change directory in to it 
```
mkdir AppService 
cd AppService 
```

Run the following command 

```
dotnet new webapi
```

This will setup the web api libraries. Now you need to install Dapper. We are also installing Bcrypt here when we build the authentication service.

```
dotnet add package Dapper
dotnet add package BCrypt.Net-Core --version 1.6.0
dotnet add package Newtonsoft.Json
dotnet add package Microsoft.AspNetCore.Mvc.NewtonsoftJson
```

This will add Dapper package to you project and add the necessary references. Now you are ready to code.

### New Data Access Layer

Create a new Folder called DAL and create a new File called `BaseRepository.cs`

```
mkdir DAL
cd DAL
touch BaseRepository.cs
```
Base Repo will be used for by all our Data Access Layer classes

```csharp
using System;
using System.Data;
using System.Data.SqlClient;
using Dapper;

namespace AppService.DAL{
    public class BaseRepository{
        public IDbConnection mDb;

        public BaseRepository(){
            this.mDb = new SqlConnection("<connectionString>");
        }
    }
}
```


Lets create a new `AuthRepository.cs` class

```csharp
using System;
using Dapper;
using System.Linq;

namespace AppService.DAL{
    public class AuthRepository:BaseRepository{

        public AuthRepository():base(){

        }

        public dynamic Authenticate(string username,string password){
            var passwordRow = mDb.QuerySingle<dynamic>("SELECT Password from [User] where username=@Username",new {Username=username});
            if(passwordRow==null){
                return new {status="erorr",message="Authentication Failed"};
            }

            if(BCrypt.Net.BCrypt.Verify(password,passwordRow.Password)){
                var obj = mDb.Query<dynamic>("SELECT UID,FirstName,LastName from [User] where Username=@Username",new {Username=username});
                return new {status="ok",result=obj};
            }
            else{
                return new {status="erorr",message="Authentication Failed"};
            }
        }

        public dynamic AddUser(string firstName,string lastName,string username,string password){
            string hashedPassword = BCrypt.Net.BCrypt.HashPassword(password);
            mDb.Execute("INSERT into [User](FirstName,LastName,Username,Password) VALUES (@FirstName,@LastName,@Username,@Password)",
            new {
                FirstName = firstName,
                LastName = lastName,
                Username = username,
                Password = hashedPassword
            });

            return new {status="ok",result="User Added"};
        }
    }
}

```

### Controllers

Remove any existing Controllers in the Controllers folder and Create a new AuthController

```csharp
 using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using AppService.DAL;

namespace AppService.Controllers
{
    public class AuthController : ControllerBase
    {
        // Login service
        [Route("Authenticate")]
        [HttpPost]

        public dynamic Authenticate([FromBody]dynamic obj){
            AuthRepository repo = new AuthRepository();
            return repo.Authenticate(obj.username.ToString(),obj.password.ToString());
        }

        //Adding Users

        [Route("AddUser")]
        [HttpPost]

        public dynamic AddUser([FromBody]dynamic obj){
            AuthRepository repo = new AuthRepository();
            return repo.AddUser(obj.FirstName.ToString(),obj.LastName.ToString(),obj.username.ToString(),obj.password.ToString());
        }
    }
}

```

### Startup.cs
Since we want to use Newtonsoft.Json with dotnet core 3, make the following chnages in the repo

```csharp
 public void ConfigureServices(IServiceCollection services)
{
        services.AddControllers().AddNewtonsoftJson(options => {
        options.SerializerSettings.ContractResolver = new  Newtonsoft.Json.Serialization.DefaultContractResolver();});
}

```

