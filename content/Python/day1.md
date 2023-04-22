+++
title = "Day1"
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Python

- Introduction
- Variables
- Functions
- Data Structure : List, Tuple, Dict

#### Comments

```python
# comments
or
""" multi lines """
```

#### Casino

## Data structure

#### Packages

pypi

requests :

```python
from requests import get
```

- Variables
- Algorithms and flowcharts

- Operators
- While, For
- Error Handle
- Functions, Modules,
- Strings

- Files

- List
- Tuple
- Dictionary

- Regex

- Exceptions
- OOP
- Scope

---

## Introduction

#### Input

input()

comment: # ~~

#### Conversion

```python
float(x)
int(a)
str(z)
type(a)
```

#### Constant

- variable that shouldn't be changed.
- AAA_AAA = 'a'
- Capitals_Capitals

#### Built-in Functions

print()

type()

id()

#### Arithmetic Operators

+, -, \*, /, \*\*

#### Errors

- Syntax Errors : A violation of the programming language rules
- Runtime Errors : Correct syntax but the program attempts to do somthing impossible (like divide by zero or multiply string)
  (Runtime : Time the program is running)

#### Runtime Error

- Syntax Error : Cannot be understood but was not caught ut start
- IndentationError : Lines are not indented
- ValueError : Invalid value is used
- NameError : variables that don't exist
- TypeError : incorrect types

#### Semantic (Logic) Error

The program runs but does not do what you intended it to do

#### Coding standards

https://peps.python.org/pep-0008/

#### Operators

- Arthmetic Operator : + 1 \* / % \*\* //
- Assignment : = += -= \*= /= %= //=
- Comparison : == != > < >= <=
- Logical : and or not
- Identity : is / is not
- Membership : in / not in
- Logical : & | ^ ~

## Function

## While

First, understand the boolean expressions

#### Boolean expressions order

- Parentheses : ()
- Arithmetic : + /
- Relational operator : > >=
- not
- and
- or

Python evaluates boolean expression from left to right

- short-circuit evaluation : a or b 에서 이미 a가 true면 evaluating no need
- use the first expression to guard the second expression

## If

Branching : do different things based on o logical condition

Successive if : multiple if statements

## Loop

Repeatedly executes the one or more statement

Looping = iterating

loop body = iteration

type : for, while

for : range(), index

```python

for name in reversed(names)

for index in range(5)
```

range(start, end, step)

## While

```python

```

sentinal value : a value that causes the loop to terminate

when the while loop ends

- boolean expression to false
- break sentence is encountered
- exit(0)
- return sentence

continue is continuing

#### Function parameters

def name(param, param2=1)

required parameter : param
optional : param=None
default : param='value'

return multiple values from function : use Tuple ()
