---
layout:       post
title:        "SQL Server Remote Connection"
subtitle:     "SQL Server Remote Connection Configuration"
date:         2016-05-20 12:00:00
author:       "Gary"
header-img:   "img/post-bg-android.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - SQL Server
---

#### MSSQL Remote Connection
------------------

1. Change `Server Authentation` in Server Properties, this operation requires restart service
![Server Properties](/img/MSSQLRemote/1.png)

2. Make sure `Allow remote connections to this server` checked
![Server Properties](/img/MSSQLRemote/3.png)

3. In instance **Facets**, make sure `RemoteAccessEnabled` in `Server Configuration` is `True`
![RemoteAccessEnabled](/img/MSSQLRemote/4.png)

4. Make sure both your instance and `SQL Server Browser Service` is running
![RemoteAccessEnabled](/img/MSSQLRemote/6.png)

5. In `SQL Server Configuration Manager`, enable the `TCP/IP` for given instance, then restart service
![RemoteAccessEnabled](/img/MSSQLRemote/5.png)

#### What is `SQL Server Browser Service` For
------------------

The `SQL ServerBrowser` program runs as a Windows service. SQL Server Browser listens for incoming requests for Microsoft SQL Server resources and provides information about SQL Server instances installed on the computer. SQL Server Browser contributes to the following actions:

- Browsing a list of available servers

- Connecting to the correct server instance

- Connecting to dedicated administrator connection (DAC) endpoints

- For each instance of the Database Engine and SSAS, the SQL Server Browser service (sqlbrowser) provides the instance name and the version number. SQL Server Browser is installed with SQL Server.

- SQL Server Browser can be configured during setup or by using SQL Server Configuration Manager. By default, the SQL Server Browser service starts automatically:

- When upgrading an installation.

- When installing on a cluster.

- When installing a named instance of the Database Engine including all instances of SQL Server Express.

- When installing a named instance of Analysis Services.