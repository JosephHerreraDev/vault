---
date: 2025-02-04
tags:
  - note/herrerasoft/setup
---
[Configure firewall](https://learn.microsoft.com/en-us/sql/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access?view=sql-server-ver16)
[SQL Server Reporting Services](https://learn.microsoft.com/en-us/sql/reporting-services/install-windows/install-reporting-services?view=sql-server-ver16)
[Change server authentication mode](https://learn.microsoft.com/en-us/sql/database-engine/configure-windows/change-server-authentication-mode?view=sql-server-ver16)
# Installation Script

```
.\SQLEXPR_x64_ENU /QS /IACCEPTSQLSERVERLICENSETERMS /Action="install" /FEATURES=SQLEngine /INSTANCENAME=SQLEXPRESS /SECURITYMODE=SQL /INDICATEPROGRESS /SAPWD="database@2024"
```
# Uninstall script

```
.\SQLEXPR_x64_ENU /Action=Uninstall /QS /FEATURES=SQLEngine /INSTANCENAME=SQLEXPRESS
```

Recommended
```
.\SQLEXPR_x64_ENU\SETUP.EXE /Q /IACCEPTSQLSERVERLICENSETERMS /Action="install"  /FEATURES=SQL /INSTANCENAME=SQLEXPRESS /SQLSVCACCOUNT="MyDomain\MyAccount"  /SQLSVCPASSWORD="password" /SQLSYSADMINACCOUNTS="MyDomain\MyAccount"
/AGTSVCACCOUNT="MyDomain\MyAccount" /AGTSVCPASSWORD="password"
/ASSVCACCOUNT="MyDomain\MyAccount" /ASSVCPASSWORD="password"
/ISSVCAccount="MyDomain\MyAccount" /ISSVCPASSWORD="password"
/ASSYSADMINACCOUNTS="MyDomain\MyAccount" /SECURITYMODE=SQL /SAPWD="password"
```
Run SQL2022-SSEI-Expr.exe, and choose Download Media.  Choose "Express Core", and you'll download SQLEXPR_x64_ENU.exe.  Run that to extract the full install media to a folder, which you can use for the unattended installation. 

---
### Remove permission from user
```
USE [master]
GO
DENY CONNECT SQL TO [SERVERPC\Pepe]
GO
ALTER LOGIN [SERVERPC\Pepe] DISABLE
GO
```
---
## Change authentication mode

```
EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE',
    N'Software\Microsoft\MSSQLServer\MSSQLServer',
    N'LoginMode', REG_DWORD, 2;
GO
USE Master; 
ALTER LOGIN sa ENABLE
ALTER LOGIN sa WITH PASSWORD = 'Fancy@Dance2024'
```

---
## Change the default as to custom username

```
USE Master;
ALTER LOGIN [sa] WITH NAME = [USAEASYSOFT@SA24]
```

---
## Delete windows login

```
use master;
DROP LOGIN [Batcomputer\josep];
DROP LOGIN [BUILTIN\Users];
```

---
## Configure remote access

```
EXEC sp_configure 'remote access', 0;
GO
RECONFIGURE;
GO
```

---
## Add new user 
1. **In SSMS, expand the "Security" node.**
2. Right-click on "Logins" and select "New Login".
3. In the "Login - New" window:
    - **Enter a login name**.
    - Select "SQL Server authentication".
    - **Enter a password** and confirm it.
    - Uncheck "Enforce password policy" if you don't want to enforce it (optional).
4. Under "Default database", select a default database for the user (optional).
5. Go to the "Server Roles" page and assign appropriate server roles (e.g., `sysadmin`, `dbcreator`, etc.).
6. Go to the "User Mapping" page:
    - Map the login to relevant databases.
    - Assign appropriate database roles (e.g., `db_owner`, `db_datareader`, `db_datawriter`, etc.).
7. Click "OK" to create the new SQL Server authenticated user.
---
## Change installation address and instance name
SQLEXPRESS

