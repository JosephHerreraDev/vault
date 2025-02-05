---
date: 2025-02-04
tags:
  - notes
  - Wix
---
Version 5
[docs](https://wixtoolset.org/docs/intro/)
[video](https://www.youtube.com/watch?v=6Yf-eDsRrnM)
## Folder Structure
* CARS Setup
	* CarsProyect 
		* Files and components to install
		* .Net check
		* ManufacturerFolder (help, manual and scriptDB)
		* Desktop shortcut
		* Uninstall action
	* CarsSetup
		* Extensive .Net Framework check
		* ReportViewer install
* DataBase Setup
	* DB2
		* Files
		* SQLCustomAction
			* Configure SQL Server connection
			* Create DataBase
			* Create DataTables
			* Insert Data
		* SQLProyect
			* Runs Custom Action
	* Files
	* SQLCustomAction
		* Configure SQL Server connection
		* Create DataBase
		* Create DataTables
		* Insert Data
	* SQLProyect
		* Runs Custom Action
	* SQLSERVER2014Sp2
		* Installs SQL Server
## Notes
* The upgrade code should never change, the Id code should change with every version.
# Routing
## Routes
ProgramFiles64Folder/ProductManufacturerDir/ProductNameDir/HelpDir
ProgramFiles64Folder/ProductManufacturerDir/ProductNameDir/ManualDir
ProgramFiles64Folder/ProductManufacturerDir/ProductNameDir/ScriptDB
ProgramFiles64Folder/ProgramMenuFolder/ProductManufacturerProductNameDir/ProductNameShortcut

## Shortcuts
ProgramFiles64Folder/ProgramMenuFolder/ProductManufacturerProductNameDir/
ProgramFiles64Folder/ProductManufacturerDir/ProductNameDir/DesktopFolder/DesktopShortcut

# Enable logging
```
msiexec /i HerreraSoftPackage.msi /l*v log.log
```

Wix Toolset Tips
https://www.youtube.com/watch?v=usOh3NQO9Ms

https://www.youtube.com/watch?v=6Yf-eDsRrnM

Sql Server 2024 Command Prompt
https://learn.microsoft.com/en-us/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-ver16