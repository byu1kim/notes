+++
title = "Module"
weight = 3
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Script

Script is a Python code that is passed to the Python Interpreter to be executed. By convention, the script have a main() function.

## Modules

Module is a collection of functions. Nothing is running unless it is called from script.

#### Importing Modules

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
