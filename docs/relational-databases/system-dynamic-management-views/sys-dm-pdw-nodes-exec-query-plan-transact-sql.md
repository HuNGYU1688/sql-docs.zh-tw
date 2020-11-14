---
title: sys.dm_pdw_nodes_exec_query_plan (Transact-sql) |Microsoft Docs
description: 動態管理檢視，可針對計畫控制碼所指定的批次，以 XML 格式傳回顯示計畫。 計畫控制代碼指定的計畫可以是快取或目前正在執行的計畫。
ms.custom: ''
ms.date: 10/14/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: ''
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: 06c5acb9480f52d0cadf84c54aa39bbc9bae12d9
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584932"
---
# <a name="syspdw_nodes_dm_exec_query_plan-transact-sql"></a>sys.pdw_nodes_dm_exec_query_plan (Transact-sql) 
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

針對計畫控制代碼指定的批次，以 XML 格式傳回顯示計畫。 計畫控制代碼指定的計畫可以是快取或目前正在執行的計畫。  

> [!note] 
> 在 Synapse SQL 中，在查詢中加入空白字元會構成查詢變更，而這項變更會導致重新計算查詢雜湊，而不會重複使用先前的快取執行計畫。


## <a name="table-returned"></a>傳回的資料表  
  
|資料行名稱|資料類型|描述|  
|-----------------|---------------|-----------------|  
|**pdw_node_id**|**int**|與節點相關聯的唯一數值識別碼。| 
|**dbid**|**smallint**|當編譯對應於這個計畫的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 陳述式時，作用中內容資料庫的識別碼。 若為未規劃和已備妥的 SQL 語句，則為編譯語句的資料庫識別碼。<br /><br /> 資料行可為 Null。|  
|**objectid**|**int**|這個查詢計畫的物件識別碼 (如預存程序或使用者自訂函數)。 若為特定和準備批次，這個資料行是 **Null** 。<br /><br /> 資料行可為 Null。|  
|**number**|**smallint**|編號預存程序整數。 若為特定和準備批次，這個資料行是 **Null** 。<br /><br /> 資料行可為 Null。| 
|**加密**|**bit**|指出對應的預存程序是否加密。<br /><br /> 0 = 未加密<br /><br /> 1 = 加密<br /><br /> 資料行不可為 Null。|  
|**query_plan**|**xml**|包含以 *plan_handle* 指定之查詢執行計畫的編譯階段顯示計畫標記法。 顯示計畫是 XML 格式。 每個包含諸如特定 [!INCLUDE[tsql](../../includes/tsql-md.md)] 陳述式、預存程序呼叫和使用者自訂函數呼叫的批次，都會產生一份計畫。<br /><br /> 資料行可為 Null。|  
  
## <a name="remarks"></a>備註  
[Sys.dm_exec_query_plan](./sys-dm-exec-query-plan-transact-sql.md?view=sql-server-ver15)中的相同備註也適用。  
  
## <a name="permissions"></a>權限  
 需要伺服器的 **系統管理員（sysadmin** ）伺服器角色或 `VIEW SERVER STATE` 許可權。  
  
## <a name="see-also"></a>另請參閱  
 [Azure Synapse Analytics 和平行處理資料倉儲動態管理檢視 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  

 ## <a name="next-steps"></a>後續步驟
 如需更多開發秘訣，請參閱 [Azure Synapse Analytics 開發總覽](/azure/sql-data-warehouse/sql-data-warehouse-overview-develop)。
