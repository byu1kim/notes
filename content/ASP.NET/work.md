+++
title = "Work"
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

#### Debugging

1. Step Into:

Shortcut: F11 (in Visual Studio)
Action: When you encounter a method call, "Step Into" will take you inside the method being called, allowing you to debug through the code inside that method. It enables you to delve into the details of the called method and understand how it works.

2. Step Over:

Shortcut: F10 (in Visual Studio)
Action: "Step Over" allows you to execute the current line of code without entering any methods that may be called from that line. It lets you bypass the method calls and simply move to the next line of code in the current method.
Step Out:

3. Shortcut: Shift + F11 (in Visual Studio)

Action: When you are inside a method being debugged, "Step Out" allows you to quickly execute the remaining lines of the current method without stepping through each line individually. It helps you to quickly return to the calling method.

#### Add-migration / update database

Add-migration / update-database : only with creating or updating table not seeding data

ex) when you change the default value, you don't need to do a migration ( do not migrate often)

Make sure everything is correct before migrating and updating database!

#### Setting default value in DB first approach

```c#
  public ExternalOrderType ExternalOrderType { get; set; } = ExternalOrderType.Food;
```

### Input Type = time

https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/time#time_value_format
