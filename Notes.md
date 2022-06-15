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

## Databricks

There are 3 type of Environment and Workloads in Databricks

- Data Science and Engineer
- SQL
- Machine Learning

### Workspaces

Unified environment where users can access their Databricks assets.
It can also be understood as the UI of your environment. This is applicable for Data Science and Engineer not to SQL.

Bulling: Based on DBU (Databricks units). It is based on the hours of the VM instance type used to run the clusters.

Authentication and authorization can be done via ACL (Access control lists) where you can specify who can access what.

#### Data Science and Engineering

Classical workspace where data scientists, data analysts and data engineering can collaborate.

A workspace have components like

- Notebooks
- Dashboard
- Library
- Repo : a folder whose contents are co-versioned together by syncing them to a remote git repository.
- Experiments

Databricks can be accessed via the UI (Web browser), a CLI and also a rest API. Three versions 2.0, 2.1 and 1.2. The preferred method is the 2. versions.

Data Management:

DBFS (Databricks file system). It is an storage layer abstraction that sits on a blog storage.

Database: Collection of information that are organized and be easily accessed

Table: Representation of a data structure

Metastore: store structure information about tables and partitions and metadata about the tables and where it is. It is based on Hive metastore and accessed by all clusters to persist table metadata.

**computation**

#### Clusters

a set of computation resources and configuration on which you run notebooks and jobs.

Types of cluster

- all purpose clusters. For collaboration and can terminate and re-start.
- job cluster (created by a job and terminate when it is complete). You cannot re-start it.
  x

#### Pool

A set of idle, ready to use instances that reduce cluster start and auto-scaling times. You can attach a cluster to a pool than it allocated its drivers and worker nodes from the pool.

#### Runtime

Offers various such as

- Databricks: Spark based for big data
- Machine Learning: Contain popular ML Libraries such as Tensorflow, Keras, Pytorch and XGboost
- Genomics: Optimized for genomics analysis
- Light: Open source Apache Spark. Use for Jar, Python and Spark submit jos. **Do not select this to interact with notebooks**.

#### Workflows

With jobs: non interactive mechanism for running notebooks on a schedule
Delta live tables: framework to building reliable pipelines

#### Workload

Data Engineering (automated): Workload run jobs on a job cluster and a schedule run each workload.
Data Analytics (interactive): Run on a all-purpose cluster. Run on a notebook.

#### Machine Learning

Experiments: Maintain track of models for developed
Feature Store: Repository of features
Models: Keep a registry of models developed

#### Databricks SQL

Data Management:

- Visualisation
- Dashboard
- Alert: Notification when a query reached a threshold

Computation:

- Query
- [SQL Endpoint](https://docs.microsoft.com/en-us/azure/databricks/sql/admin/sql-endpoints): A computation resource on with you execute the SQL query
- Query history

Authentication:

- User groups
- Personal token
- Access Control list (ACL)

### Objects in Databricks

**Metastore**

Contains the metadata and has 3 options

Unity Catalogue
Hive Metastore
External metastore

**Catalogue**

Highest abstraction. Like "schema" a database will belong to a catalogue

```bash
catalog_name.database_name.table_name
```

**database**

collection of data object including tables, views and functions

**Table**

A collection of structured data.
It is stored in a directory of the file system.
A Delta table is the default storage in databricks.
When a Delta table is created it provides references to data through a metastore.
The delta table will allow ACID transaction and allow time travel.

this is how it works like SQL

```sql
CREATE TABLE table_name AS SELECT * FROM another_table
```

There are 2 types of tables.

managed and unmanaged

A managed table behaves more like a Table in a database and live in a `LOCATION` if the table is deleted aldo it's data is deleted in the `LOCATION` it is registered.

os opposed to an UNmanaged table or external tables. You will always have to specify the location of the unmanaged table.

```sql
CREATE TABLE table_name
USING DELTA
LOCATION '/path/to/existing/data'
```

**View**

store the text for a query that can refer to 1 or more tables.

**Temporary View**

Has limited scope and persistence and is not registered to a schema or catalogue.

**Function**

Allow to store a user defined logic to be reused.
