---
description: sys.fulltext_index_fragments (Transact-SQL)
title: sys.fulltext_index_fragments (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fulltext_index_fragments
- sys.fulltext_index_fragments_TSQL
- fulltext_index_fragments_TSQL
- sys.fulltext_index_fragments
dev_langs:
- TSQL
helpviewer_keywords:
- full-text indexes [SQL Server], fragments
- full-text indexes [SQL Server], metadata
- troubleshooting [SQL Server], full-text search
- sys.fulltext_index_fragments catalog view
ms.assetid: a82e5018-5d88-45c0-9a47-c251e17a6cdb
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5e0f1882c6840048e3308120e6b8768897b43d6b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475179"
---
# <a name="sysfulltext_index_fragments-transact-sql"></a>sys.fulltext_index_fragments (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  全文檢索索引會使用稱為 *全文檢索索引片段* 的內部資料表來儲存反向索引資料。 此檢視表可用來查詢有關這些片段的中繼資料， 此檢視表針對每一個資料表內包含全文檢索索引的每一個全文檢索索引片段各包含一個資料列。  
 
  
|資料行名稱|資料類型|描述|  
|-----------------|---------------|-----------------|  
|table_id|**int**|包含全文檢索索引片段之物件的物件識別碼。|  
|fragment_object_id|**int**|與此片段相關聯之內部資料表的物件識別碼。|  
|fragment_id|**int**|全文檢索索引片段的邏輯識別碼。 這在此資料表的所有片段中是唯一的。|  
|timestamp|**timestamp**|與片段建立有關聯的時間戳記。 最近片段的時間戳記大於較舊片段的時間戳記。|  
|data_size|**int**|片段的邏輯大小 (以位元組為單位)。|  
|row_count|**int**|片段中個別資料列的數目。|  
|status|**int**|片段的狀態，以下其中一項：<br /><br /> 0 = 新建且尚未使用<br /><br /> 1 = 在全文檢索索引母體擴展或合併期間用於插入<br /><br /> 4 = 已關閉。 準備查詢<br /><br /> 6 = 用於合併輸入及準備查詢<br /><br /> 8 = 標示為刪除。 將不會用於查詢和合併來源。<br /><br /> 如果狀態為4或6，表示片段是邏輯全文檢索索引的一部分，而且可以查詢;也就是說，它是可 *查詢* 的片段。|  
  
## <a name="remarks"></a>備註  
 sys.fulltext_index_fragments 目錄檢視可用來查詢包含全文檢索索引的片段數。 如果您遇到全文檢索查詢效能緩慢的問題，您可以使用 sys.fulltext_index_fragments 來查詢全文檢索索引中的可查詢片段數 (狀態 = 4 或 6)，如下所示：  
  
```  
SELECT table_id, status FROM sys.fulltext_index_fragments  
   WHERE status=4 OR status=6;  
```  
  
 如果有許多可查詢的片段存在，Microsoft 建議您重新組織包含全文檢索索引的全文檢索目錄，將這些片段合併在一起。 若要重新組織全文檢索目錄，請使用 [ALTER 全文檢索目錄](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)*catalog_name* 重新組織。 例如，若要重新組織 `ftCatalog` 資料庫內名為 `AdventureWorks2012` 的全文檢索目錄，請輸入：  
  
```  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT CATALOG ftCatalog REORGANIZE;  
GO  
```  
  
## <a name="permissions"></a>權限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>另請參閱  
 [物件目錄檢視 &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [擴展全文檢索索引](../../relational-databases/search/populate-full-text-indexes.md)  
  
  
