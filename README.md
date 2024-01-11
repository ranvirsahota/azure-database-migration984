# Azure Database Migration


## Set up Production Environment
- Procued a Windows VM to serve as production environement, it has: Windows 11, Standards D2 v3 and disk is just local standard HDD
- Used SQL Server and SSMS to resotre the renowned Microsoft SQL Server database known as AdventureWorks
- AdventureWorks is hosted locally to serve as the on-premises database

## Migrage to Azure SQL Database
- Set up an Azure SQL Server to host the Azure SQL Database which will serve as the replacement for the on-premises database
- Installed Azure Data Studio and an extension SQL Server Scheme Compare, then connected to the on premises and the Azure SQL database 
- Used Azure Data Studio and SQL Server Scheme Compare were used for migrating the scheme


## Data Backup and Resotre
- Backed up the on-premises datbase in-case anything happens to the data during the migration, backup stored as .bak file 
- To keep the .bak file safe it has been uploaded to Azure Storage also allowing .bak file to be accessed remotely
- Procured another VM to serve as the development environment and restored the databasa via the backup uploaded to Azure
- Automted backup to take place every sunday with no end

## Disaster Recovery Simulation
- DELETE TOP (10000) FROM Person.EmailAddress;
- DROP TABLE Production.TransactionHistory;
- DROP TABLE Production.TransactionHistoryArchive;
- ALTER TABLE HumanResources.Department DROP CONSTRAINT DF_Department_ModifiedDate;
- DROP TABLE Person.PersonPhone;
- DROP TABLE Production.ProductCostHistory, Production.ProductInventory, Production.ProductReview;

The above list is what was destroyed in the disaster simulation. This has been recoved due to take backups take by azure. The resotoration process took 2 hours.

## Geo Replication and Failover
- Created a replica database of the production database, hosted on a new SQL server in the east of the US, all other paramaters are default the same as the primary
- Executed failover and reverted no data loss observed and both transistions were executed in under 5 minutes

## Microsoft Entra Directory Integration
- Configured Azure SQL Server to accept Microsoft Entra ID, set myself as admin. This was verfied by establsihing a connection to the SQL Server in the production VM. I did have some initial difficulty with process, I forgot to click save button. When the authentication failed I went back to the tutorial and saw my error.
- Create a new user named 'sukuna_db_datareader' as their user principal name and ran an SQL query to db_datareader role to sukuna. I then went to connect to the SQL Server as sukuna but came across an error:
    - 'Login failed for user '<token-identified principal>'
- I found the resolution to the error to be specificing in the connection parameters which database I intended to connect to. As it turns out Azure Data Studio defualt is to attempt to connect first to the 'masterdb'. Following the steps on this link helped me find a solution: https://techcommunity.microsoft.com/t5/azure-database-support-blog/aad-auth-error-login-failed-for-user-lt-token-identified/ba-p/1417535

With this the migration is compelete