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
- DELETE TOP (5000) FROM Person.EmailAddress
- DROP TABLE Production.TransactionHistory
- DROP TABLE Production.TransactionHistoryArchive
- DROP TABLE HumanResources.Department