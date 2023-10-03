# Databricks Lakehouse Platform
- Lakehouse can replace traditional warehouses by leveraging storage and compute optimizations like caching to serve them with low query latency with high reliability.
- Key features
  - Transaction Support * Schema Enforcement and Governance * BI support * Storage decouple with Compute * Openness * Support for diverse datatypes (Structured and Unstructured) * Diverse Workloads * End-to-End Streaming

    '''
    DF.write.format("delta").option("mergeSchema", "true").saveAsTable("table_name")
      
    spark.databricks.delta.schema.autoMerge = True  ## Spark session
    '''





# ELT with Spark SQL and Python




# Incremental Data Processing




# Production Pipelines




# Data Governance
