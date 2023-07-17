+++
title = "Work"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

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
