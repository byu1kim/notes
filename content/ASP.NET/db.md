+++
title = "DB Connection"
weight = 2
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## DB First

MVC Entity framwork has NO connection string in appsettings.json

Install Packages

- EntityFrameworkCore.Tools
- EntityFrameworkCore.SqlServer
- EntityFrameworkCore

#### Connect to DB (Initial Connection)

Run the command line

```
Scaffold-DbContext "Server=[serverName];Database=[dbName];Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

- OutputDir Models : Create class inside `Models` folder

#### Update DB

```
Scaffold-DbContext "Server=[serverName];Database=[dbName];Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -force
```

Same as connection, but add `-force` at the end.

#### Database Context

nameContext.cs file contains information about the `schema` and `database connection`

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
        public virtual TableName? MfgNavigation { get; set; } // One (Parent)

        public virtual ICollection<TableName> PrimaryKey { get; set } // Many
    }
}
```

Navigation properties references neighbouring tables.

ICollection vs IQueryable vs IEnumerable... ㅆㅃ

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

The one you are sending from Controller `View(test)` and from razor `@model` should be the SAME MODEL

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

---

## Code First

#### 1. Change the Connection String (Initial Connection)

[appsettings.json]

```
"ConnectionStrings": {
  "DefaultConnection": "Server=[Server Name];Database=[DB Name];Trusted_Connection=True;MultipleActiveResultSets=true" },
```

Apply the changes

```
Update-Database
```

#### 2. Create Table

Create `class` file inside `Models` folder (Name: Singular, Pascal)

[Models/EntityName.cs]

```c#
public class EntityName
{
    [Key] // for primary key
    [Display (Name = "First Name")]
    [Required]
    public string ColumnName { get; set; } // Singular, Pascal
}
```

#### 3. Import in DbContext

Import inside [Data/ApplicationDbContext.cs]

```c#
public class ApplicationDbContext : IdentityDbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    // Add this line
    public DbSet<EntityName> TableNames { get; set; } // Table name as plural
}
```

#### 4. Apply the updates

Whenever you make changes in DB,

`Make a change > Migration > Update` One at a time

```
add-migration description
update-database
```
