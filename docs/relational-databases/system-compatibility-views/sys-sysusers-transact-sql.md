---
description: sys.sysusers (Transact-SQL)
title: sys.sys(Transact-sql) 的使用者 |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysusers
- sys.sysusers_TSQL
- sysusers_TSQL
- sysusers
dev_langs:
- TSQL
helpviewer_keywords:
- sysusers system table
- sys.sysusers compatibility view
ms.assetid: 5f0e6a8d-c983-44f6-97e9-aab5bff67d18
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2a38e8fd5593228ca831c39add1942ce6b0b6621
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97428386"
---
# <a name="syssysusers-transact-sql"></a>sys.sysusers (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  針對資料庫中的每個 [!INCLUDE[msCoName](../../includes/msconame-md.md)] windows 使用者、windows 群組、 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用者或角色，各包含一個 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 資料列。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|資料行名稱|資料類型|描述|  
|-----------------|---------------|-----------------|  
|**Uid**|**smallint**|使用者識別碼，在這個資料庫中是唯一的。<br /><br /> 1 = 資料庫擁有者<br /><br /> 如果使用者和角色數目超過 32,767 個，則會造成溢位或傳回 NULL。|  
|**status**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**name**|**sysname**|使用者名稱或群組名稱，在這個資料庫中是唯一的。|  
|**希**|**varbinary(85)**|這個項目的安全性識別碼。|  
|**角色**|**varbinary(2048)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**createdate**|**datetime**|加入帳戶的日期。|  
|**updatedate**|**datetime**|上次變更帳戶的日期。|  
|**altuid**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 如果使用者和角色數目超過 32,767 個，則會造成溢位或傳回 NULL。|  
|**password**|**varbinary(256)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**gid**|**smallint**|這位使用者所屬的群組識別碼。 如果 **uid** 與 **gid** 相同，則此專案會定義群組。 如果群組和使用者結合的數目超過 32,767，則會造成溢位或傳回 NULL。|  
|**環境**|**varchar(255)**|保留的。|  
|**hasdbaccess**|**int**|1 = 帳戶有資料庫存取權。|  
|**islogin**|**int**|1 = 帳戶是具有登入帳戶的 Windows 群組、Windows 使用者或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用者。|  
|**isntname**|**int**|1 = 帳戶是 Windows 群組或 Windows 使用者。|  
|**isntgroup**|**int**|1 = 帳戶是 Windows 群組。|  
|**isntuser**|**int**|1 = 帳戶是 Windows 使用者。|  
|**issqluser**|**int**|1 = 帳戶是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用者。|  
|**isaliased**|**int**|1 = 帳戶是另一位使用者的別名。|  
|**issqlrole**|**int**|1 = 帳戶是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 角色。|  
|**isapprole**|**int**|1 = 帳戶是應用程式角色。|  
  
## <a name="see-also"></a>另請參閱  
 [&#40;Transact-sql&#41;將系統資料表對應至系統檢視 ](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [相容性檢視 &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
