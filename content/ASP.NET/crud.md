+++
title = "CRUD"
weight = 3
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Create

#### Get

> ##### Controller

```c#
public IActionResult Create()
{
    FoodStoreContext db = new FoodStoreContext();

    ViewData["Mfg"] = new SelectList(db.Table, "Mfg", "Mfg");
    return View();
}
```

> ##### View

Right click on `View` in Contrller and add `Create` template

#### Post

Controller

```c#
[HttpPost]

public IActionResult Create([Bind("ProductId,Name,Mfg,Vendor,Price")] Product product)
{
    FoodStoreContext db = new FoodStoreContext();

    // Ensure data is valid.
    if (ModelState.IsValid) {
        db.Add(product);
        db.SaveChanges(); // Commit changes to database.

        // Save is successful so show updated listing.
        return RedirectToAction(nameof(Index));
    }

    // Data not valid so show form again with populated drop downs.
    ViewData["Mfg"]    = new SelectList(db.Manufacturers, "Mfg", "Mfg", product.Mfg);
    ViewData["Vendor"] = new SelectList(db.Suppliers, "Vendor", "Vendor", product.Vendor);

    return View(product);
}

```

## Detail

Controller

```c#
 public IActionResult Detail(string? id)
        {

            testContext context = new testContext();
            var test = (from p in context.Test1s where p.Firstname == id select p).FirstOrDefault();

            return View(test);
        }
```

Razor

```
@model WebApplication1.Models.Test1

@{
    ViewData["Title"] = "Detail";
}

<div>
    @Model.Firstname @Model.LastName
</div>
```

## Update (Edit)

## Delete

Controller

```c#
public IActionResult Delete(string? id)
    {
        testContext db = new testContext();
        var test = (from p in db.Test1s where p.Firstname == id select p).FirstOrDefault();

        db.Remove(test);
        db.SaveChanges();

        return RedirectToAction(nameof(Index));
    }
```

```razor
 <a asp-action="Delete" asp-route-id="@item.Firstname">Delete</a>
```

## View Model

1. Create a new ViewModel
2. Add a new method inside Home controller

1) put everything into anonymous type (var)
2) using forEach, store it to VM (List)

3. Store Data(DB) to VM
4. Pass to the view (@IEmnumaerable?)
5. Create the view and import the VM
6. Use it

- Code First + View Model Practice \*
