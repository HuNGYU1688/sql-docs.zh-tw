---
description: SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)
title: SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET QUERY_GOVERNOR_COST_LIMIT
- SET_QUERY_GOVERNOR_COST_LIMIT_TSQL
- QUERY_GOVERNOR_COST_LIMIT
- QUERY_GOVERNOR_COST_LIMIT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET QUERY_GOVERNOR_COST_LIMIT statement
- connections [SQL Server], overriding
- QUERY_GOVERNOR_COST_LIMIT option
- overriding connection values
ms.assetid: 3424bb44-6915-462d-a8d7-fe834af81387
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bb7b16a18832bbdea950843409b0a17153c7a05b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "89547451"
---
# <a name="set-query_governor_cost_limit-transact-sql"></a>SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  覆寫目前針對目前連接所設定的 **查詢管理員成本限制值**。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```syntaxsql
  
SET QUERY_GOVERNOR_COST_LIMIT value  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
 *value*  
 這是指定查詢可以執行的最長時間之數值或整數值。 值會捨到最接近的整數。 負值會進位到 0。 查詢若超過該值的估計成本，查詢管理員就不允許執行此查詢。 將這個選項指定為 0 (預設值) 會關閉查詢管理員，並允許無限期地執行所有查詢。  
  
 「查詢成本」代表在特定的硬體組態上，預估完成查詢所需的時間 (以秒為單位)。  
  
## <a name="remarks"></a>備註  
 使用 SET QUERY_GOVERNOR_COST_LIMIT 只適用於目前的連接，在目前連接的期間會持續有效。 請利用 **sp_configure** 的 [設定查詢管理員成本限制伺服器組態選項](../../database-engine/configure-windows/configure-the-query-governor-cost-limit-server-configuration-option.md)選項來變更伺服器範圍的查詢管理員成本限制值。 如需有關設定這個選項的詳細資訊，請參閱 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 和[伺服器組態選項 &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)。  
  
 SET QUERY_GOVERNOR_COST_LIMIT 的設定是在執行階段進行設定，而不是在剖析階段進行設定。  
  
## <a name="permissions"></a>權限  
 需要 **public** 角色的成員資格。  
  
## <a name="see-also"></a>另請參閱  
 [SET 陳述式 &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
