- GET INTO THE POSTGRES SQL SHELL  
  * `psql` or `psql [your_db_name]`

- Create database  
  * `createdb [your_db_name]` (this is from oustide psql, just at the CLI)
  * or `CREATE DATABASE [your_db_name];`

- Create table:
  * `CREATE TABLE [your_table_name] (col_name data type, col_name data type,...);`

    |Data Types|   |
    |---|---|
    |Serial |auto generates id numbers  |
    |Text |has a large area for text  |
    |varchar |has a default limit of 255 characters |
    |integer |integers (duh) |
    |timestamp| timestamps for table creation, table updates |


  * `INSERT INTO [your_table_name] (col_name, col_name, col_name,..) VALUES (col_val, col_val, col_val,...);`
  * `SELECT [col_name] FROM [your_table_name];` (to see specific data inside a column)
  * `SELECT * FROM [your_table_name];` (to see all data inside table)
  * `SELECT * FROM  [your_table_name] WHERE [col_name = some_value];` (to filter)

  * `UPDATE [your_table_name] SET [col_name = updated_value] WHERE [col_name = identifying_value];`

- Delete database
  * `deletedb [your_db_name]` (this is from outside psql, just at the CLI)
  * `DROP DATABASE [your_db_name];`

* * *

SOME COMMANDS 

|command|what it does|
|---|---|
|l+ |list all dbs, + = more detail |
|\c [your_db_name] |to get into db (connect to db) |
|\d [table_name] |see specific table|
|\dt + |list all tables in db|
|\q |quit|

