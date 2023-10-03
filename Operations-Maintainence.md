- Operations Team can schedule the query to refresh every 12 hours from the SQL endpoint’s page in Databricks SQL. The query pane view in Databricks SQL workspace provides the ability to add or edit and schedule individual queries to run
- Setup an Alert for a query to notify them if the returned value is greater than 60.


 ## Previleges

- **SELECT**: gives read access to an object.
- **CREATE**: gives ability to create an object (for example, a table in a schema).
- **MODIFY**: gives ability to add, delete, and modify data to or from an object.
- **USAGE**: does not give any abilities, but is an additional requirement to perform any action on a schema object.
- **READ_METADATA**: gives ability to view an object and its metadata.
- **CREATE_NAMED_FUNCTION**: gives ability to create a named UDF in an existing catalog or schema.
- **ALL PRIVILEGES**: gives all privileges (is translated into all the above privileges).

## Upgrading
```
CREATE TABLE <catalog>.<new-schema>.<new-table>
AS SELECT * FROM hive_metastore.<old-schema>.<old-table>;
```
