+++
title = "ETC"
date = 2023-04-12T02:13:09-07:00
weight = 25
chapter = true
pre = "*"
+++

### Link

https://learn.netlify.app/en/shortcodes/

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
