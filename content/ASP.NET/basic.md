+++
title = "MVC Basics"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## MVC

- Model : Database Logic
- View : Razor markup (HTML embedded C# code)
- Controller : User interacts

{{< vbox blue >}}
Why MVC? For the rapid application development and maintenance
{{< /vbox >}}

#### DbContext

- Class
- The link between the db and the c#
- It contains database connection and schema

---

## Entity Framwork

Automate database related activities with .NET

1. Open a connection to the db
2. Create dataset
3. Convert data from the dataset (ApplicationDbContext)

#### Create Entity Framework

1. Create ASP .Net Core Web App (Model-View-Controller)
2. Check `Place solution and project in the same directory`
3. Framework: `.NET 6.0 (Lng-term support)`, Authentication type: `None`
4. Check `Check Configure for HTTPS`

#### Entry Point

program.cs & appsettings.json

```c#
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

- MapControllerRoute() : defines where the processing flow will go first.
- app.Run() : Application start.

#### Layout

Shared templates are in `Views>Shared` folder.

```
<main role="main" class="pb-3">
   @RenderBody()
</main>
...
@await RenderSectionAsync("Scripts", required: false)
```

- @RenderBody() : It is a placed where the current view is swapped int.
- @RenderSection() : JavaScript files that get pulled into this View.
- `@` symbol indicates to the compiler that C# code is to follow.

#### Connect with DB

Connect SQL Sever, Create DB

Run script in Package Manager Console. Install the right version (Match with Framework Core version)

```
Install-Package Microsoft.EntityFrameworkCore.Tools
Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore
```

Then, Run script below (Initial Connection to DB)

```
Scaffold-DbContext "Server=[SERVERNAME];Database=[DBNAME];Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

---

## Identity Framework

Manages authenticated users in your web application

#### Create Identity Framework

1. Create **ASP .Net Core Web App (Model-View-Controller)**
2. Check `Place solution and project in the same directory`
3. Framework: `.NET 6.0 (Lng-term support)`
4. Authentication type: `Individual Account`
5. Check `Check Configure for HTTPS`

> ##### Required Packeges (Match the version with framwork version)

Entity Framework packages will be installed automatically.

- Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
- Microsoft.AspNetCore.Identity.EntityFrameworkCore
- Microsoft.AspNetCore.Identity.UI
- Microsoft.EntityFrameworkCore
- Microsoft.EntityFrameworkCore.SqlServer
- Microsoft.EntityFrameworkCore.Tools

#### Visible the Identity Files

Pages relateted to Identity are not visible in the project. To make it visible,

`Right click Areas > Add > New Sacffold Item > Identity > Override all files`

And then, select the `ApplicationDbContext` file and add

{{% notice warning %}}
If you don't match the framework version and the packages, it causes the Scaffolding error.
{{% /notice %}}

#### Edit Identity Files

Create an additional tables in the same database for Identity Information such as `RegisteredUser`. Modifying ASP Tables directly is not recommended. Create a class for RegisteredUser(Code First) and then come back here.

To enable access to the code first data, the context should be accessed through dependency injection through the controller's constructor.

> ##### 1. Import DB inside identity.cshtml.cs file

```c#
private readonly ApplicationDbContext _context;

public RegisterModel(..., ApplicationDbContext context)
{
    ...;
    _context= context;
}

```

> ##### 2. Save to DB

After `if (result.Succeeded)`

```c#
MyRegisteredUser registerUser = new MyRegisteredUser()
{
    Email = Input.Email,
    FirstName = Input.FirstName
};

_context.MyRegisteredUsers.Add(registerUser);
_context.SaveChanges();
```

> ##### 3. Accessing Secured Areas

In Identity framework, you can use `[Authorize]` annotation. Also, user name can be accessed to `User.Identity.Name`

**[HomeController.cs]**

```c#
// Dependency Injection
private readonly ApplicationDbContext _context; // Import DB

// Add to Controller constructor
public HomeController(ILogger<HomeController> logger, ApplicationDbContext context)
{
    _logger  = logger;
    _context = context;
}

// Only logged in user can access to this controller
[Authorize]
public IActionResult SecureArea() {
    string userName = User.Identity.Name; // User Info

    var registeredUser = _context.MyRegisteredUsers
                                 .Where(ru => ru.Email == userName)
                                 .FirstOrDefault(); // DB logic

    return View(registeredUser); // send the registeredUser model to the view
}
```

**[View]**

```
@model MvcWebAppDay3Exe.Models.MyRegisteredUser // import model
or
@using AppName.ViewModels
@model IEnumerable<practiceVM> // passed from controller

@{
    ViewData["Title"] = "SecureArea";
}

<h1>SecureArea</h1>

<div>
    <h4>MyRegisteredUser</h4>
    <hr />
    <dl class="row">
        <dt class = "col-sm-2">
            @Html.DisplayNameFor(model => model.Email)
        </dt>
        <dd class = "col-sm-10">
            @Html.DisplayFor(model => model.Email)
        </dd>
        <dt class = "col-sm-2">
            @Html.DisplayNameFor(model => model.FirstName)
        </dt>
        <dd class = "col-sm-10">
            @Html.DisplayFor(model => model.FirstName)
        </dd>
    </dl>
</div>
<div>
    <a asp-action="Edit" asp-route-id="@Model?.Email">Edit</a> |
    <a asp-action="Index">Back to List</a>
</div>
```
