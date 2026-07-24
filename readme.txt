ClickOnce has prerequisite only for

- “Sql server 2012 Express localDb”
- “Sql server 2016 Express localDb”
- “Sql server 2017 Express localDb”
- “Sql server 2012 Express localDb”

but not for

- “Sql server 2022 Express localDb”.


- "create custom prerequisite" for sql server 2022 local db
	- see Dropbox\backup\Custom-prerequisite-for-SqlLocalDB 
	- see https://github.com/heckl/Custom-prerequisite-for-SqlLocalDB
	- copy the files into c:\Program Files (x86)\Microsoft SDKs\ClickOnce Bootstrapper\Packages
		- you need admin rights
	- now the new dependency should appear in VS
		- you do not have to restart VS, just press in "project / publish" and the "more actions / edit"
	- creating from scratch
		- start from "c:\Program Files (x86)\Microsoft SDKs\ClickOnce Bootstrapper\Packages\SqlLocalDB2019"
		- modify product.xml: ProductCode, SearchPath
			- <BypassIf Property="sqllocaldbVersion" Compare="VersionGreaterThanOrEqualTo" Value="2022.1.1.1"/>
			- I did not change: vc_redist.x64.exe
		- modify en / eula.rtf, 2019 -> 2022
		- modify en / package.xml, 2019 -> 2022
			- it is not easy to figure out the direct download link (sqllocaldb_64)
			- https://blog.dotsmart.net/2022/11/24/sql-server-2022-localdb-download/
- "create custom prerequisite" for sql server 2025 local db
	- copy the files into c:\Program Files (x86)\Microsoft SDKs\ClickOnce Bootstrapper\Packages
		- you need admin rights
		- notice that SqlLocalDB.msi appears beside Product.xml, as I understand, this is needed on the developer machine, so Visual Studio can compute the correct hash/signature locally
			- this copy isn't what end users download
	- now the new dependency should appear in VS
		- you do not have to restart VS, just press in "project / publish" and the "more actions / edit"
	- SQL Server 2025 LocalDB has no offical direct-download MSI, so I hosted it at https://github.com/ScutumSoft/sqllocaldb2025-redist/releases/download/v17.0.4065.4/SqlLocalDB.msi
		- generally you should not use some other person's host
	- if you want to host the msi yourself
		- download the latest update
			-
			https://learn.microsoft.com/en-us/troubleshoot/sql/releases/download-and-install-latest-updates
			- select the latest CU, CU7
			- this is the update for the standalone sql server also
		- use the update file for extraction
			- cmd: SQLServer2025-KB5096981-x64.exe /x:D:\SQL2025CU7
			- you find the msi here: d:\SQL2025CU7\1033_ENU_LP\x64\Setup\x64\SqlLocalDB.msi  
		- create a public repo at github to host the msi
			- create a release
			- create a tag and a title
			- add SqlLocalDB.msi to the binaries at the bottom
			- in the release right click on the msi / open link address
				- https://github.com/<YOUR_NAME>/sqllocaldb2025-redist/releases/download/v17.0.4065.4/SQLLOCALDB.MSI
			- copy the link into package.xml
