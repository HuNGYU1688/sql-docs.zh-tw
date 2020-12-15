---
description: sys.server_principals (Transact-SQL)
title: sys.server_principals (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- server_principals
- sys.server_principals_TSQL
- sys.server_principals
- server_principals_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_principals catalog view
ms.assetid: c5dbe0d8-a1c8-4dc4-b9b1-22af20effd37
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: daa784b6bb1c54de2f5efea90ac017621b037780
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464639"
---
# <a name="sysserver_principals-transact-sql"></a>sys.server_principals (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  針對每一個伺服器層級的主體，各包含一個資料列。  
  
|資料行名稱|資料類型|描述|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|主體的名稱。 在伺服器中，這是唯一的。|  
|**principal_id**|**int**|主體的識別碼。 在伺服器中，這是唯一的。|  
|**希**|**varbinary(85)**|主體的 SID (安全性識別碼)。 如果是 Windows 主體，則與 Windows SID 相符。|  
|**type**|**char(1)**|主體類型：<br /><br /> S = SQL 登入<br /><br /> U = Windows 登入<br /><br /> G = Windows 群組<br /><br /> R = 伺服器角色<br /><br /> C = 對應至憑證的登入<br /><br /> E = 來自 Azure Active Directory 的外部登入<br /><br /> X = 來自 Azure Active Directory 群組或應用程式的外部群組<br /><br /> K = 對應至非對稱金鑰的登入|  
|**type_desc**|**nvarchar(60)**|主體類型的描述：<br /><br /> SQL_LOGIN<br /><br /> WINDOWS_LOGIN<br /><br /> WINDOWS_GROUP<br /><br /> SERVER_ROLE<br /><br /> CERTIFICATE_MAPPED_LOGIN<br /><br /> EXTERNAL_LOGIN<br /><br /> EXTERNAL_GROUP<br /><br /> ASYMMETRIC_KEY_MAPPED_LOGIN|  
|**is_disabled**|**int**|1 = 登入已停用。|  
|**create_date**|**datetime**|建立主體的時間。|  
|**modify_date**|**datetime**|上次修改主體定義的時間。|  
|**default_database_name**|**sysname**|這個主體的預設資料庫。|  
|**default_language_name**|**sysname**|這個主體的預設語言。|  
|**credential_id**|**int**|與這個主體相關聯的認證識別碼。 如果沒有與這個主體相關聯的認證，則 credential_id 為 NULL。|  
|**owning_principal_id**|**int**|伺服器角色擁有者的 **principal_id** 。 如果主體不是伺服器角色，則為 NULL。|  
|**is_fixed_role**|**bit**|如果主體是具有固定許可權的其中一個內建伺服器角色，則會傳回1。 如需詳細資訊，請參閱 [伺服器層級角色](../../relational-databases/security/authentication-access/server-level-roles.md)。|  
  
## <a name="permissions"></a>權限  
 任何登入都可以查看他們自己的登入名稱、系統登入和固定伺服器角色。 若要查看其他登入，則需要 ALTER ANY LOGIN 或該登入的權限。 若要查看使用者定義伺服器角色，則需要 ALTER ANY SERVER ROLE 或該角色的成員資格。  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 如需相關資訊，請參閱 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>範例  
 下列查詢會列出已明確授與或拒絕伺服器主體的權限。  
  
> [!IMPORTANT]  
>   (公用) 以外的固定伺服器角色的許可權不會出現在 sys.server_permissions 中。 因此，伺服器主體可能仍有其他未列於此處的權限。  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pe.state_desc, pe.permission_name   
FROM sys.server_principals AS pr   
JOIN sys.server_permissions AS pe   
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
## <a name="see-also"></a>另請參閱  
 [安全性目錄檢視 &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [目錄檢視 &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [主體 &#40;Database Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [權限階層 &#40;Database Engine&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)  
  
  
