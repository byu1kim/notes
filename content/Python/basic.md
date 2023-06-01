+++
title = "Basics"
weight = 2
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

- [Variables](#variables)
- [Operators](#operators)
- [Functions](#functions)
- [Module](#module)
- [Flows : If, For, While](#flow)
- [Errors](#errors)

# Variables

Variable is a type of virtual box, created in the memory of the computer, which we can keep a value. A variable has `name`, `data type`, `value`.

Variables are volatile, they exist in the memory just while the program is running. they are erased when the program ends.

Python read codes from top to bottom. If variable has changed at the end, it will take the latest value.

> #### Constant

Variable that shouldn't be changed.

```python
CONS_TANT = 'a'
```

> #### Name convention

Name cannot starts with a number, cannot use reserved python words.

- Variables : snake_case
- Constants : UPPER_UPPER
- Classes : UpperCamelCase
- Functions : snake_case

{{< vbox blue >}}
<b>Reserved Python keywords</b><br>

and, del, from, None, True, as, elif, global, nonlocal, try, assert, else, if, not, while, break, except, import, or, with, class, False, in , pass, yield, continue, finally, is, raise, async, def, for, lambda, return, await
{{< /vbox >}}

> #### Data types

```python
# String
my_name = "text"

# Number
my_num = 1

# Boolean
my_bool = True or False
```

> #### Data Conversion

```python
float(x)
int(a)
str(z)
type(a)
```

> #### Comments

```python
# Single line
'''
DocString : multiple lines
'''
```

> #### Variable Scope : Global

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

---

# Operators

> #### Arthmetic Operators

`\+`, `\-`, `\*`, `/`, `%`(Modulus), `\*\*`(Power), `//`(Rounding after division)

> #### Assignment Operators

`=`, `+=`, `-=`, `\*=`, `/=`, `%=`, `//=`

> #### Comparison or Relational Operators

`==`, `!=`, `>`, `<`, `>=`, `<=`

> #### Logical Operators

`and`, `or`, `not`

- short-circuit evaluation : a or b 에서 이미 a가 true면 evaluating no need
- use the first expression to guard the second expression

> #### Identity Operators

`is`, `is not`

> #### Membership Operators

`in`, `not in`

> #### Logical Operators

`&`(AND), `|`(OR), `^`(XOR), `~`(NOT)

> #### Operators Precedence

Python evaluates boolean expression from left to right

1. Parentheses : ()
2. Arithmetic : + /
3. Relational operator : > >=
4. not
5. and
6. or

---

# Functions

A function is a group of related statements performing an action in the system. It should `perform a single task` and has input parameters and returns.

Nickname(?) : Procedure, sub-routine, method

> #### Name convention

Starts with a verb, is descriptive, **lower_snake_case**.

> #### Function Definition

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

> #### Function Stub

pass keyword does nothing. It is sued as a placeholder.

```python
def some_function():
    # function that does nothing yet
    pass
```

> #### Calling a Function

```python
name(1, 2)
```

> #### Void Functions

is a function that returns nothing

> #### Parameter (Argument)

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

## Built-in Functions

print()

input() : return string type

input returns whatever user inputs..
it automatically become string type

type()

id()

#### Python Standard Library

- Built in functions in python
- how to use?
- import module

```
from random import randit
randint(1, 30)
```

Install Requests package

```
pip install requests
```

```python
from requests import get

response = get("https://google.ca")
print(response.status_code)

```

---

# Module

## Script

Script is a Python code that is passed to the Python Interpreter to be executed. By convention, the script have a main() function.

## Modules

Module is a collection of functions. Nothing is running unless it is called from script.

> #### Importing Modules

```python
import math
import utilities

a = utilities.sample_function(10, 2)
```

#### Benefits of using modules

- Organizing and grouping the code based on functionality or purpose
- Reuse code
- Breaks donw a large program into smaller pieces
- Can build libraries of modules, use existing built-in libraries or purchase and use spcialized libraries.

## Built-in Modules

#### datetime

```python
import datetime

date = datetime.datetime.now()
day = date.day
week = date.weekday()
mon = date.month
full_mont = date.strftime('%B') # Full month name, %b for an abbreviated one
year = date.year
```

#### random

```python
import random

a = random.random()
b = random.randint(0, 5) # 0~5
```

## Packages

https://pypi.org/

#### Requests

```python
from requests import get
```

---

# Flow

## Boolean Expression

Boolean expressions are evaluated to `True`(0) or `False`(1).

- true and true = true
- false and true = false
- true and false = false
- false and false = false
- true or true == true
- true or false = true
- false or true = true
- false or false = false

## If

{{% notice note %}}
Branching : Do different things based on a logical condition.
{{% /notice %}}

Example

```python
age = 50

if age > 18 and age < 35:
    print("true")
elif age > 16 or age age == 20:
    print("t and f")
else:
    print("false")
```

Successive if statement

```python
if condition:
    pass
if condition2:
    pass
```

- = : assign data
- == : compare data

## For

For loops over each element in a sequence or container (list, set, dict, string) one at a time.

```python
web = ["google.com", "facebook.com"]

for a in web:
    print(a)

# reversed
for name in reversed(names):
    pass

# use index, range
for index in range(5):
    pass
```

{{% notice info %}}
range(start, end, step)
{{% /notice %}}

## While

First, understand the boolean expressions since boolean condition is evaluated at the beginning of the loop.

```python
while count < 30:
   if count < 10:
      break
   elif count > 15:
      continue
   else:
      pass
```

{{% notice info %}}
Sentinal value : a value that causes the loop to terminate.
{{% /notice %}}

#### When the while loop ends

- boolean expression(condition) to false
- break sentence is encountered
- exit(0)
- return sentence

continue is continuing

---

# Errors

> #### Syntax Errors

A violation of the programming language rules
{{< vbox blue >}}
<b>Syntax</b><br>
Set of keywords and set of rules for writing the code.
{{< /vbox >}}

> #### Runtime Errors

Correct syntax but the program attempts to do somthing impossible (like divide by zero or multiply string)

- Syntax Error : Cannot be understood but was not caught at start
- IndentationError : Lines are not indented
- ValueError : Invalid value is used
- NameError : variables that don't exist
- TypeError : incorrect types

{{< vbox blue >}}
<b>Runtime</b><br>
Time the program is running.
{{< /vbox >}}

> #### Semantic (Logic) Error

The program runs but does not do what you intended it to do

> #### Error Handling

An exception is an error that happens during the execution of a program.

```python
while True:
    try:
        # Code
        pass
    except ValueError:
        print("Error message")
    except ZeroDivisionError:
        print("Infinity")
    finally:
        print("This block is executed whether the error is or not")
```

> #### Coding standards

https://peps.python.org/pep-0008/
