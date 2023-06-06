+++
title = "Intro"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Entity Framework

: Automates all database related activities

- Open a connection to the database
- Create a DataSet class to
- Convert data from the DataSet to .NET object

Table = Class

## Database First

#### Create your database

#### Connect the DB and .NET App

1. Create a `Console App (.NET Framework)`

2. Tools > NuGEt Package Manager > Manage NuGet Packages for solution : Download `EntityFramework`

3. Right clcik the project node and select : Add > New Item > Show All Items > ADO .NET Entity Data Model > Add

4. EF Designer from database > New Connection > Microsoft SQL Server > Continue

5. Type your servername where you can get from SQL server, select the db.

6. Check Save connection settings in App.Config as: and **next**

#### DbContext

- Class
- The link between the db and the c#
- It contains database connection and schema

```c#
static void Main(){
    FoodStoreEntities db = new FoodStoreEntities();

    DbSet<Product> products = db.Products;

    foreach (Product products in products){
        decimal price = Convert.ToDecimal(product.price);
        Console.WriteLine(product.name + " " + price.ToString("C"));
    }

    Console.ReadLine();
}
```
