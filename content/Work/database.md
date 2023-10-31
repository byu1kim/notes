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

## Roll Back Migration

1. Update whatever you need

2. update-database -Migration:"MigrationName"
   Migration name is the ealier one than you want to revert

3. remove-migration

4. add-migration name
