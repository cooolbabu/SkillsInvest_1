### Columnar transformations
```
# Importing the when condition
from pyspark.sql.functions import when
countries.withColumn('name_length', when(countries['population']>100000000, 'large').otherwise('not large')).display()

# Expr allows the use of SQL syntax, using the SQL aliasing method
countries.select(expr('NAME as country_name')).display()

# Expr allows the use of SQL syntax, using the SQL left function
countries.select(expr('left(NAME,2) as name')).display()

# Expr allows the use of SQL syntax, using the SQL case statement
countries.withColumn('population_class', expr("case when population > 100000000 then 'large' when population >50000000 then 'medium' else 'small' end")).display()

# Use the agg function for multiple aggregations
countries.groupBy('region_id', 'sub_region_id').agg(avg('population'), sum('area_km2')).display()

# Using withColumnRenamed
countries.groupBy('region_id', 'sub_region_id'). \
agg(sum('population'), sum('area_km2')). \
withColumnRenamed('sum(population)', 'total_pop'). \
withColumnRenamed('sum(area_km2)', 'total_area'). \
display()



# Use the sort function to sort the resulting Dataframe
countries.groupBy('region_id', 'sub_region_id'). \
agg(max('population').alias('max_pop'), min('population').alias('min_pop')). \
sort(countries['region_id'].asc()). \
display()
```
### Pivot Tables
```
# Using pivot after groupyBy to pivot on the region_id column
countries.groupBy('sub_region_id').pivot('region_id').sum('population').display()
```
### Joins
```
# Joining and sorting the countries and regions dataframes, also selecting only specific columns
countries. \
join(regions, regions['Id']==countries['region_id'], 'inner'). \
select(countries['name'].alias('country_name'), regions['name'].alias('region_name'), countries['population']). \
sort(countries['population'].desc()). \
display()
```

### SQL Samples  
  ```
    DESCRIBE HISTORY table_name
    DESCRIBE EXTENDED table_name
    REFRESH TABLE table_name
    OPTIMIZE table_name

    DROP DATABASE customers CASCADE
    SHALLOW CLONE OR DEEP CLONE for cloning tables as an option on create tables
      CREATE OR REPLACE TABLE {new_table_name} SHALLOW CLONE {source_table_name}|[LOCATION path]

    ## Create Delta Table
    CREATE TABLE users_jdbc
    USING org.apache.spark.sql.jdbc
    OPTIONS (
        url = "jdbc:sqlite:/sqmple_db",
        dbtable = "users"
    )

    ## Create Delta Live Table
    CREATE LIVE TABLE sales_order_in_chicago
    AS
    SELECT order_date, city, sum(price) as sales,
    FROM sales_orders_cleaned
    WHERE city = 'Chicago')
    GROUP BY order_date, city

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

## Different ways of Schema evolution
**Provide schema hints.**
```
  spark.readStream \
    .format("cloudFiles") \
    .option("cloudFiles.format", "csv") \
    .option("header", "true") \
    .option("cloudFiles.schemaLocation", schema_location) \
    .option("cloudFiles.schemaHints", "id int, description string")
    .load(raw_data_location)
    .writeStream \
    .option("checkpointLocation", checkpoint_location) \
    .start(target_delta_table_location)
```

```
WITH CTE AS (
    SELECT *,
           ROW_NUMBER() OVER (
               PARTITION BY lead_id, lead_status
               ORDER BY (SELECT NULL)
           ) AS RowNumber
    FROM Leads
)
select * FROM CTE WHERE RowNumber > 1;
```

## Handling complex json
[Five Spark SQL Helper Utility Functions to Extract and Explore Complex Data Types](https://docs.databricks.com/en/optimizations/semi-structured.html)
