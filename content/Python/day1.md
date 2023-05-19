+++
title = "Day1"
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Python

- Exceptions
- OOP
- Scope

---

{{< vbox blue >}}
{{< /vbox >}}

{{% notice %}}
{{% /notice %}}

## Standard Library

- [ ] What is standard Library ?
- [ ] built in function list up
- [ ] how to setup the python file? like...packages

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

### Status Codes

## Beautiful Soup

```
pip install beautifulsoup4
```

python3 -m poetry add beautifulsoup4

\*\* scrap for commercial perpose > follow the policy

get the html of the website

```python
from requests import get

base_url = "https://weworkremotely.com/remote-jobs/search?term="
search_term = "python"

response = get(f"{base_url}{search_term}")
print(response.text)
```

#### Beautiful soup

soup.find_all("a", class="sister")

https://www.crummy.com/software/BeautifulSoup/bs4/doc/#installing-beautiful-soup

If you know the argument name, specifying order of param name doesn't matter

```python
from requests import get
from bs4 import BeautifulSoup

base_url = "https://weworkremotely.com/remote-jobs/search?term="
search_term = "python"

response = get(f"{base_url}{search_term}")
if response.status_code != 200:
    print("Cant'request")
else:
    soup = BeautifulSoup(response.text, "html.parser")
    print(soup.find_all('title'))
    print(soup.find_all('section', class_="jobs"))
```

#### Python destructing (when you know the length of array)

```
list_ex = [1,2,3]
a, b, c = list_ex
# a == 1, b == 2
```

---

#### Beatiful Soup Methods

- find_all()
- find()

`.string`

Get string from HTML

### Selenium

It is automates browsers.
It will actually start the browser. so indeed think we are not a bot.

replit

pkgs.chromium
pkgs.chromedriver

main.py

```py
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

options = Options()
options.add_argument("--no-sandbox")
options.add_argument("--disable-dev-shm-usage")

browser = webderiver.Chrome(options=options)
browser.get("url")

response = browser.page_source
```

## Se

- virtualenvwrapper
- pipenv
- poetry
