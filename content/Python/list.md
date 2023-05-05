+++
title = "Data Structure"
menuTitle = "List, Tuple, Set, Dict"
weight = 5
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Lists

- Related data can be grouped together into a single variable
- Python list can have different type of values : array, string, number, etc..
- List is `mutable`

```python
list_sample = ['a','b','c']
```

{{% notice info %}}
Mutate means change. You can modify something mutuable.
{{% /notice %}}

#### List Comprehensions

A shorter way to create a new list, from another list, based upon conditions that you set.

```python
provinces = ['bc', 'ab']
new_prov = [province for province in provinces if 'a' in province]
```

#### Range

`range(start, stop, step)`

```python
x = range(3, 6) # 3, 4, 5, 6
```

#### Loop

Loop list with index

```python
for i in range(len(my_array)):
  pass
```

Loop list with index and its item.

```python
for index, item in enumerate(my_array):
    pass
```

## Methods

#### Mapping list

`map(function, array)`

Convert to list before using mapped list : `list(words)`

```python
def myfunc(a, b):
  return a + b

x = map(myfunc, ('apple', 'banana', 'cherry'), ('orange', 'lemon', 'pineapple'))
```

Convert string items to integers

```python
int_nums = map(int, ['1', '2', '3'])
words = map(len, ['haha', 'hoho'])
```

Use lambda

```python
squared = map(lambda num: num**2, [1,2,3,4,5])
```

#### Get sum of array

`sum(list or tuple or dict, start:optional)`

start is starting number (10+list items)

```python
a = sum(map(int, ['1', '2', '3']))
```

#### remove(value)

```python
p_list = ['a', 'b', 'c', 'd', 'e']
p_list.remove()
```

#### count()

```python
p_list.count('c')
```

#### clear()

```python
p_list.clear()
```

#### reverse()

```python
p_list.reverse()
```

#### append(value)

```python
p_list
```

#### pop()

delete last item

```python
p_list
```

#### min()

```python
min(p_list)
```

#### max()

```python
max(p_list)
```

#### index('item')

```python
p_list.index('a')
```

#### sort()

```python
p_list
```

---

## Tuples

An ordered sequence of elements. `Duplicate` elements are allowed.

```python
tuple_sample = ('a', 'b')
```

{{% notice note %}}
Tuple elements are **immutable**. Cannot be added, removed or updated once declared.
{{% /notice %}}

#### Multiple Return with Tuple

```python
...
return (a, b)
```

---

## Set

- Set is an `object` that stores a colletion of data
- All elements are `unique`, no duplicates
- Elements in a set can be of different data types
- Elements in a set are not stored in any particular order
- Cannot access items by referring to an index
- Can loop through the set
- Once a set is created, you cannot change its items, but you can `add/remove` items.
  Add one item using add() method

```python
# Convert List to Set
p_list = (['a', 'b', 'b'])
print(set(p_list)) # {'a', 'b'}

# Define Set
p_set = {'a', 'b', 'c'}

# Add a single item
p_set.add('aa')

# Add multiple items
p_set.update(['a', 'b'])

# Remove item
p_set.remove('a') # error if item does not exists
p_set.discard('a') # if the item does not exists, it won't raise an error
```

#### clear()

Empty the set

```python
p_set.clear()
```

#### union()

Return a new set with all itmes from both sets :

```python
set_a = {1, 2, 3}
set_b = {'a', 'b', 'c'}
new_set = set_a.union(set_b)
```

Can also be done using `update()`

| List      | Tuple     | Set                       |
| --------- | --------- | ------------------------- |
| []        | ()        | {}                        |
| Mutable   | Immutable | No change, Yes add/remove |
| Duplicate | Duplicate | No Duplicate              |

---

## Dictionary

- Dictionary is an object that stores an `unordered` collection of data
- Each element has `key`-`value` pair

```python
# Definition
p_dict = { 'name': 'aa', 'age': 1 }

# Dict() to define
p_dict = dict(name='aa', age=1)

# Dict() + List, Tuple
p_dict = dict([('name', 'aa'), ('age', 1)])
```

#### Access values

```python
p_dict['name']
# or
p_dict.get('name')
```

#### Add data

```python
p_dict['newKey'] = 'haha'
```

#### Delete

```python
# Using del keyword
del p_dict['newKey']

# Remove all item
p_dict.clear()
```

#### Merge dictionaries

Overwritten if the same key exists

```python
p_dict.update(new_dict)
```

#### Iterating

```python
for key in p_dict:
  print(key) # name
  print(p_dict[key]) # aa

for val in p_dict.values():
  print(val) # aa

for key, val in p_dict.items():
  print(name, val) # name aa
```
