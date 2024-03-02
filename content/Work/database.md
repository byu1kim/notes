+++
title = "Database"
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Db Update

1. Update `Data` File

2. Update `Context` file

3. Add-Migration Name

- if there are build errors, migration will fail.

4. Check migration file : it only contains updates

- Up : for executing, Down : for roll back, both should match. (If `drop` in Up, `add` in Down)

5. Check migration designer file

6. Check snapshot file : it contains entire db structure (Compare difference)

7. If everything is okay, update-database

## Roll Back Migration / When you want to pull from dev branch

0. Change the connection string to local DB

1. Update whatever you need

2. update-database -Migration:"MigrationName"
   Migration name is the ealier one than you want to revert

3. remove-migration

4. add-migration name

// CleanUpOrderFlow

## remove migration without update

just remove-migration
and
comment
<PropertyGroup Condition="'$(Configuration)'=='DEBUG'">
<CurrentMonth>$([System.DateTime]::Now.Month)</CurrentMonth>
		<YearOfCurrentMonth>$([System.DateTime]::Now.Year)</YearOfCurrentMonth>
<LastMonth>$([System.DateTime]::Now.AddMonths(-1).Month)</LastMonth>
		<YearOfLastMonth>$([System.DateTime]::Now.AddMonths(-1).Year)</YearOfLastMonth>
<DefaultItemExcludes>$(DefaultItemExcludes);Migrations\*.Designer.cs</DefaultItemExcludes>
</PropertyGroup>

    <ItemGroup Condition="'$(Configuration)'=='DEBUG'">
    	<Compile Include="Migrations\$(YearOfCurrentMonth)$(CurrentMonth)*.Designer.cs" />
    	<Compile Include="Migrations\$(YearOfLastMonth)$(LastMonth)*.Designer.cs" />
    </ItemGroup>

this out from web.

## force change : assigning default value

change data file

and then change context file (NOT a snapshot)

```c#

modelBuilder.Entity<RestaurantGroupOption>()
	.Property(x => x.Status)
	.HasDefaultValue(EntityStatus.Active);
modelBuilder.Entity<RestaurantGroupOptionProduct>()
	.Property(x => x.Status)
	.HasDefaultValue(EntityStatus.Active);

```

and then migration

## Setting up Local DB

1. find your local db name
   Open the Start menu, and enter services.

2. Change the connection string in your project (Visual studio)

3. When there is new migration ? : Check EFMigration history just in case

- Pull from master branch
- Check the migration
- update-database (Make sure you are using local connection string)
