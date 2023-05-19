+++
title = "String"
weight = 5
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## String Manipulation

> #### Escape string

|              |      |
| ------------ | ---- |
| Double Quote | \\"  |
| New Line     | \n   |
| Back Slash   | \\\  |
| Tab          | \t   |

> #### Verbatim

Verbatim "@" allows you to write a string with backslash and newline without using escape characters

```c#
var message = @"Hi,
Hello"
```

> #### Format

```c#
var name = "aaa";
var message = String.Format("{0} {1}", name, "hello")
```

`F` Floating-Point, `D` Decimal, `G` General, `N` Number, `E` Exponential, `C` Currency

```c#
$"{val:F}"
```

> #### Interpolation

```c#
var a = $"I have {var}";
```

## String Methods

> #### Get substring of string

`text.Substring(start, characters)`

```c#
var text = "Hello World";
var sub_text = text.Substring(6,5); // World
```

> #### ToUpper()

`text.ToUpper()`

> #### ToLower()

`text.ToLower()`

> #### Trim()

`text.Trim()`

> #### Split()

`text.Split(',')`

```c#
var csv = "a,b,c";
string[] names = csv.Split(','); // array of 3 items
```

> #### Join()

`string.Join(",", arrayName)`

```c#
string aa = string.Join(",", names);
```

> #### Contains()

`text.Contains("~~")`

```c#
text.Contains("Hello") // true or false
```

> #### Replace()

```c#
text.Replace("World", "Hi");
```

> #### Get Index of the first occurrence

`text.IndexOf("string or char")`

```c#
text.IndexOf("a") // index number
```

> #### Remove last item from string

`string.Remove(string.Length-1, 1)`

> #### Last item from list

```c#
list.LastOrDefault();
```

## DateTime

```c#
DateTime now = DateTime.Now;

now.DayOfWeek;
now.DayOfYear;
now.AddYears(1000);
now.AddMonths(12);
now.AddDays(5);
now.AddHours(48);
now.AddSeconds(11);
```
