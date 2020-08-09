# SQL SERVER
----------

Always Encrypted with the Windows Certificate Store allows data to be encrypted both in transit and at rest, and allows only app services with the key to see the data whenever it is in a decrypted state.

A Single Database is the best fit solution to meet the needs of a few small microservices. 

A Fully Managed SQL Database is a fully managed SQL Server Database Engine Instance which will be over provisioned for small microservices usage. 

SQL Database Elastic Pools are a simple, cost-effective solution for managing and scaling multiple databases that have varying and unpredictable usage demands. 

A SQL Server Virtual Machine is best giving you complete control over your database, settings and server, but comes at a higher price. 

https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted
https://docs.microsoft.com/en-us/azure/sql-database/

The cmdlet Set-AzSqlDatabase will allow you to set properties for a database, or move an existing database into, out of, or between elastic pools. 

Set-AzSqlElasticPool - modifies properties of an elastic pool For example, use the StorageMB property to modify the max storage of an elastic pool.

Get-AzSqlDatabase - retrieves one or more databases.

New-AzSqlElasticPool - creates an elastic pool.