---
description: STATS_DATE (Transact-SQL)
title: STATS_DATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STATS_DATE_TSQL
- STATS_DATE
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], last time updated
- STATS_DATE function
- query optimization statistics [SQL Server], last time updated
- last time statistics updated
- stats update date
ms.assetid: f9ec3101-1e41-489d-b519-496a0d6089fb
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f7aa9aaee8737a7e142b9717788a8d42647af38c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468119"
---
# <a name="stats_date-transact-sql"></a>STATS_DATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  針對資料表或索引檢視表的統計資料傳回最近更新的日期。  
  
 如需有關更新統計資料的詳細資訊，請參閱 [統計資料](../../relational-databases/statistics/statistics.md)。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```syntaxsql
STATS_DATE ( object_id , stats_id )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
 object_id  
 包含統計資料之資料表或索引檢視表的識別碼。  
  
 *stats_id*  
 統計資料物件的識別碼。  
  
## <a name="return-types"></a>傳回型別  
 成功時傳回 **datetime**。 若未建立統計資料 Blob，則傳回 **NULL**。  
  
## <a name="remarks"></a>備註  
 系統函數可以用於選取清單、WHERE 子句以及任何可以使用運算式的位置。  
 
 統計資料更新日期儲存在[統計資料 Blob 物件](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics)中，其中還有[長條圖](../../relational-databases/statistics/statistics.md#histogram)和[密度向量](../../relational-databases/statistics/statistics.md#density)，不是儲存在中繼資料中。 如果沒有讀取資料以產生統計資料，則不會建立統計 Blob，而且日期也不可使用。 這是已篩選統計資料的情況，其中述詞未傳回任何資料列，或為新的空白資料表的情況。
 
 如果統計資料對應到索引，[sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) 目錄檢視中的 *stats_id* 值會與 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 目錄檢視中的 *index_id* 值相同。
  
## <a name="permissions"></a>權限  
 需要 db_owner 固定資料庫角色中的成員資格或權限，才能檢視資料表或索引檢視表的中繼資料。  
  
## <a name="examples"></a>範例  
  
### <a name="a-return-the-dates-of-the-most-recent-statistics-for-a-table"></a>A. 針對資料表傳回最近更新統計資料的日期  
 下列範例會針對 `Person.Address` 資料表的每個統計資料物件傳回最近更新的日期。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_update_date  
FROM sys.stats   
WHERE object_id = OBJECT_ID('Person.Address');  
GO  
```  
  
 如果統計資料對應到索引，[sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) 目錄檢視中的 *stats_id* 值會與 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 目錄檢視中的 *index_id* 值相同，而且下列查詢會傳回與之前查詢相同的結果。 如果統計資料未對應到索引，統計資料會在 sys.stats 結果中而不是 sys.indexes 結果中。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name AS index_name,   
    STATS_DATE(object_id, index_id) AS statistics_update_date  
FROM sys.indexes   
WHERE object_id = OBJECT_ID('Person.Address');  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>範例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-learn-when-a-named-statistics-was-last-updated"></a>B. 了解具名統計資料上次更新時間  
 下列範例會建立 DimCustomer 資料表的 LastName 資料行的統計資料。 然後，它會執行查詢，以顯示統計資料的日期。 接著它會更新統計資料，並再次執行查詢，以顯示更新的日期。  
  
```sql
--First, create a statistics object  
USE AdventureWorksPDW2012;  
GO  
CREATE STATISTICS Customer_LastName_Stats  
ON AdventureWorksPDW2012.dbo.DimCustomer (LastName)  
WITH SAMPLE 50 PERCENT;  
GO  
  
--Return the date when Customer_LastName_Stats was last updated  
USE AdventureWorksPDW2012;  
GO  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer')  
    AND s.name = 'Customer_LastName_Stats';  
GO  
  
--Update Customer_LastName_Stats so it will have a different timestamp in the next query  
GO  
UPDATE STATISTICS dbo.dimCustomer (Customer_LastName_Stats);  
  
--Return the date when Customer_LastName_Stats was last updated.  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer')  
    AND s.name = 'Customer_LastName_Stats';  
GO    
```  
  
### <a name="c-view-the-date-of-the-last-update-for-all-statistics-on-a-table"></a>C. 檢視資料表上所有統計資料的上次更新日期  
 這個範例會傳回 DimCustomer 資料表上每個統計資料物件上次更新的日期。  
  
```sql  
--Return the dates all statistics on the table were last updated.  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer');  
GO  
```  
  
 如果統計資料對應到索引，[sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) 目錄檢視中的 *stats_id* 值會與 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 目錄檢視中的 *index_id* 值相同，而且下列查詢會傳回與之前查詢相同的結果。 如果統計資料未對應到索引，統計資料會在 sys.stats 結果中而不是 sys.indexes 結果中。  
  
```sql  
USE AdventureWorksPDW2012;  
GO  
SELECT name AS index_name,   
    STATS_DATE(object_id, index_id) AS statistics_update_date  
FROM sys.indexes   
WHERE object_id = OBJECT_ID('dbo.DimCustomer');  
GO  
```  
  
## <a name="see-also"></a>另請參閱  
 [系統函數 &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_autostats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)   
 [統計資料](../../relational-databases/statistics/statistics.md)    
 [sys.dm_db_stats_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
  
  

