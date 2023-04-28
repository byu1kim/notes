+++
title = "Control Flow"
menuTitle = "If, For, While"
weight = 4
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Boolean Expression

Boolean expressions are evaluated to `True`(0) or `False`(1).

- true and true = true
- false and true = false
- true and false = false
- false and false = false
-
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
