---
description: sp_fulltext_database (Transact-SQL)
title: sp_fulltext_database (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_fulltext_database_TSQL
- sp_fulltext_database
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_database
ms.assetid: eeb1e151-eb00-484c-8fd1-5641e621ffc6
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: daa24fdd0bbba0ff949b7b43a74e312d114d4e0d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439443"
---
# <a name="sp_fulltext_database-transact-sql"></a>sp_fulltext_database (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  對於 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更新版本中的全文檢索目錄沒有任何影響，而且是為了回溯相容性才提供支援。 **sp_fulltext_database** 不會停用指定資料庫的 Full-Text 引擎。  中所有使用者建立的資料庫一定會啟用全文檢索索引。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 請改用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```  
  
sp_fulltext_database [@action=] 'action'  
```  
  
## <a name="arguments"></a>引數  
`[ @action = ] 'action'` 這是要執行的動作。 **動作** 是 **Varchar (20)**，它可以是下列值之一。  
  
|值|描述|  
|-----------|-----------------|  
|**enable**|支援這個項目的目的，只是為了與舊版相容。 對於 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更新版本中的全文檢索目錄沒有任何影響。|  
|**disable**|支援這個項目的目的，只是為了與舊版相容。 對於 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更新版本中的全文檢索目錄沒有任何影響。|  
  
## <a name="return-code-values"></a>傳回碼值  
 0 (成功) 或 1 (失敗)  
  
## <a name="result-sets"></a>結果集  
 None  
  
## <a name="remarks"></a>備註  
 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更新版本中，全文檢索索引無法關閉。 停用全文檢索索引並不會從 **sysfulltextcatalogs** 中移除資料列，也不表示啟用全文檢索的資料表不再標示全文檢索索引。 所有全文檢索中繼資料定義都仍在系統資料表中。  
  
## <a name="permissions"></a>權限  
 只有 **系統管理員（sysadmin** ）固定伺服器角色和 **db_owner** 固定資料庫角色的成員，才能夠執行 **sp_fulltext_database**。  
  
## <a name="see-also"></a>另請參閱  
 [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/databasepropertyex-transact-sql.md)   
 [FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
 [系統預存程序 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
