+++
title = "Selenium"
weight = 12
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

https://www.selenium.dev/
https://selenium-python.readthedocs.io/

It is automates browsers.
It will actually start the browser. so indeed think we are not a bot.

#### Install

```
pip install selenium
pip install webdriver_manager
```

https://pypi.org/project/webdriver-manager/

#### Initializing

```python
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager

browser = webdriver.Chrome(ChromeDriverManager().install())

browser.get('http://google.com')
```

This works too

```py
from selenium import webdriver

driver = webdriver.Chrome()
driver.get('url')

# If only you want to prevent closing the browser automatically
while (True):
    pass
```

Detail

```py
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from webdriver_manager.chrome import ChromeDriverManager

chrome_options = Options()
chrome_options.add_experimental_option("detach", True) #브라우저 꺼짐 방지 코드

browser = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options = chrome_options) #크롬드라이버를 최신으로 유지해줍니다.
```

https://goddino.tistory.com/353

1. write code without thinking
2. refactor and clean
