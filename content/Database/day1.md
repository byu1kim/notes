+++
title = "Database"
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Database

```sql
INSERT INTO table (table) VALUES (1);
```

#### Dynamic (Programmatic) SQL

Dynamic SQL is referring to SQL statements that are constructed and executed at runtime.

1. Variable Declaration

- Variables are declared with the 'DECLARE' construct
- Variable names are preceded with the @ symbol

2. Assigning value

- SET : single value assignment
- SELECT : multiple value assignment

#### SET Variale assignemnt

```sql
DECLARE @max_students INTEGER;
SET @max_student = 55;
SELECT @max_student AS 'Minimum Age';
```
