# Bulkification

Bulkification is a pattern in Apex that allows you to process multiple records safely without hitting governor limits. This is especially important in Salesforce, where you have very specific limits of SOQL queries and DML operations.

Some of the most common governor limits, in a single transaction, are:
- **SOQL Queries**: You can only execute 100 SOQL queries.
- **DML Operations**: You can only perform 150 DML operations.

## Table of Contents
- [SOQL Queries](#soql-queries)
- [DML Operations](#dml-operations)

## SOQL Queries

Let's imagine you have a list of Account IDs and you need to update these accounts. You could easily write a loop that queries each account one by one, like this:

```apex
for (Id accountId : accountIds) {
    Account account = [SELECT Id, Name FROM Account WHERE Id = :accountId];
    
    someLogicToAccount(account);
}
```

It would work fine for a small number of records, but if you have a large number of account IDs, you will hit the governor limit for SOQL queries.

Instead, you should query all the accounts in a single SOQL query and then process them in memory. Here's how you can do that:

```apex
List<Account> accounts = [SELECT Id, Name FROM Account WHERE Id IN :accountIds];

for (Account account : accounts) {
    someLogicToAccount(account);
}
```

This way, you only perform one SOQL query, regardless of how many account IDs you have. This is the essence of bulkification: process multiple records in a single transaction to avoid hitting governor limits.

## DML Operations
Similarly, when you need to perform DML operations (like insert, update, delete), you should also do it in bulk. Instead of performing DML operations inside a loop, you should collect all the records you want to update in a list and then perform a single DML operation.


Let's say you want to update the Parent Account for a list of accounts. Instead of updating each account one by one, like this:

```apex
for (Account account : accounts) {
    account.ParentId = parentAccountId;
    
    update account;
}
```

You should collect all the accounts in a list and perform a single update operation:

```apex
List<Account> accountsToUpdate = new List<Account>();

for (Account account : accounts) {
    account.ParentId = parentAccountId;

    accountsToUpdate.add(account);
}

update accountsToUpdate;
```

This way, you only perform one DML operation, which is much more efficient and avoids hitting governor limits.
