+++
title = "Flask"
weight = 11
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

#### Install

pip install Flask

#### Use

```python
from flask import Flask

app = Flask("Name")

@app.route("/")
def home():
    return "Home"

app.run("127.0.0.1")
```

#### HTML

```python
from flask import Flask, render_template

app = Flask("JobScrapper")

@app.route("/")
def home():
    return render_template("home.html", name="hey")

```

You should create home.html file inside `templates` folder. Flask looks for the templates folder to get html files.

Use the variable from rendering

```
<h1>{{name}}</h1>
```

1 + tap : bolierplate in html

#### Pico

https://picocss.com/

#### Full code

```python
from flask import Flask, render_template, request, redirect, send_file
from extractors.wwr import extract_wwr_jobs
from file import save_to_file

app = Flask("JobScrapper")

# Syntax sugar : looks simple, but actually complex behind the scene. No space between function and decorative

db = {}


@app.route("/")
def home():
    return render_template("home.html", name="hey")


@app.route("/search")
def search():
    keyword = request.args.get("keyword")
    if keyword == None:
        return redirect("/")

    if keyword in db:
        jobs = db[keyword]
    else:
        jobs = extract_wwr_jobs(keyword)
        db[keyword] = jobs

    return render_template("search.html", search=keyword, jobs=jobs)


@app.route("/export")
def export():
    keyword = request.args.get("keyword")
    if keyword == None:
        return redirect("/")
    if keyword not in db:
        return redirect(f"/serach?keyword={keyword}")
    save_to_file(keyword, db[keyword])
    return send_file(f"{keyword}.csv", as_attchment=True)


app.run("127.0.0.1")
```
