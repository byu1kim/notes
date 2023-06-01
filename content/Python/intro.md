+++
title = "Introduction"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Install Python

Check Python version

```
python3 --version
```

If Python doesn't exist, install python here
https://www.python.org/downloads/

```
brew install python3
```

## Pipenv

Pipenv is a packaging tool that provides all necessary means to create a virtual environment for the Python project.

> #### Install

Install python > Install pip > Install pipenv

```
pip install pipenv
pipenv --version
```

> #### Create a new project

In the root of working directory

```
pipenv --python 3.11
```

> #### Activate the virtual environment

You have to activate to use virtual environment and use python command line

```
pipenv shell
```

> #### Install packages

```
pipenv install [name]
```

> #### Run the app

```
pipenv run python name.py
or
python name.py
```

> #### Install dependencies

```
pipenv install
```

> #### VSC Extension

Python

(I'm going to use VSC for Python for now but soon will shift to Pycharm)

> #### File extension

name.py

> #### Official Docs

https://www.python.org/

https://docs.python.org/

---

## Python

Python is `script Language`. It is one of the most powerful programming languages, mostly used in data science, machine learning, and big data analytics.

{{< vbox blue >}}
<b>Programming Language</b> <br>
A combination of words and symbols that is used to write programs. It is a way programmers communicate with computers through set of instructions known as code or program. Programming languages are <line>compiled languages. </line><br><br>

<b>Compiled Language</b> <br>
Language that requires a compiler to convert the source code to machine code(bits 0, 1). <br><br>

<b>Script Language</b> <br>
Programming language that supports <line>scripts.</line> <br><br>

<b>Scripts</b> <br>
Scripts are small programs mainly used to automate the execution of a specific function in a specific runtime environment. <point>The scrpts are not compiled.</point> The code is transformed to binary code and executed line by line by interpreter.

{{< /vbox >}}

#### How Python interpreter works

Editor -> Source File -> Interpreter (Compiler, Virtual Machine) -> Running Program

#### Compiler vs Interpereter

**Compiler** converts the code into machine code `before program run`

**Interpreters** converts the code into machine code `When the program is run`

{{% notice note %}}
Script languages : Python, Javascripts
{{% /notice %}}

#### Software Development Process

Analysis > Design > Coding > Testing > Deploy > Maintain

#### Debug

A mode to run the code step by step

#### Unit Test

A part of code that the developer needs to write. It is a function that test the application functions.

---
