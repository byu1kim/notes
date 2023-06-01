+++
title = "Beautiful Soup"
weight = 10
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

Beautiful Soup is a Python library for pulling data out of HTML and XML files.

https://www.crummy.com/software/BeautifulSoup/bs4/doc/

\* When the website is blocked a bot, you need to use Selenium.

\* scrap for commercial purpose > follow the policy

#### Install

```
pip install beautifulsoup4
```

#### Initializing

```python
from requests import get
from bs4 import BeautifulSoup

response = get("url")
if response.status_code != 200:
    print("Cant'request")
else:
    soup = BeautifulSoup(response.text, "html.parser")
    # response.text : <tag>...</tag>
```

## Methods

> #### Find all

Scan the entire document looking for results as **list**

`find_all(tag, attrs, recursive, string, limit)`

```python
soup.find_all('section', class_="jobs")
```

- **attrs** : id, href. or `{"aria-label": "pagination"}`
- **recursive** : recursive=False, only direct child
- **string** : where string inside the tag matches
- **limit** : number of items

> #### find()

Find one, returns the result

`find(name, attrs, recursive, string)`

> #### string()

Get string from HTML

`.string`

```python
soup.find('tag').string
```
