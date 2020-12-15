---
description: sp_createstats (Transact-SQL)
title: sp_createstats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_createstats_TSQL
- sp_createstats
dev_langs:
- TSQL
helpviewer_keywords:
- sp_createstats
ms.assetid: 8204f6f2-5704-40a7-8d51-43fc832eeb54
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7af34bd1bbe065012b18826f7edaec31940d1e50
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466869"
---
# <a name="sp_createstats-transact-sql"></a>sp_createstats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  呼叫 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 語句來建立資料行的單一資料行統計資料，而這些資料行不是統計資料物件中的第一個資料行。 建立單一資料行統計資料會增加長條圖的數目，而且可能會改善基數估計值、查詢計劃和查詢效能。 統計資料物件的第一個資料行具有長條圖，但其他資料行則沒有長條圖。  
  
 當查詢執行時間很重要而且無法等候查詢最佳化工具產生單一資料行統計資料時，sp_createstats 對於效能評定等應用程式會很有用。 在大多數情況下，不需要使用 sp_createstats;當 **AUTO_CREATE_STATISTICS** 選項為 on 時，查詢最佳化工具會視需要產生單一資料行統計資料，以改善查詢計劃。  
  
 如需統計資料的詳細資訊，請參閱[統計資料](../../relational-databases/statistics/statistics.md)。 如需有關產生單一資料行統計資料的詳細資訊，請參閱 [ALTER DATABASE SET Options](../../t-sql/statements/alter-database-transact-sql-set-options.md)中的 **AUTO_CREATE_STATISTICS** 選項 &#40;transact-sql&#41;。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```  
  
sp_createstats   
    [   [ @indexonly =   ] { 'indexonly'   | 'NO' } ]   
    [ , [ @fullscan =    ] { 'fullscan'    | 'NO' } ]   
    [ , [ @norecompute = ] { 'norecompute' | 'NO' } ]  
    [ , [ @incremental = ] { 'incremental' | 'NO' } ]  
```  
  
## <a name="arguments"></a>引數  
`[ @indexonly = ] 'indexonly'` 只在現有索引中的資料行上建立統計資料，而不是任何索引定義中的第一個資料行。 **indexonly** 是 **char (9)**。 預設值是 NO。  
  
`[ @fullscan = ] 'fullscan'` 使用 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 語句搭配 **FULLSCAN** 選項。 **fullscan** 是 **char (9)**。  預設值是 NO。  
  
`[ @norecompute = ] 'norecompute'` 使用 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 語句搭配 **NORECOMPUTE** 選項。 **norecompute** 是 **char (12)**。  預設值是 NO。  
  
`[ @incremental = ] 'incremental'` 使用 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 語句搭配 **遞增 = ON** 選項。 **增量** 是 **char (12)**。  預設值是 NO。  
  
## <a name="return-code-values"></a>傳回碼值  
 0 (成功) 或 1 (失敗)  
  
## <a name="result-sets"></a>結果集  
 每個新的統計資料物件都與建立時所在的資料行具有相同的名稱。  
  
## <a name="remarks"></a>備註  
 sp_createstats 不會針對屬於現有統計資料物件中第一個資料行的資料行建立或更新統計資料。 這包括針對索引所建立之統計資料的第一個資料行、使用 AUTO_CREATE_STATISTICS 選項產生之單一資料行統計資料的資料行，以及使用 CREATE STATISTICS 語句所建立之統計資料的第一個資料行。 除非在另一個已啟用的索引中使用資料行，否則 sp_createstats 不會在已停用索引的第一個資料行上建立統計資料。 sp_createstats 不會在已停用叢集索引的資料表上建立統計資料。  
  
 當此資料表包含資料行集時，sp_createstats 就不會建立疏鬆資料行的統計資料。 如需資料行集和稀疏資料行的詳細資訊，請參閱 [使用資料行集](../../relational-databases/tables/use-column-sets.md) 和 [使用稀疏資料行](../../relational-databases/tables/use-sparse-columns.md)。  
  
## <a name="permissions"></a>權限  
 需要 db_owner 固定資料庫角色中的成員資格。  
  
## <a name="examples"></a>範例  
  
### <a name="a-create-single-column-statistics-on-all-eligible-columns"></a>A. 針對所有適合的資料行建立單一資料行統計資料  
 下列範例會針對目前資料庫中所有適合的資料行建立單一資料行統計資料。  
  
```  
EXEC sp_createstats;  
GO  
```  
  
### <a name="b-create-single-column-statistics-on-all-eligible-index-columns"></a>B. 針對所有適合的索引資料行建立單一資料行統計資料  
 下列範例會針對已經位於索引中而且不是索引中第一個資料行的所有適合資料行建立單一資料行統計資料。  
  
```  
EXEC sp_createstats 'indexonly';  
GO  
```  
  
## <a name="see-also"></a>另請參閱  
 [統計資料](../../relational-databases/statistics/statistics.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [ALTER DATABASE SET 選項 &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [&#40;Transact-sql&#41;的資料庫引擎預存程式 ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [系統預存程序 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
