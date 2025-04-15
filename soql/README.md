# SOQL

Salesforce Object Query Language (SOQL) is the query language used to retrieve data from Salesforce. It is similar to SQL but is specifically designed for Salesforce's data model and architecture.

It can be used to query Salesforce objects, such as Accounts, Contacts, and Opportunities, and retrieve specific fields and records based on certain criteria.

## Table of Contents
  - [Query a single object](#query-a-single-object)
  - [Query a single object with criteria](#query-a-single-object-with-criteria)
  - [Query multiple objects](#query-multiple-objects)
  - [Query multiple objects with criteria](#query-multiple-objects-with-criteria)
  - [Query using IN](#query-using-in)
  - [Query by IDs](#query-by-ids)
  - [Limits](#limits)
  - [Further Reading](#further-reading)

## Query a single object

To query a single object, you can use the following syntax:

```apex
Account account = [SELECT Id, Name FROM Account];
```

This query retrieves a single record from the `Account` object with the `Id` and `Name` fields. However, if the query returns more than one record, an `System.QueryException` will be thrown. Therefore, you can use the `LIMIT` clause to limit the number of records returned to one:

```apex
Account account = [SELECT Id, Name FROM Account LIMIT 1];
```
> [!WARNING]
> When you use `LIMIT 1`, it is important to note that, if there are multiple records, only one record will be returned. This may not be the record you expect, so it is important to ensure that your query is specific enough to return the desired record.

There are cases in which allowing the system to throw an exception is more appropriate. For example, if you are querying for a single record based on a unique field, such as `Email` or `Username`, it is expected that only one record will be returned. If multiple records are returned, it indicates that there is a data integrity issue that needs to be addressed. In this case, you can use a try-catch block to handle the exception and return null or throw a custom exception:

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

## Query a single object with criteria

To query a single object with criteria, you can use the `WHERE` clause to filter the records based on specific conditions. For example, to retrieve a single account with a specific name, you can use the following syntax:

```apex
Account account = [SELECT Id, Name FROM Account WHERE Name = 'Acme Corporation'];
```

This query retrieves a single record from the `Account` object where the `Name` field is equal to 'Acme Corporation'. If the query returns more than one record, an `System.QueryException` will be thrown. Therefore, you can use the `LIMIT` clause to limit the number of records returned to one. So if the query matches multiple records, only the first one will be returned, and no exception will be thrown.

## Query multiple objects

To query multiple objects, you must use a `List<SObject>` to store the results. For example, to retrieve all accounts, you can use the following syntax:

```apex
List<Account> accounts = [SELECT Id, Name FROM Account];
```

## Query multiple objects with criteria
To query multiple objects with criteria, you can use the `WHERE` clause to filter the records based on specific conditions. For example, to retrieve all accounts with a specific name, you can use the following syntax:

```apex
List<Account> accounts = [SELECT Id FROM Account WHERE Name = 'Acme Corporation'];
```

## Query using IN

To query records using the `IN` operator, you can use the following syntax:

```apex
List<Lead> leads = [SELECT Id, FirstName, LastName FROM Lead WHERE LeadSource IN ('Web', 'Phone', 'Email')];
```

It will retrieve all leads where the `LeadSource` is either `Web`, `Phone` or `Email`. It is similar to use `LeadSource = 'Web' OR LeadSource = 'Phone' OR LeadSource = 'Email'`, but it is more concise and easier to read.

## Query by IDs

To query records by IDs, you can use the `IN` operator with a `Set<Id>` or `List<Id>`. For example, to retrieve leads by their IDs, you can use the following syntax:

```apex
public class LeadSelector {
    public static List<Lead> getLeadsByIds (Set<Id> leadIds) {
        return [SELECT Id, FirstName, LastName FROM Lead WHERE Id IN :leadIds];
    }
}
```

> [!TIP]
> It is a good idea to use `Set<Object>` because SOQL does not care about order or duplicates. So using a `Set<Object>` can be more efficient than using a `List<Object>`.

> [!WARNING]
> Do not hardcode IDs in your code. It is a bad practice and can lead to issues when deploying to different environments. Instead, use a `Set<Id>` or `List<Id>` to pass the IDs dynamically.

## Limits

## Further Reading