+++
title = "Function"
weight = 2
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

A function is a group of related statements performing an action in the system. It should `perform a single task` and has input parameters and returns.

Nickname(?) : Procedure, sub-routine, method

#### Name convention

Starts with a verb, is descriptive, **lower_snake_case**.

#### Function Definition

The indentation defines the code block (2-4 spaces/tab). Use DocString for the details.

```python
def name(x, y):
    '''
    :param x: first num
    :param y: second num
    :return the sum of the passed numbers
    '''
    a = x + y
    return a
```

{{% notice note %}}
return multiple values from function : use Tuple ()
{{% /notice %}}

#### Function Stub

pass keyword does nothing. It is sued as a placeholder.

```python
def some_function():
    # function that does nothing yet
    pass
```

#### Calling a Function

```python
name(1, 2)
```

#### Void Functions

is a function that returns nothing

#### Parameter (Argument)

Parameters can be anything : string, number, list or function

```python
def name(parameter);
  print("user", parameter)
```

Default parameter

```python
def name(aa="am")
    print(aa)
```

Optional parameter

```python
def name(aa=None)
    print(aa)
```

## Main function

Python convention is to define a main function.

```python
import sys
import os

def main():
    pass

if __name__ == "__main__":
    main()
```

---

## Built-in Functions

print()

input() : return string type

input returns whatever user inputs..
it automatically become string type

type()

id()
