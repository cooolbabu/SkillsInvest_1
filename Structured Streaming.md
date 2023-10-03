# Let's talk about Structured Streaming

## Different ways to create Structured Streaming


## Write modes of Structured streaming
  - **Append mode (default)** This is the default mode, where only the new rows added to the Result Table since the last trigger will be outputted to the sink. This is supported for only those queries where rows added to the Result Table is never going to change. Hence, this mode guarantees that each row will be output only once (assuming fault-tolerant sink). For example, queries with only select, where, map, flatMap, filter, join, etc. will support Append mode.
  - **Complete mode** - The whole Result Table will be outputted to the sink after every trigger. This is supported for aggregation queries.
    - This is helpful if the target table needs aggregated data
    - Typically ends in Gold layer
  - **Update mode** - (Available since Spark 2.1.1) Only the rows in the Result Table that were updated since the last trigger will be outputted to the sink. More information to be added in future releases.


## A DELTA LIVE TABLE pipelines can be scheduled to run in two different modes
  - Triggered pipelines update each table with whatever data is currently available and then stop the cluster running the pipeline. Delta Live Tables automatically analyzes the dependencies between your tables and starts by computing those that read from external sources. Tables within the pipeline are updated after their dependent data sources have been updated.
  
  - Continuous pipelines update tables continuously as input data changes. Once an update is started, it continues to run until manually stopped. Continuous pipelines require an always-running cluster but ensure that downstream consumers have the most up-to-date data.

## Creating Delta Live Pipelines
  - Select Workflows UI and Delta live tables tab, under task type select Delta live tables pipeline and select the notebook.
  - What are the other options after clicking Workflows on the side bar of the UI
