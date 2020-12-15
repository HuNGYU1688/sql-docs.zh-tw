---
description: sys.foreign_keys (Transact-SQL)
title: sys.foreign_keys (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- foreign_keys
- sys.foreign_keys
- sys.foreign_keys_TSQL
- foreign_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.foreign_keys catalog view
ms.assetid: e960df1a-13fc-43ee-ba91-34c1b719ac2c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4b802b75733116ec468cf82f20ad11ccab960e43
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97413159"
---
# <a name="sysforeign_keys-transact-sql"></a>sys.foreign_keys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  針對每個物件，各包含一個資料列，該物件是 FOREIGN KEY 條件約束，而 **sys. object. type** = F。  
  
|資料行名稱|資料類型|描述|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.objects>**||如需此視圖所繼承之資料行的清單，請參閱 [sys. objects &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)。|  
|**referenced_object_id**|**int**|被參考的物件識別碼。|  
|**key_index_id**|**int**|被參考的物件內之金鑰索引識別碼。|  
|**is_disabled**|**bit**|FOREIGN KEY 條件約束已停用。|  
|**is_not_for_replication**|**bit**|FOREIGN KEY 條件約束是利用 NOT FOR REPLICATION 選項來建立的。|  
|**is_not_trusted**|**bit**|FOREIGN KEY 條件約束尚未經過系統驗證。|  
|**delete_referential_action**|**tinyint**|在進行刪除時，針對這個 FOREIGN KEY 而宣告的參考動作。<br /><br /> 0 = 沒有動作<br /><br /> 1 = 串聯<br /><br /> 2 = 設為 NULL<br /><br /> 3 = 設為預設值|  
|**delete_referential_action_desc**|**nvarchar(60)**|在進行刪除時，針對這個 FOREIGN KEY 而宣告的參考動作之描述。<br /><br /> NO_ACTION<br /><br /> CASCADE<br /><br /> SET_NULL<br /><br /> SET_DEFAULT|  
|**update_referential_action**|**tinyint**|在進行更新時，針對這個 FOREIGN KEY 而宣告的參考動作。<br /><br /> 0 = 沒有動作<br /><br /> 1 = 串聯<br /><br /> 2 = 設為 NULL<br /><br /> 3 = 設為預設值|  
|**update_referential_action_desc**|**nvarchar(60)**|在進行更新時，針對這個 FOREIGN KEY 而宣告的參考動作之描述。<br /><br /> NO_ACTION<br /><br /> CASCADE<br /><br /> SET_NULL<br /><br /> SET_DEFAULT|  
|**is_system_named**|**bit**|1 = 名稱是系統所產生。<br /><br /> 0 = 名稱是使用者所提供。|  
  
## <a name="permissions"></a>權限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 如需相關資訊，請參閱 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另請參閱  
 [目錄檢視 &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [物件目錄檢視 &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [查詢 SQL Server 系統目錄 FAQ](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
