## I. Introduction ![[Pasted image 20250514092129.png |100]]

1. #Serverless offering to perform analytics directly against S3 files.
2. Uses SQL to query the files, supports JDBC/ODBC driver.
3. **Charged per query and amount of data scanned.**
4. **Supports CSV, JSON, ORC, Avro and Parquet.**
5. It's built on top of Presto.
6. Used in BI scenarios and log management.

## II. Athena for SA

1. Operations: serverless so not needed
2. Security: IAM + S3 managed
3. Reliability: managed service, uses Presto, HA
4. Performance: queries scale based on size
5. Cost: pay per query and per TB of data scanned