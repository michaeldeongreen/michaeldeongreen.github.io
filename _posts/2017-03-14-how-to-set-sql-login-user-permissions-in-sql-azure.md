---
layout: post
title:  "How to Set SQL Login User Permissions in SQL Azure"
date:  2017-03-14
categories: [technology]
images: sql-azure.png
tags: [sql azure, sql logins]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/sql-azure.png)

In this blog post I wanted to take some time to demonstrate how to control SQL Login permissions in SQL Azure. When you create a SQL Azure Database, an admin user will be created in the SQL Azure Database. In most cases, you will not want to give normal users access to this account and you probably don’t want applications accessing your SQL Azure Database using this account.  
  
**This tutorial will assume:**  
  
* You have access to an Azure Subscription
* You have access to SQL Server Management Studio
* You have access to a SQL Azure Database
* You have the ability to log into the SQL Azure Database as the default Admin user

**Create a Read Only User!**  
  
Step 1) Open up SQL Server Management Studio and log into your SQL Azure Database as the Admin user. Navigate to the “master”, right-click and choose “New Query”.  
  
Step 2) In the Query Window, paste the following script and change the Login and password to those of your choosing:  
  
```sql
    /*
    Run on the master database
    */
    CREATE LOGIN SomeReadOnlyLogin WITH password='somepassword'
    GO
    CREATE USER [SomeReadOnlyLoginUser] FOR LOGIN [SomeReadOnlyLogin] WITH DEFAULT_SCHEMA=[dbo]
    GO
```

In SQL Azure, SQL Users access the Server though Logins and Logins are Server-wide.  
  
Step 3) Next, since the Using command does not work in SQL Azure, you will want to navigate to each Database on the server that you wish to give the Read Only User access to and execute the following script:  
  
```sql
    /*
    Run on each database the user will have access too
    */
    CREATE USER [SomeReadOnlyLoginUser] FOR LOGIN [SomeReadOnlyLogin] WITH DEFAULT_SCHEMA=[dbo]
    GO
    /*
    Add user to the db_datareader role to restrict permissions to read only
    */
    EXEC sp_addrolemember 'db_datareader', 'SomeReadOnlyLoginUser';
    GO
```

**Create a READ/WRITE/EXECUTE Stored Procedures User!**  
  
Step 1) Open up SQL Server Management Studio and log into your SQL Azure Database as the Admin user. Navigate to the “master”, right-click and choose “New Query”.  
  
Step 2) In the Query Window, paste the following script and change the Login and password to those of your choosing:  
  
```sql
    /*
    Run on the master database
    */
    CREATE LOGIN SomeReadWriteExecuteProcedureLogin WITH password='somepassword'
    GO
    CREATE USER [SomeReadWriteExecuteProcedureLoginUser] FOR LOGIN [SomeReadWriteExecuteProcedureLogin] WITH DEFAULT_SCHEMA=[dbo]
    GO
```

Step 3) Next, since the Using command does not work in SQL Azure, you will want to navigate to each Database on the server that you wish to give the READ/WRITE/EXECUTE Stored Procedures User access to and execute the following script:  
  
```sql
    /*
    Run on each database the user will have access too
    */
    /*
    Create a new database role that allows users to execute stored procedures
    */
    CREATE ROLE [db_executor] AUTHORIZATION [dbo]
    GO
    GRANT EXECUTE TO [db_executor]
    GO
    /*
    Add user to roles for reading, writing and executing stored procedures
    */
    CREATE USER [SomeReadWriteExecuteProcedureUser] FOR LOGIN [SomeReadWriteExecuteProcedureLogin] WITH DEFAULT_SCHEMA=[dbo]
    GO
    EXEC sp_addrolemember 'db_datareader', 'SomeReadWriteExecuteProcedureUser';
    GO
    EXEC sp_addrolemember 'db_datawriter', 'SomeReadWriteExecuteProcedureUser';
    GO
    EXEC sp_addrolemember 'db_executor', 'SomeReadWriteExecuteProcedureUser';
```

**Resources**  
  
* You can find a list of built in SQL Azure roles [here](https://msdn.microsoft.com/en-us/library/ms189121.aspx)
* You can find more information about SQL Logins in Azure [here](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-manage-logins)
