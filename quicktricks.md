## Find metadata of a table.
```sql
SELECT
    *
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_CATALOG = 'YourDataBaseName'
    AND TABLE_SCHEMA = 'YourSchemaName'
    AND TABLE_NAME = 'YourTableName'
ORDER BY ORDINAL_POSITION ASC
```

## Find primary key of any table
```sql
SELECT
    *
FROM
    INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE
    TABLE_NAME = 'YourTableName'
    AND CONSTRAINT_NAME LIKE 'PK%'
```

## Find auto increment columns.
```sql
SELECT
    *
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'YourTableName'
    AND COLUMN_NAME = 'YourColumnName'
    AND COLUMNPROPERTY(OBJECT_ID(TABLE_NAME), COLUMN_NAME, 'IsIdentity') = 1
```

## Find from which stored procedure, the table is getting populated
```sql
SELECT DISTINCT
    o.name AS OBJECT_NAME,
    o.type_desc AS OBJECT_DESCRIPTION
FROM
    sys.sqlmodules M
    INNER JOIN sys.objects O
    ON M.object_id = O.object_id
WHERE
    M.definition LIKE '%INTO YourTableName%'  
    
-- Sometimes tablename might be having different syntax in the stored procedure as follows.
-- [TableName]
-- [DatabaseName].dbo.[TableName]
-- [DatabaseName]..[TableName]
-- DatabaseName.dbo.[TableName]
-- DatabaseName..[TableName]
-- [DatabaseName].dbo.TableName
-- [DatabaseName]..TableName
-- DatabaseName.dbo.TableName
-- DatabaseName..TableName
-- You can try different combinations to get expected results.
```
