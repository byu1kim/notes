+++
title = "Razor"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

#### Anchor

```
<a asp-action="Link" asp-route-id="id?">Haha</a>
```

#### Drop down menu

```c#
ViewData["AAA"] = new SeletList(db.Table, "AAA", "AAA")
```

Razor

```
<select asp-for="Table" asp-items="ViewBag.AAA"></select>
```
