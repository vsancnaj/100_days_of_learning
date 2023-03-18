# Relational Database Pt 2

Created: March 14, 2023 11:38 AM

## PostgreSQL

### Basic Terminal Commands

```sql
\l
```

```sql
\c second_database
```

```sql
\d 

--- 
\d <table_name> 
```

### Data Types

- INT
- VARCHAR - *a short string of characters VARCHAR(30)*
- SERIAL - *will make your column an `INT`with a `NOT NULL`constraint, and automatically increment the integer when a new row is added.*

### Altering/Creating Database/Tables & Columns & Rows

- **DB / TABLE**
    
    ```sql
    CREATE DATABASE name_database;
    ```
    
    ```sql
    CREATE TABLE table_name();
    CREATE TABLE table_name(column_name DATATYPE CONSTRAINTS);
    ```
    
    ```sql
    DROP TABLE table_name;
    ```
    
    ```sql
    ALTER DATABASE database_name RENAME TO new_database_name;
    ```
    
- **COLUMN**
    
    ```sql
    ALTER TABLE table_name ADD COLUMN column_name DATATYPE;
    ```
    
    ```sql
    ALTER TABLE table_name DROP COLUMN column_name;
    ```
    
    ```sql
    ALTER TABLE table_name RENAME COLUMN column_name TO new_name;
    ```
    
- **ROW**
    
    ```sql
    INSERT INTO table_name(column_1, column_2) VALUES(value1, value2);
    ```
    
    ```sql
    INSERT INTO characters(name, homeland, favorite_color)
    VALUES('Mario', 'Mushroom Kingdom', 'Red'),
    ('Luigi', 'Mushroom Kingdom', 'Green'),
    ('Peach', 'Mushroom Kingdom', 'Pink');
    ```
    
    ```sql
    DELETE FROM table_name WHERE condition;
    
    # example
    DELETE FROM second_table WHERE username='Luigi';
    ```
    
    ```sql
    UPDATE users SET age = 30 WHERE id = 1;
    ```
    

### Querying

```sql
SELECT columns FROM table_name;
```

```sql
SELECT columns FROM table_name ORDER BY column_name;
```

```sql
ALTER TABLE characters ADD PRIMARY KEY(name);
```

```sql
ALTER TABLE table_name DROP CONSTRAINT constraint_name;
```

---

**One-to-many relationship**

```sql
ALTER TABLE table_name ADD COLUMN column_name DATATYPE REFERENCES referenced_table_name(referenced_column_name);
```

- example of foreign key
    
    ```sql
    ALTER TABLE sounds ADD COLUMN character_id INT NOT NULL REFERENCES characters(character_id);
    ```
    
    `character_id`
     This will be a "one-to-many" relationship because **one**
     character will have **many** sounds, but no sound will have more than one character.
    

**Many-to-many relationship**

- This is because **many**
 of the characters can perform **many**
 actions. And many actions can belong to many characters

---

```sql
ALTER TABLE more_info ADD UNIQUE(character_id);
```

```sql
ALTER TABLE table_name ADD PRIMARY KEY(column1, column2);
```

```sql
SELECT columns FROM table_1 
	FULL JOIN table_2 
	ON table_1.primary_key_column = table_2.foreign_key_column;

--Example
SELECT * FROM characters 
	FULL JOIN more_info 
	ON characters.character_id = more_info.character_id;
```
