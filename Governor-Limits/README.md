# Salesforce Governor Limits

## Why Governor Limits Exist
Salesforce is a multi-tenant platform where multiple customers share resources.

## Common Governor Limits

### SOQL Queries
- 100 synchronous SOQL queries per transaction
- 200 asynchronous SOQL queries per transaction

### DML Statements
- 150 DML statements per transaction

### Records Retrieved
- 50,000 records via SOQL

### Heap Size
- 6 MB synchronous
- 12 MB asynchronous

## Best Practices

- Avoid SOQL inside loops
- Avoid DML inside loops
- Use collections (List, Set, Map)
- Use bulkified code
- Use asynchronous processing when needed

## Example

Bad:
```apex
for(Account acc : accounts){
    Contact c = [SELECT Id FROM Contact WHERE AccountId = :acc.Id];
}

Good:
```apex
Set<Id> accountIds = new Set<Id>();

for(Account acc : accounts){
    accountIds.add(acc.Id);
}

List<Contact> contacts = [
    SELECT Id, AccountId
    FROM Contact
    WHERE AccountId IN :accountIds
];
