---
description: sys.filegroups (Transact-SQL)
title: sys. filegroup (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.filegroups_TSQL
- filegroups
- sys.filegroups
- filegroups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.filegroups catalog view
ms.assetid: 9e851f72-1f8e-4515-a25d-152ebc12ed56
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d5e55e91e0a2ad031236f3d5fc7a255e9752f2a2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473019"
---
# <a name="sysfilegroups-transact-sql"></a>sys.filegroups (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  針對身為檔案群組的每個資料空間，各包含一個資料列。  
  
|資料行名稱|資料類型|描述|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|--|如需此 view 所繼承之資料行的清單，請參閱 [sys.data_spaces &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)。|  
|**filegroup_guid**|**uniqueidentifier**|檔案群組的 GUID。<br /><br /> NULL = PRIMARY 檔案群組|  
|**log_filegroup_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，這個值是 NULL。|  
|**is_read_only**|**bit**|1 = 檔案群組是唯讀的。<br /><br /> 0 = 檔案群組是可讀寫的。|  
|**is_autogrow_all_files**|**bit**|**適用於**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 至 [目前版本](../../sql-server/what-s-new-in-sql-server-2016.md))。<br /><br /> 1 = 當檔案群組中的檔案符合自動成長閾值時，檔案群組中的所有檔案都會成長。<br /><br /> 0 = 當檔案群組中的檔案符合自動成長閾值時，只會增加該檔案。 此為預設值。|  
  
## <a name="permissions"></a>權限  
 需要 **public** 角色的成員資格。 如需相關資訊，請參閱 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另請參閱  
 [目錄檢視 &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [資料空間 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/data-spaces-transact-sql.md)  
  
