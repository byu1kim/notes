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

ex) when you change the default value, you don't need to do a migration ( do not migrate often)

Make sure everything is correct before migrating and updating database!

#### Setting default value in DB first approach

```c#
  public ExternalOrderType ExternalOrderType { get; set; } = ExternalOrderType.Food;
```

## Input Type = time

https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/time#time_value_format

## COMPONENT

Build as component as possible...
when you call multiple component is faster than calling multiple controllers (if there are multiple controllers in one page, controllers will be called one by one..-lazy loading). When you use component, you can use async/await and call multiple components at the same time. (reduing the loading time)

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
