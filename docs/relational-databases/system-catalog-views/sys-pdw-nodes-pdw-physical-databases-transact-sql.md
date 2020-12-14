---
description: 'sys.pdw_nodes_pdw_physical_databases (Transact-sql) '
title: 'sys.pdw_nodes_pdw_physical_databases (Transact-sql) '
ms.custom: seo-dt-2019
ms.date: 03/09/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 70e0939d-4d97-4ae0-ba16-934e0a80e718
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: d32c6afc32fbea99aba927404d871ba3731fa86e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97404467"
---
# <a name="syspdw_nodes_pdw_physical_databases-transact-sql"></a>sys.pdw_nodes_pdw_physical_databases (Transact-sql) 
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  針對計算節點上的每個實體資料庫，各包含一個資料列。 匯總實體資料庫資訊，以取得有關資料庫的詳細資訊。 若要結合資訊，請將加入 `sys.pdw_nodes_pdw_physical_databases` 至 `sys.pdw_database_mappings` 和 `sys.databases` 資料表。  
  
|資料行名稱|資料類型|描述|  
|-----------------|---------------|-----------------|  
|database_id|**int**|資料庫的物件識別碼。 請注意，這個值與 sys. 資料庫中的 database_id 不同 [&#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) view。|  
|physical_name|**sysname**|Shell/計算節點上資料庫的機構名稱。 這個值與 [ [sys.pdw_database_mappings &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-pdw-database-mappings-transact-sql.md) 視圖] 的 [physical_name] 資料行中的值相同。|  
|pdw_node_id|**int**|與節點相關聯的唯一數值識別碼。|  
  
## <a name="examples-sspdw"></a>範例：[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-returning"></a>A. 返回  
 下列查詢會傳回 master 中每個資料庫的名稱和識別碼，以及每個計算節點上的對應資料庫名稱。  
  
```  
SELECT D.database_id AS DBID_in_master, D.name AS UserDatabaseName,   
PD.pdw_node_id AS NodeID, DM.physical_name AS PhysDBName   
FROM sys.databases AS D  
JOIN sys.pdw_database_mappings AS DM  
    ON D.database_id = DM.database_id  
JOIN sys.pdw_nodes_pdw_physical_databases AS PD  
    ON DM.physical_name = PD.physical_name  
ORDER BY D.database_id, PD.pdw_node_ID;  
```  
  
### <a name="b-using-syspdw_nodes_pdw_physical_databases-to-gather-detailed-object-information"></a>B. 使用 sys.pdw_nodes_pdw_physical_databases 來收集詳細的物件資訊  
 下列查詢會顯示索引的相關資訊，並包含有關物件屬於資料庫中物件之資料庫的有用資訊。  
  
```  
SELECT D.name AS UserDatabaseName, D.database_id AS DBIDinMaster,  
DM.physical_name AS PhysDBName, PD.pdw_node_id AS NodeID,   
IU.object_id, IU.index_id, IU.user_seeks, IU.user_scans, IU.user_lookups, IU.user_updates  
FROM sys.databases AS D  
JOIN sys.pdw_database_mappings AS DM  
    ON D.database_id = DM.database_id  
JOIN sys.pdw_nodes_pdw_physical_databases AS PD  
    ON DM.physical_name = PD.physical_name  
JOIN sys.dm_pdw_nodes_db_index_usage_stats AS IU  
    ON PD.database_id = IU.database_id  
ORDER BY D.database_id, IU.object_id, IU.index_id, PD.pdw_node_ID;  
```  
  
### <a name="c-using-syspdw_nodes_pdw_physical_databases-to-determine-the-encryption-state"></a>C. 使用 sys.pdw_nodes_pdw_physical_databases 來判斷加密狀態  
 下列查詢會提供 AdventureWorksPDW2012 資料庫的加密狀態。  
  
```  
WITH dek_encryption_state AS   
(  
    SELECT ISNULL(db_map.database_id, dek.database_id) AS database_id, encryption_state  
    FROM sys.dm_pdw_nodes_database_encryption_keys AS dek  
        INNER JOIN sys.pdw_nodes_pdw_physical_databases AS node_db_map  
            ON dek.database_id = node_db_map.database_id AND dek.pdw_node_id = node_db_map.pdw_node_id  
        LEFT JOIN sys.pdw_database_mappings AS db_map  
            ON node_db_map .physical_name = db_map.physical_name  
        INNER JOIN sys.dm_pdw_nodes AS nodes  
            ON nodes.pdw_node_id = dek.pdw_node_id  
    WHERE dek.encryptor_thumbprint <> 0x  
)  
SELECT TOP 1 encryption_state  
       FROM  dek_encryption_state  
       WHERE dek_encryption_state.database_id = DB_ID('AdventureWorksPDW2012 ')  
       ORDER BY (CASE encryption_state WHEN 3 THEN -1 ELSE encryption_state END) DESC;  
```  
  
## <a name="see-also"></a>另請參閱  
 [Azure Synapse Analytics 和平行處理資料倉儲目錄檢視](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.pdw_database_mappings &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-pdw-database-mappings-transact-sql.md)  
  
  

