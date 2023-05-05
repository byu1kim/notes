+++
title = "String"
weight = 3
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

A sequence of characters. String is `immutable`.

#### Usage

- Single line string : "~~"
- Multiple lines : '''~~~'''

#### Escape special characters

```python
print("this is \"example\"")
# this is "example"
```

#### String Concatenate

```python
con = "a" + "Python"
con2 = f"Wow {con}"
```

#### Repeat

```python
print("ba" + "na"*2)
# banana
```

#### String Formatting

```python
a = 12.1
b = 22.32
c = 3.021
output = "a: %s b: %d c: %f c: %.2f" %(a, b, c, c)
# a: 12.1 b: 22 c: 3.021000 c: 3.02
```

| Sign  | Meaning                                                                       |
| ----- | ----------------------------------------------------------------------------- |
| %s    | String (%5s : min 5chracters)                                                 |
| %d    | Integers (%3d : min 3 digits, %03d : zeros to fill the spaces)                |
| %f    | Floating point numbers                                                        |
| %.nf  | Floating point numbers with a fixed amount of digits to the right of the dot. |
| %x/%X | Integers in hex representation (lowercase/uppercase)                          |
| %e/%E | Floating points in Exponential format (lowercase/uppercase ex: 1.7e3)         |
| %%    | percentage sign (%)                                                           |

```python
message = "{} and {}".format("value1", "value2")
# value1 and value2
```

{} : you can put order number (index) or name of value

---

## String Methods

#### String Slicing

```python
message = "string"

print(message[2:4])
# ri

print(message[:-1])
# strin : all but the last
```

#### Length

```python
len(message)
# 6
```

#### split()

`str.split('char')`

```python
message = "test:aa"
print(message.split(":"))
# ['test', 'aa']
```

#### join()

`str.join(array)`

```python
message = ['a', 'b']
print("+".join(message))
# a+b
```

#### replace()

`str.replace(old, new, count)`

replace returns new value, count times. it there's nothing to replace, it returns original string.

```python
# Single usage
test = "banana"
replaced_text = text.replace('ba', 'ro') # ronana

# Multiple usage
replaced = text.replace('a', 'b').replace('n', 'm')
```

#### More String methods

- find(x)
- find(x, start) : find from start point
- find(x, start, end) : fint range from start to end
- rfind(x)
- count(x)

#### String methods returns boolean

- isalnum() : lowercase or uppoercase or number from 0-9
- isdigit()
- islower()
- isupper()
- isspace()
- startswith(x)
- endswith(x)

#### String methods returns string

- capitalie()
- lower()
- upper()
- strip()
- title()
