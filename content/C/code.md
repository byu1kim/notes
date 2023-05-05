+++
title = "Code"
weight = 2
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

####

```c#
Console.ReadLine();
Console.WriteLine("Hi");
Console.Write("Hi ");
```

#### Name Convention

Namespace, class, variable names : nouns
Method names : verb

PascalCase : Namespace, Class, Property, Method, Enumeration Type, Enumeration Value, Identifier
Local Variable, Parameter : camelCase
Constant : UPPER_CASE

#### Namespace

Namespaces are used to organize classes into logical groups. They are also referred to as libraries. Referencing the namespace with a `using` statement. It allows you to access its classes within the file.

```c#
using Name;

namespace ConsoleApp1 {
    ...
}
```

### C# Last item from list

```c#
list.LastOrDefault();
```

### for

```c#
for (int i = 0; i < 5; i++){
    Console.WriteLine("Hi");
}
```
