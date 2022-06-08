## Data Lake

Hierarchical namespaces organize blob data into directories and stores metadata about each directory and the files within it.

Because Azure Data Lake Storage Gen2 is integrated into the Azure Storage platform, applications can use either the Blob APIs or the Azure Data Lake Storage Gen2 file system APIs to access data.

### Massive Parallel Processing

DMS : Data Movement System

distributions is where data is calculated (unit) there are 60 distributions that are divided on the number of compute nodes. A node can have between 1 to 60 distributions.

Types of distributions.

- Hash:
  Big tables and fact tables. 2GB size minimum
- Round Robin
  This is default
  Distribution evenly
  Staging tables
  Small tables
- Replicate
  Small lookup tables

Syntax

```sql
create table mytable

( id int not null,
   fname varchar(20))

with
(
    distribution = Hash (id),
    Clustered columnstore Index (fname)
)

```

### Indexes in Synapses

#### Columnstore

Default is columnstore index

Best query performance and level of performance

does not support varchar(max), nvarchar(max) or varbinary(max)
use on tables with more than 60 million rows.

#### Head tables

landing table
When index would slow down and take unnecessary space

#### clustered and nonclustered indexes

clustered the field needs to be unique.

## Polybase

Access hadoop or storage data via a SQL interface

## Azure SQL DB

Pricing tiers

### DTU (Database Transaction Unit)

- Basic
- Standard
- Premium

### Vcore

Recommended for production environments.

- General Purpose
- Hyperscale (can configure secondary replicas)
- Business critical (highest resilience)

Options (serverless or provisioned if it is general purpose option)

serverless are based on usage
provisioned is fixed cost

Hybrid benefit if you already a SQL service license.

## Datalake Gen 1

- To control access use POSIX not RBAC
  https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-access-control

for Gen2

https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-access-control-model
