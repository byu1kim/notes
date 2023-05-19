+++
title = "Database"
weight = 1
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

### aa

```sql
  SELECT *
    FROM words
    WHERE user_id = $1 AND name ILIKE $2
    ORDER BY id
    LIMIT $3
    OFFSET $4
```

### Back

```
const getWords = async (userId, searchTerm, limit, offset) => {
  // Construct the SQL query with placeholders for dynamic values
  const query = `
    SELECT *
    FROM words
    WHERE user_id = $1 AND name ILIKE $2
    ORDER BY id
    LIMIT $3
    OFFSET $4
  `;

  // Execute the query with the provided parameters
  const result = await client.query(query, [userId, `%${searchTerm}%`, limit, offset]);

  // Return the result rows as an array
  return result.rows;
};
```
