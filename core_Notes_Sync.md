# ASP.NET Core 2.x
1. Microsoft.NetCore.App
   1. The DotNet core Framework
      1. All Standard libraries
         1. Collection
         2. Xml
         3. Security
2. Microsoft.AspNetCore.All
   1. ASP.NET Core Framework
      1. Security Packages
      2. Razor Packages
      3. Hosting
      4. Http
      5. Identity
      6. Localization
      7. SpService
      8. SignalR
3. There is no web.config file, all configurations settings are added in appsettings.json file
   1. ConnectionStrings
   2. Logging
   3. Any other settings
   4. This file will be loaded in application using IConfiguration interface 
4. Program.cs
   1. Contains Main Method for Creating an instance of WebHost
      1. Build all required dependencies for Hosting Current application.
      2. Run the application based on application depednencies and Middleware configuration froom StartUp class
5. Startup.cs
   1. Provides StartUp class for
      1. IConfiguration Injected as constrctor injection for Application Configuration
      2. ConfigureSerivices() method
         1. Provide Ddependency Injection Container (DI) for Registering application level dependencies like
            1. Identity
            2. Cookies
            3. DbContext
            4. Other Custom Services
            5. MVC
         2. For all DI register services we need to set life time
            1. ServiceLifeTime Enum
               1. Singleton, the Global Singleton Object 
               2. Scoped, the statefull for teh active session
               3. Transient, the stateless
            2. The Lifetime DI Methods
               1. services.AddSingleton<T,U>()
               2. services.AddScoped<T,U>()
               3. services.AddTransient<T,U>()
            3. T is the interfeace and U is class implemening T interface
            4. 
      3. Configure() method for managing Middlewares
         1. Database Error Page
         2. Application Errors
         3. Static Files
         4. Authentication
         5. Custom Middlweares
         6. MVC with Routing     
#=============================================================================
# Entity Framework Core aka EF Core
1. Code-First Approach
   1. Create Model classes and map then with Database using Migrations
2. Packages for EF Core
   1. Microsoft.EntityFrameworkCore
      1. DbContext
         1. Manages DB Connections
         2. Managed DB Transactions using SaveChanges() and SaveChangesAsync() methods to commit Db Transactions
         3. Manages Mapping of Model class with DB Tables using DbSet<T> class
      2. DbSet<T>
         1. A Cursor that maps Entity Class / Model class of name T with DB table of name T
         2. Provides facility to
            1. Read Data
            2. Add new record also adding multiple records
            3. Update Records
            4. Delete Records
      3. Consider ctx is an instance of DbContext and Emps is DbSet<Emp>
         1. To Read all data from Emp Table
            1. var emps = ctx.Emps.ToListAsync(); 
               1. (Note: Microsoft.EntotyFrameworkCore is needed for ToListAsync())
         2. To Read a single records based in Primary Key (P.K.)
            1. var emp = ctx.Emps.FindAsync(id);
         3. To Add New Record
            1. Create an enstance of Emp class e.f. emp and set its property values
            2. Add this instance inf Emps Cursor
               1. ctx.Emps.AddAsync(emp);
            3. Commit Transactions
               1. ctx.SaveChangesAsync();
         4. To Update record
            1. Search record based on P.K.
            2. Modify its property values
            3. Commit Transaction
         5. To Delete Record
            1. Search record based on P.K.
            2. Call Remove method on Cursor and pass searched record
               1. ctx.Emps.Remove(/*Searched Record*/);
            3. Commit Transaction
   2. Microsoft.EntityFrameworkCore.SqlServer
   3. Microsoft.EntityFrameworkCore.Relational
   4. Microsoft.EntityFrameworkCore.Tools
3. EF Core Data Migrations using .NET Core CLI
   1. dotnet ef migrations add <MIGRATION-NAME> -c <Full-Qualify-Path-Of-DbContext-Class>
      1. Generate Migration and ShapShot file
         1. Migration class will define the Model-To-Table Mapping for Creating table
         2. Shanshop class will define the Constraints
4. Update / Generate Database from Migrations
   1. dotnet ef database update  -c <Full-Qualify-Path-Of-DbContext-Class>
      1. Load latest migrations and generate database if not exists or else update it.

#=============================================================================

# ASP.NET Core MVC Controllers

1. Base class is "Controller"
2. Controller Contains Action Methods
   1. Each Action Method is 'HttpGet' by default
   2. Explicitely decorate its as HttpPost/HttpPut/HttpDelete
   3. In WEB API it is Mandatory to Decorate each method
3. Each Action Method Returns IActionResult
4. The IActionResult can be
   1. View --> ViewResult
   2. RedirectToAction
   3. ObjectResult --> Used in WEB API
      1. Ok()
      2. NotFound()
      3. Conflict()
      4. BadRequest()
   4. JsonResult()
   5. FileResult()
#============================================================================
# ASP.NET Core MVC Views
1. View templates
   1. List  
      1. Accepts IEnumerable of Model class
   2. Create
      1. Accepts Empty Model class
   3. Edit
      1. Accepts Model class with values to be edited
   4. Delete
      1. Accepts Read-Only Model class with values to be deleted
   5. Details
      1. Accepts Read-Only Model class with values
   6. Empty (Without-Model)
      1. Free-Hand View Creation
2. Tag Helpers
   1. Microsoft.AspNetCore.Mvc.TagHelpers library
   2. HTML attributes with predefined behavior
   3. Prefexed using 'asp-'
      1. asp-controller
         1. Controller in Http Request
      2. asp-action
         1. Action to be requested
      3. asp-items
         1. Accepts collection to generate <select> with <option>
      4. asp-route-all
         1. Route parameters
      5. asp-for
         1. Used for Model Binding means, it will bind the model proerty with HTML input element e.g. text/radio/check, ect.
         2. This is also used for <select> element 