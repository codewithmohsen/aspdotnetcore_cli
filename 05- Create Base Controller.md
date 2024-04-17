# Create Base API Controller
```
dotnet new apicontroller --output Presentation/Controllers --actions false --name BaseController
```s
now we have
```
using Microsoft.AspNetCore.Mvc;
namespace MyApp.Namespace
{
    [Route("api/[controller]")]
    [ApiController]
    public class BaseController : ControllerBase
    {
    }
}
```
