## Query Based Syntax

## Method Based (Lambda) Syntax

#### Select

```c#
IEnumerable<Table> name = db.TableName.Select(p=>p);
```

#### Filter

```c#
var aa = db.Table.Where(x=> x.Price == 1)
```

#### Contains

```c#
string[] array = {"a", "b"}
var aa = db.Table.Where(x=>array.Contains(x.Name))
```

#### Join : Query multiple tables

```c#
TableA.Join(TableB, a=>a.Name, b=>b.Name, (a,b)=> {new { a.Name, b.Number, b.Capacity }})
```

#### Extension Methods

StartsWith()
IndexOf()
IsMatch()
Contains()
ToUpper()
ToLower()

#### Sorting

.OrderBy(p=>p.Name)
.ThenByDescending(p=>p.Name)

OrderBy
OrderByDescending

ThenBy
ThenByDescending

#### Distinct

.Select(p=>p).Distinct();

## Using View Model

1. Create a new ViewModel
2. Add a new method inside Home controller

1) put everything into anonymous type (var)
2) using forEach, store it to VM (List)

3. Store Data(DB) to VM
4. Pass to the view (@IEmnumaerable?)
5. Create the view and import the VM
6. Use it

- Code First + View Model Practice \*
