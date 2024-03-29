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

## Design

### Queries first

Design of DynamoDB tables requires query patterns to be evaluated *first*.

**Note**: DynamoDB does not support joins.

In order to limit the number of round trips, the data must be ordered in a way
to minimize the number of calls to the database.

### Partitioning

DynamoDB is designed to partition the underlying data into different storages.

The partitioning is guided by the partition key, therefore it is very important
to design the partition key so that the data is distributed equally. Otherwise
you can get hot partitions that impede the performance. 

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

### Item types

- `S` – String
- `N` – Number
- `B` – Binary
- `BOOL` – Boolean
- `NULL` – Null
- `M` – Map
- `L` – List
- `SS` – String Set
- `NS` – Number Set
- `BS` – Binary Set

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

Retrieves all items in a table. More costly than a query.

Scans can be supplied with the `--max-items <NUMBER>` flag.

**Note**: filters will happen *after* a scan is complete, so it will **not**
make the scan more efficient.

**Note**: Scans only returns data up to a limit (by default 1MB).

**Note**: Scans are by default paginated.

## Secondary Index

Basically creates a copy of the table with an alternate key schema.

- *Local secondary index*, allows to create an alternate sort key
- *Global secondary index*, allows to create an alternate partition and sort key

## `BatchGetItem` & `BatchWriteItem`

Each write in `BatchWriteItem` is atomic, but the command as whole is **not**.

`BatchGetItem` is *eventually consistent* by default.

**Note**: You will be provided with a list of unprocessed items.

## AWS CLI

```
aws dynamodb list-tables
aws dynamodb put-item --table-name <TABLE> --item file://<JSON FILE>
aws dynamodb delete-item --table-name <TABLE> --key file://<JSON FILE>
aws dynamodb query --table-name <TABLE> --key-condition-expression ... --expression-attribute-values file://<JSON FILE>
```
