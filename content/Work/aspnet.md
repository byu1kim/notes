+++
title = "ASP .NET"
weight = 5
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

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

# Work

You need to import right model to use asp-for and htmlhiddenfor.

`asp-for` should be inside the form

search this in the project and figure out what it's doing

```c#
@HtmlhiddenFor
```

## Project Sturcture

- Data Folder : Entities (Table Classes)
- Services Folder : DB Logics (Like repositories?)

## Code First DB

#### Add Column

Go to class (inside data > fine the right table) and add

```c#
public Type Name { get; set;}
```

Type? : Nullable
Type : Not Nullable

#### Create Enum Type

Enum files inside data folder

```c#
public enum ExternalOrderType
    {
        Food = 1,
        Retail = 2
    }
```

## Debugging

1. Step Into:

- Shortcut: `F11` (in Visual Studio)
- Action: When you encounter a method call, "Step Into" will take you inside the method being called, allowing you to debug through the code inside that method. It enables you to delve into the details of the called method and understand how it works.

2. Step Over:

- Shortcut: `F10` (in Visual Studio)
- Action: "Step Over" allows you to execute the current line of code without entering any methods that may be called from that line. It lets you bypass the method calls and simply move to the next line of code in the current method.

3. Step Out:

- Shortcut: `Shift + F11` (in Visual Studio)
- Action: When you are inside a method being debugged, "Step Out" allows you to quickly execute the remaining lines of the current method without stepping through each line individually. It helps you to quickly return to the calling method.

## Add-migration / update database

Add-migration / update-database : only with creating or updating table not seeding data

ex) when you change the default value, you don't need to do a migration (do not migrate often)

Make sure everything is correct before migrating and updating database!

#### Setting default value in DB first approach

```c#
  public ExternalOrderType ExternalOrderType { get; set; } = ExternalOrderType.Food;
```

## Input Type = time

https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/time#time_value_format

## COMPONENT

Build as component as possible...
when you call multiple component is faster than calling multiple controllers (if there are multiple controllers in one page, controllers will be called one by one..(lazy loading). When you use component, you can use async/await and call multiple components at the same time. (reduing the loading time)

## NOTE

all database records are assumed stored in UTC format.

## StringBuilder

- string : static value
- StringBuilder : variable value (when value changes often)

```c#
StringBuilder sb = new StringBuilder("ABC", 50);

// Append three characters (D, E, and F) to the end of the StringBuilder.
sb.Append(new char[] { 'D', 'E', 'F' });

// Append a format string to the end of the StringBuilder.
sb.AppendFormat("GHI{0}{1}", 'J', 'k');

// Display the number of characters in the StringBuilder and its string.
Console.WriteLine("{0} chars: {1}", sb.Length, sb.ToString());

// Insert a string at the beginning of the StringBuilder.
sb.Insert(0, "Alphabet: ");

// Replace all lowercase k's with uppercase K's.
sb.Replace('k', 'K');

// Display the number of characters in the StringBuilder and its string.
Console.WriteLine("{0} chars: {1}", sb.Length, sb.ToString());

```

## Status

EntityStatus inside BaseEntity

## DB Comments

The purpose of [Column(“CartTypeID”)] is so that the column will be named CartTypeID (it gets saved as an INT) in the database instead of CartType.

= OrderType.Restaurant; just sets the default property as Restaurant type. This is the easiest “catch-all” way to handle enums. The underlying type of an enum is INT, and INT defaults with value of 0, which is not a valid enum value (you’ve defined 1 and 2 only)

## Update DB

Custom Class: ServiceResult
Use this class when you update DB or create DB or delete DB, it returns the result of the DB.

#### Save multiple columns in the DB

```c#
this.Context.Table.AddRange(List)
```

### update db query example

```c#
public ServiceResult<List<CartItem>> UpdateCartItems(List<CartItem> cartItems)
{
    var res = new ServiceResult<List<CartItem>>();

    this.Context.CartItems.AddRange(cartItems);

    try
    {
        this.Context.SaveChanges();
    }
    catch (DbUpdateException e)
    {
        Console.WriteLine("Exception Message:" + e.Message);
        res.AddError("Error", e.InnerException.ToString());

        res.Success = false;
        return res;
    }

    res.Object = cartItems;
    res.Success = true;

    return res;
}
```

```c#
[AllowAnonymous] // Can access outside of the main browser(web?)
[HttpGet] // Add this annotation, like Put, Delete, Post
```

## DB

sysError : usually system error
80% related DB

## round number

```c#
Math.Round(originalNumber, 2);
```

## DB

```c#
private IQueryable<CartItem> GetAllCartItems(params Func<IQueryable<CartItem>, IIncludableQueryable<CartItem, object>>[] includes)
{
    var res = this.Context.CartItems.Includes(includes);
    if (ClientModel.ClientID.HasValue)
    {
        res = res.Where(x => x.ClientID == ClientModel.ClientID.Value);
    }
    return res;
}
```

Params : Query
ex) GetByMultipleIds(cartItems.Select(x => x.ID, cartItems.Where(x=>x=1)).ToArray()).ToList();

- Never! use <img> tag. use span and its background in css

what is includes T_T

## When create db fucntions

1. create getAll : this filters records that is already deleted or should be filtered
2. what you have to filter ? : if clientId exists

- DB call : make it less (as least as you can)

## How to create service

1. add it to startupd
2. create iservice interface
3. create class
4. import the iservice in controller

## Controller Structure

```c#
[HttpPut]
[Route("fullfilled-status")]
public dynamic ([FromBody] SalesOrderFullfilledApiModel model) // if you have one property, don't need to use FromBody. usually Frombody used with Post request (get request is used with params in url)
{
    string errorMessage;

    if (!ModelState.IsValid)
    {
        errorMessage = "Model is invalid";
        goto Error;
    }

    var res = SalesOrderService.UpdateFullfilledStatus(model.SalesOrderId);

    if (!res.Success)
    {
        errorMessage = res.Errors.ToString();
        goto Error;
    }
    return CustomJson(res.Object.Status, null, null, AlertType.Success);

Error:
    return CustomJson(null, null, new List<string> { errorMessage }, AlertType.Error);
}

```

- only return necessary data. if you return entire object which includes uncessary data, it is exposing too much data to the browser

## DB (Service) Structure

```c#
public ServiceResult<SalesOrder> UpdateFullfilledStatus(int id)
{
    var res = new ServiceResult<SalesOrder>();
    var dbSalesOrder = Get(id);
    if (dbSalesOrder == null)
    {
        goto Error;
    }

    dbSalesOrder.Status = SalesOrderStatusType.FoodFullfilled;

    try
    {
        Context.SaveChanges();
    }
    catch (DbUpdateException e)
    {
        res.AddError("Error", e.InnerException.ToString());
        goto Error;
    }

    res.Success = true;
    res.Object = dbSalesOrder;

    return res;

Error:
    res.Success = false;
    return res;
}
```

## Javascript Debouncer?

Prevent multi submit (even though user clicks the button multiple times, only a few request will be submited.)

## Auto Mapper

do not assign the value to entity. for example inside detailpage.
Use Auto Mapper

```c#
this.Mapper.
```

## Form

```
<form asp-action="test">
<input asp-for="name"/>
</form>
```

asp-for="name" goes to the form

## Error

An unhandled exception occurred while processing the request.
InvalidOperationException: A circular dependency was detected for the service of type 'Services.ISalesOrderService'.
Services.IReportService(Services.ReportService) -> Services.ISalesOrderService(Services.SalesOrderService) -> Services.ICartService(Services.CartService) -> Services.ISalesOrderService

So when compiling your first project, the compiler will run into CustomClass1 definition, it knows it lays into Project2.dll and therefore will compile Project2 before, in order to be able to add that reference in your first project.

That's what a dependency is, it's hierarchical, there must be a starting point.

## Code order (refactor)

- Db structure
- Service layer
- Controller
- View
- JS

## TDD : Test Driven Development

- Tdd file refers test file
- Test Service layer and Api at the same time!

## Unit Test

xUnit (Test Libraray) is already included in the heymate project

Create tests for Data, Test, and Controller (at each folder)

#### Test for Files in Servies

1. Create a method inside test class

One test method for one method in services/data class

2. Structure

   - Arrange : Setup data for executing, use memory database whids is temporary
   - Act : Calling the method
   - Assert : Check the result

3. Execute test : Test > Test Explorer > Find your test file > Right click > Run

```c#
[Fact]
public void CreateUpdate_ValidNewEntity_ShouldCreateNewCart() // Name should be unique
{
    // Arrange
    var databaseName = "CartServiceTestCreateUpdateValidNewEntityShouldCreateNewCart";
    var context = DbUtility.GetContext(databaseName); // memory database

    var client = new Client
    {
        Name = "Owner",
        TimeZone = "Pacific Standard Time"
    };

    context.Add(client);

    var store = new Store
    {
        Name = "Bubble Waffle"
    };

    context.Add(store);

    var table = new RestaurantTable
    {
        MaximumQuantity = 1,
        Store = store,
        Client = client,
        Status = SynicTools.EntityStatus.Active
    };

    context.Add(table);
    context.SaveChanges();

    var voucherService = new VoucherService(DbUtility.GetContext(databaseName), new Mock<IStoreModel>().Object);

    var service = new RestaurantCartService(DbUtility.GetContext(databaseName),
        new Mock<ISalesOrderService>().Object,
        new Mock<IUserService>().Object,
        new Mock<IStoreModel>().Object,
        new Mock<IStoreService>().Object,
        new Mock<IProductComboService>().Object,
        new Mock<IOptionItemService>().Object,
        voucherService);


    // Act
    var result = service.CreateUpdate(new RestaurantCart
    {
        CustomerPhoneNumber = "123",
        Status = SynicTools.EntityStatus.Active,
        StoreID = store.ID, // no foriegn key in the test, so you have to write down eveyrthing
        Guid = Guid.NewGuid(),
        RestaurantTableID = table.ID,
        InternalType = SalesOrderTypeInternal.TableOrder
    });

    // Assert
    result.Should().NotBeNull();
    result.Success.Should().BeTrue();
    result.Object.CustomerPhoneNumber.Should().Be("123");
    result.Object.Status.Should().Be(SynicTools.EntityStatus.Active);
    result.Object.StoreID.Should().Be(store.ID);
    result.Object.RestaurantTableID.Should().Be(table.ID);
    result.Object.InternalType.Should().Be(SalesOrderTypeInternal.TableOrder);
}

```

## ?

```c#

        // 2. product with option (reuse code.....)
        public ServiceResult<List<RestaurantProductOption>> CreateUpdateProductOption(RestaurantProduct entity)
        {
            if (entity.IsCreate)
                return this._CreateProductOptions(entity.ProductOptions);
            return this._UpdateProductOption(entity.ProductOptions);
        }

        private ServiceResult<List<RestaurantProductOption>> _CreateProductOptions(List<RestaurantProductOption> entity)
        {
            var res = new ServiceResult<List<RestaurantProductOption>>();

            // 1) create product option
            this.Context.RestaurantProductOptions.AddRange(entity);

            // 2) create product option items
            foreach (var option in entity)
            {
                this.Context.RestaurantOptionItems.AddRange(option.OptionItems);
            }

            // 3) save db
            try
            {
                Context.SaveChanges();
                res.Success = true;
                res.Object = entity;
            }
            catch (DbUpdateException e)
            {
                res.AddError("Error", e.InnerException.ToString());
                res.Success = false;
            }

            return res;
        }

        private ServiceResult<List<RestaurantProductOption>> _UpdateProductOption(List<RestaurantProductOption> entity)
        {
            var res = new ServiceResult<List<RestaurantProductOption>>();
            return res;
        }


```

## Create a subPage (side menu)

to create subpage, you have to update MenuSubItemMappingHelper class file first (and create new enum too!)

1. [2:10 PM] Create new value on SubPageType enum
2. Update MenuSubItemMappingHelper class file (not MenuItemMapping)
3. Add new row in SubPages table in db (mapping menu - submenu)
4. Sign-out and Sign-in account again

## Sorting Table

#### HTML

```html
   <th class="sort-column" onclick="setSort('Customer', '@((Int16)Model.SortDirection)', @((Int16)OrderByDirection.Asc), this)">
                        <div class="d-flex fw-bold">
                            Customer
                            <div class="sort-icon-wrapper @(SortIconHelper.GetSortIconStatusClass(Model.SortColumn, "Customer", Model.SortDirection))">
                                <i class="hm-sort-icon sort-icon"></i>
                            </div>
                        </div>
                    </th>
```

The sort column will go to SortColumn and SortDirection due to setSort function in javascript

#### Controller

```c#
model.Items = ...Get()....OrderByProperty(model.SortColumn, model.SortDirection)
```

#### Model

set a default value

```c#
   public class SalesTableFilterViewModel
    {
...
        public SalesTableFilterViewModel()
        {
            base.SortColumn = nameof(RestaurantSalesOrder.Placed);
            //base.SortDirection = Helpers.OrderByDirection.Decending; // find helpers!!
        }
    }

```

## Problem Solving

Try to see the details when debuging !
What function, What arguments, Api calls,.....
Observe what is happening in both server side and client side (browser) and then debug

## Auto mapper

auto mapper => define in web profile and then use

## Service

BaseCoreService is where basic CRUD services are

## Abstract vs Virtual

In ASP.NET and C#, the terms "abstract" and "virtual" are related to object-oriented programming concepts, specifically in the context of classes and methods.

Abstract (Keyword):

In C#, the abstract keyword is used to define abstract classes or abstract methods.
An abstract class cannot be instantiated on its own; it serves as a blueprint for other classes.
An abstract method is a method declared in an abstract class that has no implementation in the abstract class. The derived classes must provide an implementation for abstract methods.
Example of an abstract class with an abstract method:

csharp
Copy code
public abstract class Shape
{
public abstract void Draw();
}
Virtual (Keyword):

The virtual keyword is used to declare methods, properties, or indexers that can be overridden by derived classes.
When a method is marked as virtual, it means that the method can be overridden in a derived class using the override keyword.
Example of a virtual method:

csharp
Copy code
public class BaseClass
{
public virtual void SomeMethod()
{
Console.WriteLine("BaseClass.SomeMethod");
}
}

public class DerivedClass : BaseClass
{
public override void SomeMethod()
{
Console.WriteLine("DerivedClass.SomeMethod");
}
}
In this example, the SomeMethod in BaseClass is marked as virtual, and the DerivedClass provides its implementation using the override keyword.

In the context of ASP.NET, these concepts are often used when creating and extending classes, especially in scenarios like creating custom controllers, models, or other components. Abstract classes and virtual methods provide a way to define a common structure and allow for customization in derived classes.

In summary, while both abstract and virtual methods provide ways to allow derived classes to provide their own implementations, the key difference is that abstract methods have no implementation in the base class, and derived classes must provide their own, while virtual methods have a default implementation in the base class that can be optionally overridden in derived classes. Abstract classes can have both abstract and virtual members.
