+++
title = "Introduction"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Install

Check Python version

```
python3 --version
```

If Python doesn't exist, install python here
https://www.python.org/downloads/

```
brew install python3
```

#### VSC Extension

Python

(I'm going to use VSC for Python for now but soon will shift to Pycharm)

#### File extension

name.py

#### Official Docs

https://www.python.org/

https://docs.python.org/

---

## Python

Python is `script Language`. It is one of the most powerful programming languages, mostly used in data science, machine learning, and big data analytics.
<point></point> <line></line>
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

## Operators

#### Arthmetic Operators

`\+`, `\-`, `\*`, `/`, `%`(Modulus), `\*\*`(Power), `//`(Rounding after division)

#### Assignment Operators

`=`, `+=`, `-=`, `\*=`, `/=`, `%=`, `//=`

#### Comparison or Relational Operators

`==`, `!=`, `>`, `<`, `>=`, `<=`

#### Logical Operators

`and`, `or`, `not`

- short-circuit evaluation : a or b 에서 이미 a가 true면 evaluating no need
- use the first expression to guard the second expression

#### Identity Operators

`is`, `is not`

#### Membership Operators

`in`, `not in`

#### Logical Operators

`&`(AND), `|`(OR), `^`(XOR), `~`(NOT)

#### Operators Precedence

Python evaluates boolean expression from left to right

1. Parentheses : ()
2. Arithmetic : + /
3. Relational operator : > >=
4. not
5. and
6. or

---

## Errors

#### Syntax Errors

A violation of the programming language rules
{{< vbox blue >}}
<b>Syntax</b><br>
Set of keywords and set of rules for writing the code.
{{< /vbox >}}

#### Runtime Errors

Correct syntax but the program attempts to do somthing impossible (like divide by zero or multiply string)

- Syntax Error : Cannot be understood but was not caught at start
- IndentationError : Lines are not indented
- ValueError : Invalid value is used
- NameError : variables that don't exist
- TypeError : incorrect types

{{< vbox blue >}}
<b>Runtime</b><br>
Time the program is running.
{{< /vbox >}}

#### Semantic (Logic) Error

The program runs but does not do what you intended it to do

#### Error Handling

An exception is an error that happens during the execution of a program.

```python
while True:
    try:
        # Code
        pass
    except ValueError:
        print("Error message")
    except ZeroDivisionError:
        print("Infinity")
    finally:
        print("This block is executed whether the error is or not")
```

#### Coding standards

https://peps.python.org/pep-0008/
