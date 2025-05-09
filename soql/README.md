# SOQL

Salesforce Object Query Language (SOQL) is the query language used to retrieve data from Salesforce. It is similar to SQL but is specifically designed for Salesforce's data model and architecture.

It can be used to query Salesforce objects, such as Accounts, Contacts, and Opportunities, and retrieve specific fields and records based on certain criteria.

## Table of Contents
- [Query a single object](#query-a-single-object)
- [Query a single object with criteria](#query-a-single-object-with-criteria)
- [Query multiple objects](#query-multiple-objects)
- [Query multiple objects with criteria](#query-multiple-objects-with-criteria)
- [Query using binding](#query-using-binding)
- [Query using IN](#query-using-in)
- [Query by IDs](#query-by-ids)
- [Subqueries](#subqueries)
- [Subqueries for Custom Relationships](#subqueries-for-custom-relationships)
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

## Query using binding

Binding is a way to pass variables into a SOQL query. This allows you to use dynamic values in your queries, making them more flexible and reusable. Binding is done using the `:` character followed by the variable name.
For example, to retrieve contacts with a specific name, you can use the following syntax:

```apex
String contactName = 'John Doe';

List<Contact> contacts = [SELECT Id, FirstName, LastName FROM Contact WHERE Name = :contactName];
```

This query retrieves all contacts with the name `John Doe`. The `contactName` variable is bound to the query, allowing you to use dynamic values in your queries.

You can use binding in `WHERE` and `LIMIT` clauses.


## Query using IN

To query records using the `IN` operator, you can use the following syntax:

```apex
List<Lead> leads = [SELECT Id, FirstName, LastName FROM Lead WHERE LeadSource IN ('Web', 'Phone', 'Email')];
```

It will retrieve all leads where the `LeadSource` is either `Web`, `Phone` or `Email`. It is similar to use `LeadSource = 'Web' OR LeadSource = 'Phone' OR LeadSource = 'Email'`, but it is more concise and easier to read.

You can also use the `IN` operator with a binding. For example, to retrieve leads with specific lead sources, you can use the following syntax:

```apex
public class LeadSelector {
    public static List<Lead> getLeadsBySource (Set<String> leadSources) {
        return [SELECT Id, FirstName, LastName FROM Lead WHERE LeadSource IN :leadSources];
    }
}
```

## Query by IDs

To query records by IDs, you can use the `IN` operator with a `Set<Id>` or `List<Id>` to pass the IDs dynamically. For example, to retrieve leads with specific IDs, you can use the following syntax:

```apex
public class LeadSelector {
    public static List<Lead> getLeadsByIds (Set<Id> leadIds) {
        return [SELECT Id, FirstName, LastName FROM Lead WHERE Id IN :leadIds];
    }
}
```

> [!TIP]
> It is a good idea to use `Set<Object>` because `IN` clause does not care about element order or duplicated elements. So using a `Set<Object>` can be more efficient than using a `List<Object>`.

> [!WARNING]
> Do not hardcode IDs in your code. It is a bad practice and can lead to issues when deploying to different environments. Instead, use a `Set<Id>` or `List<Id>` to pass the IDs dynamically.

## Subqueries

Suppose you want to retrieve all accounts with their related contacts. You could use two separate queries to achieve this. For example, you could first retrieve all accounts and then retrieve all contacts related to those accounts based on the account IDs that you retrieved in the first query. The code would look like this:

```apex
List<Account> accounts = [SELECT Id, Name FROM Account];

Set<Id> accountIds = new Set<Id>();

for (Account account : accounts) {
    accountIds.add(account.Id);
}

List<Contact> contacts = [SELECT Id, FirstName, LastName FROM Contact WHERE AccountId IN :accountIds];
```

This code retrieves all accounts and then retrieves all contacts related to those accounts. However, this approach is not efficient because it requires two separate queries.

Instead, you can use a subquery to retrieve the related records in a single query. Subqueries are used to retrieve related records in a single query. For example, to retrieve all accounts with their related contacts, you can use the following syntax:

```apex
List<Account> accounts = [SELECT Id, Name, (SELECT Id, FirstName, LastName FROM Contacts) FROM Account];
```
This query retrieves all accounts with their related contacts. The subquery `(SELECT Id, FirstName, LastName FROM Contacts)` retrieves all contacts related to each account.

You can also use subqueries with `WHERE` and `LIMIT` clauses. For example, to retrieve all accounts with their related contacts where the contact's last name is 'Smith', you can use the following syntax:

```apex
List<Account> accounts = [SELECT Id, Name, (SELECT Id, FirstName, LastName FROM Contacts WHERE LastName = 'Smith') FROM Account];
```

Now, to access the related contacts, you can use the `Contacts` relationship name. For example, to debug each contact related to each account, you can use the following code:

```apex
for (Account account : accounts) {
    System.debug('Account: ' + account.Name);

    for (Contact contact : account.Contacts) {
        System.debug('Contact: ' + contact.FirstName + ' ' + contact.LastName);
    }
}
```

> [!TIP]
> You can use subqueries to retrieve related records in a single query. This can improve performance and reduce the number of queries required to retrieve related records.

## Subqueries for Custom Relationships
If you have a custom relationship, you can also use a subquery. For example, if you have a custom object `Visit__c` with a lookup relationship to `Account`, you can use the following syntax:

```apex
List<Account> accounts = [SELECT Id, Name, (SELECT Id, Date__c FROM Visits__r) FROM Account];
```

Note that the subquery uses the relationship name `Visits__r` instead of the object name `Visit__c`. This is because the relationship name is used to reference the related records in the subquery.

> [!TIP]
> You must use the relationship name, not the object name, in the subquery. The relationship name is defined in the custom object definition and is used to reference the related records in the subquery.

## Limits

## Further Reading