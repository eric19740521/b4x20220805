# b4x20220805
B4X實驗:  異質資料庫服務器之間如何轉移資料庫/資料表結構,從一個資料庫(MSSQL/MySQL/SQLite)轉移一個資料庫(MSSQL/MySQL/SQLite) 建立資料表.並且轉移資料
B4X實驗:  異質資料庫服務器之間如何轉移資料庫/資料表結構,從一個資料庫(MSSQL/MySQL/SQLite)轉移一個資料庫(MSSQL/MySQL/SQLite) 建立資料表.並且轉移資料
參考資料:




0.
(MSSQL/MySQL/SQLite) 做成一個class
包裝成函數 物件 有個 好處.以後如果要改的話.只要改一個地方




1.MSSQL
LIB
#AdditionalJar: jtds-1.3.1.jar	

連接字串 MSQL 7.0之後的版本,試了很多版本,才找出正確的方式連上

				SQLDriver = "net.sourceforge.jtds.jdbc.Driver"
				jdbcUrl = $"jdbc:jtds:sqlserver://${uc_DB_SERVER}"$

				'MSQL 7.0
				'jdbcUrl = jdbcUrl & $";databaseName=${tc_DATABASE};user=${uc_DB_USER};password=${uc_DB_PASSWORD};appname=SKMJL;wsid=TEST;loginTimeout=10;socketTimeout=10"$
				'MSSQL SQLEXPRESS 2019
				jdbcUrl = jdbcUrl & $"/${tc_DATABASE};instance=SQLEXPRESS;user=${uc_DB_USER};password=${uc_DB_PASSWORD};integratedSecurity=true;"$
				
				
				'
				'jdbcUrl="jdbc:jtds:sqlserver://127.0.0.1/tudou;integratedSecurity=true;user=sa;password=Ho123456789;instance=SQLEXPRESS;"
				
				Log("jdbcUrl= "&jdbcUrl )
				'sql.Initialize("net.sourceforge.jtds.jdbc.Driver", "jdbc:jtds:sqlserver://localhost:1433/test;instance=SQLEXPRESS;?user=test&password=test")

				
				MSSQL.Initialize( SQLDriver, jdbcUrl  )
		



列出資料庫
SELECT * FROM master.dbo.sysdatabases

列出資料表
SELECT  table_name FROM Information_Schema.TABLES

列出PK
select * From INFORMATION_SCHEMA.KEY_COLUMN_USAGE   where table_name='factory'

列出欄位
SELECT * FROM Information_Schema.COLUMNS where table_name='product' order by ordinal_position


2.MySQL
LIB
#AdditionalJar: mysql-connector-java-5.1.34-bin.jar

連接字串

				SQLDriver = "com.mysql.jdbc.Driver"
				SQLJDBC = "jdbc:mysql://"
			
			
				jdbcUrl =  SQLJDBC & $"${uc_DB_SERVER}:3306/${tc_DATABASE}?characterEncoding=utf8"$

				MySQL.Initialize2( SQLDriver, jdbcUrl , uc_DB_USER , uc_DB_PASSWORD)



列出資料庫
SHOW DATABASES;

列出資料表 
SELECT table_name FROM INFORMATION_SCHEMA.TABLES where table_schema='newpos'
 

列出欄名/PK
SHOW COLUMNS FROM Table_Name  


3.SQLite
LIB
#AdditionalJar: sqlite-jdbc-3.7.2	


連結字串
				Dim dbName As String = tc_DATABASE & ".db"
				'檢查資料庫檔案是否存在
				If File.Exists(uc_DB_Path, dbName)==False Then
					Log("DB不存在??程式將自動建立 ")
				End If
				
				Log("uc_DB_Path= "&uc_DB_Path)
				
				SQLite.InitializeSQLite(uc_DB_Path, dbName, True)
				SQLite.ExecQuerySingleResult("PRAGMA journal_mode = wal")



列出資料表
select * from sqlite_master where type='table';

列出欄名/PK
pragma table_info('apgrids');


 

