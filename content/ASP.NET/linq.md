+++
title = "Linq"
weight = 4
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

LINQ (Language-Integrated Query). Syntax to query the database in Entity Framework (belong to the System.Linq namespace)

## Query Based Syntax

> #### Select

Explicit

```c#
IQueryable<Invoice> query = from p in db.Invoices select p;
```

Implicit (Using an anonymous type)

```c#
var query = from p in db.Products select  new  { p.name, p.vendor };
```

> #### Filter

```c#
from   p in context.Products
where  p.price > 0.99m && p.price < 2.00m
select p;
```

> ##### Filter with Contains, StartWith

```c#
var query =   from p in context.Products
where p.name.StartsWith("Ca") || p.name.Contains("Juice")
select p;
```

> #### Sort

```c#
var query = from p in context.Products
where p.price > 0.99m && p.price < 2.00m
orderby p.price
descending
select p;
```

---

## Handling Database (Query)

> #### Grouping

```c#
var vendorCounts = from p in context.Products
                   group p by p.vendor
                   into aggregateTable // Store the key. (Temp table)
                   select new
                    { // The key is the grouping column.
                        Vendor = aggregateTable.Key,
                        ProductCount = aggregateTable.Count()
                    };
```

> #### Filtered Group By

```c#
from p in context.Products
group p by p.mfg
into aggregateTable
where aggregateTable.Count() > 1
select new { Manufacturer = aggregateTable.Key,
             ProductCount = aggregateTable.Count()
           };
```

> #### Bridge Table Query : bridge Table does not have model if they have only foreign keys in the table.

```c#
from p in db.Products
from i in p.Invoices // Products Navigation property
select new { ProductID = p.productID,
             InvoiceNum = i.invoiceNum,
             Branch = i.branch,
             ProductName = p.name };
```

> #### Inserting Objects

```c#
context.Products.Add(product);
context.SaveChanges(); // Commit the data.
```

> #### Inserting Bridge Table Objects

```c#
Product product = (from p in context.Products where p.productID == 4 select p).FirstOrDefault();
Invoice invoice = (from i in context.Invoices where i.invoiceNum == 1004 select i).FirstOrDefault();

invoice.Products.Add(product);
context.SaveChanges(); // Commit the data.
```

> #### Updating objects

```c#
product.price = 4.88m;
context.SaveChanges();
```

> #### Deleting

```c#
context.Products.Remove(product);
context.SaveChanges();
```

> #### Deleting Bridge Table

```c#
invoice.Products.Remove(product);
context.SaveChanges();
```

> #### Combining Queries

Due to Lazy loading, you can use two separate queries.

```c#
var gfsItems = from p in db.Products
where p.Vendor == "GFS"
select p;

// Second query using first query.
var inexpesiveGFSItems = from p in gfsItems
                            where p.Price < 2.50M
                            select p;

return View(inexpesiveGFSItems);
```

---

## Lambda (Method Based) Syntax

> #### Select

```c#
context.Products.Select(p => p);
context.Products.Select(p => new { Name = p.Name, Vendor = p.Vendor });
```

> #### Select, Filter

```c#
db.Products.Where(p => p.ProductId == id);
```

> #### First of Default

```c#
db.Products.Where(p => p.ProductId == id).FirstOrDefault();
```

> #### Contains

```c#
string[] array = {"a", "b"};
var aa = db.Table.Where(x=>array.Contains(x.Name))
```

> #### Join : Query multiple tables

```c#
TableA.Join(TableB, a=>a.Name, b=>b.Name, (a,b)=> {new { a.Name, b.Number, b.Capacity }})
```

> #### Sort

```c#
context.Products.OrderBy(p => p.Vendor).ThenByDescending(p => p.Name);
```

OrderBy

OrderByDescending

ThenBy

ThenByDescending

> #### Extension Methods

StartsWith()

IndexOf()

IsMatch()

Contains()

ToUpper()

ToLower()

> #### Distinct

.Select(p=>p).Distinct();

> #### Combination

tableA.Union(tableB) : merge the results of two queries where the column names, type, and sequence are the same.

```c#
var table = from p in db.Products select p.column;
```

table.Average()

table.Count()

table.Min()

table.Max()

table.Sum()
