---
description: 在作業步驟中使用 Token
title: 在作業步驟中使用 Token
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server Agent]
- security [SQL Server Agent], enabling alert job step tokens
- SQL Server Agent jobs, job steps
- tokens [SQL Server]
- escape macros [SQL Server Agent]
ms.assetid: 105bbb66-0ade-4b46-b8e4-f849e5fc4d43
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: f888cc014c5cce3280f572c8a7eeddf194704624
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482153"
---
# <a name="use-tokens-in-job-steps"></a>在作業步驟中使用 Token
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 受控執行個體](/azure/sql-database/sql-database-managed-instance)目前支援多數 (但非全部) 的 SQL Server Agent 功能。 如需詳細資料，請參閱 [Azure SQL 受控執行個體與 SQL Server 之間的 T-SQL 差異](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 可讓您在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 作業步驟指令碼中使用 Token。 撰寫作業步驟時使用 Token，所賦予您的彈性與撰寫軟體程式時使用的變數一樣。 在作業步驟指令碼中插入 Token 後， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 就會先在執行階段取代此 Token，然後再由 [!INCLUDE[tsql](../../includes/tsql-md.md)] 子系統執行作業步驟。  
  
  
## <a name="understanding-using-tokens"></a>了解如何使用 Token  
  
> [!IMPORTANT]  
> 對 Windows 事件記錄檔具有寫入權限的任何 Windows 使用者，都可以存取由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 警示或 WMI 警示啟動的作業步驟。 為了避免此安全性風險，依預設會停用在警示啟動的作業中可以使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent Token。 這些 Token 包括：**A-DBN**、**A-SVR**、**A-ERR**、**A-SEV**、**A-MSG** 及 **WMI(** <屬性> **)** 。 請注意在此版本中，Token 的使用擴充到所有警示。  
>   
> 如果需要使用這些 Token，請先確定只有受信任的 Windows 安全性群組的成員 (例如 Administrators 群組) 才對 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所在電腦的事件記錄檔具有寫入權限。 然後以滑鼠右鍵按一下物件總管中的 [SQL Server Agent]、選取 [屬性]，然後在 [警示系統] 頁面上選取 [取代回應警示之所有作業的 Token]，以啟用這些 Token。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent Token 取代功能既簡單又有效率： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 會使用精確的常值字串值來取代 Token。 所有 Token 需區分大小寫。 您的作業步驟必須將這點納入考量，並且必須正確引用您所用的 Token 或將取代字串轉換成正確的資料類型。  
  
例如，您可能會使用下列陳述式，在作業步驟中列印資料庫的名稱：  
  
`PRINT N'Current database name is $(ESCAPE_SQUOTE(A-DBN))' ;`  
  
在此範例中， **ESCAPE_SQUOTE** 巨集是使用 **A-DBN** Token 插入的。 在執行階段， **A-DBN** Token 將會被取代成正確的資料庫名稱。 逸出巨集會逸出不慎傳入 Token 取代字串的任何單引號。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 將在最終字串中使用兩個單引號來取代一個單引號。  
  
例如，如果針對取代 Token 所傳遞的字串是 `AdventureWorks2012'SELECT @@VERSION --`，則由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 作業步驟所執行的命令將會是：  
  
`PRINT N'Current database name is AdventureWorks2012''SELECT @@VERSION --' ;`  
  
在此情況下，插入的陳述式 `SELECT @@VERSION`不會執行。 額外的單引號反而會導致伺服器將插入的陳述式剖析成字串。 如果 Token 取代字串不包含單引號，就不會逸出任何字元，而且包含 Token 的作業步驟會如預期方式執行。  
  
若要在作業步驟中偵錯 Token 的使用方式，請使用 PRINT 陳述式 (例如 `PRINT N'$(ESCAPE_SQUOTE(SQLDIR))'`)，並將作業步驟輸出儲存至檔案或資料表。 您可以使用 [作業步驟屬性] 對話方塊的 [進階] 頁面來指定作業步驟輸出檔或資料表。  
  
## <a name="sql-server-agent-tokens-and-macros"></a>SQL Server Agent Token 和巨集  
下表將列出並描述 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 支援的 Token 和巨集。  
  
### <a name="sql-server-agent-tokens"></a>SQL Server Agent Token  
  
|Token|描述|  
|---------|---------------|  
|**(A-DBN)**|資料庫名稱。 若作業是由警示執行，則資料庫名稱值會自動取代作業步驟中的此 Token。|  
|**(A-SVR)**|伺服器名稱。 若作業是由警示執行，則伺服器名稱值會自動取代作業步驟中的此 Token。|  
|**(A-ERR)**|錯誤號碼。 若作業是由警示執行，則錯誤號碼值會自動取代作業步驟中的此 Token。|  
|**(A-SEV)**|錯誤的重要性。 若作業是由警示執行，則錯誤嚴重性值會自動取代作業步驟中的此 Token。|  
|**(A-MSG)**|訊息文字。 若作業是由警示執行，則訊息文字值會自動取代作業步驟中的此 Token。|  
|**(JOBNAME)**|作業的名稱。 此權杖僅適用於 SQL Server 2016 和更新版本。|  
|**(STEPNAME)**|步驟的名稱。 此權杖僅適用於 SQL Server 2016 和更新版本。|  
|**(DATE)**|目前日期 (格式為 YYYYMMDD)。|  
|**(INST)**|執行個體名稱。 如果是預設執行個體，此 Token 將具有預設執行個體名稱：MSSQLSERVER。|  
|**(JOBID)**|作業識別碼。|  
|**(MACH)**|電腦名稱。|  
|**(MSSA)**|主要 SQLServerAgent 服務名稱。|  
|**(OSCMD)**|用於執行 **CmdExec** 作業步驟之程式的前置詞。|  
|**(SQLDIR)**|安裝有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的目錄。 根據預設，此一值為 C:\Program Files\Microsoft SQL Server\MSSQL。|  
|**(SQLLOGDIR)**|SQL Server 錯誤記錄檔資料夾路徑的取代 Token，例如 $(ESCAPE_SQUOTE(SQLLOGDIR))。 此 Token 僅適用於 SQL Server 2014 和更新版本。|  
|**(STEPCT)**|此步驟已執行之次數的計數 (不包含重試)。 可利用步驟命令來強制結束多重步驟迴圈 (Loop)。|  
|**(STEPID)**|步驟識別碼。|  
|**(SRVR)**|執行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的電腦名稱。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 是具名執行個體，這也包括執行個體名稱。|  
|**(TIME)**|目前時間 (格式為 HHMMSS)。|  
|**(STRTTM)**|開始執行作業的時間 (格式為 HHMMSS)。|  
|**(STRTDT)**|開始執行作業的日期 (格式為 YYYYMMDD)。|  
|**(WMI(** <屬性> **))**|對於回應 WMI 警示所執行的作業，這是 <屬性> 指定的屬性值。 例如，`$(WMI(DatabaseName))` 提供造成警示執行之 WMI 事件的 **DatabaseName** 屬性值。|  
  
### <a name="sql-server-agent-escape-macros"></a>SQL Server Agent 逸出巨集  
  
|逸出巨集|描述|  
|-----------------|---------------|  
|**$(ESCAPE_SQUOTE(** _token\_name_ **))**|在 Token 取代字串中逸出單引號 (')。 使用兩個單引號來取代一個單引號。|  
|**$(ESCAPE_DQUOTE(** _token\_name_ **))**|在 Token 取代字串中逸出雙引號 (")。 使用兩個雙引號來取代一個雙引號。|  
|**$(ESCAPE_RBRACKET(** _token\_name_ **))**|在 Token 取代字串中逸出右方括號 (])。 使用兩個右方括號來取代一個右方括號。|  
|**$(ESCAPE_NONE(** _token\_name_ **))**|取代 Token，但不逸出字串中的任何字元。 提供這個巨集的目的，是為了在 Token 取代字串只能由受信任使用者提供的環境下，支援回溯相容性。 如需詳細資訊，請參閱本主題後面的「將作業步驟更新成使用巨集」。|  
  
## <a name="updating-job-steps-to-use-macros"></a>將作業步驟更新成使用巨集  
下表說明 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 如何處理取代 Token。 若要開啟或關閉取代 Token，請以滑鼠右鍵按一下物件總管中的 [SQL Server Agent]，並選取 [屬性]，然後在 [警示系統] 頁面上選取或清除 [取代回應警示之所有作業的 Token] 核取方塊。  
  
|Token 語法|警示 Token 取代開啟|警示 Token 取代關閉|  
|----------------|------------------------------|-------------------------------|  
|使用 ESCAPE 巨集|作業中的所有 Token 都會順利被取代。|由警示啟動的 Token 不會被取代。 這些 Token 包括： **A-DBN**、 **A-SVR**、 **A-ERR**、 **A-SEV**、 **A-MSG** 及 **WMI(** _&lt;屬性&gt;_ **)** 。 其他靜態 Token 則會順利被取代。|  
|不使用 ESCAPE 巨集|所有包含 Token 的作業都會失敗。|所有包含 Token 的作業都會失敗。|  
  
## <a name="token-syntax-update-examples"></a>Token 語法更新範例  
  
### <a name="a-using-tokens-in-non-nested-strings"></a>A. 在非巢狀字串中使用 Token  
下列範例將說明如何使用正確的逸出巨集來更新簡單的非巢狀指令碼。 執行更新指令碼之前，下列作業步驟指令碼會使用一個作業步驟 Token 來列印正確的資料庫名稱：  
  
`PRINT N'Current database name is $(A-DBN)' ;`  
  
執行更新指令碼之後，會在 `ESCAPE_NONE` Token 前面插入 `A-DBN` 巨集。 由於單引號是用來分隔 PRINT 字串，所以您必須依照下列方式插入 `ESCAPE_SQUOTE` 巨集，藉以更新此作業步驟：  
  
`PRINT N'Current database name is $(ESCAPE_SQUOTE(A-DBN))' ;`  
  
### <a name="b-using-tokens-in-nested-strings"></a>B. 在巢狀字串中使用 Token  
在 Token 用於巢狀字串或陳述式的作業步驟指令碼中，巢狀陳述式應該先重新撰寫成多個陳述式，然後再插入正確的逸出巨集。  
  
例如，請考慮下列使用 `A-MSG` Token 而且尚未使用逸出巨集更新的作業步驟：  
  
`PRINT N'Print ''$(A-MSG)''' ;`  
  
執行更新指令碼之後，會使用此 Token 插入 `ESCAPE_NONE` 巨集。 不過，在此情況下，您必須依照下列方式重新撰寫不使用巢狀的指令碼，並插入 `ESCAPE_SQUOTE` 巨集，以便適當逸出可能會傳入 Token 取代字串的分隔符號：  
  
```sql
DECLARE @msgString nvarchar(max);
SET @msgString = '$(ESCAPE_SQUOTE(A-MSG))';
SET @msgString = QUOTENAME(@msgString,'''');
PRINT N'Print ' + @msgString;
```
  
此外，請注意此範例中，QUOTENAME 函數會設定引號字元。  
  
### <a name="c-using-tokens-with-the-escape_none-macro"></a>C. 使用 Token 搭配 ESCAPE_NONE 巨集  
下列範例是指令碼的一部分，而這個指令碼會從 `job_id` 資料表擷取 `sysjobs` 並使用 `JOBID` Token 來擴展 `@JobID` 變數 (之前在指令碼中宣告成二進位資料類型)。 請注意，由於二進位資料類型不需要任何分隔符號，所以 `ESCAPE_NONE` 巨集會搭配 `JOBID` Token 使用。 您不需要在執行更新指令碼之後，更新此作業步驟。  
  
```sql
SELECT * FROM msdb.dbo.sysjobs  
WHERE @JobID = CONVERT(uniqueidentifier, $(ESCAPE_NONE(JOBID)));
```
  
## <a name="see-also"></a>另請參閱  
[實作作業](../../ssms/agent/implement-jobs.md)  
[管理作業步驟](../../ssms/agent/manage-job-steps.md)  
