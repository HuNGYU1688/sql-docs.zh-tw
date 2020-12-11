---
description: sys.dm_fts_active_catalogs (Transact-SQL)
title: sys.dm_fts_active_catalogs (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_fts_active_catalogs_TSQL
- dm_fts_active_catalogs
- dm_fts_active_catalogs_TSQL
- sys.dm_fts_active_catalogs
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_active_catalogs dynamic management view
ms.assetid: 40ab5453-040c-4d2e-bb49-e340cf90c3ee
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3f352af03115de658f16e56e27bc5ecf3bd900c3
ms.sourcegitcommit: 2991ad5324601c8618739915aec9b184a8a49c74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2020
ms.locfileid: "97324401"
---
# <a name="sysdm_fts_active_catalogs-transact-sql"></a>sys.dm_fts_active_catalogs (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  傳回全文檢索目錄的相關資訊，這些目錄正在伺服器上進行某個母體擴展活動。  
  
> [!NOTE]
>  未來的版本將移除下列資料行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ： is_paused、previous_status、previous_status_description、row_count_in_thousands、status、status_description 和 worker_count。 請避免在新的開發工作中使用這些資料行，並規劃修改目前使用這些資料行的應用程式。  
  
 
|資料行名稱|資料類型|描述|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|包含使用中全文檢索目錄的資料庫識別碼。|  
|**catalog_id**|**int**|使用中全文檢索目錄的識別碼。|  
|**memory_address**|**varbinary(8)**|配置給與這個全文檢索目錄有關之母體擴展活動的記憶體緩衝區位址。|  
|**name**|**nvarchar(128)**|使用中全文檢索目錄的名稱。|  
|**is_paused**|**bit**|指出使用中全文檢索目錄的母體擴展是否已經暫停。|  
|**status**|**int**|全文檢索目錄的目前狀態。 下列其中之一：<br /><br /> 0 = 正在初始化<br /><br /> 1 = 已就緒<br /><br /> 2 = 已暫停<br /><br /> 3 = 暫時錯誤<br /><br /> 4 = 需要重新掛載<br /><br /> 5 = 已關閉<br /><br /> 6 = 默認備份<br /><br /> 7 = 已備份整個目錄<br /><br /> 8 = 目錄已損毀|  
|**status_description**|**nvarchar(120)**|使用中全文檢索目錄目前狀態的描述。|  
|**previous_status**|**int**|全文檢索目錄的先前狀態。 下列其中之一：<br /><br /> 0 = 正在初始化<br /><br /> 1 = 已就緒<br /><br /> 2 = 已暫停<br /><br /> 3 = 暫時錯誤<br /><br /> 4 = 需要重新掛載<br /><br /> 5 = 已關閉<br /><br /> 6 = 默認備份<br /><br /> 7 = 已備份整個目錄<br /><br /> 8 = 目錄已損毀|  
|**previous_status_description**|**nvarchar(120)**|使用中全文檢索目錄先前狀態的描述。|  
|**worker_count**|**int**|目前在使用這個全文檢索目錄的執行緒數目。|  
|**active_fts_index_count**|**int**|擴展中的全文檢索索引數目。|  
|**auto_population_count**|**int**|正為此全文檢索目錄進行自動母體擴展的資料表數目。|  
|**manual_population_count**|**int**|正為此全文檢索目錄進行手動母體擴展的資料表數目。|  
|**full_incremental_population_count**|**int**|針對這個全文檢索目錄在進行完整或累加母體擴展的資料表數目。|  
|**row_count_in_thousands**|**int**|這個全文檢索目錄中所有全文檢索索引中的預估資料列數 (以千為單位)。|  
|**is_importing**|**bit**|指出是否正在匯入全文檢索目錄：<br /><br /> 1 = 正在匯入此目錄。<br /><br /> 2 = 沒有正在匯入此目錄。|  
  
## <a name="remarks"></a>備註  
 Is_importing 的資料行是中的新資料行 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 。  
  
## <a name="permissions"></a>權限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 許可權。   
在 SQL Database Basic、S0 和 S1 服務目標上，以及針對彈性集區中的資料庫，則 `Server admin` `Azure Active Directory admin` 需要或帳戶。 在所有其他 SQL Database 服務目標上， `VIEW DATABASE STATE` 資料庫中都需要有許可權。   
   
## <a name="physical-joins"></a>實體聯結  
 ![這個動態管理檢視的重要聯結](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-active-catalogs-1.gif "這個動態管理檢視的重要聯結")  
  
## <a name="relationship-cardinalities"></a>關聯性基數  
  
|寄件者|收件者|關聯性|  
|----------|--------|------------------|  
|dm_fts_active_catalogs.database_id|dm_fts_index_population.database_id|一對一|  
|dm_fts_active_catalogs.catalog_id|dm_fts_index_population.catalog_id|一對一|  
  
## <a name="examples"></a>範例  
 下列範例會傳回目前資料庫上之作用中全文檢索目錄的詳細資訊。  
  
```  
SELECT catalog.name, catalog.is_importing, catalog.auto_population_count,  
  OBJECT_NAME(population.table_id) AS table_name,  
  population.population_type_description, population.is_clustered_index_scan,  
  population.status_description, population.completion_type_description,  
  population.queued_population_type_description, population.start_time,  
  population.range_count   
FROM sys.dm_fts_active_catalogs catalog   
CROSS JOIN sys.dm_fts_index_population population   
WHERE catalog.database_id = population.database_id   
AND catalog.catalog_id = population.catalog_id   
AND catalog.database_id = (SELECT dbid FROM sys.sysdatabases WHERE name = DB_NAME());  
GO  
```  
  
## <a name="see-also"></a>另請參閱  
 
 [&#40;Transact-sql&#41;的全文檢索搜尋和語義搜尋動態管理檢視和函數 ](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  
