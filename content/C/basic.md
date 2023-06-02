+++
title = "Basic"
weight = 2
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

####

```c#
Console.ReadLine();
Console.WriteLine("Hi");
Console.Write("Hi ");
```

F5 -> Run
Ctrl(Command) + B -> build

## Basic Code Structures

> #### Namespace

Namespaces are used to organize classes into logical groups. They are also referred to as libraries. Referencing the namespace with a `using` statement. It allows you to access its classes within the file.

```c#
using Name;

namespace ConsoleApp1 {
    ...
}
```

> #### Access Modifier

The Access Modifier controls how a class or its components can be used by other code. (internal/static etc..)

> #### Method

Blocks of code

> #### Class

A group of related methods and properties

> #### Name Convention

| Identifier         | Name Convention |
| ------------------ | --------------- |
| Namespace          | PascalCase      |
| Class              | PascalCase      |
| Property           | PascalCase      |
| Method             | PascalCase      |
| Variable/Parameter | camelCase       |
| Parameter          | camelCase       |
| Constant           | UPPER_CASE      |
| Enumeration Type   | PascalCase      |
| Enumeration Value  | PascalCase      |

- Namespace, class, variable names : **nouns**
- Method names : **verb**

## Data Types

```c#
// Numeric
byte num = 255; // 0-255
int min = 21; // -2147483648 - 2147483647
long distance = 3548455; // -9223372036854775808 - 9223372036854775807

// Floating
float price = 20.99f; // 7dp
double interest = 2.625; // 16dp
decimal temp = 21.5m; // 29dp

// Boolean
bool isTrue = true;

// String
char letter = 'a';
string st = "Ceasars";
string user = Console.ReadLine(); // user prompt

DateTime rn = DateTime.Now;
object car = new { make = "Mustang", colour = "blue"}

```

{{% notice note %}}
**var** type has to be declared and assigned at initial
{{% /notice %}}

## Error Handling

```c#
try {
  ///
} catch (Exception e){
  ///
}
```

---

## Casting

Convert into differnt data type

> #### Implicit Conversion

Converting from **smaller** to **larger**, there is no risk of data loss.

`char -> int -> long -> float -> double`

In implicit casting, small into larger is done automatically.

> #### Explicit Conversion

Converting from **larger** to **smaller**

`double -> float -> long -> int -> char`

```c#
int aa = 23;
byte num;
num = (byte)aa
```

> #### String to Int ?

///

---

## Operator

| Type       | Operator             |
| ---------- | -------------------- |
| Arthmetic  | +, -, \*, /, %       |
| Comparison | ==, !=, >, <, >=, <= |
| Logical    | &&, !                |

---

## Conditional

> #### If/Else

```c#
if(){
  ...
} else if () {
  ...
} else {
  ...
}
```

> #### Ternary Operator

```c#
condition ? true statement : false statement
```

> #### Switches

```c#
switch (condition){
  case 1:
     ...
     break;
  case 2:
     ...
     break;
  default:
     break;
}
```

---

## Loops

> #### for

```c#
for (int i = 0; i < 5; i++){
    Console.WriteLine("Hi");
}
```

> #### foreach

```c#
// string[] str
foreach (string item in strs)
{
  //
}
```

> #### while

```c#
while(i<7){
  ...
  i++
}
```

> #### Do-while

```c#
do {
  ...
}
while (i < 6);
```

---

## Log Data

Refer C# Day1

## .NET Memory Management

C# Day3
