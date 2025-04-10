# SOQL

Salesforce Object Query Language (SOQL) is the query language used to retrieve data from Salesforce. It is similar to SQL but is specifically designed for Salesforce's data model and architecture.

It can be used to query Salesforce objects, such as Accounts, Contacts, and Opportunities, and retrieve specific fields and records based on certain criteria.

## Table of Contents
  - [Query a single object](#query-a-single-object)
  - [Query a single object with criteria](#query-a-single-object-with-criteria)
  - [Query multiple objects](#query-multiple-objects)
  - [Query multiple objects with criteria](#query-multiple-objects-with-criteria)
  - [Grouping and Aggregation](#grouping-and-aggregation)
    - [Aggregate Functions](#aggregate-functions)
  - [Filtering](#filtering)
    - [Equal](#equal)
    - [Not Equal](#not-equal)
    - [Greater Than](#greater-than)
    - [Less Than](#less-than)
    - [Greater Than or Equal](#greater-than-or-equal)
    - [Less Than or Equal](#less-than-or-equal)
    - [IN](#in)
    - [LIKE](#like)
    - [AND](#and)
    - [OR](#or)
    - [NULL](#null)
    - [BETWEEN](#between)
  - [SOQL Syntax](#soql-syntax)
    - [SOQL Basic Syntax](#soql-basic-syntax)
    - [SOQL Syntax Components](#soql-syntax-components)
  - [Limits](#limits)
  - [Further Reading](#further-reading)

## Query a single object

To query a single object, you can use the following syntax:

```apex
Account account = [SELECT Id, Name FROM Account];
```

This query retrieves a single record from the `Account` object with the `Id` and `Name` fields.

If the query returns more than one record, an exception will be thrown. You can use the `LIMIT` clause to limit the number of records returned to one:

```apex
Account account = [SELECT Id, Name FROM Account LIMIT 1];
```

Now, if the query matches multiple records, only the first one will be returned, and no exception will be thrown.

But sometimes you may want to handle the exception when a query returns more than one record. For example, if you are querying for a single record based on a unique field, such as an email address or a username, it is expected that only one record will be returned. If multiple records are returned, it indicates that there is a data integrity issue that needs to be addressed. In this case, you can use a try-catch block to handle the exception and return null or throw a custom exception:

```apex
class ContactSelector {
    public static Contact getContactByEmail (String email) {
        try {
            return [SELECT Id, FirstName, LastName FROM Contact WHERE Email = :email];
        } catch (QueryException ex) {
            return null;
        }
    }
}
```

> [!CAUTION]
> To query a single object may throw an exception if the query returns more than one record.

> [!TIP]
> To avoid exceptions when querying a single object, you can use the `LIMIT` clause to limit the number of records returned to one or use a try-catch block to handle the exception.

## Query a single object with criteria

To query a single object with criteria, you can use the `WHERE` clause. For example, to retrieve all accounts with a specific name, you can use the following syntax:

```sql
SELECT Id, Name FROM Account WHERE Name = 'Acme Corporation'
```

This query retrieves the `Id` and `Name` fields from all records in the `Account` object where the `Name` field is equal to 'Acme Corporation'.

## Query multiple objects
To query multiple objects, you can use a subquery. For example, to retrieve all accounts and their related contacts, you can use the following syntax:

```sql
SELECT Id, Name, (SELECT Id, FirstName, LastName FROM Contacts) FROM Account
```

> [!IMPORTANT]
> A subquery must be enclosed in parentheses and must use the relationship name. The relationship name is the name of the child object in the parent-child relationship. It can differ from the object name itself.

> [!NOTE]  
> No more than 55 child relationship queries are allowed in a single SOQL query.

## Query multiple objects with criteria
To query multiple objects with criteria, you can use the `WHERE` clause in the main query or in the subquery. For example, to retrieve all accounts with a specific name and their related contacts, you can use the following syntax:

```sql
SELECT Id, Name, (SELECT Id, FirstName, LastName FROM Contacts) FROM Account WHERE Name = 'Acme Corporation'
```

This query retrieves the `Id` and `Name` fields from all records in the `Account` object where the `Name` field is equal to 'Acme Corporation', along with the `Id`, `FirstName`, and `LastName` fields from the related `Contacts` object.

Alternatively, to retrieve all accounts and their related contacts with a specific Last Name, you can use the following syntax:

```sql
SELECT Id, Name, (SELECT Id, FirstName, LastName FROM Contacts WHERE LastName = 'Smith') FROM Account
```

This query retrieves the `Id` and `Name` fields from all records in the `Account` object, along with the `Id`, `FirstName`, and `LastName` fields from the related `Contacts` object where the `LastName` field is equal to 'Smith'.

## Grouping and Aggregation
SOQL supports grouping and aggregation functions, such as `COUNT()`, `SUM()`, and `MAX()`. You can use the `GROUP BY` clause to group records by a specific field and apply aggregation functions to the grouped records.

For example, to count the number of accounts by industry, you can use the following syntax:
```sql
SELECT Industry, COUNT(Id) FROM Account GROUP BY Industry
```

This query retrieves the `Industry` field and the count of `Id` fields from all records in the `Account` object, grouped by the `Industry` field.


> [!IMPORTANT]
> You can only use aggregate functions in the `SELECT` clause when using the `GROUP BY` clause. You cannot use aggregate functions in the `WHERE` clause.

> [!IMPORTANT]
> You can only use select fields using aggregate functions in the `SELECT` clause or fields that are included in the `GROUP BY` clause. You cannot use other fields in the `SELECT` clause.

### Aggregate Functions
SOQL supports several aggregate functions that can be used to perform calculations on the data returned by a query. These functions can be used in conjunction with the `GROUP BY` clause to summarize data. Here's the list of aggregate functions supported by SOQL:
- `COUNT()`: Returns the number of records that match the query.
- `COUNT(field)`: Returns the number of non-null values in a specified field for the records that match the query.
- `SUM()`: Returns the sum of a numeric field for the records that match the query.
- `AVG()`: Returns the average of a numeric field for the records that match the query.
- `MIN()`: Returns the minimum value of a field for the records that match the query.
- `MAX()`: Returns the maximum value of a field for the records that match the query.

## Filtering
SOQL supports filtering records using the `WHERE` clause. You can use various operators to filter records based on specific criteria.

### Equal
### Not Equal
### Greater Than
### Less Than
### Greater Than or Equal
### Less Than or Equal
### IN
### LIKE
### AND
### OR
### NULL
### BETWEEN

## SOQL Syntax
SOQL queries are case-insensitive, but it is a good practice to use uppercase for keywords and lowercase for object and field names. The basic syntax of a SOQL query is as follows:

### SOQL Basic Syntax

```sql
SELECT <fields|subqueries> FROM <object> [WHERE <condition>] [ORDER BY <field> ASC|DESC] [LIMIT <number>] 
```

### SOQL Syntax Components
SOQL queries are written in a specific syntax that consists of the following components:
- **SELECT**: Specifies the fields to retrieve from the object.
- **FROM**: Specifies the object to query.
- **WHERE**: Specifies the criteria to filter the records.
- **ORDER BY**: Specifies the order in which to return the records.
- **LIMIT**: Specifies the maximum number of records to return.
- **OFFSET**: Specifies the number of records to skip before returning the results.
- **GROUP BY**: Specifies the fields to group the results by.
- **HAVING**: Specifies the criteria to filter the grouped records.
- **IN**: Specifies a list of values to match against.
- **LIKE**: Specifies a pattern to match against.
- **AND**: Combines multiple criteria.
- **OR**: Combines multiple criteria.
- **NOT**: Negates a condition.
- **NULL**: Represents a null value.


## Limits

## Further Reading