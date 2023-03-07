# DynamoDB

- NoSQL
- key-value

Supports `AWS::Backup`

## CAP Theorem

CAP

- Consistency (same across all replication points before returning result)
  - DynomDB supports *strongly* or *eventually* consistent reads
- Availability
- Partition Tolerance

Of these 3, we can only have 2 at the same time.

## NoSQL

No cross table relationships

3 main types:

- Key-value (DynamoDB)
- Column-based (eg Cassandra)
- Document-based (eg MongoDB)

---

## Table

A collection of 0 or more items.
Items can be queried through their keys.
Items are made up of their attributes

`primary key` (or `partition key`) is mandatory.
`sort key` is optional.

**Note**: Primary key or sort key **can** be duplicated, but **not both** at the same time.

Extra keys are attributes.

By default queries are only supported on partition and sort key. To support
queries on other keys, secondary indexes are needed.

## Item

- Max size is 400 kB
- `partition key` min length is 1 byte, max length 2048 bytes
- `sort key` min length is 1 byte, max length 1024 bytes

Items need not have the same number of attributes.

## Query

Find items based on

- `partition key` value alone, or
- `partition key` value and `sort key` value in cases where sort key is defined

## Scan

```
aws dynamodb scan --table-name <TABLE>
```

Retrieves all items in a table.

More costly than a query.

**Note**: filters will happen *after* a scan is complete, so it will **not**
make the scan more efficient.

## Secondary Index

Basically creates a copy of the table with an alternate key schema.

- *Local secondary index*, allows to create an alternate sort key
- *Global secondary index*, allows to create an alternate partition and sort key

## AWS CLI

```
aws dynamodb list-tables
aws dynamodb put-item --table-name <TABLE> --item file://<JSON FILE>
aws dynamodb delete-item --table-name <TABLE> --key file://<JSON FILE>
aws dynamodb query --table-name <TABLE> --key-condition-expression ... --expression-attribute-values file://<JSON FILE>
```
