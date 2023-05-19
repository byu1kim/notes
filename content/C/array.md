+++
title = "Array"
weight = 3
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Enumeration

///

## Array

Array class stores `a fixed length` sequentail collection of elements of the `same type`

> #### Declare

```c#
// 1.
string[] nameOfArray = new string[2];
nameOfArray[0] = "aa";
nameOfArray[1] = "bb";

// 2.
string[] nameOfArray = new string[] { "aa", "bb" }
```

> #### Type of Array

console let user knows the type of array, not the item of array. if you want to print the specific item, refer the index of the item.

```c#
Console.Write(nameOfArray)
```

> #### Length

```c#
nameOfArray.Length
```

> #### Sort

Numerical or Alphabetical order

```c#
Array.Sort(nameOfArray)
```

> #### Multidimensional Arrays

```c#
string[,] nameOfArray = new string[]
{ {"aa", "bb"}, {"aa", "bb"} }

```

> #### Length of Multidimensional Array

```c#
nameOfArray.GetLength(index)
```

---

## List

Simmilar to array but the size can be modified during run-time

> #### Declare, Assign

```c#
List<T> name = new List<T>();

List<T> aa = new List<T>() { ... }
```

T: string, int, obejct, custom class

> #### Custom Class

```c#
class FullName {
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

class program {
    static void Main(){
        List<FullName> fullName = new List<FullName>();
        fullName.Add(new FullName(){FirstName = "aa", LastName="bb"})
    }
}
```

### List Methods

> #### Add()

`list.Add(item)`

> #### Insert()

`list.Insert(index, item)`

Insert item at index

> #### InserRange()

`list.InsertRange(index, Collection)`

Insert collection(IEnumrable) at index

> #### Remove()

Remove item by value

> #### RemoveRange()

Remove range at index

> #### RemoveAt()

Remove item at index

> #### Clear();

Remove all items

---

## Dictionaries

```c#
Dictionary<string, int> pair = new Dictionary<string, int>();
```

> #### ContainsKey(key)

> #### TryGetValue(key, value)
