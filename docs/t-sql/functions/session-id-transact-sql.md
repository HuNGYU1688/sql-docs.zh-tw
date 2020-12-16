---
description: SESSION_ID (Transact-SQL)
title: SESSION_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/23/2018
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
author: VanMSFT
ms.author: vanto
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 0a3b16a5e7c5fd45f1349822afb28af4a5123557
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481879"
---
# <a name="session_id-transact-sql"></a>SESSION_ID (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  傳回目前 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 或 [!INCLUDE[ssPDW_md](../../includes/sspdw-md.md)] 工作階段的識別碼。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例 &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```syntaxsql  
-- Azure Synapse Analytics and Parallel Data Warehouse  
SESSION_ID ( )  
```  
  
## <a name="return-value"></a>傳回值  
 傳回 **nvarchar(32)** 值。  
  
## <a name="general-remarks"></a>一般備註  
 工作階段識別碼是在建立連接時，指派給每個使用者連接的。 它在連接期間持續存在。 連線結束時，就會釋出工作階段識別碼。  
  
 工作階段識別碼的開頭為依字母順序排列的字元 'SID'。 在 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] 命令中使用工作階段識別碼時，這些是區分大小寫而且必須大寫的。  
  
 您可以查詢檢視 [sys.dm_pdw_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md) 來擷取與此函數相同的資訊。  
  
## <a name="examples"></a>範例  
 下列範例會傳回目前的工作階段識別碼。  
  
```sql  
SELECT SESSION_ID();  
```  
  
## <a name="see-also"></a>另請參閱  
 [DB_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/db-name-transact-sql.md)   
 [VERSION &#40;Azure Synapse Analytics&#41;](../../t-sql/functions/version-transact-sql-configuration-functions.md)
  
  
