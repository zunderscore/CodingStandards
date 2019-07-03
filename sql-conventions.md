# SQL Conventions

## Tables

### Naming Conventions

Tables will be named using **PascalCase** in a plural form of the object they store. Columns will also be in **PascalCase**, named using a short but descriptive name.

_Example:_

A table that stores application errors will be named `ApplicationErrors`. A column in that table that stores a stack trace of the error message would be named `StackTrace`.

### Identity Column

All tables must have a single, unique, numeric (either `int` or `bigint`, depending on projected table size) identity column named based on the singular form of the table name. Anywhere an identity column is used, whether in the table where it is the primary key or another table where it is a foreign key, it must have the `ID` postfix attached to it.

_Example:_

The `ApplicationErrors` table will have an identity column named `ApplicationErrorID`.

### Primary Key

The identity column will ALWAYS be the primary key for a table, no exceptions. The primary key will be named with the prefix `PK`, the table name, and the column name, all separated by underscores.

_Example:_

The `ApplicationErrors` table has a primary key named `PK_ApplicationErrors_ApplicationErrorID` based on the `ApplicationErrorID` identity column.

### Foreign Keys

Any tables that contain identity columns that refer to other tables MUST have a foreign key relationship setup. Unless otherwise specified in application requirements, foreign keys should be setup to cascade on `DELETE` and `UPDATE` operations. Foreign key columns should have the same name as the primary key column they reference. Foreign keys will be named with the prefix `FK`, the foreign key table’s name, the primary key table’s name, and the primary key column’s name, all separated by underscores.

_Example:_

The `UserRoles` table has a `UserID` column that is a foreign key referring to the `UserID` column in the `Users` table, which will be used as the primary key in the foreign key relationship. The foreign key is named `FK_UserRoles_Users_UserID`. If a row in `Users` is deleted, all `UserRoles` rows associated with that user should be deleted automatically.

### Indexes

All tables and/or partition schemes will have at least one index. The primary key will be used for the clustered index for the table in most instances. However, there will be instances where another column is used instead, and the primary key index will be non-clustered (e.g. tables where a specific date column is the preferred primary sort column). Indexes will be named with the prefix `IX`, the table/partition scheme name, and any columns (usually only one) that are part of that specific index, all separated by underscores.

_Example:_

The `ApplicationErrors` table has an index on the `ErrorOccurred` datetime column because that column is frequently used in queries. The index is named `IX_ApplicationErrors_ErrorOccurred`.

## Views

### Usage

Views are to be used to perform repeated simple read operations against aggregate data from two or more tables. If you need to perform more than a single read operation against a set of data, then you should use a stored procedure instead.

### Naming Conventions

Stored procedures will be named with a short but descriptive name in **PascalCase**, prefixed with the letter `v` and an underscore.

_Example:_

`v_VehicleOperators`

## Stored Procedures

### Usage

Stored procedures are to be used to perform repeated complex database operations (anything beyond basic CRUD operations).

### Naming Conventions

Stored procedures will be named with a short but descriptive name in **PascalCase**, prefixed with the letters `sp` and an underscore.

_Example:_

`sp_GetEventsByVehicleID`

### Stored Procedures vs. In-Memory Code Operations

In-memory code should be the default for any operation that is NOT data-heavy (e.g. reporting or dashboards). However, if an in-memory operation is proving to be an application performance bottleneck, it should be converted to a stored procedure to offload the operation to the database engine. For any data-heavy operations (e.g. reporting or dashboards), stored procedures are always preferred over in-memory operations.
