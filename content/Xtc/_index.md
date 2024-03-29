+++
title = "ETC"
date = 2023-04-12T02:13:09-07:00
weight = 30
chapter = true
pre = ""
+++

## Theme Link

https://learn.netlify.app/en/shortcodes/

## Install Hugo

#### Windows

1. Download Prebuilt Binary

https://gohugo.io/installation/windows/

Download windows-amd-64.zip

2. Add PATH environment Variable

```
System > Advanced system settings > Advanced > Environment Variables
'Path' under the both User variables and System variables
```

3. Install Hugo Learn Theme

Download the zip and paste into the theme folder

https://learn.netlify.app/en/basics/installation/

---

#### MAC

1. Create new project

```
hugo new site <new_project>
```

2. Start the app

```
hugo server
```

3. Apply the learn them

https://learn.netlify.app/en/basics/installation/

Download the zip file and paste to the theme folder

Modify the configuration file `config.toml`

```
# Change the default theme to be use when building the site with Hugo
theme = "hugo-theme-learn"

# For search functionality
[outputs]
home = [ "HTML", "RSS", "JSON"]
```

#### Start the project

```
hugo server
```

---

# Heading

## H1

### H2

#### h3

# Lines

---

# Text Style

**bold** _italic_ ~~cross~~

# quote

> I want a quote!
>
> > sad..
> > sago

# List

- exam
- pleple
  - tab tab! (twice)
    - what
- tab

1. hoho
2. what

# code

`<tag>this is code</tag>`

```
<html>
    line 1 of code
    line 2 of code
    line 3 of code
</html>
```

```js
const aa = "string";
```

```css
.class {
  background-color: "pink";
}
```

```php
for a in b
```

instead of js, you can use
: json, c#, go, html, css, sql, typescript, kotlin, javascript
php, scss, swift, python,,,,,

# Tables

| header | header |
| ------ | ------ |
| aa     | bb     |
| cc     | dd     |

# Link

[Assemble](http://assemble.io)

# Image

![Minion](https://octodex.github.com/images/minion.png)

# Tab View

{{< tabs >}}
{{% tab name="python" %}}
{{% /tab %}}

{{% tab name="R" %}}
{{% /tab %}}
{{< /tabs >}}

---

{{< tabs >}}
{{% tab name="python" %}}

```python
print("Hello World!")
```

{{% /tab %}}
{{% tab name="R" %}}
This is R

```R
> print("Hello World!")
```

{{% /tab %}}
{{% tab name="Bash" %}}

```Bash
echo "Hello World!"
```

{{% /tab %}}
{{< /tabs >}}

# Accordian

{{%expand "Is this learn theme rocks ?" %}}Yes !.{{% /expand%}}

# Note

{{% notice note %}}
A notice disclaimer
{{% /notice %}}

{{% notice info %}}
An information disclaimer
{{% /notice %}}

{{% notice tip %}}
A tip disclaimer
{{% /notice %}}

{{% notice warning %}}
A warning disclaimer
{{% /notice %}}

{{< vbox >}}
blue, pink, cyan, green, orange, indigo
{{< /vbox >}}

{{< hbox >}}
blue, pink, cyan, green, orange, indigo
{{< /hbox >}}
You can add tags inside vbox, hbox
