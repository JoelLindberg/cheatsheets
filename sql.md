# SQL Server

## Database

Create:
~~~sql
CREATE DATABASE database_name
~~~

More: https://learn.microsoft.com/en-us/sql/t-sql/statements/create-database-transact-sql?view=sql-server-ver16&tabs=sqlpool

Remove:
~~~sql
DROP DATABASE database_name
~~~

More: https://learn.microsoft.com/en-us/sql/t-sql/statements/drop-database-transact-sql?view=sql-server-ver16

<br />
<br />
<br />

## Table

Create:
~~~sql
CREATE TABLE Transfers (
    TransferId int NOT NULL IDENTITY,
    TransferData nvarchar(1024)
)
~~~

Remove:
~~~sql
DROP TABLE Transfers;
~~~

<br />
<br />
<br />

### Relationships

~~~sql
CREATE TABLE [Posts] (
    [Id] int NOT NULL IDENTITY,
    [Title] nvarchar(max) NULL,
    [Content] nvarchar(max) NULL,
    [PublishedOn] datetime2 NOT NULL,
    [Archived] bit NOT NULL,
    [BlogId] int NOT NULL,
    CONSTRAINT [PK_Posts] PRIMARY KEY ([Id]),
    CONSTRAINT [FK_Posts_Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [Blogs] ([Id]) ON DELETE CASCADE);
~~~

~~~sql
CREATE TABLE [Blogs] (
    [Id] int NOT NULL IDENTITY,
    [Name] nvarchar(max) NULL,
    [SiteUri] nvarchar(max) NULL,
    CONSTRAINT [PK_Blogs] PRIMARY KEY ([Id]));
~~~

More: https://learn.microsoft.com/en-us/sql/t-sql/statements/create-database-transact-sql?view=sql-server-ver16&tabs=sqlpool

<br />
<br />
<br />

### Insert data

~~~sql
INSERT INTO {table_name} [column_name]
{
  VALUES ()
}
~~~

~~~sql
INSERT INTO Transfers (TransferData)
VALUES ('some data here');
~~~

More: https://learn.microsoft.com/en-us/sql/t-sql/data-types/date-and-time-types?view=sql-server-ver16

<br />
<br />
<br />

### Update data

More: https://learn.microsoft.com/en-us/sql/t-sql/statements/delete-transact-sql?view=sql-server-ver16

<br />
<br />
<br />

### Delete data

More: https://learn.microsoft.com/en-us/sql/t-sql/queries/update-transact-sql?view=sql-server-ver16

<br />
<br />
<br />

### Data types

* int
* nvarchar

More: https://learn.microsoft.com/en-us/sql/t-sql/data-types/data-types-transact-sql?view=sql-server-ver16

Date and time:
More: https://learn.microsoft.com/en-us/sql/t-sql/data-types/date-and-time-types?view=sql-server-ver16

<br />
<br />
<br />

# PostgreSQL

## Create table
CREATE TABLE beverages (
    id bigserial,
    name varchar(64),
    cost real
)

<br />

## Insert data
INSERT INTO beverages(id, name, cost) 
VALUES (0, 'Coffee', 4.28);

<br />

## Delete data
DELETE FROM beverages WHERE id = 0;
