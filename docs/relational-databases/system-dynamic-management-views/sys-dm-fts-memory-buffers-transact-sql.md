---
description: sys.dm_fts_memory_buffers (Transact-SQL)
title: sys.dm_fts_memory_buffers (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_fts_memory_buffers
- dm_fts_memory_buffers_TSQL
- dm_fts_memory_buffers
- sys.dm_fts_memory_buffers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_memory_buffers dynamic management view
ms.assetid: 56895fe5-e8df-4d75-9adc-c1f7757cdef8
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d4948ec70d4073f3c3fbae8774b1974c84af84e2
ms.sourcegitcommit: 2991ad5324601c8618739915aec9b184a8a49c74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2020
ms.locfileid: "97323762"
---
# <a name="sysdm_fts_memory_buffers-transact-sql"></a>sys.dm_fts_memory_buffers (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  傳回屬於特定記憶體集區之記憶體緩衝區的相關資訊，這些緩衝區會做為全文檢索搜耙的一部分或全文檢索搜耙範圍使用。  
  
> [!NOTE]
> 未來的版本將移除下列資料行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ： **row_count**。 請避免在新的開發工作中使用這個資料行，並請規劃修改目前使用這個資料行的應用程式。  

  
|資料行|資料類型|描述|  
|------------|---------------|-----------------|  
|**pool_id**|**int**|配置的記憶體集區識別碼。<br /><br /> 0 = 小緩衝區<br /><br /> 1 = 大緩衝區|  
|**memory_address**|**varbinary(8)**|配置記憶體集區的識別碼。|  
|**name**|**nvarchar(4000)**|共用記憶體緩衝區的名稱，這項配置就是針對該緩衝區而建立。|  
|**is_free**|**bit**|記憶體緩衝區的目前狀態。<br /><br /> 0 = 可用<br /><br /> 1 = 忙碌|  
|**row_count**|**int**|這個緩衝區目前正在處理的資料列數。|  
|**bytes_used**|**int**|這個緩衝區目前正在使用的記憶體量 (以位元組為單位)。|  
|**percent_used**|**int**|所用的配置記憶體百分比。|  
  
## <a name="permissions"></a>權限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 許可權。   
在 SQL Database Basic、S0 和 S1 服務目標上，以及針對彈性集區中的資料庫，則 `Server admin` `Azure Active Directory admin` 需要或帳戶。 在所有其他 SQL Database 服務目標上， `VIEW DATABASE STATE` 資料庫中都需要有許可權。   
  
## <a name="physical-joins"></a>實體聯結  
 ![這個動態管理檢視的重要聯結](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-memory-buffers-1.gif "這個動態管理檢視的重要聯結")  
  
## <a name="relationship-cardinalities"></a>關聯性基數  
  
|寄件者|收件者|關聯性|  
|----------|--------|------------------|  
|dm_fts_memory_buffers.pool_id|dm_fts_memory_pools.pool_id|多對一|  
  
## <a name="see-also"></a>另請參閱  
 [動態管理檢視與函數 &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [&#40;Transact-sql&#41;的全文檢索搜尋和語義搜尋動態管理檢視和函數 ](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  

