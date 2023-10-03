  ```
    DESCRIBE HISTORY table_name
    DESCRIBE EXTENDED table_name
    REFRESH TABLE table_name
    OPTIMIZE table_name

    DROP DATABASE customers CASCADE
    SHALLOW CLONE OR DEEP CLONE for cloning tables as an option on create tables
      CREATE OR REPLACE TABLE {new_table_name} SHALLOW CLONE {source_table_name}|[LOCATION path]

    CREATE TABLE users_jdbc
    USING org.apache.spark.sql.jdbc
    OPTIONS (
        url = "jdbc:sqlite:/sqmple_db",
        dbtable = "users"
    )

    ## Nulls and constraints
    CREATE TABLE events( id LONG,
                         date STRING, 
                        location STRING,
                        description STRING 
                        ) USING DELTA;
    ALTER TABLE events CHANGE COLUMN id SET NOT NULL;    
    ALTER TABLE events ADD CONSTRAINT dateWithinRange CHECK (date > '1900-01-01');

    ## Streaming - SQL
    CREATE OR REFRESH STREAMING TABLE customers
      AS SELECT * FROM cloud_files("/databricks-datasets/retail-org/customers/", "csv", map("delimiter", "\t", "header", "true"))


    ## Streaming - python
    @dlt.table
    def wiki_raw():
      return (
        spark.readStream.format("cloudFiles")
          .schema("title STRING, id INT, revisionId INT, revisionTimestamp TIMESTAMP, revisionUsername STRING, revisionUsernameId INT, text STRING")
          .option("cloudFiles.format", "parquet")
          .load("/databricks-datasets/wikipedia-datasets/data-001/en_wikipedia/articles-only-parquet")
      )

    ## Create Database
    CREATE DATABASE sales LOCATION 'dbfs:/mnt/delta/databases/sales.db/'

```
## Explode, Flatten
- **Explode** is table-valued function, takes an array or map and returns a row for each element in the array.
- **Flatten** is a function that takes an array of elements and converts them to a list


