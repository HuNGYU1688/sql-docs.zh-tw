---
title: 使用格式檔案大量匯入資料
description: 在 SQL Server 中，您可在大量匯入作業中使用格式檔案。 格式檔案會將資料檔案的欄位對應到資料表的資料行。
ms.date: 09/20/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], format files
- format files [SQL Server], importing data using
ms.assetid: 2956df78-833f-45fa-8a10-41d6522562b9
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 8ffaa55c1d4e6b1346d238663eb65d9caec40c2e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485320"
---
# <a name="use-a-format-file-to-bulk-import-data-sql-server"></a>使用格式檔案大量匯入資料 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此主題說明格式檔案在大量匯入作業中的用法。  格式檔案會將資料檔案的欄位對應到資料表的資料行。  如需其他資訊，請參閱 [建立格式檔案 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) 。
  
## <a name="before-you-begin"></a>開始之前
* 如果格式檔案要與 Unicode 字元資料檔案搭配使用，則所有的輸入欄位都必須是 Unicode 文字字串 (也就是固定大小或以字元結束的 Unicode 字串)。
* 若要大量匯出或匯入 [SQLXML](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md) 資料，請在格式檔案中使用下列其中一種資料類型：
  * SQLCHAR 或 SQLVARCHAR (資料是使用用戶端字碼頁或定序所隱含的字碼頁所傳送)
  * SQLNCHAR 或 SQLNVARCHAR (資料會以 Unicode 傳送)
  * SQLBINARY 或 SQLVARBIN (未經任何轉換即傳送這份資料)。
* Azure SQL Database 和 Azure Synapse Analytics 只支援 [bcp](../../tools/bcp-utility.md)。  如需相關資訊，請參閱：
  * [將資料載入 Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading)
  * [將資料從 SQL Server 載入 Azure Synapse Analytics (一般檔案)](/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading)
  * [移轉資料](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-develop)

## <a name="example-test-conditions"></a>範例測試條件
本主題中的格式檔案範例是以下面定義的資料表和資料檔案為基礎。

### <a name="sample-table"></a>範例資料表
下列指令碼會建立測試資料庫和名為 `myFirstImport`的資料表。  請在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中執行下列 Transact-SQL：
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.MyFirstImport (
   PersonID smallint,
   FirstName varchar(25),
   LastName varchar(30),
   BirthDate Date
   );
```

### <a name="sample-data-file"></a>範例資料檔案
使用 [記事本] 建立空白檔案 `D:\BCP\myFirstImport.bcp` ，並插入下列資料：
```
1,Anthony,Grosse,1980-02-23
2,Alica,Fatnowna,1963-11-14
3,Stella,Rosenhain,1992-03-02
```

您也可以執行下列 PowerShell 指令碼以建立並填入資料檔案：
```powershell
Clear-Host
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = Join-Path -Path $dir -ChildPath 'MyFirstImport.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '1,Anthony,Grosse,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,1963-11-14';
Add-Content -Path $bcpFile -Value '3,Stella,Rosenhain,1992-03-02';

#Review content
Get-Content -Path $bcpFile;
Notepad.exe $bcpfile;
```

## <a name="creating-the-format-files"></a>建立格式檔案
SQL Server 支援兩種類型的格式檔案：非 XML 格式和 XML 格式。  非 XML 格式是舊版 SQL Server 所支援的原始格式。

### <a name="creating-a-non-xml-format-file"></a>建立非 XML 格式檔案
如需詳細資訊，請參閱 [非 XML 格式檔案 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) 。  下列命令將使用 [bcp 公用程式](../../tools/bcp-utility.md) ，根據 `myFirstImport.fmt`的結構描述產生非 XML 格式檔案 `myFirstImport`。  使用 bcp 命令建立格式檔案時，請指定 **format** 引數並使用 **nul** 取代資料檔案路徑。  format 選項也需要 **-f** 選項。  在這個範例中，另外還會使用限定詞 **c** 來指定字元資料，使用 **t,** 來指定逗號作為 [欄位結束字元](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)，並使用 **T** 來指定使用整合式安全性的信任連接。  請在命令提示字元之下，輸入下列命令：

```cmd
bcp TestDatabase.dbo.myFirstImport format nul -c -f D:\BCP\myFirstImport.fmt -t, -T

REM Review file
Notepad D:\BCP\myFirstImport.fmt
```

您的非 XML 格式檔案 `D:\BCP\myFirstImport.fmt` 應該看起來像這樣︰
```
13.0
4
1       SQLCHAR             0       7       ","      1     PersonID               ""
2       SQLCHAR             0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR             0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR             0       11      "\r\n"   4     BirthDate              ""
```

> [!IMPORTANT]
> 請確認您的非 XML 格式檔案以歸位字元\換行字元結尾。  否則您可能會收到下列錯誤訊息︰
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

### <a name="creating-an-xml-format-file"></a>建立 XML 格式檔案
如需詳細資訊，請參閱 [XML 格式檔案 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md) 。  下列命令將使用 [bcp 公用程式](../../tools/bcp-utility.md) ，根據 `myFirstImport.xml`的結構描述建立 XML 格式檔案 `myFirstImport`。 使用 bcp 命令建立格式檔案時，請指定 **format** 引數並使用 **nul** 取代資料檔案路徑。  format 選項一律需要 **-f** 選項，您也必須指定 **-x** 選項才能建立 XML 格式檔案。  在這個範例中，另外還會使用限定詞 **c** 來指定字元資料，使用 **t,** 來指定逗號作為 [欄位結束字元](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)，並使用 **T** 來指定使用整合式安全性的信任連接。  請在命令提示字元之下，輸入下列命令：
```cmd
bcp TestDatabase.dbo.myFirstImport format nul -c -x -f D:\BCP\myFirstImport.xml -t, -T

REM Review file
Notepad D:\BCP\myFirstImport.xml
```

您的 XML 格式檔案 `D:\BCP\myFirstImport.xml` 應該看起來像這樣︰
```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="http://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="11"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="2" NAME="FirstName" xsi:type="SQLVARCHAR"/>
  <COLUMN SOURCE="3" NAME="LastName" xsi:type="SQLVARCHAR"/>
  <COLUMN SOURCE="4" NAME="BirthDate" xsi:type="SQLDATE"/>
 </ROW>
</BCPFORMAT>
```

## <a name="using-a-format-file-to-bulk-import-data"></a>使用格式檔案大量匯入資料
下列範例會使用上面建立的資料庫、資料檔案和格式檔案。

### <a name="using-bcp-and-non-xml-format-file"></a>使用 [bcp](../../tools/bcp-utility.md) 和[非 XML 格式檔案](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
請在命令提示字元之下，輸入下列命令：
```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.MyFirstImport;"

REM Import data
bcp TestDatabase.dbo.myFirstImport IN D:\BCP\myFirstImport.bcp -f D:\BCP\myFirstImport.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.MyFirstImport"
```


### <a name="using-bcp-and-xml-format-file"></a>使用 [bcp](../../tools/bcp-utility.md) 和 [XML 格式檔案](../../relational-databases/import-export/xml-format-files-sql-server.md)
請在命令提示字元之下，輸入下列命令：
```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.MyFirstImport;"

REM Import data
bcp TestDatabase.dbo.myFirstImport IN D:\BCP\myFirstImport.bcp -f D:\BCP\myFirstImport.xml -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.MyFirstImport;"
```


### <a name="using-bulk-insert-and-non-xml-format-file"></a>使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和[非 XML 格式檔案](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
請在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中執行下列 Transact-SQL：
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
BULK INSERT dbo.myFirstImport   
   FROM 'D:\BCP\myFirstImport.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myFirstImport.fmt');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### <a name="using-bulk-insert-and-xml-format-file"></a>使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和 [XML 格式檔案](../../relational-databases/import-export/xml-format-files-sql-server.md)
請在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中執行下列 Transact-SQL：
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
BULK INSERT dbo.myFirstImport   
   FROM 'D:\BCP\myFirstImport.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myFirstImport.xml');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### <a name="using-openrowsetbulk-and-non-xml-format-file"></a>使用 [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 和[非 XML 格式檔案](../../relational-databases/import-export/non-xml-format-files-sql-server.md)    
請在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中執行下列 Transact-SQL：
```sql
USE TestDatabase;
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
INSERT INTO dbo.myFirstImport
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myFirstImport.bcp',
        FORMATFILE = 'D:\BCP\myFirstImport.fmt'
        ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### <a name="using-openrowsetbulk-and-xml-format-file"></a>使用 [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 和 [XML 格式檔案](../../relational-databases/import-export/xml-format-files-sql-server.md)
請在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中執行下列 Transact-SQL：
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
INSERT INTO dbo.myFirstImport 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myFirstImport.bcp',
        FORMATFILE = 'D:\BCP\myFirstImport.xml'  
       ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```
  
## <a name="more-examples"></a>更多範例
 [建立格式檔案 &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
 [使用格式檔案略過資料表資料行 &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
 [使用格式檔案略過資料欄位 &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
 [使用格式檔案將資料表資料行對應至資料檔案欄位 &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
## <a name="see-also"></a>另請參閱  
 [bcp 公用程式](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [非 XML 格式檔案 &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [XML 格式檔案 &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md)  
  [匯入或匯出資料的格式檔案 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)
