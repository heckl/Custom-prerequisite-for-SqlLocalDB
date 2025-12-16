# ClickOnce Prerequisites for SQL Server LocalDB

ClickOnce has prerequisites only for:

- SQL Server 2012 Express LocalDB
- SQL Server 2016 Express LocalDB
- SQL Server 2017 Express LocalDB
- SQL Server 2019 Express LocalDB

but **not** for:

- SQL Server 2022 Express LocalDB

---

To create a custom prerequisite for SQL Server 2022 LocalDB:

**References:**

- Local backup: `Dropbox\backup\Custom-prerequisite-for-SqlLocalDB`
- GitHub repository: [Custom prerequisite for SQL Server 2022 LocalDB](https://github.com/heckl/Custom-prerequisite-for-sql-server-2022-localDb)

**Installation steps:**

1. Copy the prerequisite files into `C:\Program Files (x86)\Microsoft SDKs\ClickOnce Bootstrapper\Packages`  
   > You need administrator rights.

2. After copying, the new dependency should appear in Visual Studio.
   - You **do not need to restart Visual Studio**.
   - Go to **Project → Publish → More actions → Edit prerequisites**.

**Creating the prerequisite from scratch:**

1. Start from the existing package: `C:\Program Files (x86)\Microsoft SDKs\ClickOnce Bootstrapper\Packages\SqlLocalDB2019`

2. Modify `product.xml`:
   - Update `ProductCode` and `SearchPath`
   - Add a bypass rule:

     ```xml
     <BypassIf Property="sqllocaldbVersion" Compare="VersionGreaterThanOrEqualTo" Value="2022.1.1.1"/>
     ```

   - `vc_redist.x64.exe` does **not** need to be changed

3. Modify the EULA:
   - File: `en\eula.rtf`
   - Replace `2019` → `2022`

4. Modify `en\package.xml`:
   - Replace `2019` → `2022`
   - Finding the direct download link for `sqllocaldb_64` is not straightforward
   - Reference: [SQL Server 2022 LocalDB download](https://blog.dotsmart.net/2022/11/24/sql-server-2022-localdb-download/)

**Notes:**

- SQL Server 2022 LocalDB does not have a built-in ClickOnce prerequisite.
- Creating a custom bootstrapper package is the solution.
- This integrates directly with Visual Studio's Publish workflow.
