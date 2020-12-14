---
description: sys.memory_optimized_tables_internal_attributes (Transact-SQL)
title: sys.memory_optimized_tables_internal_attributes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.memory_optimized_tables_internal_attributes
- sys.memory_optimized_tables_internal_attributes_TSQL
- memory_optimized_tables_internal_attributes
- memory_optimized_tables_internal_attributes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.memory_optimized_tables_internal_attributes catalog view
ms.assetid: 78ef5807-0504-4de8-9a01-ede6c03c7ff1
author: kevin-farlee
ms.author: kfarlee
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a6a32b926eebf2e8a7d80823004824ec377ec199
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97404884"
---
# <a name="sysmemory_optimized_tables_internal_attributes-transact-sql"></a>sys.memory_optimized_tables_internal_attributes (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

讓用於儲存使用者記憶體最佳化資料表的每個內部記憶體最佳化資料表都包含一列資料列。 每個使用者資料表都會對應一或多份內部資料表。 單一資料表用於儲存核心資料。 其他的內部資料表用來支援功能，例如暫時的資料行存放區索引與記憶體最佳化資料表的非資料列 (LOB) 儲存。
 
| 資料行名稱  | 資料類型  | 描述 |
| :------ |:----------| :-----|
|object_id  |**int**|       使用者資料表識別碼。 為支援使用者資料表而存在的內部記憶體最佳化資料表 (例如 Hk/資料行存放區組合時的非資料列儲存或刪除的資料列) 與其父系有相同的 object_id。 |
|xtp_object_id  |**bigint**|    對應至內部記憶體最佳化資料表的記憶體內部 OLTP 物件識別碼，可用於支援使用者資料表。 它是資料庫內唯一且可因物件的存留期而變更。 
|類型|  **int** |   內部資料表的類型。<br/><br/> 0 => DELETED_ROWS_TABLE <br/> 1 => USER_TABLE <br/> 2 => DICTIONARIES_TABLE<br/>3 => SEGMENTS_TABLE<br/>4 => ROW_GROUPS_INFO_TABLE<br/>5 => INTERNAL OFF-ROW DATA TABLE<br/>252 => INTERNAL_TEMPORAL_HISTORY_TABLE | 
|type_desc| **nvarchar(60)**|   類型的描述<br/><br/>DELETED_ROWS_TABLE -> 追蹤資料行存放區索引之已刪除資料列的內部資料表<br/>USER_TABLE -> 包含同資料列使用者資料的資料表<br/>DICTIONARIES_TABLE -> 資料行存放區索引字典<br/>SEGMENTS_TABLE -> 資料行存放區索引的壓縮區段<br/>ROW_GROUPS_INFO_TABLE -> 資料行存放區索引之壓縮資料列群組的相關中繼資料<br/>INTERNAL OFF-ROW DATA TABLE -> 用來儲存非資料列資料行的內部資料表。 在此情況下，minor_id 會反映 column_id。<br/>INTERNAL_TEMPORAL_HISTORY_TABLE -> 磁碟式記錄資料表的熱結尾。 記錄中插入的資料列會先插入此內部記憶體最佳化資料表。 有一項背景工作會以非同步方式，將此內部資料表的資料列移至磁碟式記錄資料表。 |
|minor_id|  **int**|    0 表示使用者或內部資料表<br/><br/>非 0 表示 off-row 儲存的資料行識別碼。 sys.columns 中有 column_id 的結合。<br/><br/>每個 off-row 儲存的資料行在此系統檢視中都有對應的資料列。|

## <a name="permissions"></a>權限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 如需相關資訊，請參閱 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>範例  
  
### <a name="a-returning-all-columns-that-are-stored-off-row"></a>A. 傳回所有 off-row 儲存的資料行

以下 T-SQL 指令碼會示範具有多個大型非 LOB 資料行和單一 LOB 資料行的資料表：

```Transact-SQL
CREATE TABLE dbo.LargeTableSample
(
      Id   int IDENTITY PRIMARY KEY NONCLUSTERED,
      C1   nvarchar(4000),
      C2   nvarchar(4000),
      C3   nvarchar(4000),
      C4   nvarchar(4000),
      Misc nvarchar(max)
) WITH (MEMORY_OPTIMIZED = ON);
GO
```

下列查詢會顯示 off-row 儲存的所有資料行，以及其大小。 大小為 -1 表示 LOB 資料行。 所有 LOB 資料行都會 off-row 儲存。

```Transact-SQL
SELECT 
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table', 
  c.name AS 'column', 
  c.max_length
FROM sys.memory_optimized_tables_internal_attributes moa
     JOIN sys.columns c ON moa.object_id = c.object_id AND moa.minor_id=c.column_id
     JOIN sys.objects o on moa.object_id=o.object_id 
WHERE moa.type=5;
```

### <a name="b-returning-memory-consumption-of-all-columns-that-are-stored-off-row"></a>B. 傳回所有 off-row 儲存之資料行的記憶體耗用量

若要取得 off-row 資料行記憶體耗用量的更多詳細資訊，您可以使用下列查詢，顯示所有儲存 off-row 資料行的內部資料表及其索引的記憶體耗用量︰

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  c.name AS 'column',
  c.max_length,
  mc.memory_consumer_desc,
  mc.index_id,
  mc.allocated_bytes,
  mc.used_bytes
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.columns c ON moa.object_id = c.object_id AND moa.minor_id=c.column_id
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id 
WHERE moa.type=5;
```

### <a name="c-returning-memory-consumption-of-columnstore-indexes-on-memory-optimized-tables"></a>C. 傳回記憶體最佳化資料表的資料行存放區索引的記憶體耗用量

使用下列查詢來顯示記憶體優化資料表上資料行存放區索引的記憶體耗用量：

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  i.name AS 'columnstore index',
  SUM(mc.allocated_bytes) / 1024 as [allocated_kb],
  SUM(mc.used_bytes) / 1024 as [used_kb]
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.indexes i ON moa.object_id = i.object_id AND i.type in (5,6)
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id
WHERE moa.type IN (0, 2, 3, 4)
GROUP BY o.schema_id, moa.object_id, i.name;
```

使用下列查詢，將記憶體優化資料表上用於資料行存放區索引的內部結構的記憶體耗用量細分：

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  i.name AS 'columnstore index',
  moa.type_desc AS 'internal table',
  mc.index_id AS 'index',
  mc.memory_consumer_desc,
  mc.allocated_bytes / 1024 as [allocated_kb],
  mc.used_bytes / 1024 as [used_kb]
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.indexes i ON moa.object_id = i.object_id AND i.type in (5,6)
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id
WHERE moa.type IN (0, 2, 3, 4)
```


