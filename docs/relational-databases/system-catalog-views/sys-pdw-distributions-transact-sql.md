---
description: 'sys.pdw_distributions (Transact-sql) '
title: sys.pdw_distributions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 572b5187-9753-4063-adf8-65dea87d11f8
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 054dc5391ce03fc079bae505ec3f8fbe2eca2ea2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461589"
---
# <a name="syspdw_distributions-transact-sql"></a>sys.pdw_distributions (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  保存設備上分佈的相關資訊。 它會針對每個設備散發列出一個資料列。  
  
|資料行名稱|資料類型|描述|範圍|  
|-----------------|---------------|-----------------|-----------|  
|distribution_id|**int**|與散發相關聯的唯一數值識別碼。<br /><br /> 此視圖的索引鍵。|1到設備中的計算節點數目乘以每個計算節點的散發數目。|  
|pdw_node_id|**int**|此散發所在之節點的識別碼。|請參閱 [sys.dm_pdw_nodes &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md)中的 pdw_node_id。|  
|NAME|**nvarchar(32)**|與散發相關聯的字串識別碼，用來做為分散式資料表的尾碼。|由 ' A-z '、' a-z '、' 0-9 '、' _ '、'-' 組成的字串。|  
|position|**int**|分佈于該節點上其他散發之節點內的位置。|1到每個節點的散發數目。|  
  
## <a name="see-also"></a>另請參閱  
 [Azure Synapse Analytics 和平行處理資料倉儲目錄檢視](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
