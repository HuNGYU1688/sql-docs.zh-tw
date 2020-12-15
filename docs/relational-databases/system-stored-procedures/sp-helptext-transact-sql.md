---
description: sp_helptext (Transact-SQL)
title: sp_helptext (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_helptext
- sp_helptext_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helptext
ms.assetid: 24135456-05f0-427c-884b-93cf38dd47a8
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e6d2fbb2311392ee046b4aad84705b8ccd91a48c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468369"
---
# <a name="sp_helptext-transact-sql"></a>sp_helptext (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  顯示使用者自訂規則定義、預設值、未加密的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 預存程序、使用者自訂的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函數、觸發程序、計算資料行、CHECK 條件約束、檢視，或系統預存程序之類的系統物件。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```  
  
sp_helptext [ @objname = ] 'name' [ , [ @columnname = ] computed_column_name ]  
```  
  
## <a name="arguments"></a>引數  
`[ @objname = ] 'name'` 這是使用者定義之架構範圍物件的限定或非限定名稱。 只有在指定限定物件時，才會用到引號。 如果提供其中包括資料庫名稱的完整名稱，資料庫名稱就必須是目前資料庫的名稱。 這個物件必須在目前的資料庫中。 *名稱* 是 **Nvarchar (776)**，沒有預設值。  
  
`[ @columnname = ] 'computed_column_name'` 這是要顯示定義資訊之計算資料行的名稱。 包含資料行的資料表必須指定為 *名稱*。 *column_name* 是 **sysname**，沒有預設值。  
  
## <a name="return-code-values"></a>傳回碼值  
 0 (成功) 或 1 (失敗)  
  
## <a name="result-sets"></a>結果集  
  
|資料行名稱|資料類型|描述|  
|-----------------|---------------|-----------------|  
|**Text**|**nvarchar(255)**|物件定義|  
  
## <a name="remarks"></a>備註  
 sp_helptext 會顯示在多個資料列中建立物件所用的定義。 每一個資料列都包含 255 個字元的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 定義。 定義位於 [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)目錄檢視的 **定義** 資料行中。  
  
## <a name="permissions"></a>權限  
 需要 **public** 角色的成員資格。 系統物件定義是公開顯示的。 凡具有下列任一權限的物件擁有者或承授者，都看得到使用者物件的定義：ALTER、CONTROL、TAKE OWNERSHIP 或 VIEW DEFINITION。  
  
## <a name="examples"></a>範例  
  
### <a name="a-displaying-the-definition-of-a-trigger"></a>A. 顯示觸發程序的定義  
 下列範例會顯示 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 資料庫中 `dEmployee` 觸發程序的定義。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_helptext 'HumanResources.dEmployee';  
GO  
```  
  
### <a name="b-displaying-the-definition-of-a-computed-column"></a>B. 顯示計算資料行的定義  
 下列範例會顯示 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 資料庫中的 `TotalDue` 資料表的計算資料行 `SalesOrderHeader` 的定義。  
  
```  
USE AdventureWorks2012;  
GO  
sp_helptext @objname = N'AdventureWorks2012.Sales.SalesOrderHeader', @columnname = TotalDue ;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Text`  
  
 `---------------------------------------------------------------------`  
  
 `(isnull(([SubTotal]+[TaxAmt])+[Freight],(0)))`  
  
## <a name="see-also"></a>另請參閱  
 [&#40;Transact-sql&#41;的資料庫引擎預存程式 ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [系統預存程序 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
