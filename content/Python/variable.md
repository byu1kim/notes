+++
title = "Variable"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Variable

Variable is a type of virtual box, created in the memory of the computer, which we can keep a value. A variable has `name`, `data type`, `value`.

Variables are volatile, they exist in the memory just while the program is running. they are erased when the program ends.

Python read codes from top to bottom. If variable has changed at the end, it will take the latest value.

#### Constant

Variable that shouldn't be changed.

```python
CONS_TANT = 'a'
```

#### Name convention

Name cannot starts with a number, cannot use reserved python words.

- Variables : snake_case
- Constants : UPPER_UPPER
- Classes : UpperCamelCase

{{< vbox blue >}}
<b>Reserved Python keywords</b><br>

and, del, from, None, True, as, elif, global, nonlocal, try, assert, else, if, not, while, break, except, import, or, with, class, False, in , pass, yield, continue, finally, is, raise, async, def, for, lambda, return, await
{{< /vbox >}}

#### Data types

```python
# String
my_name = "text"

# Number
my_num = 1

# Boolean
my_bool = True or False
```

#### Data Conversion

```python
float(x)
int(a)
str(z)
type(a)
```

#### Comments

```python
# Single line
'''
DocString : multiple lines
'''
```

#### Variable Scope : Global

`global` keyword must be used to change the global variable inside of a function

```python
name = 'aa'

def function():
    global name = 'bb'
```

## Data formats

#### CSV : Comma Separated Values

"a, b, c, d"

#### JSON : Javascript Object Notation

[{"a":"a", "b":"b"}]
