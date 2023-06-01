+++
title = "Files"
weight = 6
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

#### Secondary Memory

Secondary memory is file. It is not erased when the power is turned off.

#### Open file

```python
file = open('file_name.txt', 'r')
string = file.read()
file.close()

with open('file.txt', 'r')
string = file.read()
file.close()
```

Always close a file you open. If you don't close, you might have

- a permanently-locked state(inaccessible for operations now)
- a corrupted file
- any writing may have gone uncommitted

#### Reading and Writing

```python
f = open('file.txt', 'w')
f.write('text')
f.close()
```

#### Appending

```python
f = open('file.text', 'a')
```

#### Reading

```python
f = open('file.text', 'r')
aa = f.readlines() # as array by lines
ab = f.read() # as string
f.close()
```

#### Mode

- `w` : overwrite an existing file, create file if it doesn't exist
- `x` : create a file, error if the file already exists
- `a` : write to the end of an existing file, create file if it doesn't exist
- `\+` : open a file for reading or writing

#### Directory Listing

```python
import os

with os.scandir('my_directory/') as entries:
    for entry in entries:
        print(entry.name)
```

```python
from pathlib import Path

entries = Path('my_directory/')
for entry in entries.iterdir():
    print(entry.name)
```

#### Open File Dialog

```python
from tkinter import Tk
from tkinte.filedialog import askopenfilename

Tk().withdraw()
filename = askopenfilename()
print(filename)
```
