  ```
    DESCRIBE HISTORY table_name
    OPTIMIZE table_name

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

