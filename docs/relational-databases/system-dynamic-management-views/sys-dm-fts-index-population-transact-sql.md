---
description: sys.dm_fts_index_population (Transact-SQL)
title: sys.dm_fts_index_population (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_fts_index_population
- dm_fts_index_population
- sys.dm_fts_index_population_TSQL
- dm_fts_index_population_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_population dynamic management view
ms.assetid: 82d1c102-efcc-4b60-9a5e-3eee299bcb2b
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 06e804011443a4486c8ffbfd18d5ed639a3a3ec9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484680"
---
# <a name="sysdm_fts_index_population-transact-sql"></a>sys.dm_fts_index_population (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  傳回有關 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中目前進行中之全文檢索索引和語意關鍵片語擴展的資訊。  
 
|資料行名稱|資料類型|描述|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|包含要擴展之全文檢索索引的資料庫識別碼。|  
|**catalog_id**|**int**|包含這個全文檢索索引之全文檢索目錄的識別碼。|  
|**table_id**|**int**|要擴展全文檢索索引的資料表識別碼。|  
|**memory_address**|**varbinary(8)**|用來表示使用中母體擴展之內部資料結構的記憶體位址。|  
|**population_type**|**int**|母體擴展的類型。 下列其中之一：<br /><br /> 1 = 完整母體擴展。<br /><br /> 2 = 累加、以時間戳記為基礎的母體擴展<br /><br /> 3 = 追蹤變更的手動更新。<br /><br /> 4 = 追蹤變更的背景更新。|  
|**population_type_description**|**nvarchar(120)**|母體擴展類型的描述。|  
|**is_clustered_index_scan**|**bit**|指出母體擴展是否涉及叢集索引上的掃描。|  
|**range_count**|**int**|這個母體擴展平行處理的子範圍數目。|  
|**completed_range_count**|**int**|完成處理的範圍數目。|  
|**outstanding_batch_count**|**int**|目前此母體擴展未處理的批次數目。 如需詳細資訊，請參閱 [sys.dm_fts_outstanding_batches &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md)。|  
|**status**|**int**|**適用對象**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更新版本。<br /><br /> 這個母體擴展的狀態。 注意：有些狀態是暫時性。 下列其中之一：<br /><br /> 3 = 啟動中<br /><br /> 5 = 正常處理<br /><br /> 7 = 已停止處理<br /><br /> 例如，當自動合併正在進行時，就會發生這個狀態。<br /><br /> 11 = 母體擴展中止<br /><br /> 12 = 正在處理語意相似度擷取|  
|**status_description**|**nvarchar(120)**|母體擴展狀態的描述。|  
|**completion_type**|**int**|這個母體擴展如何完成的狀態。|  
|**completion_type_description**|**nvarchar(120)**|完成類型的描述。|  
|**worker_count**|**int**|這個值一定是 0。|  
|**queued_population_type**|**int**|母體擴展的類型，以追蹤變更為基礎，將遵照目前的母體擴展 (如果有的話)。|  
|**queued_population_type_description**|**nvarchar(120)**|要遵照之母體擴展的描述 (如果有的話)。 例如，當 CHANGE TRACKING = AUTO 而且初始完整母體擴展正在進行時，這個資料行就會顯示「自動母體擴展」。|  
|**start_time**|**datetime**|啟動母體擴展的時間。|  
|**incremental_timestamp**|**timestamp**|代表完整母體擴展的起始時間戳記。 如果是其他所有母體擴展類型，這個值是最後一個認可的檢查點，代表母體擴展的進度。|  
  
## <a name="remarks"></a>備註  
 如果除了全文檢索索引之外還啟用了統計語意索引，則關鍵片語的語意擷取和擴展以及擷取文件相似度資料會與全文檢索索引同步執行。 擴展文件相似度索引會在稍後的第二個階段執行。 如需詳細資訊，請參閱 [管理和監視語義搜尋](../../relational-databases/search/manage-and-monitor-semantic-search.md)。  
  
## <a name="permissions"></a>權限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 許可權。   
在 SQL Database Basic、S0 和 S1 服務目標上，以及針對彈性集區中的資料庫，則 `Server admin` `Azure Active Directory admin` 需要或帳戶。 在所有其他 SQL Database 服務目標上， `VIEW DATABASE STATE` 資料庫中都需要有許可權。   
  
## <a name="physical-joins"></a>實體聯結  
 ![這個動態管理檢視的重要聯結](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-index-population-1.gif "這個動態管理檢視的重要聯結")  
  
## <a name="relationship-cardinalities"></a>關聯性基數  
  
|寄件者|收件者|關聯性|  
|----------|--------|------------------|  
|dm_fts_active_catalogs.database_id|dm_fts_index_population.database_id|一對一|  
|dm_fts_active_catalogs.catalog_id|dm_fts_index_population.catalog_id|一對一|  
|dm_fts_population_ranges.parent_memory_address|dm_fts_index_population.memory_address|多對一|  
  
## <a name="see-also"></a>另請參閱  
 [動態管理檢視與函數 &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [&#40;Transact-sql&#41;的全文檢索搜尋和語義搜尋動態管理檢視和函數 ](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  

