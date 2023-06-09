+++
title = "Intro"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Entity Framework

: Automates all database related activities

- Open a connection to the database
- Create a DataSet class to
- Convert data from the DataSet to .NET object

Table = Class

## Database First

#### Create your database

#### Connect the DB and .NET App (Console App)

1. Create a `Console App (.NET Framework)`

2. Tools > NuGEt Package Manager > Manage NuGet Packages for solution : Download `EntityFramework`

3. Right clcik the project node and select : Add > New Item > Show All Templates > ADO .NET Entity Data Model > Add

4. EF Designer from database > New Connection > Microsoft SQL Server > Continue

5. Type your servername where you can get from SQL server, select the db.

6. Check Save connection settings in App.Config as: and **next**
   Name should be dbnameEntities

7. Entity Framework Version : 6.x

8. Selet Tables (or procedure) -> **Finish**

#### DbContext

- Class
- The link between the db and the c#
- It contains database connection and schema

```c#
static void Main(){
    FoodStoreEntities db = new FoodStoreEntities();

    DbSet<Product> products = db.Products;

    foreach (Product products in products){
        decimal price = Convert.ToDecimal(product.price);
        Console.WriteLine(product.name + " " + price.ToString("C"));
    }

    Console.ReadLine();
}
```

# MVC Day 1

## MVC

#### Create an app

1. Create a new project

2. ASP.NET Core Web App (Model-View-Controller)

3. Check Place solution and project in the same directory

4. Framework : .NET 6.0

5. Authentication type : None, Configure for HTTPS -> **create**

#### Program.cs

```c#
// Define Entry Point
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

// Run the app
app.Run();
```

#### Layouts

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

> **Basic structure of Layout in MVC**

```
<!DOCTYPE html>
<html lang="en">
<head>
</head>
<body>
 <!-- Current View -->
 @RenderBody()

 <!-- Reference jQuery scripts that are included with the web template -->
 @RenderSection("Scripts", required: false)
</body>
</html>
```

#### Entity Framework (DB First)

**1. Create DB**

**2. Install the right programs**

- EntityFrameworkCore.Tools
- EntityFrameworkCore.SqlServer
- EntityFrameworkCore

Command Line Download (Tools > Nuget Package Manager > Package Manager Console )

```
Install-Package Microsoft.EntityFrameworkCore.Tools
Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore
```

Install the right version (Match with Framework Core version)

**3. Connect to the DB**

Run the command line

```
Scaffold-DbContext "Server=localhost\SQLEXPRESS;Database=[dbName];Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

- OutputDir Models : Create class inside `Models` folder

Once you run the command, it will be connected with DB, creating all the tables as class.

#### Database Context

nameContext.cs file continas information about the `schema` and `database connection`

This inhertis from .Net's DbContext class that has the database connection and contians information about the overall database schema.

nameContext class is referenced whenever a query is made to the database. This is done through the object Relational Mapping mechanism (ORM)

#### Entity Classes

Tables in the DB become `Entity Classes`

Example

```c#
namespace WebApplication1.Models
{
    public partial class Test1
    {
        // Constructor can exist.

        // Columns
        public string? Firstname { get; set; }
        public string? LastName { get; set; }

        // Navigations properties : Relations
        public virtual TableName? MfgNavigation { get; set; }

        public virtual ICollection<TableName> PrimaryKey { get; set }
    }
}
```

Navigation properties ?

It references neighbouring tables.

Still don't get it. Refer MVC Day1 for more information about navigation property. (Or create tables with relationships and see what's going to happen)

#### Linq

LINQ (Language-Integrated Query)

#### Controller

```c#
public IActionResult Index()
    {

        testContext context = new testContext();

        // This is where you query your linq
        IQueryable<Test1> test = from p in context.Test1s select p;

        return View(test);
    }
```

The one you are sending from Controller `View(test) `
and from razor `@model` should be the SAME MODEL!!!

#### Razor

```
@model IEnumerable<WebApplication1.Models.Test1>

<div>
    @Html.DisplayNameFor(model=>model.Firstname)
</div>
<div>
@foreach(var item in Model){
    @Html.DisplayFor(modelItem => item.Firstname)
}
</div>
```

## CRUD

#### List

#### Create

1. Get

Controller

```c#
public IActionResult Create()
    {
        return View();
    }
```

Razor

Right click on `View` and add Create template

2. Post

Controller

```c#

```

#### Detail

Controller

```c#
 public IActionResult Detail(string? id)
        {

            testContext context = new testContext();
            var test = (from p in context.Test1s where p.Firstname == id select p).FirstOrDefault();

            return View(test);
        }
```

Razor

```
@model WebApplication1.Models.Test1

@{
    ViewData["Title"] = "Detail";
}

<div>
    @Model.Firstname @Model.LastName
</div>
```

#### Edit

#### Delete

Controller

```c#
public IActionResult Delete(string? id)
    {
        testContext db = new testContext();
        var test = (from p in db.Test1s where p.Firstname == id select p).FirstOrDefault();

        db.Remove(test);
        db.SaveChanges();

        return RedirectToAction(nameof(Index));
    }
```

```razor
 <a asp-action="Delete" asp-route-id="@item.Firstname">Delete</a>
```

#### Update DB

```
Scaffold-DbContext "Server=localhost\SQLEXPRESS;Database=test;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -force
```

Same as connection, but add `-force` at the end.

---

- `Interface` :

- `IActionResult` : An interface that represents the result of an action method in an MVC. Actually **return type** for action methods

- `action method` is a public method within a controller class that is responsible for handling a specific HTTP request and producing a response.

- IQueryable ?

IQueryable is an interface in the .NET framework that represents a queryable data source. It is primarily used in LINQ (Language-Integrated Query) to perform query operations on data.

IQueryable extends the IEnumerable interface and provides additional functionality for querying and manipulating data before executing the query.

The key advantage of using IQueryable is that it provides deferred execution. This means that the query is not executed immediately when it is defined. Instead, the query is translated into an appropriate query language (such as SQL for a database) and executed when the data is needed, such as when iterating over the query results or explicitly calling a terminal operation like ToList() or FirstOrDefault().

var is used for type inference when declaring variables, allowing the compiler to determine the type based on the assigned value. IQueryable, on the other hand, is an interface used for building and executing queries against queryable data sources, primarily in conjunction with LINQ.

- IEnumerable?
- @Model ?

- how to refresh after db updates? (like new table added...)

# Work

You need to import right model to use asp-for and htmlhiddenfor.

`asp-for` should be inside the form

search this in the project and figure out what it's doing

```c#
@HtmlhiddenFor
```
