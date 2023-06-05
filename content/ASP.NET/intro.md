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
