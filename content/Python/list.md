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

> #### List Comprehensions

A shorter way to create a new list, from another list, based upon conditions that you set.

```python
fruits = ["apple", "banana", "cherry", "kiwi", "mango"]
newlist = []

for x in fruits:
  if "a" in x:
    newlist.append(x)

newlist = [x for x in fruits if 'a' in x] # x at the front is the item added to the list

provinces = ['bc', 'ab']
new_prov = [province for province in provinces if 'a' in province]
```

> #### Range

`range(start, stop, step)`

```python
x = range(3, 6) # 3, 4, 5, 6
```

> #### Python destructing

When you know the length of array

```
list_ex = [1,2,3]
a, b, c = list_ex
# a == 1, b == 2
```

> #### Loop with List

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

> #### Mapping list

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

Use lambda (lambda is function itself.)
`lambda parameter: return`

```python
squared = map(lambda num: num**2, [1,2,3,4,5])
```

> #### Get sum of array

`sum(list or tuple or dict, start:optional)`

start is starting number (10+list items)

```python
a = sum(map(int, ['1', '2', '3']))
```

> #### Remove item from array

`list.remove(value)`

Remove the first instance of a value in a list.

```python
p_list = ['a', 'b', 'b', 'd', 'e']
p_list.remove('b') # p_list = ['a', 'b', 'd', e]
```

`list.pop(index)`

Remove an an element at a given index

```python
a = p_list.pop(0) # 'a'
print(p_list) # ['b', 'b', 'd', 'e']

# Negative index postion from end
list.pop(-1)

# Empty index remove the last item
list.pop()

```

`list.clear()`

Empty the list

```python
p_list.clear() # p_list = []
```

> #### List to String: Join

`string.join(iterable)`

Returns a string by joining all the elements of an iterable.

```python
' '.join(p_list) # a b b d
```

> #### Index : Index of in the list

`list.index(element, start, end)`

Returns only the first instance that matches.

- **element** : the element(value) to be searched
- **start** (optional) : start search from this index
- **end** (optional) : search the element up to this index

If the element is not in the list, it will throw a ValueError.

```python
p_list.index('a') # 0
```

> #### Add item in the list

```python
list.append(item)
```

> #### count()

```python
p_list.count('c')
```

> #### reverse()

```python
p_list.reverse()
```

> #### append(value)

```python
p_list
```

> #### min()

```python
min(p_list)
```

> #### max()

```python
max(p_list)
```

> #### sort()

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

> #### Access values

```python
p_dict['name']
# or
p_dict.get('name')
```

> #### Add data

```python
p_dict['newKey'] = 'haha'
```

> #### Delete

```python
# Using del keyword
del p_dict['newKey']

# Remove all item
p_dict.clear()
```

> #### Merge dictionaries

Overwritten if the same key exists

```python
p_dict.update(new_dict)
```

> #### Iterating

```python
list_dic = {'key':'value'}

# key
for key in p_dict:
  print(key) # name
  print(p_dict[key]) # aa

# value
for val in p_dict.values():
  print(val) # aa

# both
for key, val in p_dict.items():
  print(name, val) # name aa
```
